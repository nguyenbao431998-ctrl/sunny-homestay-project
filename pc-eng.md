# GIÁO TRÌNH 20 NGÀY CHINH PHỤC PHỎNG VẤN DEVOPS BẰNG TIẾNG ANH

> **Nguyên tắc vàng:** Câu ngắn (S + V + O) · Nói chậm · Nhấn rõ từ khóa · Dùng STAR cho câu hỏi tình huống.
>
> **Cách dùng giáo trình:** Mỗi ngày học 60–90 phút. Đọc ví dụ mẫu → **thay bằng thông tin thật của bạn** → đọc to 5 lần → ghi âm 1 lần. Dấu `[ ... ]` là chỗ bạn cần điền thông tin của mình.

---

## MỤC LỤC

- Giai đoạn 1: Nền tảng & Giới thiệu bản thân (Ngày 1–4)
- Giai đoạn 2: Câu hỏi Kỹ thuật (Ngày 5–10)
- Giai đoạn 3: Tình huống & Troubleshooting (Ngày 11–15)
- Giai đoạn 4: Phỏng vấn thử & Hoàn thiện (Ngày 16–20)
- Phụ lục A: Câu "cứu cánh" khi bí từ
- Phụ lục B: Bảng thay từ khó bằng từ dễ

---

# 📌 GIAI ĐOẠN 1: NỀN TẢNG & GIỚI THIỆU BẢN THÂN

## NGÀY 1 — Bộ từ vựng DevOps & Phát âm

**Mục tiêu:** Phát âm đúng trọng âm 40 từ khóa. Người nghe hiểu bạn chủ yếu nhờ **trọng âm đúng**, không phải nhờ ngữ pháp.

Quy ước: chữ IN HOA là âm được nhấn mạnh.

### Nhóm 1 — Hạ tầng & Triển khai

| Từ | Trọng âm | Nghĩa | Câu mẫu |
|---|---|---|---|
| Provision | pro-VI-sion | Cấp phát tài nguyên | I provision servers with Terraform. |
| Deploy | de-PLOY | Triển khai | We deploy ten times a day. |
| Deployment | de-PLOY-ment | Sự triển khai | The deployment failed last night. |
| Pipeline | PIPE-line | Luồng tự động | I built the CI/CD pipeline. |
| Infrastructure | IN-fra-struc-ture | Hạ tầng | I manage AWS infrastructure. |
| Environment | en-VI-ron-ment | Môi trường | We have three environments: dev, staging, and prod. |
| Configuration | con-fig-u-RA-tion | Cấu hình | I store configuration in Git. |
| Release | re-LEASE | Bản phát hành | We release every Friday. |
| Rollback | ROLL-back | Quay lại bản cũ | I rolled back to the previous version. |
| Artifact | AR-ti-fact | Sản phẩm build | The pipeline stores the artifact in S3. |

### Nhóm 2 — Container & Orchestration

| Từ | Trọng âm | Nghĩa | Câu mẫu |
|---|---|---|---|
| Container | con-TAIN-er | Container | Each service runs in a container. |
| Containerization | con-tain-er-i-ZA-tion | Container hóa | I led the containerization of our apps. |
| Orchestration | or-ches-TRA-tion | Điều phối | Kubernetes is an orchestration tool. |
| Cluster | CLUS-ter | Cụm máy | We run two EKS clusters. |
| Namespace | NAME-space | Không gian tên | Each team has its own namespace. |
| Replica | REP-li-ca | Bản sao | The service has three replicas. |
| Ingress | IN-gress | Cổng vào | Ingress routes traffic to services. |
| Registry | RE-gis-try | Kho image | We push images to ECR registry. |

### Nhóm 3 — Đặc tính hệ thống

| Từ | Trọng âm | Nghĩa | Câu mẫu |
|---|---|---|---|
| Scalable | SCA-la-ble | Mở rộng được | The system is scalable. |
| Highly available | HIGH-ly a-VAIL-a-ble | Sẵn sàng cao | We deploy in three AZs to be highly available. |
| Reliable | re-LI-a-ble | Ổn định | I want the system to be reliable. |
| Latency | LA-ten-cy | Độ trễ | Latency went up to 2 seconds. |
| Throughput | THROUGH-put | Lưu lượng xử lý | We increased throughput by 30%. |
| Downtime | DOWN-time | Thời gian ngừng | We had zero downtime. |
| Redundancy | re-DUN-dan-cy | Dự phòng | We added redundancy for the database. |
| Bottleneck | BOT-tle-neck | Điểm nghẽn | The database was the bottleneck. |

### Nhóm 4 — Vận hành & Sự cố

| Từ | Trọng âm | Nghĩa | Câu mẫu |
|---|---|---|---|
| Troubleshoot | TROU-ble-shoot | Xử lý sự cố | I troubleshoot production issues. |
| Monitor | MON-i-tor | Giám sát | I monitor the cluster with Prometheus. |
| Observability | ob-ser-va-BIL-i-ty | Khả năng quan sát | We improved observability with Grafana. |
| Alert | a-LERT | Cảnh báo | I got an alert at 2 AM. |
| Incident | IN-ci-dent | Sự cố | I handled a P1 incident. |
| Outage | OUT-age | Sập dịch vụ | The outage lasted 15 minutes. |
| Root cause | ROOT cause | Nguyên nhân gốc | I found the root cause in the logs. |
| Post-mortem | post-MOR-tem | Họp rút kinh nghiệm | We wrote a post-mortem after the incident. |
| Metrics | MET-rics | Chỉ số | I checked CPU and memory metrics. |

### Nhóm 5 — Bảo mật & Code

| Từ | Trọng âm | Nghĩa | Câu mẫu |
|---|---|---|---|
| Automate / Automation | AU-to-mate / au-to-MA-tion | Tự động hóa | I automate manual tasks. |
| Security | se-CU-ri-ty | Bảo mật | Security is part of the pipeline. |
| Permission | per-MIS-sion | Quyền | I follow least privilege for permissions. |
| Credentials | cre-DEN-tials | Thông tin đăng nhập | We store credentials in Secrets Manager. |
| Vulnerability | vul-ner-a-BIL-i-ty | Lỗ hổng | Trivy scans images for vulnerabilities. |
| Version control | VER-sion con-TROL | Quản lý phiên bản | All code is in version control. |
| Repository | re-PO-si-to-ry | Kho mã nguồn | Each service has one repository. |

### Bài tập Ngày 1
1. Mở Google Translate (hoặc youglish.com — nghe người bản xứ nói từ đó trong video thật). Nghe từng từ 3 lần, nhại lại 3 lần.
2. Đọc to toàn bộ **câu mẫu** (không chỉ từ đơn) — vì phỏng vấn bạn nói câu, không nói từ.
3. Ghi âm 10 câu khó nhất, so sánh với bản chuẩn.

⚠️ **Lỗi người Việt hay mắc:** bỏ âm cuối. Hãy bật rõ âm cuối: deploy**ed** (/d/), cluster**s** (/z/), metric**s** (/s/), scrip**t** (/t/).

---

## NGÀY 2 — "Tell me about yourself"

**Cấu trúc:** Hiện tại → Quá khứ → Tương lai. Độ dài lý tưởng: **60–90 giây** (khoảng 8–10 câu).

### Khung điền

```
[HIỆN TẠI]
Hi, my name is [Tên]. 
I am a DevOps Engineer with [X] years of experience.
Right now, I work at [Công ty]. 
I manage [Cloud] infrastructure and CI/CD pipelines.

[QUÁ KHỨ]
Before that, I worked as a [vị trí cũ] for [Y] years.
That is where I learned [kỹ năng nền tảng].

[ĐIỂM NHẤN]
One thing I am proud of: I [thành tích + con số].

[TƯƠNG LAI]
Now, I am looking for a role where I can work with [công nghệ].
I think your company is a good fit because [lý do].
```

### Ví dụ mẫu hoàn chỉnh

> Hi, my name is Minh.
> I am a DevOps Engineer with four years of experience.
> Right now, I work at ABC Tech, an e-commerce company.
> I manage AWS infrastructure and CI/CD pipelines for about 30 microservices.
>
> Before that, I worked as a Linux system administrator for two years.
> That is where I learned networking and shell scripting.
>
> One thing I am proud of: I moved our applications from EC2 to EKS.
> It reduced our cloud cost by 25 percent.
>
> Now, I am looking for a bigger challenge.
> I want to work more with Kubernetes and Terraform at a larger scale.
> I read that your team is building a new cloud platform, and that is exactly what I want to do.

### Mẹo
- **Học thuộc theo ý, không theo từng chữ.** Nhớ 5 "cột mốc": Tên & số năm → Công việc hiện tại → Việc trước đây → Thành tích có số → Lý do ứng tuyển.
- Luôn có **1 con số** (25%, 30 services, 10 minutes). Con số thay cho hàng chục từ vựng hoa mỹ.
- Kết thúc bằng lý do liên quan **đến công ty họ** — xem trang tuyển dụng, blog kỹ thuật của họ trước.

### Bài tập
Viết bản của bạn → đọc to bấm giờ → cắt bớt nếu quá 90 giây.

---

## NGÀY 3 — Project Walkthrough (Trình bày dự án)

**Cấu trúc 4 phần:** Bối cảnh → Vấn đề → Giải pháp (công cụ) → Kết quả (con số).

### Ví dụ mẫu

> **Context (Bối cảnh):**
> Let me tell you about my favorite project.
> We had a web application for online shopping.
> It ran on EC2 servers.
>
> **Problem (Vấn đề):**
> Deployment was manual. It took about two hours.
> Scaling was also slow during sales events.
>
> **Solution (Giải pháp):**
> I used Terraform to provision an EKS cluster.
> I put the cluster in private subnets.
> I added an Application Load Balancer and AWS WAF in front of it.
> I built a pipeline with GitHub Actions and Argo CD.
> I used Horizontal Pod Autoscaler to scale the pods.
>
> **Result (Kết quả):**
> Deployment time went from two hours to fifteen minutes.
> During Black Friday, the system scaled to three times the normal traffic with no downtime.

### Câu hỏi đào sâu họ hay hỏi tiếp (chuẩn bị sẵn)

| Câu hỏi | Gợi ý trả lời ngắn |
|---|---|
| What was the hardest part? | The hardest part was the database migration. We used a blue-green approach to avoid downtime. |
| Why did you choose EKS, not ECS? | Our team already knew Kubernetes. Also, EKS is more portable. |
| What would you do differently? | I would add monitoring earlier. We added it late, and we missed some issues at the start. |
| How big was the team? | It was three DevOps engineers. I was the lead for the infrastructure part. |

### Mẹo
Nếu được chia sẻ màn hình, **vẽ sơ đồ** khi nói: `User → WAF → ALB → Ingress → Pods (EKS) → RDS`. Vừa chỉ vừa nói từ khóa.

---

## NGÀY 4 — Strengths & Weaknesses

### Điểm mạnh — Công thức: Tên điểm mạnh + Ví dụ + Kết quả

**Strength 1 — Automation**
> One of my strengths is automation.
> I don't like doing the same task twice.
> For example, our team created new environments by hand. It took one day.
> I wrote Terraform modules for it. Now it takes 20 minutes.

**Strength 2 — Problem-solving under pressure**
> Another strength is that I stay calm during incidents.
> I follow a clear process: check the alert, check the metrics, check the logs.
> Last year, I fixed a payment service outage in 10 minutes because I followed this process.

### Điểm yếu — Công thức: Điểm yếu thật + Hành động cải thiện + Tiến bộ

**Weakness — English communication (khuyến nghị dùng, vì nó trung thực và họ đã nghe thấy)**
> My weakness is my spoken English.
> I can read and write technical documents well, but speaking is harder for me.
> To improve, I practice every day. I record myself and I join English meetings at work.
> I am getting better, and I always confirm important things in writing, like on Slack or in Jira tickets.

**Phương án thay thế — Taking on too much**
> Sometimes I try to fix everything myself.
> Now, I ask for help earlier and I share the work with my team.

⚠️ **Tránh:** "I am a perfectionist" (sáo rỗng), hoặc điểm yếu là kỹ năng cốt lõi của vị trí (ví dụ: "I am weak at Linux").

---

# 📌 GIAI ĐOẠN 2: CÂU HỎI KỸ THUẬT

> **Công thức trả lời kỹ thuật "3 bước":** (1) Định nghĩa 1 câu → (2) Cách bạn dùng thực tế → (3) Một ví dụ/con số. Đừng nói lý thuyết dài — nhà tuyển dụng muốn nghe **bạn đã làm gì**.

## NGÀY 5 — CI/CD

### Q: "Can you explain your CI/CD pipeline?"

> Sure. I will explain it step by step.
>
> **Step 1 — Source:** A developer pushes code to GitHub. This triggers the pipeline.
> **Step 2 — Build:** The pipeline builds a Docker image.
> **Step 3 — Test:** It runs unit tests. It also scans the code with SonarQube and scans the image with Trivy.
> **Step 4 — Push:** If all tests pass, it pushes the image to Amazon ECR.
> **Step 5 — Deploy:** Argo CD sees the new image tag and deploys it to Kubernetes.
> First it goes to staging. After approval, it goes to production.
>
> If something goes wrong in production, we can roll back with one click in Argo CD.

### Q: "What is the difference between CI, Continuous Delivery and Continuous Deployment?"

> CI means we build and test code automatically on every commit.
> Continuous Delivery means the code is always ready to release, but a human clicks the button.
> Continuous Deployment means it goes to production automatically, with no human step.
> In my company, we use Continuous Delivery for production, because we want one manual approval.

### Q: "How do you handle secrets in the pipeline?"

> I never put secrets in the code.
> I store them in AWS Secrets Manager or GitHub Secrets.
> The pipeline reads them at runtime.
> In Kubernetes, I use External Secrets Operator to sync them into the cluster.

### Câu hỏi luyện thêm
- What deployment strategies do you know? → *Rolling update, Blue-Green, Canary* (giải thích mỗi cái 1 câu).
- How do you make a pipeline faster? → *Cache dependencies, run tests in parallel, use smaller base images.*

---

## NGÀY 6 — Docker & Kubernetes

### Q: "What is the difference between Docker and Kubernetes?"

> Docker is a tool to build and run containers.
> It packages the app and its dependencies into one image.
>
> Kubernetes is an orchestration tool. It manages many containers on many servers.
> It does scheduling, scaling, self-healing, and load balancing.
>
> A simple way to say it: Docker makes the container. Kubernetes manages thousands of them.

### Q: "How do you expose a Pod to the internet?"

> A Pod has a private IP, and the IP can change. So we don't expose a Pod directly.
>
> First, I create a **Deployment** to manage the Pods.
> Then I create a **Service** to give them a stable address.
> There are three common Service types: ClusterIP for internal traffic, NodePort, and LoadBalancer.
>
> For production, I usually use an **Ingress**.
> On AWS, I use the AWS Load Balancer Controller. It creates an ALB from the Ingress.
> So the flow is: Internet → ALB → Ingress → Service → Pods.

### Q: "What is the difference between a Deployment and a StatefulSet?"

> A Deployment is for stateless apps, like a web API. All Pods are the same.
> A StatefulSet is for stateful apps, like a database. Each Pod has a fixed name and its own storage.

### Q: "How do you make a Docker image smaller?"

> I use a small base image, like Alpine or distroless.
> I use multi-stage builds. The build tools stay in the first stage, and only the binary goes to the final image.
> I also use a .dockerignore file.
> In one project, the image went from 900 MB to 80 MB.

### Từ khóa cần nói rõ
Image · Container · Pod · Deployment · Service · Ingress · ConfigMap · Secret · Liveness/Readiness probe · HPA

---

## NGÀY 7 — Infrastructure as Code (Terraform)

### Q: "Why do we use Terraform?"

> We use Terraform to manage infrastructure with code.
> There are four main benefits.
> One, it is **automated**. We don't click in the console.
> Two, it is in **version control**. We can review changes in a pull request and see the history.
> Three, it is **reusable**. I write a module once and use it for dev, staging, and prod.
> Four, it is **consistent**. All environments look the same, so we have fewer surprises.

### Q: "What is the Terraform state file?"

> The state file is how Terraform remembers what it created.
> It maps the code to the real resources in the cloud.
>
> In a team, we must not keep it on a laptop.
> I store it in an S3 bucket with encryption.
> I use DynamoDB for state locking, so two people cannot change it at the same time.

### Q: "What happens if someone changes a resource manually in the console?"

> That is called drift.
> When I run terraform plan, Terraform shows the difference.
> Then I decide: update the code to match, or apply the code to remove the manual change.
> To prevent this, we limit console access with IAM, and we run a drift check in the pipeline every day.

### Các lệnh nên nhắc tên
`terraform init` → `terraform plan` → `terraform apply` · `terraform import` · `terraform state mv`

---

## NGÀY 8 — Cloud & Security

### Q: "How do you design a secure architecture on AWS?"

> I think about security in layers.
>
> **Network layer:** I create a VPC with public and private subnets.
> Only the load balancer is in the public subnet.
> The application and the database are in private subnets. They have no public IP.
> They use a NAT Gateway to reach the internet.
>
> **Edge layer:** I put AWS WAF in front of the load balancer. It blocks attacks like SQL injection.
>
> **Access layer:** I use IAM roles, not access keys. I follow least privilege.
> In EKS, I use IRSA, so each Pod gets only the permissions it needs.
>
> **Data layer:** I encrypt data at rest with KMS and in transit with TLS.
>
> **Audit layer:** I turn on CloudTrail and GuardDuty to detect strange activity.

### Q: "What is the difference between a Security Group and a NACL?"

> A Security Group works at the instance level. It is stateful, so return traffic is allowed automatically.
> A NACL works at the subnet level. It is stateless, so I must allow both directions.

### Q: "What does least privilege mean?"

> It means we give only the permissions that are needed, nothing more.
> For example, a Lambda that reads from one S3 bucket gets read access to that bucket only.

### Bài tập
Vẽ sơ đồ này ra giấy và vừa chỉ vừa nói trong 2 phút:
```
Internet → Route 53 → WAF → ALB (public subnet)
                              ↓
                  EKS nodes (private subnet)
                              ↓
                  RDS Multi-AZ (private subnet)
```

---

## NGÀY 9 — Observability (Logging & Monitoring)

### Q: "How do you monitor your infrastructure?"

> I use the three pillars of observability: metrics, logs, and traces.
>
> **Metrics:** I use Prometheus to collect metrics, and Grafana for dashboards.
> I watch CPU, memory, error rate, and latency.
>
> **Logs:** I use Fluent Bit to collect logs from Pods. It sends them to CloudWatch or OpenSearch.
>
> **Traces:** For microservices, I use OpenTelemetry to see how a request moves between services.
>
> **Alerts:** I set alerts in Alertmanager. Important alerts go to PagerDuty. Others go to Slack.

### Q: "How do you avoid alert fatigue?"

> I only alert on things that need action.
> I alert on symptoms that users feel, like high error rate, not only on high CPU.
> Every alert has a runbook, so the on-call engineer knows what to do.
> Every month, we review alerts and delete the noisy ones.

### Q: "What are SLI, SLO and SLA?"

> An SLI is a measurement, like the percentage of successful requests.
> An SLO is our internal goal, for example 99.9 percent success.
> An SLA is a promise to the customer, with a penalty if we miss it.

---

## NGÀY 10 — Tự đánh giá & Quay video

### Quy trình
1. Viết 15 câu hỏi từ Ngày 5–9 ra các mảnh giấy. Bốc ngẫu nhiên **5 câu**.
2. Bật camera, trả lời mỗi câu **tối đa 2 phút**, không nhìn kịch bản.
3. Xem lại video và chấm theo bảng sau:

| Tiêu chí | Tự chấm (1–5) | Ghi chú cải thiện |
|---|---|---|
| Tốc độ nói (chậm, rõ) | | |
| Phát âm từ khóa đúng trọng âm | | |
| Có bật âm cuối (-s, -ed, -t) | | |
| Câu ngắn, không lặp "uhm" quá nhiều | | |
| Có ví dụ thực tế / con số | | |
| Giao tiếp mắt với camera | | |
| Ngôn ngữ cơ thể (ngồi thẳng, cười nhẹ) | | |

4. Chọn **2 câu tệ nhất** → viết lại → quay lại lần 2.

💡 Mẹo: Thay "uhm..." bằng một khoảng lặng ngắn. Im lặng 2 giây nghe chuyên nghiệp hơn nhiều so với "uhm uhm".

---

# 📌 GIAI ĐOẠN 3: TÌNH HUỐNG & TROUBLESHOOTING

## NGÀY 11 — Phương pháp S.T.A.R

| Phần | Ý nghĩa | Thì động từ | Tỷ lệ thời lượng |
|---|---|---|---|
| **S**ituation | Bối cảnh | Quá khứ đơn | 15% |
| **T**ask | Nhiệm vụ của BẠN | Quá khứ đơn | 10% |
| **A**ction | Bạn đã làm gì (phần quan trọng nhất) | Quá khứ đơn, chủ ngữ "I" | 60% |
| **R**esult | Kết quả + con số + bài học | Quá khứ đơn | 15% |

⚠️ Dùng **"I"** trong phần Action, không phải "we". Nhà tuyển dụng muốn biết **bạn** đã làm gì.

### Câu mở đầu cho từng phần (học thuộc)
- **S:** "This happened at my last company..." / "One time, ..."
- **T:** "My job was to..." / "I needed to..."
- **A:** "First, I... Then, I... After that, I... Finally, I..."
- **R:** "As a result, ..." / "In the end, ..." / "What I learned was..."

### Ví dụ mẫu — Database bị sập
> **S:** One night, our production database stopped responding. Users could not log in.
> **T:** I was on call, so I needed to bring it back quickly.
> **A:** First, I checked the CloudWatch metrics. Memory was at 100 percent.
> Then, I checked the logs. I found an OOM error caused by a slow query.
> I killed the query and restarted the service.
> After that, I increased the instance size as a short-term fix.
> **R:** The system was back online in 10 minutes.
> The next day, I worked with the developers to add an index. The query became 50 times faster.

### Bài tập
Viết 3 câu chuyện STAR thật của bạn về: (1) một sự cố, (2) một lần tự động hóa, (3) một lần làm việc nhóm. Ba câu chuyện này có thể "tái sử dụng" cho hơn 10 câu hỏi khác nhau.

---

## NGÀY 12 — "How do you handle a production outage?"

### Khung 5 bước (học thuộc thứ tự)
**Acknowledge → Assess → Mitigate → Fix → Learn**

> I follow five steps.
>
> **One — Acknowledge.** I acknowledge the alert, so the team knows someone is working on it. I open an incident channel in Slack.
>
> **Two — Assess.** I check the impact. How many users are affected? Which services? I look at the dashboards and the logs.
>
> **Three — Mitigate.** My first goal is to stop the pain for users, not to find the root cause. If there was a recent deployment, I roll it back. Or I scale up, or I fail over to another region.
>
> **Four — Fix.** When the system is stable, I find the root cause and fix it properly.
>
> **Five — Learn.** We write a blameless post-mortem. We add action items, like a new alert or a new test, so it does not happen again.
>
> During the whole process, I update the stakeholders every 15 to 30 minutes.

### Câu chuyện STAR minh họa
> **S:** After a release, our checkout API started returning 500 errors.
> **T:** I needed to stop the errors fast, because we were losing orders.
> **A:** I saw in Grafana that errors started right after the deployment. So I rolled back with Argo CD. Errors went to zero in 3 minutes. Then I checked the logs and found a wrong database connection string in the new config.
> **R:** Downtime was only 5 minutes. After that, I added a config validation step and a smoke test to the pipeline.

---

## NGÀY 13 — Debug Pod lỗi CrashLoopBackOff

### Q: "A Pod is in CrashLoopBackOff. How do you debug it?"

> CrashLoopBackOff means the container starts, crashes, and Kubernetes keeps restarting it.
> I debug it in four steps.
>
> **Step 1:** I run `kubectl describe pod`. I look at the Events section and the exit code.
> For example, exit code 137 means OOMKilled — not enough memory.
> Exit code 1 usually means an application error.
>
> **Step 2:** I check the logs with `kubectl logs`.
> If the container already restarted, I add `--previous` to see the logs from the crashed container.
>
> **Step 3:** I check the configuration. Common problems are a missing environment variable, a wrong ConfigMap or Secret, or a wrong command.
>
> **Step 4:** I check the probes. Sometimes the liveness probe is too strict, so Kubernetes kills a healthy app that is just slow to start.
>
> Then I fix the cause. For example, I increase the memory limit, or I add a startup probe.

### Bảng tra nhanh các lỗi Pod thường gặp

| Trạng thái | Nguyên nhân thường gặp | Câu nói mẫu |
|---|---|---|
| CrashLoopBackOff | App lỗi, thiếu config, OOM | "The app keeps crashing. I check the logs first." |
| ImagePullBackOff | Sai tên image/tag, thiếu quyền registry | "Kubernetes cannot pull the image. I check the image name and the registry permissions." |
| Pending | Thiếu tài nguyên node, PVC chưa bind | "The Pod cannot be scheduled. I check node resources and events." |
| OOMKilled | Vượt memory limit | "The container used more memory than the limit." |

### Lệnh cần đọc trôi chảy
`kubectl get pods` · `kubectl describe pod <name>` · `kubectl logs <name> --previous` · `kubectl get events --sort-by=.metadata.creationTimestamp` · `kubectl exec -it <name> -- sh`

---

## NGÀY 14 — Kể về một sai lầm (Failure / Mistake)

**Nguyên tắc:** Thừa nhận thật → Xử lý nhanh → Bài học → **Thay đổi hệ thống** để không ai lặp lại.

### Ví dụ mẫu
> **S:** Early in my career, I ran terraform apply on the wrong workspace.
> I thought I was in staging, but I was in production.
> **T:** It deleted a security group rule, and one service lost access to the database.
> **A:** I noticed it from the alerts in about two minutes.
> I told my team lead immediately. I did not try to hide it.
> I restored the rule from the Terraform code and the service came back.
> **R:** The impact was about five minutes.
> After that, I made three changes:
> One, we moved all Terraform runs into the CI pipeline, so nobody runs apply from a laptop.
> Two, we added a manual approval step for production.
> Three, we colored the terminal prompt red for production.
> What I learned is: good processes protect us from human mistakes.

⚠️ **Tránh:** câu chuyện không có hậu quả thật ("I never really made a big mistake"), hoặc đổ lỗi cho người khác.

---

## NGÀY 15 — Teamwork & Cultural Fit

### Q: "How do you handle disagreements with developers?"

> I think disagreements are normal. We have the same goal, but different views.
>
> First, I listen and try to understand their reason.
> Then, I use data, not opinions.
>
> For example, one developer wanted to give his app admin access to all S3 buckets, because it was faster.
> I explained the security risk.
> I showed him that we only needed access to one bucket.
> I also wrote the IAM policy for him, so it did not slow him down.
> In the end, he agreed, and we used the same approach for other teams.

### Q: "How do you work with developers who don't want to follow DevOps practices?"

> I try to make the right way the easy way.
> For example, I created a pipeline template. A new service can use it in five minutes.
> When it is easy, people use it.

### Q: "Tell me about a time you helped a teammate."

> A new teammate did not know Kubernetes.
> I paired with him for one hour every day for two weeks.
> I also wrote a short internal guide.
> After one month, he could handle on-call by himself.

### Q: "Why do you want to leave your current job?" (luôn tích cực)

> I learned a lot in my current company.
> But I want to work on bigger systems and grow my skills in [công nghệ].
> Your company is a good place for that.

⚠️ **Không bao giờ** nói xấu công ty cũ hoặc sếp cũ.

---

# 📌 GIAI ĐOẠN 4: PHỎNG VẤN THỬ & HOÀN THIỆN

## NGÀY 16 — Câu hỏi dành cho nhà tuyển dụng

Hỏi lại cho thấy bạn quan tâm thật. Chọn **2–3 câu** phù hợp với người phỏng vấn:

**Hỏi kỹ sư / Tech Lead:**
- What is the biggest challenge your DevOps team is facing right now?
- What does your CI/CD and deployment process look like?
- How does the on-call rotation work?
- How do DevOps and developers work together here?

**Hỏi Manager / HR:**
- What does success look like in this role after six months?
- How do you support learning, like certifications or training?
- What are the next steps in the interview process?

⚠️ **Tránh hỏi** về lương, ngày nghỉ ở vòng kỹ thuật (để dành cho vòng HR/offer).

### Câu kết thúc buổi phỏng vấn
> Thank you for your time today. I enjoyed our conversation, and I am excited about this role. I look forward to hearing from you.

---

## NGÀY 17 & 18 — Mock Interview (Phỏng vấn thử)

Bạn có thể dùng tôi (Claude) để phỏng vấn thử. Hãy gửi một tin nhắn như sau:

> *"Act as a DevOps interviewer at [tên công ty / loại công ty]. The job uses [AWS, Kubernetes, Terraform]. Ask me one question at a time. Wait for my answer. After each answer, give feedback in Vietnamese: what was good, grammar mistakes, and a simpler version of my answer. Start now."*

**Cách luyện hiệu quả nhất:** Đọc to câu trả lời ra miệng trước (có thể bấm giờ), sau đó mới gõ vào. Như vậy bạn luyện cả nói lẫn viết.

### Ngày 17 — Bộ câu hỏi vòng 1 (Behavioral + Basic Technical)
1. Tell me about yourself.
2. Why do you want to join our company?
3. Explain your current CI/CD pipeline.
4. What is the difference between Docker and Kubernetes?
5. Why do we use Terraform?
6. Tell me about a project you are proud of.
7. What is your biggest weakness?
8. Do you have any questions for us?

### Ngày 18 — Bộ câu hỏi vòng 2 (Deep Technical + Scenario)
1. A Pod is in CrashLoopBackOff. What do you do?
2. Our website is very slow. How do you find the problem?
3. How do you design a highly available system on AWS?
4. How do you do a zero-downtime deployment?
5. Tell me about a production outage you handled.
6. Tell me about a mistake you made.
7. How do you handle disagreements with developers?
8. How do you manage secrets?
9. How do you reduce cloud costs?
10. Where do you see yourself in three years?

### Mẫu trả lời cho 2 câu khó chưa có ở trên

**"Our website is very slow. How do you find the problem?"**
> I start from the user and go deeper, layer by layer.
> First, I check the dashboards. Is latency high for all pages or only one API?
> Then I check the load balancer metrics, then the application, then the database.
> I look for a bottleneck: high CPU, slow queries, or a slow external API.
> If we have tracing, I use it to see which service takes the most time.

**"How do you reduce cloud costs?"**
> First, I find where the money goes, with AWS Cost Explorer and tags.
> Then I do a few things:
> I right-size instances that are too big.
> I use Spot Instances for non-critical workloads.
> I buy Savings Plans for stable workloads.
> I turn off dev environments at night.
> In my last project, this saved about 30 percent per month.

---

## NGÀY 19 — Lọc lại kịch bản (Script Refinement)

### Checklist cho mỗi câu trả lời
- [ ] Mỗi câu dưới **15 từ**?
- [ ] Không có câu ghép quá 2 mệnh đề?
- [ ] Có ít nhất **1 con số** hoặc **1 ví dụ thực tế**?
- [ ] Dùng chủ ngữ **"I"** khi kể việc mình làm?
- [ ] Thì quá khứ đúng khi kể chuyện (I **checked**, I **found**, I **fixed**)?
- [ ] Đã thay từ khó phát âm bằng từ dễ (xem Phụ lục B)?

### Ví dụ: Trước và Sau khi lọc

**❌ Trước (khó nói, câu dài):**
> In my current organization, I was responsible for the implementation and subsequent maintenance of a comprehensive containerized infrastructure leveraging Kubernetes, which substantially enhanced our deployment efficiency.

**✅ Sau (dễ nói, rõ ràng):**
> In my current company, I built our Kubernetes platform. I also maintain it. It made our deployments much faster — from one hour to ten minutes.

### Câu lệnh nhờ AI lọc kịch bản
> *"Rewrite my answer below for a DevOps interview. Use simple English, short sentences (under 15 words), and easy-to-pronounce words. Keep all technical keywords. Then list my grammar mistakes in Vietnamese. My answer: [dán câu trả lời]"*

---

## NGÀY 20 — Tổng duyệt 45 phút

### Chuẩn bị
- Mặc đồ lịch sự, ngồi thẳng, ánh sáng chiếu từ phía trước mặt.
- Kiểm tra mic, camera, mạng, phần mềm (Zoom / Google Meet / Teams).
- Chuẩn bị sẵn: giấy bút, 1 ly nước, sơ đồ kiến trúc dự án.
- Dán **5 từ khóa nhắc ý** (không phải cả kịch bản) cạnh màn hình.

### Lịch trình tổng duyệt

| Thời gian | Phần | Nội dung |
|---|---|---|
| 0–5 phút | Mở đầu | Chào hỏi, small talk, Tell me about yourself |
| 5–15 phút | Dự án | Project walkthrough + 2 câu hỏi đào sâu |
| 15–30 phút | Kỹ thuật | 5 câu bốc ngẫu nhiên (CI/CD, K8s, Terraform, AWS, Monitoring) |
| 30–40 phút | Tình huống | 3 câu STAR: outage, mistake, disagreement |
| 40–45 phút | Kết thúc | Hỏi lại nhà tuyển dụng + câu cảm ơn |

### Mẫu small talk mở đầu
> **Interviewer:** How are you today?
> **You:** I'm good, thank you. And you?
>
> **Interviewer:** Can you hear me well?
> **You:** Yes, I can hear you clearly. Please let me know if my audio is not clear.

💡 **Mẹo vàng:** Ngay đầu buổi, bạn có thể nói một câu xây dựng thiện cảm:
> *"English is not my first language, so sometimes I speak slowly. If something is not clear, please ask me again."*
Câu này giúp bạn bớt áp lực và người phỏng vấn sẽ kiên nhẫn hơn.

---

# 📎 PHỤ LỤC A — CÂU "CỨU CÁNH" KHI BÍ TỪ

### Khi cần thời gian suy nghĩ
- That's a good question. Let me think for a second.
- Give me a moment to think about this.
- To answer this, I will break it down into two parts.

### Khi không nghe rõ / không hiểu câu hỏi
- Sorry, could you repeat the question, please?
- Could you say that a bit more slowly, please?
- Do you mean [X] or [Y]?
- Just to make sure I understand, you are asking about [X], right?

### Khi nói sai / muốn nói lại
- Sorry, let me rephrase that.
- Let me say that in a different way.
- What I mean is...

### Khi không biết câu trả lời (trung thực + thể hiện tư duy)
- I haven't used that tool in production, but I know the concept. I think it works like this...
- I'm not 100% sure, but my approach would be...
- I don't know the exact command, but I would check the documentation. The idea is...

### Khi muốn vẽ / chia sẻ màn hình
- Can I share my screen? It's easier to explain with a diagram.
- Let me draw it quickly.

### Khi kết thúc một câu trả lời
- Does that answer your question?
- I can go deeper on any part if you want.

---

# 📎 PHỤ LỤC B — BẢNG THAY TỪ KHÓ BẰNG TỪ DỄ

| Từ khó (dài, khó phát âm) | Thay bằng (ngắn, dễ nói) |
|---|---|
| Utilize / Leverage | Use |
| Implement | Build / Set up |
| Facilitate | Help |
| Approximately | About |
| Subsequently | Then / After that |
| Substantially | A lot / Much |
| Methodology | Way / Process |
| Enhance | Improve |
| Commence | Start |
| Terminate | Stop / Kill |
| Demonstrate | Show |
| Consequently | So |
| In order to | To |
| At this point in time | Now |
| Responsible for | I manage / I own |

---

## ✅ CHECKLIST TRƯỚC NGÀY PHỎNG VẤN

- [ ] Thuộc ý chính "Tell me about yourself" (60–90 giây)
- [ ] 1 dự án nổi bật có sơ đồ + con số
- [ ] 3 câu chuyện STAR: sự cố, sai lầm, làm việc nhóm
- [ ] Trả lời được 5 chủ đề: CI/CD, Docker/K8s, Terraform, AWS Security, Monitoring
- [ ] 2–3 câu hỏi để hỏi lại
- [ ] Đọc lại job description, gạch chân công cụ họ dùng → đảm bảo đã nhắc đến trong câu trả lời
- [ ] Ngủ đủ giấc 🙂

> **Nhớ:** Nhà tuyển dụng DevOps tuyển người **giải quyết được vấn đề**, không tuyển người nói tiếng Anh hay nhất. Nói chậm, rõ từ khóa, có ví dụ thật — bạn đã hơn rất nhiều ứng viên khác. Chúc bạn thành công! 🚀
