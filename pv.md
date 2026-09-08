# CẨM NANG ÔN TẬP PHỎNG VẤN DEVOPS ENGINEER
### Tổng hợp kiến thức hệ thống: AWS · Kubernetes · GitHub Actions · ArgoCD · Terraform · Monitoring & Logging

> **Cách dùng tài liệu này:** Đây không phải bộ câu hỏi - đáp mà là tài liệu *xây dựng nền tảng tư duy*. Mỗi phần được tổ chức theo 5 lớp:
> 1. **Mô hình tư duy (Mental Model)** — cách "nhìn" công nghệ đó, giúp bạn suy luận ra câu trả lời thay vì học thuộc
> 2. **Kiến thức cốt lõi** — những gì bắt buộc phải nắm chắc, có cấu trúc
> 3. **Điểm phỏng vấn hay đào sâu** — nơi người phỏng vấn thường hỏi tiếp "tại sao", "nếu... thì sao"
> 4. **Thực chiến** — lệnh, cấu hình, thao tác cần làm được bằng tay
> 5. **Checklist tự kiểm tra** — tự đánh giá độ sẵn sàng trước buổi phỏng vấn
>
> **Nguyên tắc vàng khi trả lời phỏng vấn DevOps:** luôn trả lời theo cấu trúc *"Khái niệm → Cơ chế hoạt động → Trade-off → Kinh nghiệm thực tế của tôi"*. Người phỏng vấn senior đánh giá cao khả năng nêu trade-off hơn là khả năng đọc định nghĩa.

---

# PHẦN MỞ ĐẦU: BỨC TRANH TỔNG THỂ — 6 MẢNG NÀY GHÉP VỚI NHAU NHƯ THẾ NÀO?

Trước khi đi vào chi tiết, bạn cần trả lời được câu hỏi mở đầu kinh điển: *"Hãy mô tả một luồng từ lúc developer commit code đến lúc chạy trên production, và giám sát nó."* Đây là câu hỏi mà 6 mảng kiến thức trên chính là các mảnh ghép.

**Luồng end-to-end chuẩn (Push CI + Pull CD):**

```
[Dev commit code]
       │
       ▼
[GitHub Actions - CI]                     ← Mảng 3
   • Lint / Unit test / SAST scan
   • Build Docker image
   • Push image lên registry (ECR/GHCR) với tag = git SHA
   • Cập nhật tag image vào repo manifest (commit vào Git)
       │
       ▼
[Git repo manifest = SINGLE SOURCE OF TRUTH]
       │
       ▼
[ArgoCD - CD, pull-based]                 ← Mảng 4
   • Phát hiện Git thay đổi (webhook/poll)
   • Diff desired state (Git) vs live state (cluster)
   • Sync → apply vào cluster
       │
       ▼
[Kubernetes cluster]                      ← Mảng 2
   • Deployment → ReplicaSet → Pod
   • Rolling update, readiness probe gate traffic
   • Service/Ingress route traffic
       │
       ▼
[Hạ tầng bên dưới: AWS]                   ← Mảng 1
   • EKS node group, ALB, RDS, S3, IAM, VPC
   • Toàn bộ hạ tầng này được provision bởi Terraform   ← Mảng 5
       │
       ▼
[Monitoring & Logging]                    ← Mảng 6
   • Metrics: Prometheus → Grafana → Alertmanager → PagerDuty
   • Logs: Fluent Bit → Loki/ELK
   • Traces: OpenTelemetry → Tempo/Jaeger
   • SLO/Error Budget quyết định có được deploy tiếp hay không
```

**Điểm mấu chốt cần hiểu về ranh giới trách nhiệm** (đây là chỗ ứng viên hay trả lời lẫn lộn):

| Công cụ | Trách nhiệm | KHÔNG phải trách nhiệm |
|---|---|---|
| Terraform | Hạ tầng dài hạn, ít thay đổi (VPC, cluster, DB, IAM) | Deploy ứng dụng hàng ngày |
| GitHub Actions | CI: build, test, tạo artifact, commit thay đổi mong muốn | Không nên tự `kubectl apply` vào prod (nếu theo GitOps) |
| ArgoCD | CD: đảm bảo cluster khớp Git, phát hiện/sửa drift | Không build image, không quyết định "deploy gì" |
| Kubernetes | Chạy và duy trì workload theo declarative spec | Không tự biết ứng dụng "đúng" về mặt business |
| Monitoring | Cho biết hệ thống có khoẻ, ở đâu sai | Không tự sửa lỗi |

**Câu trả lời mẫu ngắn gọn (30 giây) bạn nên luyện nói trôi chảy:**
> "Developer merge code vào main, GitHub Actions build image tag theo git SHA, scan bảo mật rồi push lên ECR, sau đó cập nhật tag image vào repo manifest. ArgoCD theo dõi repo đó, phát hiện diff và sync vào EKS cluster — nghĩa là Git là source of truth, CI không cần credentials vào cluster production. Toàn bộ hạ tầng bên dưới (VPC, EKS, RDS, IAM) do Terraform quản lý theo layer riêng biệt. Về quan sát, tôi dùng Prometheus cho metric, Loki cho log, OpenTelemetry cho trace, tất cả nhìn qua Grafana, alert đi qua Alertmanager tới PagerDuty theo SLO đã định."

Nếu bạn nói được đoạn trên một cách tự nhiên, bạn đã vượt qua phần "screening" của hầu hết buổi phỏng vấn DevOps.

---

# PHẦN 1: AWS

## 1.1. Mô hình tư duy

Hãy nhìn AWS qua **5 lớp**, mọi câu hỏi AWS đều rơi vào một trong 5 lớp này. Khi được hỏi bất kỳ dịch vụ nào, hãy tự định vị nó thuộc lớp nào:

```
Lớp 5 — QUẢN TRỊ & CHI PHÍ:  Organizations, SCP, Cost Explorer, Budgets, Tagging
Lớp 4 — QUAN SÁT & BẢO MẬT:  CloudWatch, CloudTrail, Config, KMS, Secrets Manager, GuardDuty
Lớp 3 — ỨNG DỤNG:            EKS/ECS, Lambda, API Gateway, SQS/SNS, EventBridge
Lớp 2 — DỮ LIỆU:             RDS/Aurora, DynamoDB, S3, ElastiCache, EBS/EFS
Lớp 1 — NỀN TẢNG:            VPC, Subnet, Route Table, SG/NACL, IAM, Route 53
```

**Ba trục tư duy xuyên suốt mọi câu hỏi AWS:**

1. **Trục Availability**: AZ → Region → Multi-Region. Mọi thiết kế HA đều là câu hỏi "chịu được mất gì?" (mất 1 instance / mất 1 AZ / mất 1 region).
2. **Trục Security**: Identity (IAM) → Network (VPC/SG) → Data (KMS mã hoá). Ba lớp phòng thủ độc lập.
3. **Trục Cost**: Compute (Reserved/Spot/Savings Plan) → Storage (tiering) → Transfer (data egress thường bị bỏ quên nhưng rất tốn).

## 1.2. Kiến thức cốt lõi

### A. IAM — nền tảng bảo mật, được hỏi nhiều nhất

**4 khái niệm cần phân biệt rạch ròi:**

- **User**: danh tính có credentials dài hạn. Best practice: gần như không dùng cho application, chỉ cho con người (và tốt nhất là thay bằng SSO/Identity Center).
- **Role**: danh tính không có credentials cố định, được "assume" để lấy token tạm thời qua STS. Đây là cách đúng cho EC2, Lambda, EKS Pod (IRSA), CI/CD (qua OIDC).
- **Policy**: file JSON mô tả quyền. Gồm `Effect`, `Action`, `Resource`, `Condition`.
- **Trust Policy**: policy đặc biệt gắn trên Role, trả lời "ai được phép assume tôi".

**Policy Evaluation Logic — cần thuộc lòng thứ tự:**
```
1. Mặc định: DENY (implicit deny)
2. Có Explicit DENY ở BẤT KỲ đâu?  →  DENY ngay, không xét gì thêm
   (bao gồm: SCP, identity-based policy, resource-based policy, permission boundary, session policy)
3. Có Explicit ALLOW?               →  ALLOW
4. Không có ALLOW nào?              →  DENY
```
Câu hỏi bẫy hay gặp: *"Nếu IAM policy cho phép nhưng SCP chặn thì sao?"* → DENY. SCP là guardrail, không cấp quyền, chỉ giới hạn quyền tối đa. *"Root user có vượt được SCP không?"* → Không.

**Ba cơ chế giới hạn cần phân biệt:**

| Cơ chế | Phạm vi áp dụng | Mục đích |
|---|---|---|
| SCP | Toàn bộ account trong OU/Organization | Guardrail cấp tổ chức (VD: cấm tắt CloudTrail) |
| Permission Boundary | Một IAM User/Role cụ thể | Giới hạn quyền tối đa khi delegate việc tạo IAM |
| Session Policy | Một session STS cụ thể | Thu hẹp quyền cho một lần assume |

**IRSA (IAM Roles for Service Accounts)** — bắt buộc phải biết nếu phỏng vấn có EKS: Pod trong EKS có thể assume IAM Role thông qua ServiceAccount được annotate, dùng OIDC provider của cluster. Ưu điểm so với gán role cho cả node: least privilege ở mức Pod, không phải mọi Pod trên node đều có quyền như nhau.

### B. VPC — nền tảng network

**Cấu trúc chuẩn 3-tier (phải vẽ được trên whiteboard):**
```
VPC 10.0.0.0/16
├── Public Subnet   10.0.1.0/24 (AZ-a) , 10.0.2.0/24 (AZ-b)
│     └── ALB, NAT Gateway, Bastion
│     └── Route: 0.0.0.0/0 → Internet Gateway
├── Private Subnet  10.0.11.0/24 (AZ-a), 10.0.12.0/24 (AZ-b)
│     └── EC2/EKS node, ECS task
│     └── Route: 0.0.0.0/0 → NAT Gateway
└── DB Subnet       10.0.21.0/24 (AZ-a), 10.0.22.0/24 (AZ-b)
      └── RDS (Multi-AZ)
      └── Route: KHÔNG có route ra internet
```

**Security Group vs NACL — bảng so sánh phải nhớ:**

| | Security Group | Network ACL |
|---|---|---|
| Tầng | Instance (ENI) | Subnet |
| State | Stateful (return traffic tự allow) | Stateless (phải khai cả 2 chiều) |
| Rule | Chỉ ALLOW | ALLOW + DENY |
| Xử lý | Đánh giá tất cả rule | Theo thứ tự rule number, dừng ở match đầu tiên |
| Dùng khi | Kiểm soát chính, hàng ngày | Chặn IP độc hại ở mức subnet, defense-in-depth |

**Kết nối liên VPC — biết chọn đúng công cụ:**
- **VPC Peering**: 1-1, không transitive, đơn giản, rẻ. Dùng khi ít VPC.
- **Transit Gateway**: hub-and-spoke, transitive routing, hỗ trợ VPN/Direct Connect. Dùng khi nhiều VPC/nhiều account.
- **PrivateLink (Interface Endpoint)**: expose *một service* thay vì cả network. Dùng cho mô hình SaaS provider–consumer, bảo mật nhất.
- **VPC Endpoint (Gateway)**: chỉ cho S3 và DynamoDB, miễn phí, đi qua route table. Nên bật để tiết kiệm phí NAT Gateway.

**Mẹo ghi điểm về chi phí:** NAT Gateway tính phí theo giờ *và* theo GB xử lý. Traffic từ private subnet đi S3 mà không có Gateway Endpoint sẽ đi qua NAT → tốn tiền vô ích. Đây là một trong những lỗi cost phổ biến nhất, nêu ra sẽ được đánh giá cao.

### C. Compute — chọn đúng dịch vụ

```
Cần chạy code?
├── Event-driven, ngắn (<15 phút), scale bất thường     → Lambda
├── Container, muốn đơn giản, all-in AWS                → ECS (+ Fargate nếu không muốn quản node)
├── Container, đã dùng K8s / cần portable / ecosystem   → EKS
└── Cần kiểm soát OS, legacy app, license đặc thù       → EC2 (+ ASG)
```

**Mô hình giá EC2 — biết khi nào dùng gì:**
- **On-Demand**: baseline linh hoạt, workload không dự đoán được.
- **Savings Plan (Compute)**: cam kết $/giờ trong 1–3 năm, linh hoạt đổi instance family/region. → Lựa chọn mặc định tốt nhất cho baseline hiện nay.
- **Reserved Instance**: cam kết cứng theo instance type, ít linh hoạt hơn Savings Plan.
- **Spot**: rẻ tới ~90%, nhưng có thể bị thu hồi với 2 phút thông báo. Dùng cho batch, CI runner, stateless worker, K8s node group phụ. Chiến lược: diversify nhiều instance type + AZ để giảm rủi ro bị thu hồi đồng loạt.

**Load Balancer:**
- **ALB** (L7): routing theo host/path/header, WebSocket, tích hợp WAF/Cognito. Mặc định cho HTTP(S).
- **NLB** (L4): TCP/UDP, static IP, throughput cực cao, latency thấp. Dùng khi cần static IP hoặc non-HTTP.
- **GWLB**: chèn appliance bảo mật (firewall) vào luồng traffic — ít gặp nhưng biết tên là điểm cộng.

### D. Storage & Database

**S3 — chọn storage class:**
```
Truy cập thường xuyên              → Standard
Không rõ pattern truy cập          → Intelligent-Tiering  (an toàn nhất khi không chắc)
Ít truy cập, cần ngay khi cần      → Standard-IA
Ít truy cập, chấp nhận 1 AZ        → One Zone-IA
Archive, cần trong vài phút        → Glacier Instant/Flexible Retrieval
Archive dài hạn, chờ được vài giờ  → Glacier Deep Archive
```
Cần biết: **Lifecycle Policy** để tự động chuyển tier; **Versioning** + **MFA Delete** để chống xoá nhầm/ransomware; **Block Public Access** bật ở cấp account là guardrail cơ bản.

**RDS — Multi-AZ vs Read Replica (câu hỏi kinh điển, đừng nhầm):**

| | Multi-AZ | Read Replica |
|---|---|---|
| Replication | Đồng bộ (synchronous) | Bất đồng bộ (asynchronous) |
| Mục đích | HA / failover tự động | Scale đọc |
| Phục vụ traffic? | Standby KHÔNG nhận traffic (trừ Aurora) | Có, nhận read query |
| Cross-region? | Không (trong region) | Có |
| RPO/RTO | RPO ~0, RTO 1–2 phút | Có lag replication |

**DynamoDB — khi nào chọn:** cần scale ngang cực lớn, latency ổn định vài ms, truy vấn theo key. Điểm cần biết sâu: **partition key phải phân tán tốt** để tránh hot partition; **GSI vs LSI** (GSI khác partition key, eventually consistent; LSI cùng partition key, tạo lúc create table); **On-Demand vs Provisioned capacity**.

### E. Reliability & DR — 4 chiến lược (thuộc lòng bảng này)

| Chiến lược | RPO | RTO | Chi phí | Cách hoạt động |
|---|---|---|---|---|
| Backup & Restore | Giờ | Giờ–Ngày | Thấp nhất | Chỉ backup, dựng lại khi cần |
| Pilot Light | Phút | 10s phút | Thấp | Giữ DB replica chạy, phần còn lại tắt |
| Warm Standby | Giây–Phút | Phút | Trung bình | Bản thu nhỏ chạy sẵn, scale up khi failover |
| Multi-Site Active-Active | ~0 | ~0 | Cao nhất | Cả 2 region phục vụ traffic thật |

Khi được hỏi "bạn chọn cái nào?", câu trả lời đúng luôn là: *"Tuỳ RPO/RTO mà business yêu cầu và ngân sách — tôi sẽ hỏi trước: mất bao nhiêu phút dữ liệu là chấp nhận được, và downtime bao lâu thì ảnh hưởng doanh thu?"* Việc hỏi lại yêu cầu trước khi thiết kế là dấu hiệu của senior.

## 1.3. Điểm phỏng vấn hay đào sâu

1. **"Sao không dùng Access Key trong CI/CD?"** → Vì là credentials dài hạn, rò rỉ là mất kiểm soát. Dùng OIDC + `AssumeRoleWithWebIdentity` để lấy token ngắn hạn, trust policy giới hạn theo repo/branch cụ thể.
2. **"Instance trong private subnet update package thế nào?"** → Qua NAT Gateway; hoặc tốt hơn dùng VPC Endpoint + repo mirror nội bộ để tiết kiệm phí và tăng bảo mật.
3. **"ALB có static IP không?"** → Không, ALB dùng DNS name (IP thay đổi). Cần static IP thì dùng NLB, hoặc Global Accelerator.
4. **"S3 có strong consistency chưa?"** → Có, từ 2020 S3 đã strong read-after-write consistency cho mọi operation. Đây là câu bẫy kiến thức cũ.
5. **"Encryption at rest có bảo vệ khỏi IAM bị lộ không?"** → Không. Nếu IAM có quyền đọc và quyền dùng KMS key thì dữ liệu vẫn đọc được. Mã hoá bảo vệ khỏi truy cập tầng vật lý/storage, không thay thế cho IAM.
6. **"Làm sao biết ai đã xoá resource?"** → CloudTrail (API audit log), nên bật organization trail ghi vào account log archive riêng để không ai xoá dấu vết.

## 1.4. Thực chiến — cần làm được bằng tay

```bash
# Kiểm tra danh tính hiện tại đang dùng (bước đầu tiên khi debug quyền)
aws sts get-caller-identity

# Mô phỏng policy trước khi áp dụng — công cụ debug IAM cực hữu ích
aws iam simulate-principal-policy \
  --policy-source-arn arn:aws:iam::123456789012:role/MyRole \
  --action-names s3:GetObject --resource-arns arn:aws:s3:::my-bucket/*

# Tìm nguyên nhân AccessDenied qua CloudTrail
aws cloudtrail lookup-events --lookup-attributes AttributeKey=EventName,AttributeValue=PutObject

# Kiểm tra tình trạng target của ALB (debug 502/503)
aws elbv2 describe-target-health --target-group-arn <arn>

# Xem instance nào đang chạy, kèm type và AZ (rà soát nhanh)
aws ec2 describe-instances \
  --query 'Reservations[].Instances[].[InstanceId,InstanceType,Placement.AvailabilityZone,State.Name]' \
  --output table
```

**Debug 502/503 từ ALB — trình tự nên nói:** 503 thường là không có healthy target (kiểm tra target health, SG của target có mở port từ SG của ALB chưa); 502 thường là target trả response không hợp lệ hoặc đóng kết nối sớm (kiểm tra app log, idle timeout của ALB vs keep-alive của app).

## 1.5. Checklist tự kiểm tra AWS

- [ ] Vẽ được kiến trúc VPC 3-tier multi-AZ trên giấy trong 3 phút
- [ ] Giải thích được Policy Evaluation Logic không cần nhìn note
- [ ] Phân biệt được SG vs NACL, Multi-AZ vs Read Replica, ALB vs NLB
- [ ] Nêu được 4 chiến lược DR và trade-off RPO/RTO/chi phí
- [ ] Giải thích được OIDC thay Access Key trong CI/CD hoạt động ra sao
- [ ] Kể được ít nhất 3 cách tối ưu chi phí đã hoặc sẽ áp dụng
- [ ] Biết dùng CloudTrail để điều tra "ai làm gì lúc nào"

---

# PHẦN 2: KUBERNETES

## 2.1. Mô hình tư duy

**Nguyên lý số 1 của Kubernetes: Reconciliation Loop (vòng lặp hoà giải).**

Mọi thứ trong K8s đều là một biến thể của vòng lặp này:
```
   ┌─────────────────────────────────────────────┐
   │  Desired State (bạn khai báo trong YAML)   │
   └────────────────────┬────────────────────────┘
                        │ so sánh
   ┌────────────────────▼────────────────────────┐
   │  Current State (thực tế trong cluster)     │
   └────────────────────┬────────────────────────┘
                        │ nếu khác → hành động
                        └──────► lặp lại mãi mãi
```

Hiểu điều này giúp bạn trả lời được vô số câu hỏi: *Vì sao xoá Pod thủ công thì nó tự mọc lại?* (ReplicaSet controller reconcile). *Vì sao K8s tự phục hồi?* (level-triggered, không phụ thuộc việc bắt được event). *Operator là gì?* (chính là một reconciliation loop bạn tự viết cho resource của mình).

**Nguyên lý số 2: Mọi thứ đều đi qua API Server.** kubectl, controller, kubelet, ArgoCD — tất cả đều nói chuyện với API Server. Không có "cửa sau". Nên: bảo mật = bảo mật API Server (authn → authz/RBAC → admission control); debug = xem API Server thấy gì.

**Nguyên lý số 3: Phân biệt "declarative" và "imperative".** `kubectl apply` (declarative, idempotent, dùng cho CI/CD) vs `kubectl create/edit/scale` (imperative, dùng cho debug tạm thời). Trong GitOps, mọi thay đổi imperative đều là "drift" cần bị ghi đè.

## 2.2. Kiến thức cốt lõi

### A. Kiến trúc — vẽ được sơ đồ này

```
CONTROL PLANE                              WORKER NODE
┌──────────────────────────────┐          ┌──────────────────────────┐
│  kube-apiserver  ◄───────────┼──────────┤  kubelet                 │
│    (cửa duy nhất, authn/     │          │   (đảm bảo Pod chạy đúng │
│     authz/admission)         │          │    spec, báo cáo status) │
│         │                    │          │       │                  │
│         ▼                    │          │       ▼                  │
│  etcd (lưu TOÀN BỘ state)    │          │  container runtime       │
│                              │          │   (containerd/CRI-O)     │
│  kube-scheduler              │          │                          │
│   (chọn node cho Pod)        │          │  kube-proxy              │
│                              │          │   (iptables/IPVS rule    │
│  kube-controller-manager     │          │    cho Service)          │
│   (các reconciliation loop)  │          │                          │
│                              │          │  CNI plugin              │
│  cloud-controller-manager    │          │   (cấp IP cho Pod)       │
└──────────────────────────────┘          └──────────────────────────┘
```

**Luồng khi bạn chạy `kubectl apply -f deployment.yaml`** (câu hỏi rất hay được hỏi, hãy kể theo trình tự):
1. kubectl gửi request tới API Server
2. API Server: **Authentication** (bạn là ai — cert/token/OIDC) → **Authorization** (RBAC: bạn được làm gì) → **Admission Control** (Mutating: sửa object, VD inject sidecar; Validating: chấp nhận/từ chối, VD Kyverno policy)
3. API Server ghi object vào **etcd**
4. **Deployment controller** thấy Deployment mới → tạo ReplicaSet
5. **ReplicaSet controller** → tạo Pod object (chưa có node)
6. **Scheduler** thấy Pod chưa có `nodeName` → Filtering (loại node không đủ điều kiện) → Scoring (chấm điểm) → gán node
7. **kubelet** trên node đó thấy Pod được gán cho mình → gọi CRI tạo container, gọi CNI cấp IP, mount volume qua CSI
8. kubelet báo status về API Server → readiness probe pass → **Endpoint controller** thêm Pod IP vào Endpoints của Service → kube-proxy cập nhật rule → Pod bắt đầu nhận traffic

Kể được 8 bước này một cách trôi chảy là dấu hiệu ứng viên có hiểu biết thật, không học vẹt.

### B. Workload — chọn đúng loại

```
Stateless app, nhiều replica giống nhau        → Deployment
Cần định danh cố định + storage riêng mỗi Pod  → StatefulSet  (DB, Kafka, Zookeeper)
Cần chạy 1 Pod trên MỌI node                   → DaemonSet    (log agent, node exporter, CNI)
Chạy 1 lần rồi xong                            → Job
Chạy theo lịch                                 → CronJob
Cần canary/blue-green nâng cao                 → Rollout (Argo Rollouts CRD)
```

**StatefulSet — 3 đặc tính khác Deployment:**
1. Tên Pod ổn định, có thứ tự (`web-0`, `web-1`)
2. Mỗi Pod có PVC riêng, giữ nguyên qua restart
3. Tạo/xoá/update tuần tự (ordered), có thể cấu hình `podManagementPolicy: Parallel` nếu không cần

### C. Networking — phần khó nhất, cần nắm chắc

**4 tầng "phơi ra" (expose) workload:**
```
ClusterIP     → chỉ nội bộ cluster (mặc định)
NodePort      → mở port 30000-32767 trên MỌI node
LoadBalancer  → cloud provider tạo LB thật (mỗi Service = 1 LB = tốn tiền)
Ingress       → 1 LB duy nhất, route L7 theo host/path cho NHIỀU service
Gateway API   → chuẩn mới thay thế Ingress, tách vai trò infra-admin / app-dev
```

**Service hoạt động thế nào?** Service không phải một proxy chạy ở đâu đó — nó là một tập rule iptables/IPVS do kube-proxy tạo trên mỗi node. Traffic tới ClusterIP bị DNAT tới một Pod IP trong Endpoints. Vì vậy: Service không tự load balance thông minh (chỉ round-robin/random), không có retry, không biết health ở tầng app — đó là lý do người ta cần service mesh.

**DNS trong cluster:** `<service>.<namespace>.svc.cluster.local`. Cùng namespace thì gọi ngắn `<service>` là đủ. Headless Service (`clusterIP: None`) trả về IP của từng Pod thay vì ClusterIP — dùng cho StatefulSet để địa chỉ từng Pod cụ thể.

**Network Policy:** mặc định K8s cho phép mọi Pod nói với mọi Pod. Network Policy là cách áp dụng zero-trust. Điểm bẫy: **cần CNI hỗ trợ** (Calico, Cilium); một số CNI mặc định (như AWS VPC CNI thuần) không enforce Network Policy nếu không cài thêm.

### D. Scheduling — điều khiển Pod chạy ở đâu

| Cơ chế | Hướng | Ý nghĩa |
|---|---|---|
| nodeSelector | Pod → Node | Đơn giản, match label chính xác |
| Node Affinity | Pod → Node | Linh hoạt (In/NotIn/Exists), có soft (`preferred`) và hard (`required`) |
| Pod Affinity | Pod → Pod | Đặt gần Pod khác (giảm latency) |
| Pod Anti-Affinity | Pod ⇸ Pod | Tránh đặt cùng node/zone → tăng HA |
| Taint + Toleration | Node đẩy Pod ra | Dành riêng node (GPU node, node của team X) |
| Topology Spread | Pod phân bổ đều | Trải đều qua zone/node theo `maxSkew` |

**Mẫu HA thường dùng:** Pod Anti-Affinity `requiredDuringScheduling` theo `topologyKey: kubernetes.io/hostname` để đảm bảo không có 2 replica cùng node; hoặc `topologySpreadConstraints` theo `topology.kubernetes.io/zone` để trải đều qua AZ.

### E. Resource & QoS — nguồn gốc của rất nhiều sự cố production

```
requests → Scheduler dùng để chọn node (đây là "cam kết" tài nguyên)
limits   → Runtime enforce:
             • vượt memory limit → OOMKilled (container bị kill NGAY)
             • vượt CPU limit    → CPU throttling (chậm lại, KHÔNG bị kill)
```

**QoS Class quyết định thứ tự bị evict khi node thiếu tài nguyên:**
```
Guaranteed  (requests == limits cho cả CPU và memory)  → evict CUỐI CÙNG
Burstable   (có requests, limits khác hoặc thiếu)      → evict thứ hai
BestEffort  (không khai gì)                            → evict ĐẦU TIÊN
```

**Kinh nghiệm thực tế nên nêu khi phỏng vấn:** đặt memory `requests == limits` cho workload quan trọng (tránh OOM do node bị overcommit), nhưng với CPU thì thường **không nên set limit quá chặt** vì CPU throttling có thể gây latency spike khó debug — đây là một quan điểm thực chiến gây ấn tượng tốt.

### F. Probe — 3 loại, đừng nhầm

| Probe | Fail thì sao? | Dùng khi |
|---|---|---|
| **Liveness** | Restart container | App bị treo/deadlock, cần restart để cứu |
| **Readiness** | Gỡ khỏi Endpoints (không nhận traffic), KHÔNG restart | App đang warm-up, hoặc tạm mất dependency |
| **Startup** | Trì hoãn liveness/readiness cho tới khi pass | App khởi động chậm (JVM, migration), tránh bị liveness kill oan |

**Bẫy kinh điển:** đặt liveness probe gọi tới database. Nếu DB chậm/down → toàn bộ Pod bị restart hàng loạt → làm tình hình tệ hơn (cascading failure). Liveness chỉ nên kiểm tra "tiến trình của tôi còn sống", readiness mới nên kiểm tra dependency.

### G. Bảo mật K8s — 5 lớp

```
1. Authentication:  cert / OIDC (SSO) / ServiceAccount token
2. Authorization:   RBAC (Role/ClusterRole + RoleBinding/ClusterRoleBinding)
3. Admission:       Kyverno / OPA Gatekeeper (enforce policy)
4. Runtime:         SecurityContext (runAsNonRoot, readOnlyRootFilesystem,
                    drop capabilities), seccomp, Pod Security Standards
5. Network:         Network Policy (micro-segmentation)
```

**Pod Security Standards** (thay thế PodSecurityPolicy đã bị loại bỏ): 3 mức `privileged` / `baseline` / `restricted`, áp dụng theo namespace label. Nên biết vì PSP đã deprecated — nhắc đúng điều này thể hiện kiến thức cập nhật.

## 2.3. Điểm phỏng vấn hay đào sâu

1. **"Pod bị CrashLoopBackOff, debug thế nào?"** → Xem mục 2.4 (framework debug bên dưới) — đây gần như chắc chắn sẽ được hỏi.
2. **"Vì sao Pod ở Pending mãi?"** → Không đủ tài nguyên trên node nào; không match nodeSelector/affinity; có taint không tolerate; PVC không bind được (không có PV/StorageClass phù hợp); vượt ResourceQuota của namespace.
3. **"Deployment rolling update mà không downtime, cần gì?"** → readiness probe đúng (quan trọng nhất — không có nó thì K8s tưởng Pod sẵn sàng khi chưa), `maxUnavailable: 0` nếu muốn tuyệt đối an toàn, PDB, `terminationGracePeriodSeconds` đủ dài + app xử lý SIGTERM để drain connection, và `preStop` hook sleep ngắn để chờ endpoint được cập nhật.
4. **"Secret trong K8s có an toàn không?"** → Mặc định chỉ base64 (không phải mã hoá). Cần bật `EncryptionConfiguration` cho etcd; giới hạn RBAC; tốt nhất dùng External Secrets Operator/Vault để không lưu secret cố định trong etcd.
5. **"Khác biệt giữa evicted và OOMKilled?"** → OOMKilled là container vượt memory limit của chính nó (kernel kill). Evicted là kubelet chủ động đẩy Pod khỏi node vì *node* thiếu tài nguyên (node pressure), chọn theo QoS.
6. **"HPA và VPA dùng cùng nhau được không?"** → Xung đột nếu cùng dựa trên một metric, vì VPA sửa requests mà HPA lại tính % theo requests. Nên tách metric hoặc để VPA ở chế độ recommendation.

## 2.4. Thực chiến — framework debug (học thuộc trình tự này)

**Framework 5 bước debug Pod (dùng được cho mọi sự cố Pod):**
```bash
# 1. Nhìn tổng quan: Pod ở state gì, restart bao nhiêu lần
kubectl get pods -n <ns> -o wide

# 2. Xem EVENTS — 80% nguyên nhân nằm ở đây
kubectl describe pod <pod> -n <ns>
#    → tìm: FailedScheduling, ImagePullBackOff, OOMKilled, Unhealthy (probe fail)

# 3. Xem log, kể cả của lần chạy TRƯỚC khi crash
kubectl logs <pod> -n <ns> --previous
kubectl logs <pod> -c <container> -n <ns>   # nếu multi-container

# 4. Vào bên trong kiểm tra (nếu container còn chạy)
kubectl exec -it <pod> -n <ns> -- sh
#    hoặc nếu container không có shell / đã crash:
kubectl debug <pod> -n <ns> -it --image=busybox --target=<container>

# 5. Nhìn rộng ra: node có vấn đề không?
kubectl describe node <node>
#    → tìm: MemoryPressure, DiskPressure, Taints, Allocated resources
kubectl get events -n <ns> --sort-by='.lastTimestamp'
```

**Bảng chẩn đoán nhanh theo triệu chứng:**

| Triệu chứng | Nguyên nhân thường gặp |
|---|---|
| `Pending` | Thiếu resource / affinity không match / taint / PVC không bind / quota |
| `ImagePullBackOff` | Sai tên image/tag / thiếu imagePullSecrets / registry không truy cập được |
| `CrashLoopBackOff` | App lỗi khi start (xem `--previous` log) / thiếu env-config / liveness quá gắt / OOMKilled |
| `OOMKilled` | memory limit quá thấp hoặc app leak memory |
| `Evicted` | Node pressure (memory/disk) — kiểm tra `describe node` |
| `Terminating` mãi | Finalizer chưa xoá / volume không unmount được / process không nhận SIGTERM |
| Service không tới được Pod | Selector label không match Endpoints (`kubectl get endpoints <svc>` — nếu rỗng là dấu hiệu rõ nhất) / readiness fail / Network Policy chặn |

**Lệnh cần nhớ thêm:**
```bash
kubectl rollout status deployment/<name>        # theo dõi rollout
kubectl rollout undo deployment/<name>          # rollback nhanh
kubectl rollout history deployment/<name>
kubectl top pod / kubectl top node              # cần Metrics Server
kubectl get endpoints <svc>                     # kiểm tra Service có backend chưa
kubectl auth can-i create pods --as=system:serviceaccount:ns:sa   # debug RBAC
kubectl drain <node> --ignore-daemonsets --delete-emptydir-data   # bảo trì node
kubectl explain deployment.spec.strategy        # tra cứu schema tại chỗ
```

## 2.5. Checklist tự kiểm tra Kubernetes

- [ ] Vẽ được kiến trúc control plane + node và giải thích vai trò từng thành phần
- [ ] Kể trôi chảy 8 bước từ `kubectl apply` đến Pod nhận traffic
- [ ] Phân biệt rõ liveness / readiness / startup probe và bẫy khi dùng sai
- [ ] Giải thích QoS class và thứ tự evict
- [ ] Nắm 5 bước framework debug Pod, không cần nhìn note
- [ ] Phân biệt OOMKilled vs Evicted
- [ ] Nêu được cách đạt zero-downtime rolling update (đủ 5 yếu tố)
- [ ] Biết Network Policy cần CNI hỗ trợ
- [ ] Biết PSP đã bị thay bằng Pod Security Standards

---

# PHẦN 3: GITHUB ACTIONS

## 3.1. Mô hình tư duy

GitHub Actions chỉ là **"event → workflow → job → step"**. Nắm 4 tầng này là nắm 80%:

```
EVENT (push, PR, schedule, manual, webhook từ repo khác)
  └── WORKFLOW  (1 file YAML trong .github/workflows/)
        └── JOB  (chạy trên 1 runner riêng; mặc định SONG SONG; dùng `needs` để tuần tự)
              └── STEP  (tuần tự, cùng filesystem; `run` = shell, `uses` = gọi action)
```

**Ba điều gây lỗi nhiều nhất mà bạn phải nhớ:**
1. **Job không chia sẻ filesystem** — muốn truyền dữ liệu giữa job phải dùng artifact hoặc job outputs.
2. **Mỗi job bắt đầu từ máy trắng** — nên hầu như luôn cần `actions/checkout` đầu tiên.
3. **Secret không tự có trong PR từ fork** — đây là tính năng bảo mật, không phải bug.

## 3.2. Kiến thức cốt lõi

### A. Cấu trúc workflow chuẩn — cần viết được từ đầu không cần tra

```yaml
name: CI/CD Pipeline

on:
  push:
    branches: [main]
  pull_request:
    branches: [main]
  workflow_dispatch:          # cho phép chạy tay, có thể khai inputs

concurrency:                  # tránh chạy chồng chéo
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true

permissions:                  # least privilege — RẤT nên khai rõ
  contents: read
  id-token: write             # bắt buộc nếu dùng OIDC

env:
  IMAGE_NAME: my-app

jobs:
  test:
    runs-on: ubuntu-latest
    timeout-minutes: 15       # tránh job treo tốn phí
    strategy:
      matrix:
        node: [18, 20]
    steps:
      - uses: actions/checkout@v4
      - uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
          cache: 'npm'
      - run: npm ci
      - run: npm test

  build-and-push:
    needs: test               # chỉ chạy sau khi test pass
    if: github.event_name == 'push' && github.ref == 'refs/heads/main'
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write
    steps:
      - uses: actions/checkout@v4
      - uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789012:role/gha-deploy
          aws-region: ap-southeast-1
      - run: |
          docker build -t $IMAGE_NAME:${{ github.sha }} .
          docker push ...
```

### B. Trigger — biết chọn đúng và hiểu hệ quả bảo mật

| Trigger | Chạy khi | Có secret? | Lưu ý |
|---|---|---|---|
| `push` | Commit được push | Có | Dùng cho deploy sau merge |
| `pull_request` | PR mở/cập nhật | **Không** (nếu từ fork) | An toàn cho code chưa tin cậy |
| `pull_request_target` | PR, nhưng chạy context của base | **Có** | ⚠️ NGUY HIỂM nếu checkout code fork |
| `workflow_dispatch` | Chạy tay | Có | Có thể khai `inputs` |
| `schedule` | Cron | Có | Chỉ chạy trên default branch |
| `workflow_call` | Được workflow khác gọi | Truyền vào | Nền tảng của reusable workflow |
| `workflow_run` | Sau khi workflow khác xong | Có | Mẫu an toàn để xử lý PR từ fork |

**Lỗ hổng `pull_request_target` — phải hiểu để trả lời câu hỏi bảo mật:**
```
SAI (cực nguy hiểm):
  on: pull_request_target        ← có secret của repo đích
  steps:
    - uses: actions/checkout@v4
      with:
        ref: ${{ github.event.pull_request.head.sha }}   ← checkout code từ FORK
    - run: npm install && npm run build   ← script trong fork chạy được, đọc được secret

ĐÚNG (mẫu 2 workflow):
  Workflow 1: on: pull_request  → build/test code fork, KHÔNG có secret, upload artifact
  Workflow 2: on: workflow_run  → có secret, chỉ đọc artifact/kết quả, KHÔNG checkout code fork
```

### C. OIDC — chủ đề "must know" hiện nay

Cách cũ: lưu `AWS_ACCESS_KEY_ID` / `AWS_SECRET_ACCESS_KEY` trong GitHub Secrets → credentials dài hạn, rò rỉ là thảm hoạ.

Cách đúng:
```
1. Tạo IAM OIDC Provider trong AWS trỏ tới token.actions.githubusercontent.com
2. Tạo IAM Role với trust policy giới hạn theo claim:
     "token.actions.githubusercontent.com:sub": "repo:my-org/my-repo:ref:refs/heads/main"
3. Workflow khai permissions: id-token: write
4. Dùng aws-actions/configure-aws-credentials với role-to-assume
   → GitHub phát JWT ngắn hạn → AWS STS AssumeRoleWithWebIdentity → credentials tạm thời
```
**Điểm cần nhấn mạnh khi trả lời:** trust policy phải giới hạn `sub` cụ thể theo repo *và* branch/environment. Nếu để `repo:my-org/*:*` thì bất kỳ repo/branch nào trong org cũng assume được — đây là lỗi cấu hình phổ biến và nghiêm trọng.

### D. Tối ưu & tái sử dụng

**Caching:**
```yaml
- uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}
    restore-keys: ${{ runner.os }}-npm-      # fallback khi không trúng key chính xác
```
Nguyên tắc: `key` phải hash theo lock file (đổi dependency → cache mới); `restore-keys` để dùng cache gần đúng làm base.

**Reusable Workflow vs Composite Action:**

| | Reusable Workflow | Composite Action |
|---|---|---|
| Là gì | Cả 1 workflow (nhiều job) | Nhóm nhiều step thành 1 action |
| Gọi ở đâu | Ở cấp `jobs:` | Ở cấp `steps:` (`uses:`) |
| Dùng khi | Chuẩn hoá cả pipeline giữa nhiều repo | Đóng gói vài step lặp lại |
| Runner | Tự khai runner riêng | Chạy trên runner của job gọi nó |

**Runner:**
- GitHub-hosted: sạch mỗi lần, không bảo trì, tính phí theo phút cho private repo.
- Self-hosted: kiểm soát tài nguyên/mạng nội bộ, tiết kiệm ở quy mô lớn. **Bắt buộc**: dùng ephemeral runner và **không bao giờ** cho chạy PR từ fork public — nếu không, kẻ tấn công có thể thực thi code trong mạng nội bộ của bạn.
- Autoscale self-hosted trên K8s: **Actions Runner Controller (ARC)** — scale theo job trong queue, scale-to-zero.

### E. Bảo mật — checklist supply chain (chủ đề rất được ưa hỏi)

```
1. Pin action theo commit SHA đầy đủ, không dùng @main / @v4 cho action bên thứ ba
   uses: some-org/some-action@a1b2c3d4e5f6...   ✅
2. permissions mặc định = read-only ở cấp Organization
3. Dùng OIDC, loại bỏ static cloud credentials
4. Environment Protection Rule + required reviewers cho production
5. Không dùng pull_request_target với checkout code fork
6. Allowlist action được phép ở cấp Organization
7. Self-hosted runner: ephemeral + cô lập network
8. Branch protection cho thư mục .github/workflows
9. Bật Dependabot + secret scanning
10. Không echo secret ra log; nhớ rằng secret bị mask nhưng có thể lộ nếu biến đổi (VD base64)
```

## 3.3. Điểm phỏng vấn hay đào sâu

1. **"Truyền dữ liệu giữa 2 job thế nào?"** → `jobs.<id>.outputs` cho giá trị nhỏ, artifact cho file. Không dùng filesystem vì khác runner.
2. **"Vì sao workflow không chạy?"** → File không nằm trong `.github/workflows/` trên branch được trigger; `on:` không match; `schedule` chỉ hoạt động trên default branch; workflow bị disable; lỗi cú pháp YAML.
3. **"Làm sao không cho deploy vào cuối tuần?"** → Thêm step kiểm tra thời gian + `exit 1`, kèm input `force_deploy` cho hotfix; hoặc quản lý qua Environment approval.
4. **"Secret có an toàn tuyệt đối không?"** → Không. Người có quyền sửa workflow có thể exfiltrate secret. Vì vậy cần branch protection cho workflow file và environment secret giới hạn theo branch.
5. **"CI nên hay không nên trực tiếp deploy vào K8s?"** → Nêu trade-off push vs pull (xem phần ArgoCD).

## 3.4. Checklist tự kiểm tra GitHub Actions

- [ ] Viết được một workflow CI/CD hoàn chỉnh từ đầu, không tra cứu
- [ ] Giải thích được lỗ hổng `pull_request_target` và mẫu 2-workflow an toàn
- [ ] Giải thích được luồng OIDC 4 bước và vai trò của claim `sub`
- [ ] Phân biệt Reusable Workflow vs Composite Action
- [ ] Nêu được 5+ điểm trong checklist bảo mật supply chain
- [ ] Biết cách tối ưu thời gian CI (cache, matrix shard, self-hosted, path filter)

---

# PHẦN 4: ARGOCD & GITOPS

## 4.1. Mô hình tư duy

**GitOps = 4 nguyên tắc (theo OpenGitOps):**
1. **Declarative** — toàn bộ hệ thống được mô tả khai báo
2. **Versioned & Immutable** — lưu trong Git, có lịch sử, không sửa trực tiếp
3. **Pulled automatically** — agent trong cluster tự kéo về (không push từ ngoài vào)
4. **Continuously reconciled** — liên tục so sánh và tự sửa drift

**Câu chốt cần nói được:** *"GitOps đảo chiều luồng deploy: thay vì CI có credentials để push vào cluster, agent trong cluster tự pull từ Git. Nhờ đó không cần cấp credentials cluster production cho hệ thống CI bên ngoài, và mọi thay đổi đều có audit trail tự nhiên từ Git history."*

**Push vs Pull — bảng trade-off (câu hỏi rất hay được hỏi):**

| | Push (CI tự `kubectl apply`) | Pull (ArgoCD) |
|---|---|---|
| Credentials cluster | CI phải giữ → bề mặt tấn công lớn | Không cần → an toàn hơn |
| Drift detection | Không có | Tự phát hiện, có thể tự sửa (self-heal) |
| Audit | Log CI | Git history + sync history |
| Độ trễ deploy | Tức thì | Theo chu kỳ poll (giảm bằng webhook) |
| Độ phức tạp | Thấp, dễ bắt đầu | Thêm 1 hệ thống phải vận hành |
| Multi-cluster | Khó quản lý | Rất mạnh (ApplicationSet) |

## 4.2. Kiến thức cốt lõi

### A. Kiến trúc ArgoCD

```
┌─────────────────────────────────────────────────┐
│  API Server        — UI, CLI, authn/authz, RBAC │
│  Repo Server       — clone Git, render manifest │
│                      (Helm/Kustomize) ← BOTTLENECK│
│  Application Ctrl  — reconcile: diff & sync     │
│  Redis             — cache                      │
│  Dex (tuỳ chọn)    — SSO/OIDC                   │
└─────────────────────────────────────────────────┘
```
Khi được hỏi scale ArgoCD: **Repo Server** thường là nghẽn đầu tiên (render Helm/Kustomize tốn CPU) → scale replica; **Application Controller** hỗ trợ sharding theo cluster.

### B. Hai trạng thái phải phân biệt rõ (câu hỏi kinh điển)

```
SYNC STATUS  — cấu hình cluster có khớp Git không?
   Synced / OutOfSync / Unknown

HEALTH STATUS — resource có đang hoạt động tốt không?
   Healthy / Progressing / Degraded / Suspended / Missing / Unknown
```
**Ma trận cần hiểu:**
- `Synced` + `Healthy` → lý tưởng
- `Synced` + `Degraded` → config đúng như Git nhưng app chạy lỗi (bug app, thiếu resource, dependency down) → **vấn đề không nằm ở ArgoCD**
- `OutOfSync` + `Healthy` → có drift hoặc Git vừa thay đổi chưa sync
- `Unknown` health → thường là CRD không có health check → cần viết Custom Health Check bằng Lua

### C. Sync Policy — 3 công tắc

```yaml
syncPolicy:
  automated:
    prune: true        # xoá resource không còn trong Git
    selfHeal: true     # tự ghi đè thay đổi thủ công (drift)
  syncOptions:
    - CreateNamespace=true
    - ApplyOutOfSyncOnly=true
  retry:
    limit: 5
    backoff: {duration: 5s, factor: 2, maxDuration: 3m}
```
**Khuyến nghị thực tế:** dev/staging bật `automated + selfHeal + prune`; production nhiều tổ chức để manual sync hoặc automated nhưng cân nhắc kỹ `prune` (tránh xoá ngoài ý muốn khi có lỗi merge).

### D. Thứ tự và hook

```
Sync Wave (annotation argocd.argoproj.io/sync-wave: "-1", "0", "1"...)
  → wave nhỏ apply trước, chờ Healthy rồi mới sang wave sau
  → dùng cho: Namespace/CRD/Secret trước, Deployment sau

Sync Hook (annotation argocd.argoproj.io/hook)
  PreSync   → DB migration (chạy trước khi deploy code mới)
  Sync      → cùng lúc
  PostSync  → smoke test sau khi healthy
  SyncFail  → gửi thông báo khi sync thất bại

Hook Delete Policy (argocd.argoproj.io/hook-delete-policy)
  HookSucceeded / HookFailed / BeforeHookCreation
  → dọn Job cũ, tránh tích luỹ rác
```

### E. Tổ chức quy mô lớn

**AppProject** — đơn vị multi-tenancy, phải nắm để trả lời câu hỏi bảo mật:
```yaml
spec:
  sourceRepos: ['https://github.com/my-org/team-a-*']   # chỉ repo này
  destinations:
    - namespace: 'team-a-*'                              # chỉ namespace này
      server: 'https://prod-cluster'
  clusterResourceWhitelist: []                           # KHÔNG cho tạo cluster-scoped
  namespaceResourceBlacklist:
    - group: '', kind: 'ResourceQuota'
```
Nếu không giới hạn, một Application có thể deploy vào `kube-system` hoặc tạo `ClusterRoleBinding` cấp quyền admin — đây là rủi ro cần nêu.

**ApplicationSet** — tự sinh nhiều Application từ template:
```
List Generator     → danh sách cứng
Git Generator      → mỗi thư mục/file trong repo = 1 Application (tự thêm service mới không cần sửa gì)
Cluster Generator  → mỗi cluster đã đăng ký = 1 Application (deploy agent lên cả fleet)
Matrix Generator   → tổ hợp 2 generator (VD: service × cluster)
PR Generator       → mỗi Pull Request = 1 preview environment (tự xoá khi PR đóng)
```
**Progressive Rollout** với `strategy: RollingSync` — deploy tuần tự theo nhóm cluster (canary trước, rồi từng region), dừng lại nếu nhóm trước không Healthy. Đây là câu trả lời cho "làm sao update cả fleet mà không rủi ro đồng loạt".

### F. Secret trong GitOps — vấn đề nan giải cần biết 3 giải pháp

| Giải pháp | Cách hoạt động | Trade-off |
|---|---|---|
| **Sealed Secrets** | Mã hoá thành SealedSecret, chỉ controller trong cluster đích giải mã được | Đơn giản; nhưng gắn với cluster cụ thể, khó rotate |
| **SOPS** (+ KMS/age) | Mã hoá từng field trong YAML, giải mã lúc render | Git-friendly, diff được; cần plugin/CMP |
| **External Secrets Operator / Vault** | Git chỉ lưu *reference*, giá trị thật lấy runtime từ Vault/Secrets Manager | Tốt nhất cho enterprise: rotation, audit, dynamic secret; thêm hệ thống phải vận hành |

### G. Promotion giữa các môi trường (GitOps thuần)

```
Cấu trúc repo manifest:
  apps/my-service/
    base/                    ← manifest chung (Kustomize base)
    overlays/dev/            ← patch riêng dev
    overlays/staging/
    overlays/prod/

Luồng promotion:
  CI build image → cập nhật tag ở overlays/dev  (tự động)
        ↓ soak time + smoke test pass
  PR copy tag từ dev → staging  (tự động hoặc review nhẹ)
        ↓ verify
  PR copy tag từ staging → prod  (BẮT BUỘC review + approve)
```
**Điểm cốt lõi cần nói:** trong GitOps, "deploy" = "merge PR". Không có nút deploy nào ngoài Git. Điều đó khiến mọi lần deploy đều có review, audit và rollback bằng `git revert`.

## 4.3. Điểm phỏng vấn hay đào sâu

1. **"Rollback trong ArgoCD làm sao?"** → 2 cách: `argocd app rollback` (nhanh, cho khẩn cấp — nhưng Git và cluster tạm lệch nhau) hoặc `git revert` rồi để ArgoCD sync (đúng triết lý GitOps, giữ Git là source of truth). Nêu được cả hai và biết khi nào dùng cái nào là điểm cộng lớn.
2. **"Application cứ nhảy qua lại Synced/OutOfSync liên tục?"** → Flapping do một controller/webhook khác liên tục sửa field (HPA sửa `replicas`, sidecar injector thêm container). Khắc phục: `ignoreDifferences` cho field đó. Ví dụ:
   ```yaml
   ignoreDifferences:
     - group: apps
       kind: Deployment
       jsonPointers: ['/spec/replicas']   # để HPA quản lý
   ```
3. **"ArgoCD chết thì production có sập không?"** → Không. Workload vẫn chạy bình thường; chỉ mất khả năng deploy mới và tự sửa drift. Đây là điểm mạnh của kiến trúc pull.
4. **"Một ArgoCD trung tâm hay mỗi cluster một cái?"** → Trade-off: trung tâm dễ quan sát tổng thể nhưng là single point of failure và phải lưu credentials nhiều cluster (rủi ro nếu bị compromise); phân tán cô lập tốt hơn nhưng khó nhìn tổng thể và tốn công vận hành nhiều instance.
5. **"ArgoCD vs Flux?"** → Cùng triết lý pull-based. ArgoCD mạnh về UI, khái niệm Application/Project, multi-tenancy trực quan. Flux nhẹ hơn, thiên toolkit/CRD-first, tích hợp tốt với Flagger. Chọn theo văn hoá team.

## 4.4. Thực chiến

```bash
argocd app list
argocd app get <app>                    # trạng thái sync + health chi tiết
argocd app diff <app>                   # xem chênh lệch Git vs cluster
argocd app sync <app> --prune
argocd app sync <app> --resource apps:Deployment:my-deploy   # sync 1 resource
argocd app history <app>                # lịch sử sync
argocd app rollback <app> <revision>
argocd app set <app> --sync-policy automated --self-heal
argocd app terminate-op <app>           # huỷ sync đang treo
argocd repo add https://... --username x --password y
argocd cluster add <kube-context>
```

## 4.5. Checklist tự kiểm tra ArgoCD

- [ ] Nói được 4 nguyên tắc GitOps và bảng trade-off push vs pull
- [ ] Phân biệt Sync Status vs Health Status, giải thích được ma trận 4 ô
- [ ] Biết Sync Wave và Hook dùng cho tình huống nào
- [ ] Biết AppProject giới hạn gì và vì sao quan trọng về bảo mật
- [ ] Kể được 4–5 loại Generator của ApplicationSet
- [ ] Nêu 3 giải pháp secret trong GitOps kèm trade-off
- [ ] Giải thích 2 cách rollback và khi nào dùng cách nào
- [ ] Biết cách xử lý Application flapping bằng `ignoreDifferences`

---

# PHẦN 5: TERRAFORM

## 5.1. Mô hình tư duy

**Terraform = 3 thực thể và mối quan hệ giữa chúng.** Hầu hết mọi vấn đề Terraform đều là sự lệch pha giữa 3 thứ này:

```
        CODE (.tf)                  ← bạn muốn gì
            │
            │  plan = so sánh 3 chiều
            ▼
        STATE (.tfstate)            ← Terraform NGHĨ là đang có gì
            │
            ▼
        THỰC TẾ (cloud provider)    ← thực sự đang có gì
```

| Lệch pha | Hiện tượng | Xử lý |
|---|---|---|
| Code ≠ State | plan báo create/update/destroy | Bình thường — apply |
| State ≠ Thực tế | **Drift** (ai đó sửa tay) | plan sẽ hiện thay đổi để đưa về code |
| Thực tế có, State không có | Resource "mồ côi" | `terraform import` |
| State có, thực tế không có | Resource bị xoá tay | plan sẽ tạo lại |

Nắm mô hình 3 chiều này, bạn tự trả lời được mọi câu hỏi về `import`, `state rm`, `state mv`, drift, và "vì sao plan hiện thay đổi lạ".

**Nguyên lý thứ hai: `plan` là hợp đồng.** `terraform plan -out=tfplan` rồi `terraform apply tfplan` đảm bảo áp dụng đúng thứ đã được review — đây là practice bắt buộc trong CI/CD production.

## 5.2. Kiến thức cốt lõi

### A. Vòng đời và các lệnh

```
init      → tải provider, cấu hình backend, tải module
validate  → kiểm tra cú pháp (không cần credentials)
fmt       → format chuẩn
plan      → tính toán thay đổi (refresh state + so sánh)
apply     → thực thi
destroy   → xoá toàn bộ resource trong state
```

**Ký hiệu trong output plan — phải đọc được ngay:**
```
+  create              → tạo mới
-  destroy             → xoá
~  update in-place     → sửa tại chỗ, không gián đoạn
-/+ destroy & recreate  → ⚠️ XOÁ RỒI TẠO LẠI (nguy hiểm với stateful resource!)
+/- create then destroy → tạo mới trước rồi xoá (do create_before_destroy)
<= read                 → data source
```
Thấy `-/+` với RDS/EBS/EIP là phải dừng lại kiểm tra ngay — đây là nơi mất dữ liệu production.

### B. State — phần quan trọng nhất

**Remote backend chuẩn (AWS):**
```hcl
terraform {
  required_version = "~> 1.9"
  required_providers {
    aws = { source = "hashicorp/aws", version = "~> 5.0" }
  }
  backend "s3" {
    bucket         = "my-tfstate"
    key            = "prod/network/terraform.tfstate"
    region         = "ap-southeast-1"
    encrypt        = true
    dynamodb_table = "terraform-locks"    # state locking
    # (Terraform mới hỗ trợ use_lockfile = true dùng S3 native locking)
  }
}
```
**Ba yêu cầu bắt buộc cho state production:** (1) remote, (2) mã hoá, (3) có locking. Nên bật thêm **S3 versioning** — đây là phao cứu sinh khi state bị corrupt.

**State chứa dữ liệu nhạy cảm dạng plaintext** (password RDS, private key...) — điểm này rất hay được hỏi. Vì vậy: mã hoá at-rest, giới hạn IAM đọc state nghiêm ngặt, không commit state vào Git.

**Các lệnh state cần biết chính xác tác dụng:**
```bash
terraform state list                          # liệt kê resource trong state
terraform state show <address>                # xem chi tiết
terraform state mv <old> <new>                # đổi địa chỉ (refactor) — KHÔNG động tới cloud
terraform state rm <address>                  # bỏ khỏi quản lý — KHÔNG xoá trên cloud
terraform import <address> <id>               # đưa resource sẵn có vào state
terraform force-unlock <lock-id>              # ⚠️ chỉ khi chắc chắn không ai đang apply
terraform state pull > backup.tfstate         # backup thủ công trước khi thao tác nguy hiểm
```
**Quy tắc an toàn:** luôn `terraform state pull > backup.json` trước khi chạy bất kỳ lệnh `state mv/rm/force-unlock`.

**Refactor không destroy — cách hiện đại (Terraform 1.1+):** dùng block `moved` trong code thay vì lệnh thủ công, vì nó declarative và review được qua PR:
```hcl
moved {
  from = aws_instance.web
  to   = module.web.aws_instance.this
}
```
Tương tự, Terraform 1.5+ có block `import` khai báo trong code.

### C. Cấu trúc dự án — thiết kế được là điểm senior

```
terraform/
├── modules/                        # module tái sử dụng nội bộ
│   ├── vpc/
│   ├── eks/
│   └── rds/
└── live/                           # từng state riêng biệt
    ├── prod/
    │   ├── 01-network/    ← state riêng, ít thay đổi
    │   ├── 02-security/   ← IAM, KMS
    │   ├── 03-data/       ← RDS, S3 (stateful, cẩn trọng)
    │   └── 04-platform/   ← EKS, ALB
    └── staging/
        └── ...
```

**Nguyên tắc chia state — nói được 3 tiêu chí:**
1. **Theo tần suất thay đổi** — network (hiếm đổi) tách khỏi application (đổi liên tục)
2. **Theo blast radius** — resource stateful (DB) tách riêng để không bị ảnh hưởng bởi apply khác
3. **Theo quyền sở hữu** — mỗi team một state riêng, giảm tranh chấp lock

**Trade-off nhiều state nhỏ vs một state lớn:**

| | Nhiều state nhỏ | Một state lớn |
|---|---|---|
| Blast radius | Nhỏ ✅ | Toàn hệ thống ❌ |
| Tốc độ plan/apply | Nhanh ✅ | Chậm khi lớn ❌ |
| Tranh chấp lock | Ít ✅ | Nhiều ❌ |
| Quản lý phụ thuộc | Phải dùng remote_state, thủ công hơn ❌ | Terraform tự lo ✅ |

**Workspace — cảnh báo:** Workspace chia state nhưng *dùng chung code*. Với môi trường có khác biệt cấu hình lớn, phần lớn thực hành tốt là **tách thư mục** thay vì Workspace. Nói được điều này thể hiện kinh nghiệm thực tế.

### D. Cú pháp cần thành thạo

```hcl
# count vs for_each
resource "aws_instance" "a" { count = 3 }                    # index: [0],[1],[2]
resource "aws_instance" "b" { for_each = toset(["x","y"]) }  # key: ["x"],["y"]
```
**Vì sao ưu tiên `for_each`?** Với `count`, xoá phần tử giữa danh sách làm mọi index sau dịch chuyển → Terraform tưởng phải destroy/recreate hàng loạt. `for_each` dùng key ổn định nên không bị vấn đề này. Đây là câu hỏi hay và câu trả lời này rất được đánh giá.

```hcl
# lifecycle — 3 tuỳ chọn phải biết
lifecycle {
  create_before_destroy = true                  # giảm downtime khi thay thế
  prevent_destroy       = true                  # chặn xoá resource quan trọng
  ignore_changes        = [tags["LastModified"]] # bỏ qua field bị hệ thống khác sửa
}

# dynamic block — sinh nested block động
dynamic "ingress" {
  for_each = var.allowed_ports
  content {
    from_port   = ingress.value
    to_port     = ingress.value
    protocol    = "tcp"
    cidr_blocks = ["10.0.0.0/8"]
  }
}

# tham chiếu output từ state khác (khi chia nhiều layer)
data "terraform_remote_state" "network" {
  backend = "s3"
  config  = { bucket = "my-tfstate", key = "prod/network/terraform.tfstate", region = "ap-southeast-1" }
}
# dùng: data.terraform_remote_state.network.outputs.vpc_id
```

**`depends_on` dùng khi nào?** Chỉ khi có phụ thuộc *ẩn* mà Terraform không suy luận được qua tham chiếu attribute (VD: resource A cần IAM policy của B đã tồn tại nhưng code không tham chiếu attribute nào của B).

### E. Version pinning — tránh "hôm qua chạy, hôm nay lỗi"

```hcl
required_version = "~> 1.9.0"          # cho phép 1.9.x
version = "~> 5.0"                     # provider: cho phép 5.x, không lên 6.x
```
Và **commit `.terraform.lock.hcl` vào Git** — file này pin chính xác version + checksum provider, đảm bảo mọi người và CI dùng đúng một version. Đây là điểm nhiều ứng viên bỏ sót.

### F. Testing & Compliance

```
Tầng 1 — fmt / validate            : cú pháp (nhanh, chạy mọi PR)
Tầng 2 — tflint / checkov / tfsec  : static analysis, phát hiện SG mở 0.0.0.0/0, S3 public
Tầng 3 — plan + policy check       : OPA/Conftest/Sentinel trên `terraform show -json tfplan`
Tầng 4 — terraform test (1.6+) / Terratest : apply thật vào sandbox rồi verify và destroy
```
**Compliance as Code — ví dụ policy cần enforce:** bắt buộc tag `Owner`/`CostCenter`; cấm SG inbound 0.0.0.0/0 với port 22/3389; cấm S3 public; giới hạn instance type theo whitelist; bắt buộc bật encryption.

### G. CI/CD cho Terraform — pipeline chuẩn

```
PR mở:
  1. terraform fmt -check && terraform validate
  2. tflint / checkov (static scan)
  3. terraform plan -out=tfplan  →  post plan output vào comment PR
  4. conftest/OPA kiểm tra plan JSON  →  fail nếu vi phạm policy
  5. Yêu cầu review + approve

Sau merge:
  6. non-prod: apply tự động
  7. prod: chờ manual approval (Environment protection) → terraform apply tfplan
           (dùng ĐÚNG plan file đã review, không plan lại)
  8. concurrency group theo state để tránh 2 apply chồng nhau
```

## 5.3. Điểm phỏng vấn hay đào sâu

1. **"State bị mất/corrupt, hạ tầng vẫn chạy — làm gì?"** → (a) Khôi phục từ S3 versioning (lý do phải bật versioning); (b) nếu không có backup: liệt kê resource thực tế, viết lại code, `import` từng resource, chạy plan tới khi sạch (không diff); (c) rút kinh nghiệm: bật versioning + backup định kỳ.
2. **"Apply bị kill giữa chừng?"** → Terraform ghi state sau mỗi resource nên thường không mất nhiều; state có thể còn lock (`force-unlock` nếu chắc chắn không ai đang chạy). Chạy `plan` để đối chiếu, xem kỹ có resource nào đã tạo mà state chưa ghi (cần import) trước khi apply tiếp. **Không** chạy `-auto-approve` trong tình huống này.
3. **"Plan hiện destroy/recreate hàng loạt mà không ai sửa code?"** → Kiểm tra: vừa nâng version provider (changelog có thể đổi force-new behavior)? vừa update module bên ngoài? có ai sửa tay? Nếu là thay đổi hợp lệ, đánh giá rủi ro mất dữ liệu trước khi apply, cân nhắc `create_before_destroy` hoặc migrate dữ liệu trước.
4. **"`-target` có nên dùng?"** → Chỉ cho tình huống khẩn cấp. Rủi ro: bỏ qua dependency graph → state không nhất quán. Luôn theo sau bằng một lần apply đầy đủ.
5. **"Secret trong Terraform xử lý sao?"** → Không hardcode; đọc từ Vault/Secrets Manager qua data source; nhưng giá trị *vẫn* vào state → bắt buộc mã hoá state + giới hạn quyền. Với secret cực nhạy cảm, để runtime (Vault injector) xử lý, đừng để Terraform biết.
6. **"CloudFormation vs Terraform?"** → CFN native AWS, rollback tự động, AWS quản state; Terraform multi-cloud, module ecosystem lớn, nhưng phải tự quản state và không có auto-rollback.
7. **"Provisioner có nên dùng?"** → Hạn chế. Không idempotent, khó debug. Ưu tiên `user_data`/cloud-init, hoặc Ansible/Packer, hoặc để K8s lo phần cấu hình app.

## 5.4. Checklist tự kiểm tra Terraform

- [ ] Giải thích mô hình 3 chiều Code–State–Thực tế và 4 kiểu lệch pha
- [ ] Đọc được mọi ký hiệu trong plan output, đặc biệt `-/+`
- [ ] Viết được backend S3 + DynamoDB lock từ đầu
- [ ] Nêu 3 tiêu chí chia state và trade-off nhiều state vs một state
- [ ] Giải thích vì sao `for_each` tốt hơn `count`
- [ ] Biết chính xác `state mv` vs `state rm` vs `import` làm gì
- [ ] Biết `moved` block và vì sao tốt hơn `state mv` thủ công
- [ ] Nhớ phải commit `.terraform.lock.hcl`
- [ ] Kể được pipeline Terraform CI/CD 8 bước có approval
- [ ] Trả lời được tình huống state mất và apply bị gián đoạn

---

# PHẦN 6: MONITORING & LOGGING (OBSERVABILITY)

## 6.1. Mô hình tư duy

**3 trụ cột và câu hỏi mà mỗi trụ cột trả lời:**

```
METRICS  →  "CÓ vấn đề không? Mức độ ra sao?"
            Số theo thời gian, nhẹ, rẻ, tổng hợp tốt, dùng để ALERT
            Điểm yếu: cardinality thấp (không được gắn user_id, request_id)

LOGS     →  "Chuyện GÌ đã xảy ra? Chi tiết ra sao?"
            Sự kiện chi tiết, đắt (storage), dùng để ĐIỀU TRA
            Điểm yếu: khó tổng hợp, dễ nổ chi phí

TRACES   →  "Vấn đề Ở ĐÂU trong chuỗi service?"
            Hành trình 1 request qua nhiều service, dùng để KHOANH VÙNG
            Điểm yếu: cần instrument code, tốn khi sample 100%
```

**Sợi dây kết nối 3 trụ cột: Trace ID.** Đây là câu trả lời cho "làm sao debug nhanh trong microservices": alert từ metric → nhảy sang trace của request lỗi → nhảy sang log của span đó (nhờ log có gắn `trace_id`). Grafana làm được liên kết chéo này (metric → trace → log) và đó là lý do người ta gom về một UI.

**Nguyên lý: alert theo triệu chứng, debug theo nguyên nhân.**
- Alert nên gắn với thứ người dùng cảm nhận: error rate tăng, latency P99 tăng, đơn hàng không xử lý được.
- Không nên alert vào "CPU 80%" — CPU cao mà user vẫn ổn thì không cần ai thức dậy lúc 3h sáng. CPU là thứ để *xem khi debug*, không phải để *đánh thức người*.

## 6.2. Kiến thức cốt lõi

### A. Golden Signals & USE — 2 framework phải thuộc

**Golden Signals (Google SRE) — cho service/ứng dụng:**
```
Latency     — thời gian phản hồi (TÁCH RIÊNG request thành công và lỗi,
              vì request lỗi thường nhanh bất thường, làm đẹp số liệu giả tạo)
Traffic     — lượng request/giây
Errors      — tỷ lệ lỗi (5xx, và cả lỗi logic trả về 200 nhưng sai)
Saturation  — mức "đầy" của tài nguyên (queue depth, connection pool, CPU/mem)
```

**USE Method (Brendan Gregg) — cho tài nguyên hạ tầng:**
```
Utilization — % thời gian tài nguyên bận
Saturation  — độ dài hàng chờ khi tài nguyên quá tải
Errors      — số lỗi của tài nguyên đó
```
Dùng Golden Signals cho service, USE cho node/disk/network. Nêu được cả hai và biết dùng cái nào ở đâu là dấu hiệu ứng viên đọc SRE nghiêm túc.

### B. SLI / SLO / SLA / Error Budget — chủ đề "phân loại" senior

```
SLI  = chỉ số ĐO ĐƯỢC          VD: tỷ lệ request trả 2xx/3xx trong 5 phút
SLO  = MỤC TIÊU nội bộ         VD: 99.9% trong 30 ngày rolling
SLA  = CAM KẾT với khách hàng  VD: 99.5%, vi phạm thì hoàn tiền
       (SLA luôn "lỏng" hơn SLO để có buffer)

Error Budget = 100% − SLO
  SLO 99.9% / 30 ngày  →  budget ≈ 43 phút downtime/tháng
```

**Error Budget Policy — cách dùng thực tế (rất đáng nói trong phỏng vấn):**
```
Budget còn nhiều  →  được release nhanh, thử nghiệm, chấp nhận rủi ro
Budget gần cạn    →  freeze feature release, ưu tiên việc ổn định hoá
Budget đã cạn     →  dừng release trừ hotfix, dồn lực vào reliability
```
Đây là cơ chế biến tranh luận "dev muốn nhanh vs ops muốn ổn định" thành một quyết định *định lượng, khách quan* thay vì cảm tính. Nói được ý này rất ghi điểm.

### C. Prometheus — cơ chế và những điểm hay bị hỏi

**Mô hình pull:** Prometheus chủ động scrape `/metrics` của target theo `scrape_interval`. Ưu điểm: dễ biết target nào down (scrape fail), không cần target biết địa chỉ Prometheus, dễ debug (curl thẳng endpoint). Push Gateway chỉ dành cho **job ngắn hạn** (batch/cron kết thúc trước khi kịp bị scrape) — đừng dùng nó thay thế mô hình pull.

**4 loại metric:**
```
Counter    — chỉ tăng (tổng số request, tổng lỗi). Luôn dùng với rate()
Gauge      — lên xuống tự do (memory đang dùng, số connection)
Histogram  — bucket theo khoảng → tính percentile được ở phía server (aggregate được)
Summary    — percentile tính tại client → KHÔNG aggregate được giữa nhiều instance
```
**Vì sao ưu tiên Histogram hơn Summary?** Vì có 10 instance, bạn không thể lấy trung bình các P99 của từng instance để ra P99 toàn hệ thống — điều đó sai về toán học. Histogram giữ bucket nên có thể cộng lại rồi tính percentile chính xác. Đây là câu hỏi phân loại rất tốt.

**PromQL — nắm những mẫu này là đủ dùng:**
```promql
# Tỷ lệ request/giây (LUÔN dùng rate với counter)
rate(http_requests_total[5m])

# Error rate (%) — SLI kinh điển
sum(rate(http_requests_total{status=~"5.."}[5m]))
  / sum(rate(http_requests_total[5m])) * 100

# Latency P99 từ histogram
histogram_quantile(0.99, sum(rate(http_request_duration_seconds_bucket[5m])) by (le))

# CPU throttling của container (nguyên nhân latency spike hay bị bỏ sót)
rate(container_cpu_cfs_throttled_seconds_total[5m])

# Pod restart trong 1 giờ
increase(kube_pod_container_status_restarts_total[1h]) > 0

# Dự báo disk đầy trong 4 giờ tới
predict_linear(node_filesystem_avail_bytes[6h], 4*3600) < 0
```
**Bẫy:** `rate()` vs `irate()` vs `increase()`. `rate` = trung bình trên khoảng (dùng cho alert/dashboard, mượt); `irate` = tức thời (dùng cho biểu đồ chi tiết, nhiễu hơn); `increase` = tổng tăng trong khoảng (số tuyệt đối).

**Cardinality — vấn đề #1 làm sập Prometheus.** Mỗi tổ hợp label duy nhất = 1 time series. Gắn `user_id`, `request_id`, `email`, `url đầy đủ có ID` vào label → hàng triệu series → Prometheus hết RAM. **Quy tắc: giá trị unbounded thuộc về LOG/TRACE, không thuộc về METRIC.**

**HA & scale:**
```
HA cơ bản: 2 Prometheus giống nhau scrape song song (không đồng bộ với nhau),
           Alertmanager tự dedupe alert trùng
Scale/long-term: Thanos hoặc Mimir/Cortex
   → đẩy block ra S3 (lưu trữ dài hạn, rẻ)
   → global query view (truy vấn nhiều cluster như một)
   → downsampling cho dữ liệu cũ
```

### D. Alerting — thiết kế alert tốt

```yaml
- alert: HighErrorRate
  expr: |
    sum(rate(http_requests_total{status=~"5.."}[5m]))
      / sum(rate(http_requests_total[5m])) > 0.05
  for: 5m                      # chống flapping — RẤT quan trọng
  labels:
    severity: critical
    team: payments             # để Alertmanager route đúng người
  annotations:
    summary: "Error rate {{ $value | humanizePercentage }} trên 5%"
    runbook_url: "https://wiki/runbooks/high-error-rate"    # BẮT BUỘC có
```

**Tiêu chí "alert tốt" — 5 câu tự hỏi:**
1. Nó phản ánh triệu chứng người dùng cảm nhận được không? (symptom-based)
2. Nếu kêu lúc 3h sáng, có cần thức dậy làm gì ngay không? Nếu không → hạ severity hoặc bỏ.
3. Có runbook chỉ rõ phải làm gì chưa?
4. `for:` có đủ dài để không kêu vì dao động tạm thời?
5. Nó có bị trùng lặp với alert khác không? (Alertmanager `inhibit_rules` để chặn alert con khi alert cha đã kêu — VD cluster down thì không cần 200 alert pod down)

**Alertmanager làm 4 việc:** grouping (gom nhiều alert liên quan thành 1 thông báo), inhibition (chặn alert phụ thuộc), silencing (tắt tạm khi bảo trì), routing (định tuyến theo label tới team/kênh đúng).

**Alert Fatigue** — vấn đề văn hoá quan trọng: quá nhiều false positive → người ta phớt lờ alert → cuối cùng bỏ sót sự cố thật. Nghĩa là false positive nhiều *gián tiếp* tạo ra false negative. Cách chữa: định kỳ review alert nào kêu nhiều nhất mà không cần hành động → xoá hoặc sửa ngưỡng.

### E. Logging

**Structured logging là bắt buộc:**
```json
{"ts":"2026-09-07T10:00:00Z","level":"ERROR","service":"payment-api",
 "trace_id":"abc123","user_id":"u_789","msg":"charge failed",
 "error":"gateway timeout","duration_ms":3021}
```
So với `"ERROR: charge failed for user u_789"` — bản JSON cho phép query chính xác theo field, không cần regex, và **có `trace_id` để liên kết sang trace**.

**Kiến trúc thu thập log trong K8s:**
```
DaemonSet agent (Fluent Bit / Vector)  ← mặc định nên chọn
   • 1 agent/node, đọc /var/log/containers/*.log
   • Hiệu quả tài nguyên, không nhân bản theo Pod
   • Tự enrich metadata K8s (namespace, pod, labels)

Sidecar
   • Chỉ khi app ghi log ra file trong container (không ra stdout),
     hoặc cần xử lý riêng biệt cho từng app
   • Tốn tài nguyên hơn nhiều
```
**Nguyên tắc app trong container:** luôn ghi log ra **stdout/stderr**, không ghi vào file. Việc lưu/vận chuyển là của hạ tầng.

**Loki vs Elasticsearch:**

| | Loki | Elasticsearch |
|---|---|---|
| Index | Chỉ index label (metadata) | Full-text index toàn bộ |
| Chi phí lưu trữ | Thấp | Cao |
| Tài nguyên vận hành | Nhẹ | Nặng (JVM, shard, tuning) |
| Search nội dung | Chậm hơn (grep-style trên chunk) | Rất nhanh, mạnh |
| Phù hợp | Log của app, dùng chung Grafana | Cần search/analytics phức tạp, SIEM |
| Cardinality | ⚠️ Cũng bị vấn đề label cardinality như Prometheus | Không |

**Kiểm soát chi phí log — 6 đòn:**
1. Production không ghi DEBUG
2. Sampling log thành công (nhưng **luôn giữ 100% log lỗi**)
3. Tiered storage: hot 7 ngày → warm 30 ngày → cold/archive
4. Bỏ field dư thừa, tránh log cả payload lớn
5. Retention khác nhau theo loại log (audit log giữ lâu, debug log giữ ngắn)
6. Định kỳ audit: log nào chưa ai từng query → xem xét bỏ

### F. Tracing & OpenTelemetry

```
Trace  = toàn bộ hành trình 1 request
Span   = 1 đơn vị công việc trong trace (có parent-child, duration, attributes)
Context propagation = truyền trace context qua ranh giới service
   • HTTP: header W3C `traceparent` (chuẩn hiện nay)
   • Message queue: PHẢI tự nhúng vào message attribute — đây là nơi trace hay bị "đứt"
```

**OpenTelemetry (OTel)** là chuẩn CNCF thống nhất API/SDK/protocol cho cả metrics, logs, traces. Giá trị: instrument một lần, xuất đi đâu cũng được (Jaeger, Tempo, Datadog...) → tránh vendor lock-in. Kiến trúc: SDK trong app → **OTel Collector** (nhận, xử lý, sampling, xuất) → backend.

**Sampling — hiểu 2 loại:**
```
Head-based  — quyết định NGAY khi request bắt đầu (VD giữ 10% ngẫu nhiên)
              Rẻ, đơn giản, nhưng có thể bỏ mất chính request lỗi mà bạn cần
Tail-based  — quyết định SAU KHI trace hoàn tất, ở Collector
              → giữ 100% trace có lỗi hoặc latency cao, sample thấp trace bình thường
              → tốn tài nguyên hơn nhưng đây là câu trả lời "cost-effective observability"
```

### G. Incident Management & vận hành

```
MTTD (Detect)   — mất bao lâu để PHÁT HIỆN     → cải thiện bằng monitoring/alert tốt hơn
MTTR (Resolve)  — mất bao lâu để KHẮC PHỤC     → cải thiện bằng runbook, tracing, tự động hoá
MTBF            — khoảng giữa 2 sự cố          → cải thiện bằng chất lượng hệ thống
```

**Quy trình incident chuẩn:**
```
Detect (alert) → Triage (đánh giá severity) → Declare incident
→ Chỉ định Incident Commander (chỉ điều phối, KHÔNG tự tay debug)
→ Mitigate TRƯỚC (rollback/scale/failover — giảm đau cho user)
→ Root cause SAU (điều tra khi hệ thống đã ổn)
→ Blameless Postmortem (tập trung vào hệ thống & quy trình, không chỉ trích cá nhân)
→ Action items có người chịu trách nhiệm và deadline
```
**Điểm rất quan trọng để nói:** *mitigate trước, root cause sau*. Nhiều ứng viên trả lời sai thứ tự này (cố tìm nguyên nhân trong khi user đang chịu ảnh hưởng). Ưu tiên số 1 lúc sự cố là dừng thiệt hại — rollback trước, hiểu sau.

**Monitoring the monitoring — Dead Man's Switch:** một alert luôn kêu định kỳ (heartbeat) gửi tới dịch vụ *bên ngoài hoàn toàn* (VD healthchecks.io); nếu dịch vụ đó **không** nhận được heartbeat → nó tự cảnh báo. Đây là cách duy nhất phát hiện khi cả hệ thống monitoring của bạn sập (vì lúc đó nó không thể tự báo về chính mình).

## 6.3. Điểm phỏng vấn hay đào sâu

1. **"Dashboard xanh nhưng user báo chậm — vì sao?"** → (a) đang xem *average* che khuất P99, hoặc vấn đề chỉ ở một phân khúc (một region/tenant); (b) vấn đề ở client-side (frontend, mạng người dùng) → cần Real User Monitoring; (c) điểm mù: một dependency (API bên thứ ba, DNS, CDN) chưa được instrument. Đây là câu trả lời "monitoring blind spot" rất được đánh giá.
2. **"Alert quá nhiều, xử lý sao?"** → Đo tần suất alert, xoá alert không actionable, chuyển từ cause-based sang symptom-based, thêm `for:`, dùng inhibition/grouping, thiết lập SLO-based alerting (multi-window multi-burn-rate) thay vì ngưỡng tĩnh.
3. **"Vì sao không nên alert CPU cao?"** → Không phản ánh trải nghiệm người dùng. CPU cao mà latency ổn thì không phải sự cố. Nên alert vào latency/error rate; CPU là dữ liệu để debug.
4. **"Chi phí observability tăng gấp 3 sau khi lên microservices, làm gì?"** → Tail-based sampling cho trace, giảm cardinality metric, tắt DEBUG log ở prod, tiered storage, review dashboard/alert không dùng, cân nhắc Loki thay ES nếu không cần full-text search.
5. **"Log lộ dữ liệu cá nhân (PII) thì sao?"** → Redaction ngay tại nguồn (trong app hoặc ở Collector/Fluent Bit filter), không log payload thô, giới hạn quyền truy cập log, có retention policy đáp ứng quy định.

## 6.4. Checklist tự kiểm tra Monitoring & Logging

- [ ] Giải thích 3 trụ cột và Trace ID là sợi dây liên kết
- [ ] Thuộc Golden Signals và USE Method, biết dùng cái nào ở đâu
- [ ] Phân biệt SLI/SLO/SLA, tính được Error Budget và nêu Error Budget Policy
- [ ] Giải thích vì sao Histogram tốt hơn Summary khi có nhiều instance
- [ ] Viết được 3 câu PromQL cơ bản (rate, error rate, histogram_quantile)
- [ ] Giải thích cardinality và quy tắc "giá trị unbounded không thuộc metric"
- [ ] Nêu 5 tiêu chí alert tốt + 4 việc Alertmanager làm
- [ ] Biết DaemonSet vs sidecar cho log và nguyên tắc log ra stdout
- [ ] So sánh Loki vs Elasticsearch
- [ ] Phân biệt head-based vs tail-based sampling
- [ ] Nói được quy trình incident, đặc biệt "mitigate trước, root cause sau"
- [ ] Biết Dead Man's Switch

---

# PHẦN 7: SYSTEM DESIGN & TÌNH HUỐNG — nơi quyết định level của bạn

Phần này thường chiếm 40–50% điểm ở vòng phỏng vấn Senior. Kiến thức rời rạc không đủ; điều họ đánh giá là **cách bạn suy nghĩ có hệ thống**.

## 7.1. Framework trả lời câu hỏi thiết kế (áp dụng cho mọi đề)

```
BƯỚC 1 — HỎI LẠI YÊU CẦU (đừng bao giờ nhảy vào giải pháp ngay)
   • Quy mô? (RPS, số user, dung lượng dữ liệu, tăng trưởng dự kiến)
   • Yêu cầu SLO? (uptime, latency mục tiêu)
   • RPO/RTO cho DR?
   • Ngân sách và quy mô team vận hành? (team 3 người ≠ team 30 người)
   • Ràng buộc compliance? (dữ liệu phải ở trong nước? PCI/HIPAA?)
   • Đang có gì rồi? (greenfield hay migrate?)

BƯỚC 2 — VẼ KIẾN TRÚC TỔNG THỂ (high-level trước, chi tiết sau)
   Traffic vào → Edge (CDN/WAF) → LB → Compute → Data → Async

BƯỚC 3 — ĐI SÂU 2–3 ĐIỂM QUAN TRỌNG NHẤT
   Không dàn trải; chọn phần rủi ro nhất và đào sâu

BƯỚC 4 — NÊU TRADE-OFF RÕ RÀNG
   "Tôi chọn X vì A, đánh đổi là B; nếu yêu cầu đổi thành C thì tôi sẽ chọn Y"

BƯỚC 5 — VẬN HÀNH & QUAN SÁT (phần ứng viên hay quên → cơ hội ghi điểm)
   Monitoring gì? Alert gì? Deploy thế nào? Rollback thế nào? DR thế nào?

BƯỚC 6 — CHI PHÍ
   Ước lượng thô + 2–3 đòn tối ưu
```

## 7.2. Đề mẫu 1: "Thiết kế nền tảng CI/CD cho 30 microservice, 5 team"

**Khung trả lời:**

```
Repo & Git strategy
   • App code: repo riêng theo service (hoặc monorepo + path filter nếu team muốn)
   • Manifest: repo riêng (tách khỏi app code) → tránh vòng lặp CI trigger vô hạn
     khi CI commit tag image vào chính repo đó
   • Trunk-based + short-lived branch, PR bắt buộc review

CI (GitHub Actions)
   • Reusable workflow dùng chung cho cả 30 service → chuẩn hoá, sửa 1 nơi
   • Các bước: lint → unit test → build image (tag = git SHA) → scan (Trivy)
     → sign image (cosign) → push ECR → cập nhật manifest repo
   • OIDC cho AWS, không static key
   • Self-hosted runner qua ARC trên K8s để tiết kiệm và truy cập mạng nội bộ

CD (ArgoCD)
   • ApplicationSet + Git Generator → thêm service mới không cần cấu hình tay
   • AppProject theo team → cô lập quyền
   • dev: auto-sync + self-heal | prod: manual/approval + prune cân nhắc
   • Argo Rollouts cho service quan trọng (canary + analysis tự động)

Môi trường
   • dev (auto) → staging (auto sau smoke test) → prod (PR + approve)
   • Preview environment cho mỗi PR bằng ApplicationSet PR Generator,
     tự xoá khi PR đóng

Guardrail
   • Kyverno: bắt buộc resource limits, cấm image tag `latest`,
     chỉ cho phép registry nội bộ, bắt buộc label owner
   • Branch protection cho manifest repo + workflow files

Quan sát
   • DORA metrics: deployment frequency, lead time, change failure rate, MTTR
   • Alert khi ArgoCD Application Degraded, khi pipeline fail rate tăng
```

## 7.3. Đề mẫu 2: "Thiết kế hạ tầng cho ứng dụng web 100k user, cần 99.9%"

```
Yêu cầu suy ra: 99.9% ≈ 43 phút downtime/tháng → cần multi-AZ, không cần multi-region

Edge:     Route 53 → CloudFront (+ WAF) → ALB
Compute:  EKS 3 AZ, node group: on-demand cho baseline + spot cho workload chịu lỗi
          HPA theo RPS/latency + Cluster Autoscaler/Karpenter
          PDB + Pod Anti-Affinity theo zone
Data:     RDS Multi-AZ (writer) + read replica cho báo cáo
          ElastiCache Redis cho session/cache
          S3 cho static + user upload (versioning + lifecycle)
Async:    SQS + worker deployment riêng, có DLQ
IaC:      Terraform chia layer: network / security / data / platform
CI/CD:    GitHub Actions + ArgoCD như đề 1
Observ.:  Prometheus + Grafana + Loki + OTel; SLO 99.9% availability, P99 < 500ms
DR:       Warm standby hoặc Pilot Light ở region 2 tuỳ RTO;
          RDS automated backup + snapshot cross-region; test restore định kỳ
Security: WAF, SG least privilege, IRSA, KMS mã hoá, Secrets Manager,
          không có public subnet cho app/DB
Cost:     Savings Plan cho baseline, Spot cho worker, S3 lifecycle,
          VPC Endpoint cho S3 (tránh phí NAT), right-sizing theo metric
```

## 7.4. Framework xử lý sự cố tổng quát (dùng cho mọi câu "hệ thống chậm/lỗi, bạn làm gì?")

Đây là câu hỏi gần như chắc chắn xuất hiện. Đừng nhảy vào chi tiết kỹ thuật — hãy trình bày *phương pháp*:

```
0. XÁC ĐỊNH PHẠM VI & MỨC ĐỘ (30 giây đầu)
   Ai bị ảnh hưởng? Tất cả user hay một nhóm? Bắt đầu từ khi nào?
   → Quyết định severity và có cần declare incident không

1. HỎI CÂU QUAN TRỌNG NHẤT: "CÓ GÌ VỪA THAY ĐỔI?"
   Deploy? config change? feature flag? cert vừa hết hạn?
   scale event? thay đổi hạ tầng? nhà cung cấp bên thứ ba?
   → ~70% sự cố production đến từ một thay đổi gần đây

2. MITIGATE NGAY nếu có nghi vấn rõ ràng
   Rollback deploy / tắt feature flag / scale up / failover
   → Dừng thiệt hại TRƯỚC, hiểu nguyên nhân SAU

3. KHOANH VÙNG THEO TẦNG (đi từ ngoài vào trong)
   DNS/CDN → LB (5xx? target healthy?) → Ingress → Pod (restart? OOM?)
   → App (log, trace) → Dependency (DB slow query? cache miss? API bên ngoài?)
   → Hạ tầng (node pressure? disk full? network?)

4. DÙNG DỮ LIỆU, KHÔNG ĐOÁN
   Metric: khi nào bắt đầu lệch? Trace: span nào chậm? Log: lỗi gì lặp lại?
   Tương quan thời điểm bắt đầu với timeline thay đổi

5. GHI CHÉP TIMELINE TRONG LÚC XỬ LÝ
   Phục vụ postmortem sau này

6. SAU SỰ CỐ: BLAMELESS POSTMORTEM
   Timeline, root cause, những gì làm chậm quá trình xử lý,
   action items cụ thể (bao gồm bổ sung alert/runbook nếu phát hiện điểm mù)
```

**Ba câu hỏi tình huống hay gặp và hướng trả lời cốt lõi:**

| Tình huống | Điểm cần nêu đầu tiên |
|---|---|
| "API latency tăng đột ngột, traffic không tăng" | Có gì vừa deploy/thay đổi? → kiểm tra tầng: ALB metric → Pod (CPU throttling!) → DB (slow query, connection pool) → dependency bên ngoài |
| "Toàn bộ Pod bị Evicted" | Node pressure (memory/disk) → `describe node`; kiểm tra ai dùng vượt requests, disk có bị đầy do log/image |
| "Deploy xong 5xx tăng, cần xử lý" | Rollback NGAY (mitigate trước), rồi mới điều tra bằng log/trace của version lỗi |

## 7.5. Câu hỏi hành vi (Behavioral) — dùng STAR

Chuẩn bị **4 câu chuyện** từ kinh nghiệm thật, mỗi câu 2 phút, theo cấu trúc STAR (Situation → Task → Action → Result). Con số cụ thể là thứ làm câu chuyện đáng tin.

```
Câu chuyện 1 — SỰ CỐ PRODUCTION bạn xử lý
   Nhấn: cách khoanh vùng, quyết định mitigate, và bài học đã biến thành cải tiến gì
   (VD: "sau đó tôi thêm alert X và runbook Y, MTTR giảm từ 45 xuống 15 phút")

Câu chuyện 2 — TỰ ĐỘNG HOÁ / CẢI TIẾN bạn chủ động làm
   Nhấn: vấn đề đo được trước/sau
   (VD: "CI từ 22 phút xuống 7 phút nhờ cache + matrix shard, tiết kiệm ~40 giờ chờ/tháng")

Câu chuyện 3 — LỖI BẠN TỪNG GÂY RA
   Đừng chọn lỗi vô hại giả tạo. Nhấn: bạn đã sửa thế nào và thay đổi
   quy trình/hệ thống gì để không lặp lại (đây là điểm họ thực sự đo)

Câu chuyện 4 — BẤT ĐỒNG QUAN ĐIỂM với dev/team khác
   Nhấn: bạn dùng dữ liệu để thuyết phục, và sẵn sàng thay đổi ý kiến khi có
   dữ liệu ngược lại ("disagree and commit")
```

## 7.6. Câu hỏi nên hỏi lại người phỏng vấn (thể hiện độ chín)

- Hiện tại quy trình deploy lên production diễn ra thế nào, mất bao lâu từ merge tới prod?
- Team đang dùng SLO không, và error budget có ảnh hưởng tới quyết định release?
- On-call rotation ra sao, tần suất bị gọi ban đêm trung bình thế nào?
- Điều gì đang là nguồn "toil" (việc lặp lại thủ công) lớn nhất của team hiện nay?
- Tỷ lệ thời gian team dành cho việc mới so với việc vận hành/khắc phục?
- 3–6 tháng đầu, thành công với vị trí này trông như thế nào?

Câu hỏi về on-call và toil cho thấy bạn hiểu công việc DevOps thật, không chỉ nghĩ nó là "viết YAML".

---

# PHẦN 8: LỘ TRÌNH ÔN 7 NGÀY & BẢNG TỰ ĐÁNH GIÁ

## 8.1. Lộ trình 7 ngày (khoảng 2–3 giờ/ngày)

| Ngày | Nội dung | Sản phẩm đầu ra bắt buộc |
|---|---|---|
| **1** | AWS: IAM, VPC, compute, LB, DR | Vẽ tay kiến trúc VPC 3-tier multi-AZ; đọc to Policy Evaluation Logic |
| **2** | Kubernetes phần 1: kiến trúc, workload, networking | Kể trôi chảy 8 bước từ `kubectl apply` → Pod nhận traffic |
| **3** | Kubernetes phần 2: scheduling, resource/QoS, probe, bảo mật, **debug** | Thuộc framework 5 bước debug + bảng chẩn đoán theo triệu chứng |
| **4** | Terraform: state, chia layer, cú pháp, CI/CD, tình huống state | Viết từ đầu backend S3+lock; trả lời được "state bị mất thì làm gì" |
| **5** | GitHub Actions + ArgoCD | Viết 1 workflow CI/CD hoàn chỉnh; giải thích push vs pull + `pull_request_target` |
| **6** | Monitoring: Golden Signals, SLO/Error Budget, PromQL, alerting, log | Viết 3 câu PromQL; tính error budget của SLO 99.9%; nêu 5 tiêu chí alert tốt |
| **7** | **Luyện nói**: 2 đề system design + framework sự cố + 4 câu chuyện STAR | Nói to (hoặc ghi âm) toàn bộ; tự nghe lại và cắt phần lan man |

**Nguyên tắc quan trọng nhất của 7 ngày này:** dành ít nhất 1/3 thời gian **nói to** thay vì chỉ đọc. Phỏng vấn là bài kiểm tra diễn đạt, không phải bài kiểm tra đọc hiểu. Rất nhiều ứng viên biết câu trả lời nhưng trình bày rời rạc và bị đánh giá thấp hơn thực lực.

## 8.2. Bảng tự đánh giá cuối cùng

Tự cho điểm 1–5 (5 = giải thích trôi chảy cho người khác được). Bất kỳ mục nào ≤ 3 → ưu tiên ôn lại.

| # | Năng lực | Điểm |
|---|---|---|
| 1 | Mô tả end-to-end từ commit → production → monitoring trong 60 giây | |
| 2 | Vẽ và giải thích kiến trúc VPC 3-tier multi-AZ | |
| 3 | IAM Policy Evaluation Logic + phân biệt SCP/Permission Boundary | |
| 4 | 4 chiến lược DR và trade-off RPO/RTO/chi phí | |
| 5 | Kiến trúc K8s + 8 bước của `kubectl apply` | |
| 6 | Framework 5 bước debug Pod + bảng chẩn đoán triệu chứng | |
| 7 | QoS class, OOMKilled vs Evicted, CPU throttling | |
| 8 | 5 yếu tố đảm bảo zero-downtime rolling update | |
| 9 | Mô hình 3 chiều Code–State–Thực tế của Terraform | |
| 10 | Chia state theo layer + trade-off nhiều state vs một state | |
| 11 | Tình huống state mất/corrupt và apply bị gián đoạn | |
| 12 | Viết workflow GitHub Actions CI/CD từ đầu | |
| 13 | Luồng OIDC 4 bước + lỗ hổng `pull_request_target` | |
| 14 | 4 nguyên tắc GitOps + bảng trade-off push vs pull | |
| 15 | Sync Status vs Health Status + ma trận 4 ô | |
| 16 | ApplicationSet generators + progressive rollout qua fleet | |
| 17 | 3 giải pháp secret trong GitOps kèm trade-off | |
| 18 | Golden Signals + USE Method | |
| 19 | SLI/SLO/SLA + tính Error Budget + Error Budget Policy | |
| 20 | Cardinality, Histogram vs Summary, 3 câu PromQL | |
| 21 | 5 tiêu chí alert tốt + Alert Fatigue | |
| 22 | Framework 6 bước xử lý sự cố (mitigate trước, root cause sau) | |
| 23 | Framework 6 bước trả lời system design | |
| 24 | 4 câu chuyện STAR với con số cụ thể | |
| 25 | 5 câu hỏi hỏi lại người phỏng vấn | |

## 8.3. Năm điều quyết định thành công của buổi phỏng vấn

1. **Luôn nêu trade-off.** Người senior không nói "X tốt hơn Y"; họ nói "X tốt hơn *trong bối cảnh này* vì..., đánh đổi là...". Đây là tín hiệu phân loại level rõ ràng nhất.

2. **Hỏi lại yêu cầu trước khi thiết kế.** Nhảy vào giải pháp ngay là dấu hiệu junior. Câu "cho tôi hỏi vài điều trước đã: quy mô bao nhiêu, SLO mục tiêu là gì?" luôn được điểm cộng.

3. **Nói "tôi không biết, nhưng tôi sẽ tìm ra thế nào".** Ví dụ: *"Tôi chưa làm trực tiếp với X, nhưng theo tôi hiểu nó giải quyết vấn đề Y, và tôi sẽ tiếp cận bằng cách đọc docs phần Z rồi thử trong môi trường sandbox trước."* Câu này an toàn và đáng tin hơn nhiều so với đoán bừa — người phỏng vấn có kinh nghiệm phát hiện đoán bừa rất nhanh, và đó là điểm trừ nặng hơn cả việc không biết.

4. **Gắn mọi câu trả lời với kinh nghiệm thật khi có thể.** "Ở dự án trước, chúng tôi gặp đúng vấn đề này và đã..." có giá trị gấp nhiều lần một định nghĩa hoàn hảo.

5. **Đừng quên phần vận hành.** Khi thiết kế, ứng viên thường dừng ở "hệ thống chạy được". Ứng viên tốt luôn nói tiếp: monitoring gì, alert gì, rollback thế nào, DR ra sao, chi phí bao nhiêu. Đó chính là bản chất công việc DevOps.

---

*Chúc bạn phỏng vấn thành công. Hãy dùng tài liệu này cùng bộ 300 câu hỏi: cẩm nang để xây tư duy, bộ câu hỏi để kiểm tra độ phủ kiến thức.*
