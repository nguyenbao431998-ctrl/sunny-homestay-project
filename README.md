# Bộ Câu Hỏi Phỏng Vấn DevOps Engineering
### 300 câu hỏi & đáp án chi tiết (50 câu/mảng) — AWS | Kubernetes | GitHub Actions | ArgoCD | Terraform | Monitoring & Logging

> Biên soạn bởi góc nhìn nhà tuyển dụng DevOps 10+ năm kinh nghiệm. Câu hỏi được sắp xếp từ cơ bản → nâng cao trong từng mảng, giúp đánh giá ứng viên theo 3 tầng: **Kiến thức nền tảng (Basic)**, **Vận hành thực chiến (Intermediate)**, **Kiến trúc & xử lý sự cố (Advanced)**.

---

## PHẦN 1: AWS (50 câu)

### Nhóm Cơ bản (1–17)

**1. EC2 là gì và khác gì so với on-premise server?**
EC2 (Elastic Compute Cloud) là dịch vụ máy chủ ảo (VM) theo yêu cầu của AWS, cho phép người dùng thuê tài nguyên compute theo giờ/giây thay vì đầu tư phần cứng. Khác biệt chính: khả năng scale nhanh (phút thay vì tuần), trả tiền theo mức sử dụng (pay-as-you-go), không cần quản lý hạ tầng vật lý, và có nhiều loại instance tối ưu cho compute/memory/GPU/storage.

**2. Phân biệt các loại EC2 Instance: On-Demand, Reserved, Spot, Savings Plan.**
On-Demand trả theo giờ, không cam kết, giá cao nhất, phù hợp workload không dự đoán được. Reserved Instance cam kết 1-3 năm để giảm giá tới ~72%. Spot Instance dùng tài nguyên dư thừa của AWS, giá rẻ nhất (giảm tới 90%) nhưng có thể bị thu hồi bất cứ lúc nào, phù hợp batch job/fault-tolerant workload. Savings Plan linh hoạt hơn Reserved vì áp dụng theo mức chi tiêu (compute $/giờ) chứ không gắn cứng vào instance type/region.

**3. S3 Storage Class khác nhau như thế nào?**
S3 Standard cho dữ liệu truy cập thường xuyên. S3 Intelligent-Tiering tự động chuyển tier dựa trên pattern truy cập. S3 Standard-IA và One Zone-IA cho dữ liệu ít truy cập, chi phí lưu trữ thấp hơn nhưng phí lấy dữ liệu cao hơn. S3 Glacier/Glacier Deep Archive cho lưu trữ lâu dài, chi phí cực thấp nhưng thời gian phục hồi từ phút đến giờ.

**4. IAM User, IAM Role, IAM Policy khác nhau ra sao?**
IAM User là danh tính cố định gắn với người/dịch vụ cụ thể, có credentials lâu dài. IAM Role là danh tính tạm thời được "assume" bởi user/service khác, cấp quyền tạm thời qua STS token, không có static credentials — đây là best practice cho EC2/Lambda truy cập tài nguyên khác. IAM Policy là văn bản JSON định nghĩa permission (allow/deny action trên resource nào), có thể gắn vào User, Group, hoặc Role.

**5. VPC là gì, các thành phần chính của VPC?**
VPC (Virtual Private Cloud) là mạng ảo cô lập logic trong AWS cho phép kiểm soát toàn bộ network layer. Thành phần chính gồm: Subnet (public/private), Route Table, Internet Gateway (IGW) cho truy cập internet, NAT Gateway cho private subnet ra ngoài, Security Group (stateful, cấp instance), Network ACL (stateless, cấp subnet), và VPC Peering/Transit Gateway để kết nối liên VPC.

**6. Sự khác biệt giữa Security Group và Network ACL?**
Security Group hoạt động ở tầng instance, stateful (traffic trả về tự động được allow), chỉ hỗ trợ rule "allow". Network ACL hoạt động ở tầng subnet, stateless (phải khai báo rule cả inbound và outbound riêng), hỗ trợ cả "allow" và "deny", và rule được xử lý theo thứ tự số (rule number thấp hơn ưu tiên trước).

**7. RDS là gì, khi nào dùng RDS thay vì tự cài DB trên EC2?**
RDS (Relational Database Service) là managed database service hỗ trợ MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, và Aurora. Nên dùng RDS khi muốn AWS tự động hoá backup, patching, failover (Multi-AZ), scaling (read replica), và giảm gánh nặng vận hành — đổi lại mất một phần khả năng tuỳ biến sâu (không SSH được vào instance DB).

**8. Auto Scaling Group (ASG) hoạt động như thế nào?**
ASG tự động điều chỉnh số lượng EC2 instance dựa trên policy (target tracking, step scaling, scheduled scaling) để đáp ứng load, dựa trên metric như CPU utilization, request count, hoặc custom CloudWatch metric. ASG đảm bảo min/max/desired capacity, tự thay thế instance unhealthy (health check qua ELB hoặc EC2 status check), và có thể trải instance qua nhiều AZ để đảm bảo high availability.

**9. Phân biệt ALB, NLB, CLB (Classic Load Balancer).**
ALB (Application Load Balancer) hoạt động ở layer 7, hỗ trợ routing theo path/host/header, phù hợp microservices và container. NLB (Network Load Balancer) hoạt động ở layer 4, throughput cực cao, độ trễ thấp, hỗ trợ static IP — phù hợp traffic TCP/UDP volume lớn. CLB là loại cũ hỗ trợ cả layer 4 và 7 nhưng thiếu tính năng nâng cao, AWS khuyến nghị migrate sang ALB/NLB.

**10. Route 53 là gì và các Routing Policy phổ biến?**
Route 53 là managed DNS service của AWS, đồng thời hỗ trợ health check và domain registration. Các routing policy chính: Simple (1 record), Weighted (chia traffic theo tỷ lệ), Latency-based (route đến region có độ trễ thấp nhất), Failover (active-passive DR), Geolocation/Geoproximity (theo vị trí địa lý người dùng), và Multivalue Answer (trả nhiều IP kèm health check).

**11. CloudFront dùng để làm gì?**
CloudFront là CDN (Content Delivery Network) của AWS, cache nội dung tĩnh/động tại các edge location gần người dùng để giảm độ trễ, giảm tải cho origin (S3/EC2/ALB), tích hợp AWS Shield/WAF để chống DDoS, và hỗ trợ HTTPS, signed URL/cookie cho nội dung private.

**12. Sự khác biệt giữa ECS và EKS?**
ECS (Elastic Container Service) là orchestrator container độc quyền của AWS, đơn giản hơn, tích hợp sâu với hệ sinh thái AWS (IAM, ALB, CloudWatch) và hỗ trợ Fargate (serverless container). EKS (Elastic Kubernetes Service) là managed Kubernetes, phức tạp hơn nhưng portable, phù hợp team đã quen K8s hoặc cần multi-cloud/hybrid.

**13. Fargate khác gì so với EC2 launch type trong ECS/EKS?**
Fargate là serverless compute engine cho container, không cần quản lý EC2 instance bên dưới — AWS tự động provision, scale tài nguyên theo spec container (CPU/memory) khai báo trong task definition. EC2 launch type yêu cầu tự quản lý cluster EC2 (patching, scaling, capacity), cho phép tối ưu chi phí/hiệu năng sâu hơn nhưng tốn công vận hành hơn.

**14. Lambda là gì, use case điển hình?**
Lambda là dịch vụ serverless compute, chạy code theo event-trigger (API Gateway, S3 event, SQS, CloudWatch schedule...) mà không cần quản lý server, tự động scale, tính phí theo số lần invoke và thời gian thực thi (ms). Use case: xử lý event bất đồng bộ, API backend nhẹ, xử lý ảnh/video khi upload S3, cron job, glue code giữa các dịch vụ AWS.

**15. CloudWatch dùng để làm gì trong hệ sinh thái AWS?**
CloudWatch là dịch vụ giám sát trung tâm của AWS: thu thập metric (CPU, memory nếu cài agent, custom metric), Logs (log group/stream từ EC2, Lambda, ECS...), Alarms (trigger action khi metric vượt ngưỡng), và Events/EventBridge (phản ứng theo sự kiện hệ thống, ví dụ tự động chạy Lambda khi EC2 instance stop).

**16. KMS là gì và vai trò trong bảo mật dữ liệu?**
KMS (Key Management Service) quản lý và luân chuyển encryption key dùng để mã hoá dữ liệu at-rest (S3, EBS, RDS...) và in some cases in-transit. KMS hỗ trợ Customer Managed Key (CMK) để kiểm soát policy truy cập key riêng, tích hợp CloudTrail để audit mọi lần dùng key, và hỗ trợ envelope encryption giúp giảm tải việc gọi API KMS trực tiếp.

**17. Sự khác biệt giữa SQS và SNS?**
SQS (Simple Queue Service) là message queue theo mô hình pull, dùng cho giao tiếp bất đồng bộ point-to-point hoặc worker queue (consumer tự lấy message xử lý, có visibility timeout, DLQ). SNS (Simple Notification Service) là pub/sub theo mô hình push, một message gửi tới nhiều subscriber (SQS, Lambda, email, HTTP endpoint) cùng lúc — phù hợp fan-out pattern.

### Nhóm Trung cấp (18–35)

**18. Giải thích CIDR block và cách chia subnet trong VPC.**
CIDR (Classless Inter-Domain Routing) định nghĩa dải IP bằng prefix (ví dụ 10.0.0.0/16 = 65536 IP). Khi thiết kế VPC, cần chia subnet nhỏ hơn cho từng AZ/tầng (public/private/database), ví dụ /16 cho VPC, /24 cho mỗi subnet, đảm bảo không chồng lấn (overlap) để hỗ trợ VPC Peering/Transit Gateway sau này, và luôn để dư địa IP cho mở rộng.

**19. NAT Gateway khác gì NAT Instance?**
NAT Gateway là dịch vụ managed của AWS, tự động scale, HA trong 1 AZ (cần deploy nhiều AZ để redundancy), không cần patch, tính phí theo giờ + data processed. NAT Instance là EC2 tự cấu hình làm NAT, rẻ hơn ở scale nhỏ nhưng phải tự quản lý HA, patch, và là single point of failure nếu không tự thiết kế thêm.

**20. Multi-AZ vs Multi-Region trong thiết kế HA/DR?**
Multi-AZ triển khai tài nguyên (RDS, EC2 qua ASG) qua nhiều Availability Zone trong cùng region để chống lỗi ở mức data center, độ trễ thấp giữa các AZ, chi phí thấp hơn. Multi-Region triển khai qua nhiều region địa lý khác nhau để chống lỗi ở mức toàn region hoặc đáp ứng yêu cầu độ trễ/pháp lý theo vùng, phức tạp hơn về data replication, DNS failover (Route 53), và chi phí cao hơn.

**21. Giải thích khái niệm IAM Policy Evaluation Logic (Allow/Deny).**
AWS mặc định Deny tất cả (implicit deny). Policy Evaluation logic: nếu có bất kỳ Explicit Deny nào trong bất kỳ policy áp dụng (identity-based, resource-based, SCP, permission boundary) thì request bị từ chối ngay lập tức, bất kể có Allow ở đâu khác. Nếu không có Explicit Deny và có ít nhất một Explicit Allow, request được cho phép; nếu không có Allow nào, mặc định vẫn là Deny.

**22. Cross-account access trong AWS được thực hiện như thế nào?**
Thường dùng IAM Role với trust policy cho phép account khác (Principal là ARN account/role) thực hiện `sts:AssumeRole`. Account đích cấp permission policy cho role đó, account nguồn gọi `AssumeRole` để lấy temporary credentials. Cách này an toàn hơn share static credentials và cho phép audit rõ ràng qua CloudTrail.

**23. Giải thích Read Replica và Multi-AZ trong RDS khác nhau thế nào?**
Multi-AZ tạo standby instance đồng bộ (synchronous replication) ở AZ khác chỉ dùng cho failover khi primary lỗi, không phục vụ read traffic trực tiếp (trừ Aurora). Read Replica là bản sao bất đồng bộ (asynchronous), có thể đặt cùng hoặc khác region, dùng để scale read traffic (offload query đọc) và có thể promote thành standalone DB khi cần DR.

**24. Giải thích cơ chế Blue/Green Deployment trên AWS (CodeDeploy/ECS).**
Blue/Green tạo môi trường mới (Green) song song với môi trường cũ (Blue) đang chạy production, deploy version mới lên Green, chạy test/health check, sau đó chuyển traffic (qua ALB target group swap hoặc Route 53 weighted) từ Blue sang Green. Nếu có lỗi, rollback tức thì bằng cách trả traffic về Blue — giảm downtime và rủi ro so với in-place deployment.

**25. VPC Endpoint là gì và tại sao cần dùng?**
VPC Endpoint cho phép truy cập dịch vụ AWS (S3, DynamoDB, hoặc các dịch vụ khác qua PrivateLink) mà không cần đi qua Internet Gateway/NAT Gateway, giữ traffic trong mạng AWS private. Gateway Endpoint (miễn phí, dùng cho S3/DynamoDB) hoạt động qua route table; Interface Endpoint (ENI + PrivateLink) dùng cho hầu hết dịch vụ khác, tính phí theo giờ + data.

**26. Giải thích cơ chế Auto Scaling dựa trên Target Tracking Policy.**
Target Tracking tự động điều chỉnh scale để giữ một metric (ví dụ CPU trung bình 50%) ở mức mục tiêu, tương tự thermostat: AWS tự tính toán scale-out/scale-in cần thiết dựa trên CloudWatch metric, không cần định nghĩa từng bước ngưỡng thủ công như Step Scaling — phù hợp cho hầu hết workload muốn scale đơn giản, ổn định.

**27. Giải thích Shared Responsibility Model của AWS.**
AWS chịu trách nhiệm bảo mật "của Cloud" (physical infrastructure, network, hypervisor, các managed service core). Khách hàng chịu trách nhiệm bảo mật "trong Cloud" (cấu hình IAM, security group, mã hoá dữ liệu, patch OS trên EC2 tự quản, application security). Mức độ trách nhiệm khách hàng giảm dần khi dùng dịch vụ managed cao hơn (ví dụ Lambda ít trách nhiệm OS hơn EC2).

**28. So sánh CloudFormation và Terraform.**
CloudFormation là IaC native của AWS, tích hợp sâu, hỗ trợ rollback tự động, không cần quản lý state file riêng (AWS quản lý), nhưng chỉ hỗ trợ AWS. Terraform (HashiCorp) là multi-cloud, cộng đồng module lớn, cú pháp HCL dễ đọc hơn, nhưng cần tự quản lý state file (thường lưu S3 + DynamoDB lock) và không có rollback tự động như CloudFormation.

**29. DynamoDB khác RDS như thế nào và khi nào chọn NoSQL?**
DynamoDB là managed NoSQL key-value/document database, scale ngang tự động, độ trễ single-digit millisecond, phù hợp workload cần throughput cao, schema linh hoạt, truy vấn theo key. RDS là relational DB phù hợp dữ liệu có cấu trúc, cần join phức tạp, transaction ACID mạnh. Chọn DynamoDB khi cần scale cực lớn và truy vấn đơn giản theo key/index, chọn RDS khi cần quan hệ dữ liệu phức tạp.

**30. Giải thích cơ chế Health Check trong ALB và ASG phối hợp ra sao?**
ALB liên tục gửi request (HTTP/HTTPS) đến path health check (ví dụ `/health`) trên từng target; nếu target fail liên tiếp theo ngưỡng cấu hình, ALB đánh dấu unhealthy và ngừng route traffic tới đó. ASG có thể dùng health check type "ELB" thay vì chỉ EC2 status check, nghĩa là nếu ALB báo instance unhealthy, ASG sẽ terminate và launch instance mới thay thế.

**31. Giải thích Service Control Policy (SCP) trong AWS Organizations.**
SCP là policy áp dụng ở cấp Organization/OU, giới hạn permission tối đa mà bất kỳ IAM entity nào trong account con có thể sử dụng — hoạt động như "guardrail", không tự cấp quyền mà chỉ giới hạn (kết hợp AND logic với IAM policy). Ví dụ SCP có thể chặn hoàn toàn việc tắt CloudTrail dù root user của account con cũng không thể vượt qua.

**32. Giải thích khái niệm Immutable Infrastructure trên AWS.**
Immutable Infrastructure nghĩa là không patch/sửa server đang chạy, thay vào đó build image mới (AMI qua Packer) chứa thay đổi, rồi deploy instance mới thay thế instance cũ (qua ASG rolling update hoặc Blue/Green). Cách này giảm configuration drift, tăng khả năng rollback (chỉ cần chuyển về AMI cũ), và nhất quán giữa các môi trường.

**33. Cost Explorer và Trusted Advisor dùng để làm gì?**
Cost Explorer giúp phân tích, dự báo chi phí AWS theo dịch vụ/tag/thời gian, hỗ trợ ra quyết định Reserved Instance/Savings Plan. Trusted Advisor kiểm tra tài khoản theo 5 tiêu chí (cost optimization, performance, security, fault tolerance, service limits) và đưa ra khuyến nghị cụ thể như security group mở quá rộng, idle resource, hay sắp chạm service quota.

**34. Giải thích cơ chế Encryption at Rest và In Transit trên AWS.**
Encryption at Rest bảo vệ dữ liệu lưu trữ (S3 SSE, EBS volume encryption, RDS storage encryption) thường dùng KMS quản lý key. Encryption in Transit bảo vệ dữ liệu khi truyền (TLS/SSL cho API, ALB HTTPS listener, VPN/Direct Connect cho traffic riêng tư). Best practice là bật cả hai, đặc biệt với dữ liệu nhạy cảm hoặc yêu cầu compliance (PCI-DSS, HIPAA).

**35. Giải thích sự khác biệt giữa Horizontal Scaling và Vertical Scaling trên AWS.**
Vertical Scaling (Scale Up) là tăng cấu hình của một instance (ví dụ đổi t3.medium sang t3.xlarge), đơn giản nhưng có giới hạn phần cứng và thường cần downtime để resize. Horizontal Scaling (Scale Out) là tăng số lượng instance chạy song song (qua ASG), không giới hạn lý thuyết, tăng độ chịu lỗi, nhưng đòi hỏi kiến trúc stateless/load balancing phù hợp.

### Nhóm Nâng cao (36–50)

**36. Thiết kế một kiến trúc Multi-Region Active-Active trên AWS cần lưu ý gì?**
Cần: DNS routing thông minh (Route 53 latency/geoproximity + health check), database đồng bộ 2 chiều (Aurora Global Database hoặc DynamoDB Global Tables xử lý conflict resolution), stateless application layer để traffic có thể route bất kỳ region nào, cơ chế cache/CDN đồng nhất, và giám sát độ trễ replication vì có thể ảnh hưởng đến consistency (thường eventual consistency).

**37. Giải thích cơ chế Split-Brain có thể xảy ra khi nào trong hệ thống AWS và cách phòng tránh.**
Split-brain xảy ra khi mất kết nối giữa các thành phần dẫn đến nhiều node đều tin mình là primary (ví dụ self-managed database cluster mất quorum giữa các AZ). Phòng tránh bằng cách dùng managed service có cơ chế quorum/consensus tích hợp (RDS Multi-AZ, Aurora), thiết kế odd-number quorum node, và dùng health check + fencing mechanism rõ ràng thay vì tự phát hiện lỗi đơn giản.

**38. Phân tích trade-off giữa Fargate và self-managed EC2 cluster cho container workload lớn.**
Fargate giảm overhead vận hành (không patch OS, không quản lý capacity), phù hợp workload biến động hoặc team nhỏ, nhưng chi phí trên mỗi vCPU/GB thường cao hơn và ít kiểm soát tối ưu (ví dụ không tận dụng được Spot sâu như tự quản EC2, giới hạn về networking/storage tuỳ chỉnh). EC2 self-managed cho phép tối ưu chi phí bằng Spot Fleet, bin-packing nhiều container/node, và tuỳ biến kernel/network sâu, đổi lại tốn công vận hành, patch, và capacity planning.

**39. Thiết kế chiến lược Zero-Downtime Database Migration trên AWS (ví dụ dùng DMS).**
Dùng AWS DMS (Database Migration Service) với Full Load + CDC (Change Data Capture) để đồng bộ liên tục dữ liệu từ nguồn sang đích trong khi ứng dụng vẫn chạy trên DB cũ. Sau khi độ trễ replication gần bằng 0, thực hiện cutover ngắn (dừng ghi, chờ đồng bộ hết, chuyển connection string/DNS sang DB mới), có kế hoạch rollback rõ ràng nếu phát hiện lỗi sau cutover.

**40. Thiết kế hệ thống xử lý event ở quy mô lớn dùng SQS + Lambda, cần chú ý gì về throttling và scaling?**
Cần cấu hình Reserved Concurrency cho Lambda để tránh một function "ăn hết" concurrency toàn account gây ảnh hưởng function khác; dùng SQS visibility timeout lớn hơn thời gian xử lý Lambda tối đa để tránh xử lý trùng; cấu hình Dead Letter Queue (DLQ) cho message lỗi liên tục; và cân nhắc Batch Size + Maximum Batching Window để tối ưu throughput mà không vượt Lambda concurrency limit.

**41. Giải thích cơ chế IAM Permission Boundary khác gì SCP?**
Permission Boundary là policy gắn trực tiếp vào một IAM User/Role cụ thể để giới hạn quyền tối đa entity đó có thể có, thường dùng khi delegate quyền tạo IAM entity cho team khác (đảm bảo họ không tự cấp quyền admin). SCP áp dụng ở cấp Organization/OU cho toàn bộ account, không gắn theo từng entity — cả hai đều hoạt động như giới hạn (không tự cấp quyền) và kết hợp AND với IAM policy thông thường.

**42. Phân tích chiến lược tối ưu chi phí (Cost Optimization) toàn diện cho hạ tầng AWS lớn.**
Bao gồm: right-sizing instance dựa trên Compute Optimizer, mix Reserved/Savings Plan cho baseline + Spot cho workload chịu lỗi + On-Demand cho spike, S3 lifecycle policy chuyển tier tự động, dọn resource idle (EBS unattached, Elastic IP không dùng), dùng Graviton (ARM) instance khi tương thích để giảm chi phí ~20%, và thiết lập budget alert + tagging strategy để phân bổ chi phí theo team/project.

**43. Giải thích cách thiết kế Landing Zone / Multi-Account Strategy trong AWS Organizations.**
Landing Zone thiết lập cấu trúc account chuẩn: account Management (root), OU Security (log archive, audit), OU Infrastructure (shared services như networking/Transit Gateway), OU Workloads (chia theo môi trường dev/staging/prod hoặc theo team). Mỗi account áp SCP guardrail phù hợp, dùng AWS Control Tower hoặc tự động hoá bằng Terraform/Account Factory, và tập trung log (CloudTrail, Config) về account Log Archive để đảm bảo audit không bị account con can thiệp.

**44. Giải thích chi tiết cơ chế Aurora Global Database và giới hạn của nó.**
Aurora Global Database cho phép một cluster primary ở một region replicate bất đồng bộ (thường độ trễ <1 giây) sang tối đa các secondary region để đọc hoặc failover nhanh (RTO thường <1 phút). Giới hạn: chỉ một region ghi (writer) tại một thời điểm — không phải active-active thực sự cho write, và ứng dụng cần xử lý eventual consistency khi đọc từ secondary region.

**45. Thiết kế cơ chế Disaster Recovery (DR) theo 4 chiến lược AWS đề xuất (Backup & Restore, Pilot Light, Warm Standby, Multi-Site Active-Active) khác nhau ra sao?**
Backup & Restore: RTO/RPO cao nhất, chi phí thấp nhất, chỉ backup dữ liệu, dựng lại hạ tầng khi cần. Pilot Light: giữ phần lõi (DB replica) chạy sẵn, còn lại scale up khi cần, RTO trung bình. Warm Standby: hạ tầng thu nhỏ chạy sẵn ở DR region, scale full khi failover, RTO thấp hơn. Multi-Site Active-Active: cả hai region phục vụ traffic thực, RTO/RPO gần bằng 0 nhưng chi phí và độ phức tạp cao nhất.

**46. Giải thích vấn đề Hot Partition trong DynamoDB và cách giải quyết.**
Hot Partition xảy ra khi phân bổ traffic không đều lên các partition key (ví dụ dùng timestamp hoặc ID tuần tự làm key khiến toàn bộ traffic dồn vào 1 partition), gây throttling dù tổng capacity đủ. Giải quyết bằng cách thiết kế partition key có độ phân tán cao (ví dụ thêm random suffix/shard số), dùng write sharding pattern, hoặc bật DynamoDB Adaptive Capacity/On-Demand mode để tự cân bằng.

**47. So sánh chi tiết PrivateLink và VPC Peering/Transit Gateway.**
VPC Peering kết nối trực tiếp 2 VPC (network layer, route table 2 chiều), phù hợp số lượng VPC ít vì không transitive. Transit Gateway là hub trung tâm kết nối nhiều VPC/VPN, hỗ trợ routing phức tạp, phù hợp quy mô lớn. PrivateLink (qua Interface Endpoint) chỉ expose một service cụ thể (không phải toàn network) qua ENI, phù hợp mô hình SaaS/service-to-service không cần full network connectivity — bảo mật cao hơn vì giới hạn phạm vi truy cập.

**48. Giải thích cách phát hiện và xử lý sự cố "noisy neighbor" trên EC2 shared tenancy.**
Noisy neighbor là hiện tượng một workload trên cùng physical host chiếm dụng tài nguyên chung (network/disk I/O) ảnh hưởng instance khác. Phát hiện qua CloudWatch metric bất thường (CPU steal time cao, network/EBS throughput giảm không rõ nguyên nhân) so với utilization thực tế. Giải quyết bằng cách chuyển sang Dedicated Instance/Host, dùng instance type có EBS-optimized/burstable credit rõ ràng, hoặc instance family mới hơn có isolation tốt hơn (Nitro-based).

**49. Phân tích cách thiết kế CI/CD pipeline an toàn theo mô hình Least Privilege trên AWS (dùng OIDC thay vì static key).**
Thay vì lưu AWS Access Key/Secret trong CI/CD (GitHub Actions, GitLab CI), cấu hình OIDC Identity Provider tin cậy giữa AWS IAM và CI/CD platform, tạo IAM Role với trust policy giới hạn theo `sub` claim (ví dụ chỉ repo/branch cụ thể), pipeline dùng `sts:AssumeRoleWithWebIdentity` để lấy short-lived credentials theo từng lần chạy — loại bỏ hoàn toàn rủi ro rò rỉ long-lived credentials và giới hạn phạm vi quyền chỉ trong thời gian pipeline chạy.

**50. Một hệ thống production trên AWS đột ngột tăng latency toàn bộ API dù traffic không tăng đột biến — bạn sẽ debug theo trình tự nào?**
Trình tự debug hệ thống: (1) Kiểm tra CloudWatch metric tầng ALB (TargetResponseTime, HTTPCode 5xx) để xác định lỗi ở tầng nào; (2) Kiểm tra EC2/ECS metric (CPU, memory, network) xem có resource nào bão hoà; (3) Kiểm tra RDS Performance Insights xem có query chậm/lock/connection pool cạn kiệt; (4) Kiểm tra downstream dependency (third-party API, cache Redis/ElastiCache hit rate giảm); (5) Kiểm tra thay đổi gần nhất (deployment, config change, DNS/Route 53 propagation, certificate expiry); (6) Dùng X-Ray tracing nếu có để xác định chính xác service/segment nào gây độ trễ trong distributed request.

---

## PHẦN 2: Kubernetes (50 câu)

### Nhóm Cơ bản (1–17)

**1. Kubernetes là gì và giải quyết vấn đề gì so với chạy container thủ công?**
Kubernetes (K8s) là hệ thống orchestration container tự động hoá việc triển khai, scale, quản lý vòng đời container trên nhiều máy chủ (node). Nó giải quyết các vấn đề: tự phục hồi khi container/node lỗi (self-healing), tự động scale theo tải, service discovery/load balancing nội bộ, rolling update không downtime, và quản lý cấu hình/secret tập trung — điều rất khó làm thủ công khi có hàng chục/hàng trăm container.

**2. Kiến trúc Control Plane của Kubernetes gồm những thành phần nào?**
Control Plane gồm: API Server (cổng giao tiếp duy nhất, xác thực và xử lý mọi request), etcd (key-value store lưu toàn bộ state cluster), Scheduler (quyết định pod chạy trên node nào dựa trên resource/affinity), Controller Manager (chạy các control loop như Node Controller, Replication Controller để đảm bảo state thực tế khớp state mong muốn), và Cloud Controller Manager (tương tác với cloud provider API).

**3. Node trong Kubernetes gồm những thành phần gì?**
Mỗi Node (worker) chạy: Kubelet (agent giao tiếp với API Server, đảm bảo container trong Pod chạy đúng theo spec), Kube-proxy (quản lý network rule, load balancing giữa các Pod cho Service), và Container Runtime (containerd, CRI-O... thực thi container theo chuẩn CRI).

**4. Pod là gì và tại sao Kubernetes không quản lý trực tiếp Container?**
Pod là đơn vị triển khai nhỏ nhất trong K8s, chứa một hoặc nhiều container chia sẻ network namespace (cùng IP) và storage volume. K8s dùng Pod thay vì Container trực tiếp vì nhiều container liên quan chặt chẽ (ví dụ sidecar pattern: app + log shipper) cần chạy cùng nhau, chia sẻ tài nguyên network/storage như một đơn vị logic duy nhất.

**5. Phân biệt Deployment, ReplicaSet, và Pod.**
Pod là đơn vị chạy container nhỏ nhất. ReplicaSet đảm bảo luôn có đúng số lượng Pod replica chỉ định đang chạy, tự tạo/xoá Pod khi cần. Deployment quản lý ReplicaSet ở tầng cao hơn, hỗ trợ rolling update/rollback bằng cách tạo ReplicaSet mới và giảm dần ReplicaSet cũ — hầu như luôn dùng Deployment thay vì tạo ReplicaSet/Pod trực tiếp.

**6. Service trong Kubernetes dùng để làm gì và các loại Service phổ biến?**
Service cung cấp một endpoint ổn định (ClusterIP cố định + DNS name) để truy cập tập Pod động (Pod có thể chết/tạo lại với IP mới). Các loại: ClusterIP (chỉ truy cập nội bộ cluster, mặc định), NodePort (expose port cố định trên mọi node), LoadBalancer (tạo external load balancer của cloud provider), và ExternalName (map tới DNS bên ngoài).

**7. Ingress là gì và khác gì với Service loại LoadBalancer?**
Ingress là tài nguyên định nghĩa routing HTTP/HTTPS (theo host/path) vào các Service nội bộ, cần một Ingress Controller (Nginx, Traefik, ALB Ingress Controller...) để thực thi rule đó. Khác với Service LoadBalancer (mỗi service cần một load balancer riêng, tốn kém khi có nhiều service), Ingress cho phép dùng một load balancer/entrypoint duy nhất để route nhiều service theo domain/path, tiết kiệm chi phí và dễ quản lý TLS tập trung.

**8. ConfigMap và Secret khác nhau như thế nào?**
ConfigMap lưu dữ liệu cấu hình không nhạy cảm dạng key-value (ví dụ biến môi trường, file config), lưu plaintext trong etcd. Secret dùng cho dữ liệu nhạy cảm (password, token, certificate), được encode base64 (không phải mã hoá thực sự trừ khi bật encryption at rest cho etcd), và K8s có cơ chế giới hạn quyền truy cập/mount Secret chặt hơn qua RBAC.

**9. Namespace trong Kubernetes dùng để làm gì?**
Namespace là cơ chế phân vùng logic trong một cluster vật lý, cho phép nhiều team/môi trường (dev, staging) chia sẻ cùng cluster mà vẫn cô lập resource (tên resource chỉ cần unique trong namespace), áp dụng ResourceQuota/LimitRange và RBAC riêng cho từng namespace.

**10. Liveness Probe và Readiness Probe khác nhau ra sao?**
Liveness Probe kiểm tra container còn "sống" hay không — nếu fail, Kubelet sẽ restart container đó. Readiness Probe kiểm tra container đã sẵn sàng nhận traffic hay chưa — nếu fail, Pod bị gỡ khỏi Endpoint của Service (không nhận traffic) nhưng KHÔNG bị restart, thường dùng khi app cần thời gian warm-up hoặc tạm thời không xử lý được request (ví dụ đang kết nối lại DB).

**11. Resource Requests và Limits trong Pod spec dùng để làm gì?**
Requests là lượng tài nguyên (CPU/memory) tối thiểu Pod cần, Scheduler dùng giá trị này để quyết định đặt Pod vào node nào có đủ tài nguyên. Limits là mức tối đa Pod được phép dùng — vượt memory limit Pod bị OOMKilled, vượt CPU limit Pod bị throttle (không bị kill). Thiết lập đúng requests/limits giúp tránh resource contention và đảm bảo QoS class phù hợp.

**12. QoS Class trong Kubernetes gồm những loại nào?**
Guaranteed: requests = limits cho cả CPU và memory, ưu tiên cao nhất, ít bị evict khi node thiếu tài nguyên. Burstable: có requests nhưng limits khác requests (hoặc chỉ định một phần), ưu tiên trung bình. BestEffort: không khai báo requests/limits, ưu tiên thấp nhất, bị evict đầu tiên khi node áp lực tài nguyên.

**13. StatefulSet khác Deployment như thế nào?**
StatefulSet dùng cho ứng dụng cần trạng thái ổn định: mỗi Pod có tên/hostname cố định theo thứ tự (pod-0, pod-1...), gắn với PersistentVolume riêng biệt không đổi khi Pod restart, và được tạo/xoá tuần tự (ordered). Deployment coi các Pod là interchangeable (không định danh cố định), phù hợp ứng dụng stateless.

**14. DaemonSet dùng để làm gì?**
DaemonSet đảm bảo mỗi Node (hoặc tập Node theo nodeSelector) chạy đúng một bản sao của Pod chỉ định — thường dùng cho agent hạ tầng cần chạy trên mọi node như log collector (Fluentd), monitoring agent (Node Exporter), hoặc network plugin (CNI).

**15. Job và CronJob trong Kubernetes khác gì Deployment?**
Job tạo Pod chạy một lần đến khi hoàn thành task rồi dừng (không restart liên tục như Deployment), phù hợp batch processing. CronJob tạo Job theo lịch định kỳ (cú pháp cron), phù hợp task lặp lại như backup định kỳ, gửi report hàng ngày.

**16. Volume và PersistentVolume (PV)/PersistentVolumeClaim (PVC) khác nhau ra sao?**
Volume thông thường (emptyDir, configMap...) gắn liền vòng đời với Pod, mất khi Pod bị xoá. PersistentVolume là tài nguyên storage tồn tại độc lập với vòng đời Pod (backed bởi EBS, NFS, hoặc storage cloud khác), PersistentVolumeClaim là "yêu cầu" từ ứng dụng để mượn một PV phù hợp — tách biệt việc ứng dụng khai báo nhu cầu storage khỏi việc admin cấu hình storage vật lý.

**17. kubectl là gì và một số lệnh cơ bản thường dùng.**
kubectl là CLI chính thức để tương tác với API Server của Kubernetes. Các lệnh cơ bản: `kubectl get pods -n <namespace>` xem danh sách Pod, `kubectl describe pod <name>` xem chi tiết/event, `kubectl logs <pod> -c <container>` xem log, `kubectl apply -f file.yaml` áp dụng manifest, `kubectl exec -it <pod> -- sh` vào shell container, `kubectl rollout status/undo deployment/<name>` theo dõi/rollback deployment.

### Nhóm Trung cấp (18–35)

**18. Giải thích cơ chế Rolling Update trong Deployment và các tham số quan trọng.**
Rolling Update dần dần thay Pod cũ bằng Pod mới, kiểm soát bởi `maxSurge` (số Pod tối đa được tạo thêm vượt số replica mong muốn trong lúc update) và `maxUnavailable` (số Pod tối đa được phép unavailable trong lúc update). K8s chỉ chuyển sang thay thế tiếp khi Pod mới pass Readiness Probe, đảm bảo không downtime nếu cấu hình đúng.

**19. Horizontal Pod Autoscaler (HPA) hoạt động dựa trên cơ chế nào?**
HPA theo dõi metric (CPU/memory qua Metrics Server, hoặc custom metric qua Prometheus Adapter) theo chu kỳ (mặc định 15s), so sánh với target đã cấu hình, và tính toán số replica cần thiết theo công thức tỷ lệ (desiredReplicas = ceil(currentReplicas × currentMetric / targetMetric]), sau đó cập nhật field `replicas` của Deployment/ReplicaSet tương ứng.

**20. Node Affinity, Pod Affinity, Anti-Affinity khác nhau ra sao?**
Node Affinity ràng buộc Pod chỉ chạy trên Node có label phù hợp (giống nodeSelector nhưng linh hoạt hơn, hỗ trợ điều kiện phức tạp và soft/hard rule). Pod Affinity ràng buộc Pod nên/phải chạy gần Pod khác có label nhất định (ví dụ cùng zone để giảm latency). Pod Anti-Affinity ngược lại, tránh đặt Pod cùng loại trên cùng Node/zone để tăng độ chịu lỗi (high availability).

**21. Taints và Tolerations dùng để làm gì?**
Taint gắn lên Node để "đẩy" các Pod ra khỏi Node đó trừ khi Pod có Toleration tương ứng chấp nhận taint đó. Cơ chế này ngược với Affinity (Affinity là Pod "kéo" về phía Node), thường dùng để dành riêng Node cho mục đích đặc biệt (GPU node, node dedicated cho một team) hoặc đánh dấu Node có vấn đề (NoExecute taint để evict Pod khỏi Node lỗi).

**22. RBAC trong Kubernetes hoạt động như thế nào?**
RBAC (Role-Based Access Control) gồm Role/ClusterRole (định nghĩa tập hành động được phép trên resource nào, Role giới hạn trong namespace còn ClusterRole áp dụng toàn cluster) và RoleBinding/ClusterRoleBinding (gán Role/ClusterRole đó cho User/Group/ServiceAccount cụ thể). Đây là cơ chế authorization chuẩn để giới hạn quyền truy cập API Server theo nguyên tắc least privilege.

**23. Network Policy trong Kubernetes dùng để làm gì?**
Network Policy định nghĩa rule kiểm soát traffic ingress/egress giữa các Pod (dựa trên label selector, namespace, hoặc CIDR), mặc định Kubernetes cho phép mọi Pod giao tiếp tự do với nhau — Network Policy giúp áp dụng mô hình zero-trust/micro-segmentation, yêu cầu CNI plugin hỗ trợ (như Calico, Cilium; CNI mặc định của một số cloud không hỗ trợ Network Policy).

**24. Giải thích cơ chế Service Discovery trong Kubernetes qua DNS.**
CoreDNS (hoặc kube-dns) chạy như một Service nội bộ cluster, tự động tạo DNS record cho mỗi Service theo định dạng `<service-name>.<namespace>.svc.cluster.local`, cho phép Pod gọi Service khác chỉ bằng tên (nếu cùng namespace) mà không cần biết IP cụ thể — IP của Pod backend có thể đổi liên tục nhưng DNS/Service ClusterIP luôn ổn định.

**25. Giải thích các thành phần chính của kiến trúc CNI (Container Network Interface).**
CNI là chuẩn plugin chịu trách nhiệm cấp phát IP và thiết lập network namespace cho Pod khi được tạo. Các CNI phổ biến: Calico (hỗ trợ Network Policy mạnh, dùng BGP routing), Flannel (đơn giản, overlay network dùng VXLAN), Cilium (dựa trên eBPF, hiệu năng cao, hỗ trợ observability sâu). Việc chọn CNI ảnh hưởng đến khả năng hỗ trợ Network Policy, hiệu năng network, và độ phức tạp vận hành.

**26. Giải thích cơ chế Pod Disruption Budget (PDB).**
PDB định nghĩa số lượng Pod tối thiểu (minAvailable) hoặc tối đa được phép gián đoạn (maxUnavailable) trong một ứng dụng khi xảy ra voluntary disruption (như node drain khi upgrade/maintenance) — không áp dụng cho involuntary disruption (node crash đột ngột). PDB giúp đảm bảo ứng dụng luôn duy trì đủ replica phục vụ traffic ngay cả khi cluster đang thực hiện bảo trì.

**27. Init Container dùng để làm gì và khác Sidecar Container ra sao?**
Init Container chạy tuần tự trước khi container chính trong Pod khởi động, dùng để chuẩn bị môi trường (chờ dependency sẵn sàng, tải config, migrate DB schema) — chạy xong thì dừng hẳn. Sidecar Container chạy song song, xuyên suốt vòng đời Pod cùng container chính, thường dùng cho proxy (Envoy trong service mesh), log shipper, hay cache local.

**28. Helm là gì và giải quyết vấn đề gì?**
Helm là package manager cho Kubernetes, đóng gói manifest YAML phức tạp thành "Chart" có thể tham số hoá qua `values.yaml`, hỗ trợ templating (Go template), quản lý version release, và rollback dễ dàng (`helm rollback`) — giải quyết vấn đề phải copy/paste và chỉnh sửa thủ công nhiều file YAML lặp lại giữa các môi trường.

**29. Giải thích cơ chế Admission Controller trong Kubernetes.**
Admission Controller can thiệp vào request sau khi qua Authentication/Authorization nhưng trước khi persist vào etcd, gồm Mutating (có thể sửa đổi object, ví dụ tự inject sidecar) và Validating (chỉ kiểm tra và chấp nhận/từ chối, không sửa). Đây là cơ chế nền cho các policy engine như OPA/Gatekeeper hoặc Kyverno để enforce rule tuỳ chỉnh (ví dụ bắt buộc mọi Pod phải có resource limits).

**30. CRD (Custom Resource Definition) và Operator Pattern là gì?**
CRD cho phép mở rộng API Kubernetes bằng resource tuỳ chỉnh (ví dụ `Database`, `Certificate`) ngoài các resource built-in. Operator Pattern kết hợp CRD với một Controller tuỳ chỉnh liên tục reconcile trạng thái thực tế khớp với spec mong muốn của resource đó — dùng để tự động hoá vận hành ứng dụng phức tạp (ví dụ Operator tự backup, failover cho một database cluster).

**31. Giải thích khái niệm Reconciliation Loop trong Kubernetes Controller.**
Reconciliation Loop là vòng lặp cốt lõi của mọi Controller: liên tục so sánh "desired state" (khai báo trong spec) với "current state" (observed thực tế trong cluster), và thực hiện hành động để đưa current state tiến gần desired state — lặp lại liên tục (level-triggered, không phải edge-triggered) giúp hệ thống tự phục hồi ngay cả khi bỏ lỡ một sự kiện.

**32. Etcd đóng vai trò gì và tại sao cần backup thường xuyên?**
Etcd là distributed key-value store lưu toàn bộ state của cluster (mọi object, config, secret) — mất etcd đồng nghĩa mất toàn bộ cluster. Cần backup định kỳ (snapshot) và test restore, vì etcd cũng nhạy cảm về hiệu năng (yêu cầu disk latency thấp, số node lẻ 3/5 để đảm bảo quorum Raft consensus).

**33. Giải thích chiến lược Cluster Upgrade an toàn trong Kubernetes.**
Nên upgrade Control Plane trước (theo thứ tự minor version, không skip quá 1 minor version mỗi lần), sau đó upgrade từng Node theo cách cordon (đánh dấu không nhận Pod mới) → drain (di chuyển Pod hiện có sang node khác an toàn, tôn trọng PDB) → upgrade kubelet/OS → uncordon. Luôn kiểm tra deprecated API trước khi upgrade để tránh manifest bị lỗi ở version mới.

**34. Giải thích sự khác biệt giữa kubectl apply và kubectl create.**
`kubectl create` tạo resource mới, báo lỗi nếu resource đã tồn tại — phù hợp thao tác một lần, không lưu lịch sử thay đổi. `kubectl apply` áp dụng cấu hình theo kiểu declarative, tự động tạo mới nếu chưa có hoặc patch phần khác biệt nếu đã tồn tại, lưu lại last-applied-configuration annotation để tính toán diff cho lần apply sau — phù hợp GitOps/CI-CD vì idempotent.

**35. Giải thích cách Kubernetes xử lý khi Node bị mất kết nối (NotReady)?**
Kubelet gửi heartbeat định kỳ tới API Server; nếu không nhận được sau `node-monitor-grace-period` (mặc định 40s), Node Controller đánh dấu Node là NotReady. Sau `pod-eviction-timeout` (mặc định 5 phút), Pod trên Node đó được đánh dấu để evict và tái tạo trên Node khác (nếu được quản lý bởi Deployment/ReplicaSet) — Pod chạy độc lập (không qua Controller) sẽ không được tự động tái tạo.

### Nhóm Nâng cao (36–50)

**36. Thiết kế chiến lược Multi-Tenancy trong một cluster Kubernetes dùng chung cho nhiều team.**
K��t hợp: Namespace theo team + ResourceQuota/LimitRange giới hạn tài nguyên mỗi team, RBAC chi tiết theo namespace, Network Policy cô lập traffic giữa namespace, Admission Policy (OPA/Kyverno) enforce chuẩn chung (labels bắt buộc, image registry được phép), và cân nhắc dùng thêm cơ chế cô lập mạnh hơn (node pool riêng, hoặc virtual cluster như vCluster/Kiosk) nếu yêu cầu cô lập bảo mật cao hơn namespace thông thường.

**37. Phân tích trade-off giữa chạy Service Mesh (Istio/Linkerd) và không dùng Service Mesh.**
Service Mesh cung cấp mTLS tự động giữa service, traffic shaping/canary release chi tiết, observability (metric/trace) đồng nhất không cần sửa code, và circuit breaking/retry policy tập trung — đổi lại thêm độ phức tạp vận hành đáng kể (thêm sidecar proxy mỗi Pod, tăng latency nhỏ, tăng resource overhead, và một control plane mới cần học/vận hành). Nên cân nhắc dùng khi số lượng microservice đủ lớn để lợi ích vượt chi phí vận hành.

**38. Giải thích cơ chế Leader Election trong Kubernetes Controller HA (ví dụ nhiều instance Controller Manager).**
Khi chạy nhiều bản sao của Controller/Scheduler để HA, chỉ một instance được là "leader" thực sự thực thi logic tại một thời điểm (tránh xung đột), các instance khác ở chế độ standby. Cơ chế thường dựa trên việc giữ một Lease object trong etcd (renew định kỳ) — nếu leader hiện tại không renew kịp thời (do crash/mất kết nối), một instance khác sẽ acquire lease và trở thành leader mới.

**39. Debug một Pod bị CrashLoopBackOff, bạn sẽ thực hiện các bước nào?**
Trình tự: (1) `kubectl describe pod` xem Events để biết lý do gần nhất (OOMKilled, lỗi image pull, failed probe...); (2) `kubectl logs <pod> --previous` xem log của lần chạy trước khi crash; (3) Kiểm tra resource limits có quá thấp gây OOMKilled không; (4) Kiểm tra command/entrypoint và biến môi trường/config có đúng không; (5) Nếu cần debug sâu, dùng `kubectl debug` tạo ephemeral container hoặc tạm sửa command thành sleep để exec vào kiểm tra filesystem/network thủ công.

**40. Giải thích chi tiết cơ chế Scheduler Kubernetes khi quyết định đặt Pod vào Node (Filtering & Scoring).**
Scheduler qua 2 giai đoạn: Filtering (loại bỏ Node không đủ điều kiện — thiếu tài nguyên, không match nodeSelector/affinity, có taint không được tolerate) và Scoring (chấm điểm các Node còn lại theo nhiều tiêu chí như mức độ cân bằng tài nguyên, độ gần Pod affinity, image đã có sẵn trên Node hay chưa), sau đó chọn Node điểm cao nhất. Có thể mở rộng logic này qua Scheduler Extender hoặc Scheduling Framework plugin tuỳ chỉnh.

**41. Phân tích chiến lược giảm thiểu "Noisy Neighbor" trong cluster Kubernetes multi-tenant.**
Áp dụng ResourceQuota/LimitRange nghiêm ngặt theo namespace, dùng CPU/Memory requests=limits (Guaranteed QoS) cho workload quan trọng để tránh bị ảnh hưởng bởi Burstable/BestEffort Pod khác, cân nhắc dùng node pool riêng cho tenant nhạy cảm (dedicated node + taint/toleration), và giám sát cgroup-level metric để phát hiện Pod nào đang "ăn" tài nguyên vượt mức cam kết.

**42. Giải thích cách thiết kế zero-downtime schema migration cho database chạy trong StatefulSet.**
Áp dụng expand-contract pattern: (1) Deploy schema change tương thích ngược (expand, ví dụ thêm cột mới không xoá cột cũ); (2) Deploy version ứng dụng mới đọc/ghi cả 2 schema hoặc chỉ dùng cột mới với backward-compatible logic; (3) Sau khi toàn bộ traffic đã chuyển hẳn sang version mới, mới thực hiện migration "contract" (xoá cột/cấu trúc cũ) — tất cả thực hiện qua Job riêng biệt kết hợp health check kỹ trước khi cho phép rolling update tiếp tục.

**43. Phân tích ưu nhược điểm giữa self-hosted Kubernetes và managed Kubernetes (EKS/GKE/AKS).**
Self-hosted (kubeadm, kops...) cho phép kiểm soát hoàn toàn version, cấu hình control plane, phù hợp yêu cầu compliance/air-gapped đặc biệt, nhưng tốn nhiều công sức vận hành/patch/HA etcd. Managed Kubernetes giảm gánh nặng vận hành control plane (cloud provider tự quản lý HA, patch, backup etcd), tích hợp sẵn với IAM/networking của cloud, nhưng giới hạn tuỳ biến sâu vào control plane và phụ thuộc vào cloud provider.

**44. Giải thích cơ chế Vertical Pod Autoscaler (VPA) và vì sao khó dùng chung với HPA trên cùng metric.**
VPA tự động điều chỉnh requests/limits của Pod dựa trên lịch sử sử dụng thực tế (thường phải recreate Pod để áp dụng thay đổi vì không thể resize container đang chạy trong hầu hết trường hợp trước Kubernetes 1.27). Dùng chung VPA và HPA trên cùng metric (ví dụ cả hai đều dựa vào CPU) có thể gây xung đột (VPA giảm requests trong khi HPA scale theo % CPU dựa trên requests đó) — nên tách metric khác nhau hoặc chỉ dùng VPA ở chế độ "recommendation only" song song HPA.

**45. Thiết kế Disaster Recovery cho một cluster Kubernetes (không chỉ dữ liệu ứng dụng mà cả cluster state).**
Cần: backup etcd định kỳ (velero hoặc snapshot etcd trực tiếp) lưu ở nơi khác cluster, backup PersistentVolume data (Velero + volume snapshot của cloud provider), lưu toàn bộ manifest/Helm chart trong Git (GitOps) để có thể tái tạo cluster từ đầu, và test định kỳ quy trình restore toàn bộ (không chỉ backup) trên cluster mới để đảm bảo RTO/RPO thực tế đạt yêu cầu.

**46. Giải thích cơ chế Cgroup và Container Isolation, và giới hạn bảo mật so với VM.**
Container dùng Linux namespaces (PID, network, mount...) để cô lập môi trường nhìn thấy và cgroups để giới hạn/kiểm soát tài nguyên (CPU, memory), nhưng vẫn chia sẻ chung kernel host — khác VM có hypervisor cô lập hoàn toàn ở tầng phần cứng ảo hoá. Vì chia sẻ kernel, một lỗ hổng kernel hoặc container escape có thể ảnh hưởng toàn bộ Node — đây là lý do cần thêm lớp bảo mật như seccomp, AppArmor/SELinux, hoặc runtime cô lập mạnh hơn như gVisor/Kata Containers cho workload nhạy cảm.

**47. Phân tích chiến lược Canary Deployment thuần Kubernetes (không dùng service mesh) thực hiện thế nào?**
Có thể thực hiện bằng cách tạo 2 Deployment (stable và canary) cùng gắn label selector chung mà Service dùng để route traffic, điều chỉnh tỷ lệ traffic gián tiếp qua tỷ lệ số Pod (ví dụ 9 Pod stable, 1 Pod canary ~10% traffic tự nhiên theo round-robin của kube-proxy) — cách này thô hơn Service Mesh (không kiểm soát % chính xác hay theo header/cookie) nhưng đơn giản, không cần thêm control plane.

**48. Giải thích các nguyên nhân phổ biến gây ra API Server chậm hoặc quá tải trong cluster lớn.**
Nguyên nhân thường gặp: quá nhiều watch connection từ client (controller/operator tuỳ chỉnh) không dùng informer cache đúng cách gây spam request; etcd chậm do disk I/O latency cao hoặc quá nhiều object/version tích luỹ (cần compaction định kỳ); số lượng object quá lớn trong một namespace/cluster (nhiều ConfigMap/Secret/Event); hoặc thiếu rate limiting/QPS phù hợp cho các client nội bộ — cần giám sát metric `apiserver_request_duration_seconds` và etcd latency để xác định nguyên nhân gốc.

**49. Thiết kế cơ chế Secrets Management nâng cao hơn Kubernetes Secret mặc định (ví dụ tích hợp Vault).**
Kubernetes Secret mặc định chỉ base64-encode (không mã hoá thực sự trừ khi bật EncryptionConfiguration cho etcd), và thường lưu lâu dài trong etcd — không lý tưởng cho compliance cao. Giải pháp nâng cao: dùng External Secrets Operator hoặc Vault Agent Injector để lấy secret động từ HashiCorp Vault/AWS Secrets Manager tại runtime (không lưu bản sao cố định trong etcd), hỗ trợ rotation tự động, audit log chi tiết, và dynamic secret (credentials ngắn hạn theo từng Pod).

**50. Một cluster production đột nhiên có hàng loạt Pod bị Evicted — bạn phân tích nguyên nhân và xử lý như thế nào?**
Đầu tiên kiểm tra `kubectl describe node` xem có Node bị áp lực tài nguyên (MemoryPressure, DiskPressure, PIDPressure) gây Kubelet chủ động evict Pod theo QoS (BestEffort bị evict trước, sau đó Burstable, cuối cùng mới Guaranteed). Kiểm tra xem có Pod nào dùng vượt quá requests đã khai (gây tranh chấp tài nguyên node), disk có bị đầy do log/image tích luỹ không dọn (image garbage collection), hoặc do một batch job/Node scale-down đột ngột. Xử lý: tăng resource requests chính xác hơn cho workload quan trọng, thiết lập PDB để giới hạn evict đồng loạt, dọn dẹp disk/log, và cân nhắc Cluster Autoscaler nếu do thiếu capacity thực sự.

---

## PHẦN 3: GitHub Actions (50 câu)

### Nhóm Cơ bản (1–17)

**1. GitHub Actions là gì?**
GitHub Actions là nền tảng CI/CD tích hợp sẵn trong GitHub, cho phép tự động hoá build/test/deploy dựa trên sự kiện xảy ra trong repository (push, pull request, issue, release...) thông qua file YAML định nghĩa workflow, không cần cài đặt CI server riêng biệt.

**2. Cấu trúc cơ bản của một workflow file gồm những gì?**
Một workflow file (đặt trong `.github/workflows/*.yml`) gồm: `name` (tên workflow), `on` (sự kiện trigger), `jobs` (danh sách job chạy độc lập hoặc phụ thuộc nhau), mỗi job có `runs-on` (loại runner) và `steps` (danh sách bước thực thi tuần tự, mỗi step có thể là `run` command hoặc `uses` một action có sẵn).

**3. Phân biệt `on: push` và `on: pull_request`.**
`on: push` trigger workflow khi có commit được push trực tiếp lên nhánh chỉ định (ví dụ merge vào main). `on: pull_request` trigger khi có PR được tạo/cập nhật/đóng, chạy trên mã nguồn merge tạm thời giữa branch nguồn và đích — thường dùng để kiểm tra chất lượng code trước khi merge, và mặc định không có quyền ghi (write) vào secret nhạy cảm khi PR đến từ fork.

**4. Job và Step khác nhau như thế nào?**
Job là một đơn vị thực thi độc lập, mặc định chạy song song trên một runner riêng (trừ khi khai báo `needs` để tạo phụ thuộc tuần tự); các job không chia sẻ filesystem trừ khi dùng artifact. Step là một hành động đơn lẻ bên trong job, chạy tuần tự trên cùng runner, chia sẻ chung filesystem và biến môi trường trong phạm vi job đó.

**5. `runs-on` dùng để làm gì, các giá trị phổ biến?**
`runs-on` chỉ định loại runner (máy chạy job) sẽ dùng. Giá trị phổ biến: `ubuntu-latest`, `windows-latest`, `macos-latest` (GitHub-hosted runner, dùng chung tài nguyên miễn phí/theo phút cho private repo), hoặc tên label tự đặt cho self-hosted runner (máy chủ riêng do người dùng tự quản lý).

**6. GitHub-hosted Runner khác Self-hosted Runner ra sao?**
GitHub-hosted Runner do GitHub cung cấp, tự động tạo mới hoàn toàn sạch cho mỗi job (ephemeral), không cần bảo trì nhưng giới hạn tài nguyên (CPU/RAM/disk) và tính phí theo phút với private repo. Self-hosted Runner do người dùng tự cài đặt và quản lý (trên máy riêng, VM, hoặc container), kiểm soát hoàn toàn tài nguyên/phần mềm cài sẵn, không tính phí theo phút của GitHub nhưng phải tự chịu trách nhiệm bảo mật, patch, và scaling.

**7. Secrets trong GitHub Actions dùng để làm gì và cách khai báo?**
Secrets lưu trữ dữ liệu nhạy cảm (API key, token, password) được mã hoá, không hiển thị trong log (tự động mask), khai báo ở cấp Repository/Organization/Environment Settings. Truy cập trong workflow qua `${{ secrets.TÊN_SECRET }}`, không bao giờ nên hardcode giá trị nhạy cảm trực tiếp trong file YAML.

**8. Artifact trong GitHub Actions là gì?**
Artifact là file/thư mục được lưu tạm sau khi job chạy xong (qua action `actions/upload-artifact`), có thể tải xuống thủ công hoặc dùng lại ở job khác trong cùng workflow (`actions/download-artifact`) — dùng để truyền dữ liệu giữa các job (vì job không chia sẻ filesystem mặc định) hoặc lưu build output/report để kiểm tra sau.

**9. Giải thích cú pháp `${{ }}` (expression) trong GitHub Actions.**
`${{ }}` là cú pháp expression cho phép truy cập context (như `github`, `secrets`, `env`, `steps`, `matrix`, `needs`), thực hiện so sánh/điều kiện, hoặc nối chuỗi. Ví dụ: `if: ${{ github.ref == 'refs/heads/main' }}` chỉ chạy step khi trên nhánh main, hay `${{ secrets.API_KEY }}` để lấy giá trị secret.

**10. `actions/checkout` dùng để làm gì và tại sao hầu như workflow nào cũng cần?**
`actions/checkout` là action chính thức để clone mã nguồn repository vào runner trước khi thực hiện các bước khác — vì mỗi job chạy trên một runner "trắng" (đặc biệt GitHub-hosted), không tự có sẵn code, nên hầu hết workflow cần bước này đầu tiên để có mã nguồn thao tác.

**11. Environment Variable trong GitHub Actions khai báo như thế nào?**
Có thể khai báo ở nhiều cấp: `env` ở cấp workflow (áp dụng toàn bộ), cấp job (áp dụng job đó), hoặc cấp step (chỉ step đó) — cấp thấp hơn override cấp cao hơn. Ví dụ: `env: NODE_ENV: production`. Cũng có thể set biến động trong runtime bằng cách ghi vào file đặc biệt `$GITHUB_ENV`.

**12. Matrix Build là gì?**
Matrix Build cho phép chạy cùng một job với nhiều tổ hợp biến khác nhau (ví dụ nhiều phiên bản Node.js, nhiều hệ điều hành) song song, khai báo qua `strategy.matrix`. Ví dụ test trên `node-version: [16, 18, 20]` và `os: [ubuntu-latest, windows-latest]` sẽ tự động tạo 6 job chạy song song với tổ hợp tương ứng.

**13. Workflow_dispatch là gì?**
`workflow_dispatch` là trigger cho phép chạy workflow thủ công qua giao diện GitHub hoặc API, có thể định nghĩa input tham số (như chọn environment để deploy) — hữu ích cho các workflow cần chạy theo yêu cầu (deploy thủ công, chạy migration) thay vì tự động theo mỗi push.

**14. `needs` trong job dùng để làm gì?**
`needs` định nghĩa phụ thuộc giữa các job, chỉ định job hiện tại chỉ chạy sau khi (các) job liệt kê trong `needs` hoàn thành thành công — dùng để tạo pipeline tuần tự (ví dụ job `build` phải xong trước khi job `deploy` chạy) thay vì mặc định chạy song song.

**15. Continue-on-error và if: always() dùng khi nào?**
`continue-on-error: true` cho phép step/job đó fail mà không làm dừng toàn bộ workflow (workflow vẫn tiếp tục các step/job sau). `if: always()` đảm bảo step/job đó luôn chạy dù các step trước có fail hay không — thường dùng cho bước dọn dẹp (cleanup) hoặc gửi thông báo kết quả bất kể pass/fail.

**16. Cách trigger workflow theo schedule (cron)?**
Dùng trigger `on: schedule` với cú pháp cron chuẩn POSIX, ví dụ `- cron: '0 2 * * *'` chạy lúc 2h sáng UTC hàng ngày — thường dùng cho task định kỳ như dọn dẹp resource, backup, hay chạy kiểm tra sức khoẻ hệ thống.

**17. Concurrency trong GitHub Actions dùng để làm gì?**
`concurrency` giới hạn số lượng workflow run đồng thời theo group được chỉ định (ví dụ theo branch), có thể kèm `cancel-in-progress: true` để tự động huỷ run cũ khi có run mới trong cùng group — hữu ích để tránh nhiều deployment chạy chồng chéo lên cùng environment hoặc tiết kiệm tài nguyên khi push liên tục lên cùng PR.

### Nhóm Trung cấp (18–35)

**18. Reusable Workflow là gì và khác Composite Action ra sao?**
Reusable Workflow là toàn bộ file workflow (`.yml`) được gọi lại từ workflow khác qua `uses: org/repo/.github/workflows/file.yml@ref`, hỗ trợ nhận input/secret và định nghĩa nhiều job — phù hợp chia sẻ pipeline hoàn chỉnh giữa nhiều repo. Composite Action là một action đóng gói nhiều step thành một action đơn (định nghĩa trong `action.yml`), được gọi như một step đơn lẻ trong job (`uses: ./path-to-action`) — phù hợp đóng gói logic step lặp lại, nhỏ gọn hơn reusable workflow.

**19. Giải thích cơ chế Caching trong GitHub Actions (`actions/cache`).**
`actions/cache` lưu lại thư mục (ví dụ `node_modules`, `~/.m2`) dựa trên `key` (thường hash theo lock file như `package-lock.json`) để tái sử dụng ở lần chạy sau, giảm thời gian cài dependency. Nếu không tìm thấy key khớp chính xác, có thể dùng `restore-keys` để lấy cache gần đúng nhất làm base, giúp tăng tốc dù không trúng 100%.

**20. GitHub Actions Environment (Environment Protection Rules) dùng để làm gì?**
Environment (khai báo trong Settings) cho phép gắn Secret/Variable riêng cho từng môi trường (staging, production), và thiết lập Protection Rule như required reviewers (cần người phê duyệt thủ công trước khi job chạy), wait timer (delay trước khi chạy), hoặc giới hạn branch được phép deploy — tăng kiểm soát an toàn cho các job deploy nhạy cảm.

**21. Giải thích cơ chế OIDC (OpenID Connect) để xác thực với cloud provider (AWS/Azure/GCP) không cần static key.**
GitHub Actions có thể phát hành một OIDC token ngắn hạn duy nhất cho mỗi lần chạy job, cloud provider (ví dụ AWS IAM) cấu hình trust relationship tin cậy token này (dựa trên claim như `repository`, `ref`), cho phép workflow `AssumeRoleWithWebIdentity` để lấy credentials tạm thời — loại bỏ hoàn toàn nhu cầu lưu Access Key/Secret Key tĩnh trong GitHub Secrets, giảm rủi ro rò rỉ credentials dài hạn.

**22. Permissions trong GitHub Actions (`GITHUB_TOKEN` permissions) hoạt động ra sao?**
Mỗi workflow run tự động có một `GITHUB_TOKEN` tạm thời với scope permissions có thể khai báo chi tiết (`contents: read`, `pull-requests: write`...) ở cấp workflow hoặc job, theo nguyên tắc least privilege — mặc định tổ chức có thể cấu hình permission mặc định là read-only toàn bộ để tăng bảo mật, và workflow cần khai báo rõ nếu muốn ghi (ví dụ tạo comment, publish package).

**23. Giải thích sự khác biệt giữa `pull_request` và `pull_request_target` về vấn đề bảo mật.**
`pull_request` chạy với code và permissions hạn chế của branch nguồn (kể cả từ fork không tin cậy), không có quyền truy cập secret nhạy cảm của repo đích — an toàn hơn cho PR từ bên ngoài. `pull_request_target` chạy với permissions và secret của repo đích (base) nhưng theo mặc định checkout code từ base ref (không phải PR code) — nếu vô tình checkout trực tiếp code từ PR (head ref) mà không kiểm soát, có thể dẫn tới thực thi mã độc từ fork với quyền truy cập secret, đây là lỗ hổng bảo mật phổ biến cần đặc biệt cẩn trọng.

**24. Cách xây dựng một CI/CD pipeline hoàn chỉnh: build → test → security scan → deploy trong GitHub Actions.**
Thiết kế nhiều job nối tiếp qua `needs`: job `build` compile/build artifact và upload artifact; job `test` chạy unit test/integration test (có thể matrix theo môi trường); job `security-scan` chạy SAST/dependency scan (ví dụ CodeQL, Trivy, Snyk) và fail pipeline nếu phát hiện lỗ hổng nghiêm trọng; job `deploy` chỉ chạy khi các job trên pass và thường giới hạn theo branch/environment protection, dùng OIDC để xác thực với cloud provider và thực hiện deploy (ví dụ apply Terraform hoặc update ECS/K8s).

**25. Giải thích cách dùng `if` condition kết hợp với `github.event_name` để chạy job có điều kiện.**
Có thể dùng `if: github.event_name == 'push' && github.ref == 'refs/heads/main'` để chỉ chạy job deploy khi push trực tiếp lên main (không chạy khi là PR), hoặc `if: github.event_name == 'pull_request'` để chỉ chạy lint/test khi là PR — giúp một workflow file phục vụ nhiều mục đích tuỳ theo loại sự kiện trigger mà không cần tách nhiều file riêng biệt.

**26. Composite Action được viết như thế nào (cấu trúc `action.yml`)?**
Composite Action cần file `action.yml` khai báo `name`, `description`, `inputs` (tham số đầu vào), và `runs: using: composite` cùng danh sách `steps` bên trong (mỗi step có thể `run` shell command hoặc `uses` action khác) — được gọi từ workflow khác qua `uses: ./đường-dẫn` (local) hoặc `uses: org/repo@ref` (từ repo khác), giúp đóng gói và tái sử dụng logic nhiều bước dưới dạng một action duy nhất.

**27. Giải thích cách quản lý version của Action bên thứ ba (`uses: actions/checkout@v4` vs `@main` vs commit SHA).**
Pin theo tag version cụ thể (`@v4`) cân bằng giữa ổn định và nhận update; pin theo `@main`/branch có rủi ro cao vì code có thể thay đổi bất cứ lúc nào (kể cả bị compromise) mà không có cảnh báo; pin theo commit SHA đầy đủ (`@a1b2c3d...`) là an toàn nhất vì immutable tuyệt đối, được khuyến nghị bởi các hướng dẫn bảo mật supply-chain (như GitHub's own security hardening guide) cho action từ bên thứ ba không kiểm soát.

**28. Self-hosted Runner scaling tự động (ví dụ dùng Actions Runner Controller trên Kubernetes) hoạt động ra sao?**
Actions Runner Controller (ARC) là operator chạy trên Kubernetes, tự động tạo/xoá Pod runner theo nhu cầu job đang chờ trong queue (dựa trên webhook hoặc polling GitHub API), cho phép scale-to-zero khi không có job và scale-out nhanh khi có nhiều job đồng thời — giải quyết vấn đề chi phí runner luôn chạy (idle) và giới hạn concurrency của GitHub-hosted runner miễn phí.

**29. Giải thích cơ chế `workflow_call` để tạo Reusable Workflow.**
Workflow được thiết kế để tái sử dụng cần trigger `on: workflow_call`, khai báo `inputs` và `secrets` mà workflow gọi nó phải truyền vào, và `outputs` để trả kết quả ngược lại. Workflow gọi nó dùng cú pháp job `uses: ./.github/workflows/reusable.yml@main` kèm `with:` cho input và `secrets: inherit` (hoặc khai báo secret cụ thể) — giúp chuẩn hoá pipeline dùng chung giữa nhiều repository trong tổ chức.

**30. Giải thích chiến lược Monorepo CI trong GitHub Actions (chỉ chạy job cho phần code thay đổi).**
Dùng action như `dorny/paths-filter` hoặc so sánh `git diff` giữa commit để phát hiện thư mục nào thay đổi, kết hợp `if` condition hoặc output từ job trước để quyết định job nào cần chạy (ví dụ chỉ build/test service A nếu có thay đổi trong thư mục `services/a`) — giúp giảm đáng kể thời gian CI cho monorepo lớn thay vì luôn chạy toàn bộ pipeline cho mọi service.

**31. Giải thích cách publish Docker image lên registry (ví dụ ECR/GHCR) trong GitHub Actions an toàn.**
Dùng `docker/login-action` xác thực registry (ưu tiên OIDC cho AWS ECR thay vì static credentials), `docker/build-push-action` để build multi-stage và push image với tag phù hợp (ví dụ theo git SHA + latest), kết hợp cache layer (`cache-from`/`cache-to` với GitHub Actions cache backend) để tăng tốc build, và nên quét lỗ hổng image (Trivy/Grype) trước khi push lên registry production.

**32. Giải thích Deployment Gate (Required Reviewers) trong Environment Protection Rule hoạt động thế nào trong pipeline thực tế?**
Khi job tham chiếu tới một `environment` có cấu hình required reviewers, workflow run sẽ tạm dừng (pending) tại job đó và gửi thông báo tới người/nhóm được chỉ định để phê duyệt thủ công trên GitHub UI — chỉ khi được approve, job mới tiếp tục chạy, tạo một "manual gate" an toàn giữa các bước tự động (ví dụ giữa deploy staging và deploy production).

**33. Giải thích cách kiểm soát chi phí (cost control) cho GitHub Actions ở tổ chức lớn.**
Áp dụng: giới hạn `timeout-minutes` cho mọi job để tránh job treo tốn phút chạy vô ích, dùng `concurrency` với `cancel-in-progress` để huỷ run cũ không cần thiết, tối ưu caching để giảm thời gian build, cân nhắc dùng self-hosted runner cho workload nặng/liên tục (rẻ hơn so với phút GitHub-hosted ở quy mô lớn), và giám sát usage qua GitHub Billing/Insights để phát hiện workflow tốn kém bất thường.

**34. Sự khác biệt giữa `actions/upload-artifact@v3` và `actions/upload-artifact@v4` về hành vi quan trọng cần lưu ý?**
Từ v4, artifact được lưu ngay lập tức (immutable) sau khi upload thay vì gộp chung ở cuối, và mỗi tên artifact phải unique trong cùng workflow run (không thể upload nhiều lần cùng tên để "gộp" như hành vi cũ ở v3) — nếu cần gộp nhiều lần từ các job matrix khác nhau vào cùng một artifact tên, cần dùng thêm action merge riêng hoặc đặt tên khác nhau theo từng job rồi tải xuống gộp ở bước sau.

**35. Giải thích khái niệm "Supply Chain Security" áp dụng vào GitHub Actions như thế nào?**
Bao gồm: pin action theo commit SHA thay vì tag/branch, hạn chế permissions của `GITHUB_TOKEN` ở mức tối thiểu cần thiết, review kỹ action bên thứ ba trước khi dùng (đặc biệt action ít sao/không rõ nguồn gốc), dùng Dependabot để tự động cập nhật version action, bật branch protection yêu cầu review trước khi merge thay đổi vào workflow file, và cân nhắc dùng allowlist action được phép sử dụng ở cấp Organization Settings.

### Nhóm Nâng cao (36–50)

**36. Thiết kế pipeline GitOps hoàn chỉnh: GitHub Actions build image → cập nhật manifest → ArgoCD tự động sync, luồng hoạt động cụ thể ra sao?**
GitHub Actions: build và push Docker image với tag theo git SHA lên registry; sau đó checkout repo riêng chứa manifest Kubernetes (hoặc thư mục config trong cùng repo), dùng công cụ như `yq`/`kustomize edit set image` để cập nhật tag image mới trong file manifest, commit và push thay đổi đó vào Git (đây là "single source of truth"). ArgoCD liên tục theo dõi Git repo đó (poll hoặc webhook), phát hiện thay đổi manifest và tự động (hoặc theo policy) sync trạng thái cluster khớp với Git — GitHub Actions không trực tiếp `kubectl apply`, việc deploy hoàn toàn do ArgoCD đảm nhiệm dựa trên Git state.

**37. Phân tích rủi ro bảo mật khi dùng `pull_request_target` kết hợp checkout code từ fork, và cách khắc phục an toàn.**
Rủi ro: nếu workflow dùng `pull_request_target` (có quyền truy cập secret của repo đích) nhưng lại checkout trực tiếp `github.event.pull_request.head.sha` (code từ fork không tin cậy) rồi chạy build/test trên code đó, kẻ tấn công có thể chèn mã độc trong PR để đánh cắp secret hoặc thực thi lệnh tuỳ ý với quyền của repo đích. Khắc phục: tách rõ 2 workflow — một chạy `pull_request` (không có secret) để build/test code chưa tin cậy, output kết quả dưới dạng artifact; một chạy `workflow_run` sau khi workflow đầu hoàn thành (có secret nhưng không checkout code fork trực tiếp, chỉ đọc artifact/kết quả đã kiểm tra) để thực hiện hành động cần secret như comment PR hay publish.

**38. Thiết kế chiến lược multi-environment deployment (dev/staging/prod) với approval gate và rollback tự động trong GitHub Actions.**
Dùng một reusable workflow deploy chung, gọi lần lượt cho từng environment (`dev` tự động sau merge, `staging` sau khi dev pass smoke test, `prod` yêu cầu required reviewers qua Environment Protection). Mỗi lần deploy ghi lại version đang chạy trước đó (ví dụ qua tag Git hoặc lưu vào artifact/parameter store); job deploy prod bao gồm bước post-deploy health check tự động, nếu fail thì trigger job rollback (redeploy lại version trước đó đã lưu) mà không cần chờ can thiệp thủ công, đồng thời gửi cảnh báo tới kênh giám sát.

**39. Giải thích cách tối ưu thời gian chạy CI cho một dự án build lâu (ví dụ build monolith 20 phút) mà không tách được monorepo.**
Các chiến lược: song song hoá test theo shard (dùng matrix chia test suite thành N phần chạy song song trên nhiều runner), tối ưu Docker layer caching và dependency caching (`actions/cache` với key chính xác theo lock file), dùng self-hosted runner có tài nguyên mạnh hơn/warm cache sẵn thay vì runner "sạch" mỗi lần, chỉ chạy full test suite trên main/PR quan trọng còn PR draft/nhánh phụ chạy subset test nhanh, và cân nhắc build tool hỗ trợ incremental build (ví dụ Bazel, Turborepo) để chỉ rebuild phần thay đổi.

**40. Giải thích cách thiết kế cơ chế "Deployment Freeze" (chặn deploy vào khung giờ nhất định, ví dụ cuối tuần/lễ) trong GitHub Actions.**
Có thể thêm một step/job kiểm tra điều kiện thời gian hiện tại (dùng `github.event.repository.updated_at` không phù hợp; thường dùng action tuỳ chỉnh hoặc script kiểm tra ngày giờ hệ thống server, hoặc gọi API bên ngoài lưu trạng thái "freeze window") ở đầu job deploy, nếu đang trong khung giờ freeze thì `exit 1` hoặc skip job kèm thông báo rõ ràng — kết hợp thêm cơ chế override thủ công (ví dụ input `force_deploy` trong `workflow_dispatch`) cho trường hợp khẩn cấp (hotfix) cần vượt qua freeze.

**41. Phân tích trade-off giữa việc để logic deploy hoàn toàn trong GitHub Actions (push-based) so với để ArgoCD đảm nhiệm (pull-based GitOps).**
Push-based (GitHub Actions tự `kubectl apply`/`helm upgrade`) đơn giản hơn để bắt đầu, nhưng yêu cầu cấp credentials truy cập cluster production cho CI (tăng bề mặt tấn công), khó biết chính xác trạng thái cluster có đúng với Git hay không nếu có thay đổi thủ công can thiệp. Pull-based (ArgoCD) không cần cấp credentials cluster cho CI (ArgoCD tự pull từ trong cluster), tự động phát hiện và cảnh báo/khắc phục drift so với Git, nhưng thêm một hệ thống cần vận hành riêng và độ trễ giữa commit và deploy phụ thuộc chu kỳ poll/sync của ArgoCD (có thể giảm bằng webhook).

**42. Giải thích chiến lược đảm bảo tính tái lập (reproducibility) của build trong GitHub Actions qua thời gian.**
Cần: pin version chính xác cho mọi action bên thứ ba (commit SHA), pin version base image Docker (không dùng tag `latest`), lock file dependency (package-lock.json, poetry.lock...) commit vào repo và cài đặt đúng theo lock file (không tự update), tránh phụ thuộc vào network call không kiểm soát trong lúc build (như tải resource từ URL không versioned), và ghi lại đầy đủ metadata build (git SHA, version tool, timestamp) vào artifact để có thể trace lại chính xác build nào tương ứng commit nào.

**43. Thiết kế cơ chế Secret Rotation tự động phối hợp giữa GitHub Actions và một Secret Manager bên ngoài (ví dụ AWS Secrets Manager).**
Không lưu secret tĩnh lâu dài trong GitHub Secrets cho các hệ thống hỗ trợ credentials động; thay vào đó, workflow dùng OIDC để assume role tạm thời, sau đó gọi API Secrets Manager để lấy secret mới nhất tại thời điểm chạy (đảm bảo luôn dùng bản mới nhất dù secret đã được rotate ở phía Secrets Manager). Đối với secret bắt buộc phải lưu trong GitHub (ví dụ token cho service không hỗ trợ OIDC), nên thiết lập một workflow scheduled riêng để tự động gọi API rotate secret định kỳ và cập nhật lại giá trị trong GitHub Secrets qua GitHub API.

**44. Phân tích cách xử lý khi Self-hosted Runner bị compromise (bị chèn mã độc chạy trong job) ảnh hưởng thế nào và cách phòng ngừa.**
Self-hosted runner (đặc biệt loại không ephemeral, dùng lại nhiều lần) nếu chạy job từ PR không tin cậy (fork) có thể bị lợi dụng để thực thi mã độc, đánh cắp credentials còn lưu lại trên máy runner (persistent state giữa các lần chạy), hoặc dùng làm bàn đạp tấn công mạng nội bộ nếu runner đặt trong VPC riêng. Phòng ngừa: không bao giờ cho phép self-hosted runner chạy trực tiếp từ `pull_request` của fork công khai, dùng runner ephemeral (huỷ và tạo mới hoàn toàn sau mỗi job), cô lập network runner (không có quyền truy cập tài nguyên nội bộ nhạy cảm không cần thiết), và giám sát log hoạt động runner chặt chẽ.

**45. Giải thích cách xây dựng hệ thống Progressive Delivery (canary/blue-green) tích hợp GitHub Actions với công cụ như Argo Rollouts.**
GitHub Actions chỉ chịu trách nhiệm build/push image và cập nhật tag image trong manifest Rollout (CRD của Argo Rollouts) trong Git; toàn bộ logic canary (tăng dần % traffic, phân tích metric tự động qua AnalysisTemplate, tự động rollback nếu metric xấu) do Argo Rollouts controller trong cluster đảm nhiệm hoàn toàn — đây là mô hình tách biệt rõ ràng giữa CI (build & commit thay đổi mong muốn) và CD (thực thi thay đổi một cách an toàn, có kiểm soát), giúp GitHub Actions không cần biết chi tiết trạng thái rollout đang diễn ra.

**46. Giải thích cách audit và enforce compliance cho toàn bộ workflow trong một Organization lớn có hàng trăm repository.**
Dùng GitHub Organization-level policy: giới hạn action được phép dùng (allowlist theo publisher/tên action), bắt buộc required workflow (Organization required workflows áp dụng cho mọi repo phù hợp điều kiện), giám sát qua GitHub Audit Log API để phát hiện thay đổi bất thường (workflow file bị sửa để thêm action lạ), định kỳ quét toàn bộ `.github/workflows` của các repo bằng script/tool riêng để phát hiện pattern không an toàn (như dùng `pull_request_target` sai cách, action pin theo branch thay vì SHA).

**47. Thiết kế chiến lược giảm thiểu thời gian chờ (queue time) khi có hàng trăm job cùng chờ self-hosted runner.**
Kết hợp: dùng Actions Runner Controller trên Kubernetes để autoscale runner Pod theo số job đang chờ (thay vì số runner cố định), phân loại job theo label runner phù hợp với tài nguyên cần thiết (tránh mọi job đều tranh nhau runner "to" trong khi job nhỏ có thể chạy runner nhỏ), thiết lập priority/concurrency group hợp lý để job quan trọng (như hotfix production) không bị chặn bởi hàng loạt job thấp ưu tiên hơn, và giám sát metric queue time để điều chỉnh capacity kịp thời.

**48. Một pipeline production dùng GitHub Actions deploy lên AWS đột nhiên fail với lỗi "AccessDenied" dù không có thay đổi gì trong code hay workflow — bạn debug theo hướng nào?**
Trình tự debug: (1) Kiểm tra xem IAM Role dùng qua OIDC có bị thay đổi trust policy hoặc permission policy gần đây không (thay đổi phía AWS, không phải phía code); (2) Kiểm tra token OIDC có hết hạn cấu hình trust (ví dụ điều kiện theo branch/tag đã đổi mà workflow chạy từ context khác — như từ tag thay vì branch); (3) Kiểm tra AWS CloudTrail xem request bị AccessDenied cụ thể ở action nào, resource nào để xác định thiếu permission gì; (4) Kiểm tra SCP ở cấp Organization có mới áp dụng guardrail chặn action đó; (5) Kiểm tra region/account đang target có đúng như mong đợi (đôi khi do biến môi trường default region thay đổi).

**49. Phân tích cách thiết kế "Preview Environment" tự động cho mỗi Pull Request (spin up môi trường riêng theo PR, tự huỷ khi PR đóng).**
Trigger `on: pull_request` (opened/synchronize) để build image từ code PR, deploy vào namespace/subdomain riêng biệt đặt tên theo số PR (ví dụ `pr-123.staging.example.com`) — thường dùng Helm với release name theo PR number hoặc ArgoCD ApplicationSet pattern tự tạo Application mới cho mỗi PR. Trigger riêng `on: pull_request: types: [closed]` để tự động xoá namespace/Application/DNS record tương ứng, tránh tích luỹ môi trường rác theo thời gian và phát sinh chi phí không cần thiết.

**50. Thiết kế toàn bộ chiến lược bảo mật (security hardening checklist) cho GitHub Actions ở cấp độ Enterprise.**
Checklist gồm: (1) Pin mọi action bên thứ ba theo commit SHA; (2) Giới hạn `GITHUB_TOKEN` permissions mặc định về read-only ở cấp Organization; (3) Dùng OIDC cho mọi kết nối cloud provider, loại bỏ hoàn toàn static credentials; (4) Bắt buộc required reviewers cho environment production; (5) Không bao giờ chạy `pull_request_target` với checkout code fork không kiểm soát; (6) Allowlist action được phép dùng theo publisher tin cậy; (7) Self-hosted runner luôn ephemeral và cô lập network; (8) Bật branch protection yêu cầu review cho thay đổi vào thư mục `.github/workflows`; (9) Giám sát Audit Log định kỳ; (10) Quét dependency/secret leak tự động (Dependabot, secret scanning) trên mọi repo.

---

## PHẦN 4: ArgoCD (50 câu)

### Nhóm Cơ bản (1–17)

**1. ArgoCD là gì và giải quyết vấn đề gì?**
ArgoCD là công cụ Continuous Delivery theo mô hình GitOps cho Kubernetes: liên tục theo dõi một Git repository chứa manifest/Helm chart, so sánh trạng thái mong muốn (trong Git) với trạng thái thực tế của cluster, và tự động (hoặc theo yêu cầu) đồng bộ để đưa cluster khớp với Git — giải quyết vấn đề thiếu nhất quán, khó audit, và thiếu "single source of truth" khi deploy thủ công qua `kubectl apply`/CI script trực tiếp vào cluster.

**2. GitOps là gì và khác gì so với CI/CD truyền thống?**
GitOps là mô hình vận hành trong đó Git là nguồn chân lý duy nhất (single source of truth) cho cả cấu hình hạ tầng lẫn ứng dụng, mọi thay đổi phải qua commit/PR vào Git, và một agent (như ArgoCD) trong cluster tự "pull" thay đổi đó để áp dụng — khác với CI/CD truyền thống thường theo mô hình "push" (CI server có credentials để chủ động đẩy thay đổi vào môi trường đích), GitOps giảm bề mặt tấn công vì không cần cấp quyền truy cập cluster cho hệ thống CI bên ngoài.

**3. Application là gì trong ArgoCD?**
Application là Custom Resource (CRD) cốt lõi của ArgoCD, định nghĩa: nguồn (source — Git repo, path, Helm chart, hoặc registry), đích (destination — cluster và namespace nào), và sync policy (tự động hay thủ công) — mỗi Application tương ứng với một đơn vị ứng dụng/service được ArgoCD quản lý và theo dõi trạng thái đồng bộ.

**4. Sync trong ArgoCD nghĩa là gì?**
Sync là hành động ArgoCD áp dụng (thường qua `kubectl apply` nội bộ) manifest từ Git vào cluster để đưa trạng thái thực tế khớp với trạng thái mong muốn khai báo trong Git — có thể chạy tự động (Auto-Sync) mỗi khi phát hiện thay đổi trong Git, hoặc thủ công (Manual Sync) khi người dùng bấm nút "Sync" trên UI/CLI.

**5. Phân biệt Auto-Sync và Manual Sync.**
Auto-Sync tự động áp dụng thay đổi từ Git vào cluster ngay khi ArgoCD phát hiện chênh lệch (không cần can thiệp người dùng), phù hợp môi trường dev/staging muốn deploy nhanh. Manual Sync yêu cầu người dùng chủ động bấm/gọi lệnh sync, phù hợp môi trường production cần kiểm soát chặt thời điểm deploy hoặc cần review trước khi áp dụng thay đổi.

**6. Health Status trong ArgoCD thể hiện điều gì?**
Health Status cho biết tình trạng "sức khoẻ" thực tế của resource sau khi deploy (không chỉ là đã sync hay chưa) — ví dụ Deployment được coi Healthy khi đủ số replica đang chạy sẵn sàng (matching desired replicas), Progressing khi đang rollout, Degraded khi có lỗi rõ ràng (như Pod CrashLoopBackOff). ArgoCD có health check logic riêng cho từng loại resource K8s phổ biến (Deployment, Service, Ingress, Job...).

**7. Sync Status khác Health Status như thế nào?**
Sync Status cho biết trạng thái cấu hình trong cluster có khớp (`Synced`) hay lệch (`OutOfSync`) so với Git — chỉ so sánh spec/config. Health Status cho biết resource đó có đang hoạt động đúng/khoẻ mạnh (`Healthy`/`Degraded`/`Progressing`) hay không — dựa trên trạng thái runtime thực tế. Một Application có thể `Synced` nhưng vẫn `Degraded` (config đúng nhưng Pod bị crash vì lý do khác như thiếu resource).

**8. ArgoCD hỗ trợ những loại nguồn manifest nào?**
ArgoCD hỗ trợ nhiều loại: raw Kubernetes YAML/JSON, Helm chart (local hoặc từ Helm repository), Kustomize, Jsonnet, và các plugin config management tool tuỳ chỉnh (Config Management Plugin) cho công cụ khác không hỗ trợ sẵn.

**9. App of Apps pattern là gì?**
App of Apps là mẫu thiết kế trong đó một Application "gốc" trỏ tới một Git repo/path chứa định nghĩa của nhiều Application con khác (mỗi Application con là một manifest ArgoCD Application resource) — cho phép quản lý và triển khai hàng loạt Application chỉ bằng cách sync một Application gốc duy nhất, thuận tiện khi cần bootstrap toàn bộ cluster hoặc quản lý nhiều microservice cùng lúc.

**10. Sync Wave dùng để làm gì?**
Sync Wave (annotation `argocd.argoproj.io/sync-wave`) cho phép định nghĩa thứ tự áp dụng resource trong một Application — resource có wave số nhỏ hơn được apply trước, ArgoCD chờ resource ở wave trước đạt trạng thái Healthy rồi mới tiếp tục wave sau, hữu ích khi có phụ thuộc rõ ràng (ví dụ cần tạo Secret/ConfigMap trước khi Deployment dùng đến nó).

**11. Sync Hook trong ArgoCD là gì?**
Sync Hook là annotation (`argocd.argoproj.io/hook`) đánh dấu một resource (thường là Job) chạy vào thời điểm đặc biệt trong quá trình sync: `PreSync` (trước khi sync resource chính, ví dụ chạy migration), `Sync` (cùng lúc), `PostSync` (sau khi sync xong và healthy, ví dụ chạy smoke test), hoặc `SyncFail` (khi sync thất bại, ví dụ gửi thông báo).

**12. ArgoCD UI/CLI cung cấp những gì để theo dõi Application?**
UI hiển thị trực quan cây resource (dependency tree) của Application, trạng thái Sync/Health từng resource, log/event, và diff giữa Git và cluster hiện tại. CLI (`argocd`) cung cấp lệnh tương đương để tự động hoá qua script (`argocd app sync`, `argocd app get`, `argocd app diff`) — hữu ích khi tích hợp vào pipeline hoặc thao tác hàng loạt.

**13. RBAC trong ArgoCD hoạt động ra sao?**
ArgoCD có hệ thống RBAC riêng (khác K8s RBAC) định nghĩa trong ConfigMap `argocd-rbac-cm`, dùng policy dạng `p, role, resource, action, object, effect` (ví dụ giới hạn team A chỉ được sync Application trong project của họ), kết hợp với SSO (Dex/OIDC) để map user/group từ Identity Provider vào các role đã định nghĩa.

**14. Project trong ArgoCD dùng để làm gì?**
AppProject là cơ chế nhóm và giới hạn phạm vi cho các Application: giới hạn Git repo nào được phép dùng làm nguồn, cluster/namespace nào được phép deploy tới, loại resource nào được phép tạo (whitelist/blacklist) — giúp áp dụng multi-tenancy an toàn khi nhiều team dùng chung một ArgoCD instance.

**15. Self-Heal trong ArgoCD nghĩa là gì?**
Self-Heal là tuỳ chọn (đi kèm Auto-Sync) khiến ArgoCD tự động sync lại ngay khi phát hiện có thay đổi thủ công (drift) trong cluster không khớp với Git — ví dụ ai đó chạy `kubectl edit` sửa trực tiếp resource, ArgoCD sẽ tự động ghi đè về đúng trạng thái khai báo trong Git, đảm bảo Git luôn là nguồn chân lý thực sự.

**16. Prune trong sync policy dùng để làm gì?**
Prune (tuỳ chọn đi kèm Auto-Sync) cho phép ArgoCD tự động xoá các resource đã tồn tại trong cluster nhưng không còn được khai báo trong Git nữa (ví dụ đã xoá manifest khỏi repo) — nếu không bật Prune, resource "mồ côi" đó vẫn tồn tại trong cluster dù không còn trong Git.

**17. ArgoCD Notifications dùng để làm gì?**
ArgoCD Notifications là component gửi thông báo (Slack, email, webhook...) dựa trên trigger định nghĩa sẵn (ví dụ khi Application chuyển sang `Degraded`, hoặc sync thành công/thất bại) — giúp team biết ngay lập tức khi có sự cố deploy mà không cần liên tục theo dõi UI thủ công.

### Nhóm Trung cấp (18–35)

**18. ApplicationSet là gì và giải quyết vấn đề gì?**
ApplicationSet là controller/CRD mở rộng của ArgoCD cho phép tự động sinh ra nhiều Application từ một template duy nhất, dựa trên các Generator (List, Cluster, Git directory/file, Matrix, Pull Request...) — giải quyết vấn đề phải tạo thủ công hàng chục/hàng trăm Application giống nhau (ví dụ một Application cho mỗi cluster, hoặc một Application cho mỗi thư mục service trong monorepo).

**19. Giải thích Git Generator trong ApplicationSet.**
Git Generator quét một Git repository theo pattern (directory hoặc file, ví dụ mỗi thư mục con trong `apps/*`) và tự động tạo một Application tương ứng cho mỗi kết quả tìm thấy — khi thêm thư mục service mới vào repo, ApplicationSet tự động tạo Application mới mà không cần sửa cấu hình ApplicationSet thủ công.

**20. Giải thích Cluster Generator trong ApplicationSet.**
Cluster Generator tự động tạo một Application cho mỗi cluster đã đăng ký với ArgoCD (dựa trên Secret cluster credentials có label phù hợp) — hữu ích khi cần deploy cùng một ứng dụng (ví dụ monitoring agent) tới tất cả cluster trong một fleet mà không cần định nghĩa Application riêng cho từng cluster.

**21. Giải thích cách ArgoCD quản lý multi-cluster deployment.**
ArgoCD có thể đăng ký (register) nhiều cluster đích ngoài cluster nó đang chạy (in-cluster), lưu credentials truy cập mỗi cluster dưới dạng Secret, và mỗi Application chỉ định rõ `destination.server` (URL cluster đích) — cho phép một ArgoCD instance trung tâm quản lý deploy tới nhiều cluster khác nhau (dev, staging, prod, hoặc theo khu vực địa lý) từ một nơi duy nhất.

**22. Giải thích cơ chế Diff và cách ArgoCD phát hiện Drift.**
ArgoCD liên tục thực hiện "dry-run diff" giữa manifest render ra từ Git (sau khi qua Helm/Kustomize nếu có) và trạng thái live hiện tại lấy từ Kubernetes API — bất kỳ khác biệt nào ở field được quản lý (không tính các field do hệ thống tự sinh như `status`, một số field mutating webhook thêm vào) sẽ khiến Application chuyển trạng thái `OutOfSync`, hiển thị chi tiết diff trên UI/CLI để review trước khi quyết định sync.

**23. Giải thích cách ArgoCD tích hợp với Helm (Helm value override, Helm hooks).**
ArgoCD có thể trỏ trực tiếp tới một Helm chart (local path hoặc Helm repo), cho phép override `values.yaml` qua field `helm.parameters` hoặc `valueFiles` trong Application spec (ví dụ dùng file values riêng cho từng môi trường) — tuy nhiên ArgoCD tự render Helm template thành manifest tĩnh (không dùng Tiller/Helm release nội bộ), và Helm hooks (`pre-install`, `post-install`...) được ArgoCD map tương ứng sang Sync Hook của riêng nó.

**24. Giải thích cơ chế Rollback trong ArgoCD.**
ArgoCD lưu lịch sử các lần sync thành công (kèm Git revision tương ứng) cho mỗi Application, cho phép rollback bằng cách chọn một revision cũ hơn trong lịch sử và sync lại về đúng trạng thái đó (`argocd app rollback`) — về bản chất đây vẫn là một lần sync bình thường nhưng target về Git SHA cũ, nên với mô hình GitOps thuần, cách khuyến nghị hơn thường là `git revert` commit trên Git rồi để ArgoCD tự sync lại, giữ Git luôn phản ánh đúng lịch sử thực.

**25. Giải thích Config Management Plugin (CMP) trong ArgoCD.**
CMP cho phép ArgoCD hỗ trợ công cụ render manifest tuỳ chỉnh ngoài Helm/Kustomize built-in sẵn (ví dụ dùng `cdk8s`, `jsonnet` phiên bản tuỳ biến, hoặc script nội bộ của công ty), bằng cách định nghĩa một plugin container/sidecar chạy lệnh generate manifest theo giao thức mà ArgoCD Repo Server hiểu — mở rộng khả năng tương thích của ArgoCD với hệ sinh thái công cụ đa dạng.

**26. Giải thích cách bảo vệ Secret trong repo Git khi dùng GitOps với ArgoCD (Sealed Secrets/SOPS/Vault).**
Vì GitOps yêu cầu mọi thứ nằm trong Git nhưng Secret không nên lưu plaintext, các giải pháp phổ biến: Sealed Secrets (Bitnami) mã hoá Secret thành SealedSecret resource chỉ giải mã được bởi controller trong cluster đích cụ thể; SOPS mã hoá từng field trong file YAML dùng KMS/PGP, giải mã lúc render qua plugin (ví dụ kustomize + sops); hoặc External Secrets Operator/Vault chỉ lưu reference (không lưu giá trị thật) trong Git, giá trị thật lấy động từ Vault tại runtime trong cluster.

**27. Giải thích Resource Hook Deletion Policy (`HookSucceeded`, `HookFailed`, `BeforeHookCreation`).**
Đây là annotation bổ sung cho Sync Hook, kiểm soát khi nào ArgoCD xoá resource hook (thường là Job) sau khi hoàn thành nhiệm vụ: `HookSucceeded` xoá ngay khi hook chạy thành công, `HookFailed` xoá khi hook thất bại, `BeforeHookCreation` xoá hook cũ (nếu tồn tại từ lần sync trước) trước khi tạo hook mới cho lần sync hiện tại — tránh tích luỹ rác các Job cũ qua nhiều lần sync.

**28. Giải thích Ignore Difference trong ArgoCD dùng khi nào?**
`ignoreDifferences` cấu hình trong Application spec để bảo ArgoCD bỏ qua một số field cụ thể khi tính toán diff/Sync Status — hữu ích khi có field bị thay đổi tự động bởi thành phần khác trong cluster (ví dụ HPA tự sửa `spec.replicas`, hoặc mutating webhook tự thêm field), tránh Application luôn hiển thị `OutOfSync` giả do những thay đổi hợp lệ đó.

**29. So sánh ArgoCD và Flux (hai công cụ GitOps phổ biến).**
Cả hai đều theo mô hình pull-based GitOps. ArgoCD có UI trực quan mạnh, khái niệm Application/Project rõ ràng, phù hợp team cần giao diện quản lý trực quan và multi-tenancy qua UI. Flux (đặc biệt Flux v2 với Source Controller/Kustomize Controller) thiên về "toolkit" nhỏ gọn, tích hợp sâu hơn với hệ sinh thái CNCF (như Flagger cho progressive delivery), thường được ưa chuộng khi muốn cấu hình hoàn toàn qua CRD/CLI không phụ thuộc UI.

**30. Giải thích cách ArgoCD xử lý khi Application có resource thuộc nhiều namespace khác nhau.**
Một Application có thể chứa manifest trải trên nhiều namespace (nếu manifest tự khai báo `metadata.namespace` cụ thể cho từng resource), ArgoCD vẫn theo dõi và đồng bộ tất cả resource đó như một đơn vị Application duy nhất — tuy nhiên `destination.namespace` trong Application spec chỉ là namespace mặc định áp dụng cho resource không tự khai báo namespace riêng.

**31. Giải thích cơ chế Webhook để ArgoCD phát hiện thay đổi Git nhanh hơn thay vì chờ polling.**
Mặc định ArgoCD poll Git repo theo chu kỳ (mặc định 3 phút) để phát hiện thay đổi mới, gây độ trễ nhất định. Có thể cấu hình Git webhook (GitHub/GitLab) trỏ tới endpoint của ArgoCD API Server để thông báo ngay lập tức khi có push mới, giúp ArgoCD refresh Application gần như tức thời thay vì chờ tới chu kỳ poll tiếp theo.

**32. Giải thích cách ArgoCD hỗ trợ Progressive Delivery qua tích hợp với Argo Rollouts.**
Argo Rollouts là công cụ riêng biệt thay thế Deployment bằng CRD `Rollout`, hỗ trợ canary/blue-green với phân tích metric tự động (AnalysisTemplate kết nối Prometheus/Datadog...) để quyết định tiếp tục tăng traffic hay tự động rollback. ArgoCD tích hợp với Argo Rollouts qua ArgoCD UI plugin (hiển thị trực quan tiến trình rollout) và cả hai chia sẻ triết lý GitOps — ArgoCD quản lý việc đồng bộ Rollout CRD từ Git, còn Argo Rollouts controller thực thi logic canary/blue-green thực tế trong cluster.

**33. Giải thích cách thiết kế cấu trúc Git repository phù hợp cho GitOps với ArgoCD (mono-repo vs multi-repo cho manifest).**
Mono-repo (một repo chứa manifest của toàn bộ ứng dụng/môi trường, tổ chức theo thư mục `apps/<service>/<env>`) dễ quản lý tập trung, dễ review thay đổi liên quan nhiều service cùng lúc, phù hợp team nhỏ/trung bình. Multi-repo (mỗi service/team có repo manifest riêng) cô lập quyền truy cập tốt hơn theo team, giảm rủi ro một team vô tình ảnh hưởng cấu hình team khác, phù hợp tổ chức lớn với nhiều team độc lập — ArgoCD hỗ trợ tốt cả hai mô hình, lựa chọn phụ thuộc vào quy mô tổ chức và yêu cầu phân quyền.

**34. Giải thích Application trong trạng thái "Unknown" nghĩa là gì và nguyên nhân phổ biến?**
Health Status "Unknown" nghĩa là ArgoCD không thể xác định được tình trạng sức khoẻ của resource đó — thường xảy ra với Custom Resource (CRD) không có health check logic được định nghĩa sẵn hoặc tuỳ chỉnh (ArgoCD chỉ biết health check built-in cho các resource K8s phổ biến). Giải quyết bằng cách định nghĩa Custom Health Check (viết bằng Lua script) trong ConfigMap `argocd-cm` cho loại CRD đó.

**35. Giải thích cách ArgoCD xử lý xung đột khi hai nguồn cùng cố gắng sửa một resource (ví dụ ArgoCD và một Operator khác).**
Nếu một Operator khác trong cluster tự động sửa đổi field của resource mà ArgoCD cũng quản lý, và Auto-Sync + Self-Heal đang bật, ArgoCD sẽ liên tục ghi đè thay đổi đó về đúng Git (gây ra vòng lặp xung đột nếu Operator kia cũng liên tục sửa lại) — cách xử lý là dùng `ignoreDifferences` cho field cụ thể mà Operator kia quản lý, để ArgoCD không coi đó là drift cần sync lại.

### Nhóm Nâng cao (36–50)

**36. Thiết kế kiến trúc ArgoCD cho một tổ chức có 50+ team, hàng trăm ứng dụng, và nhiều cluster (dev/staging/prod × nhiều region).**
Dùng mô hình ArgoCD tập trung (hub) quản lý nhiều cluster spoke qua đăng ký cluster credentials, kết hợp AppProject để cô lập phạm vi từng team (giới hạn repo/namespace/resource type được phép), dùng ApplicationSet với Cluster Generator + Git Generator kết hợp (Matrix Generator) để tự động sinh Application cho tổ hợp (team × môi trường × region) mà không cần tạo thủ công, tích hợp RBAC qua SSO/OIDC ánh xạ nhóm AD/LDAP vào ArgoCD Project role, và cân nhắc ArgoCD instance riêng theo từng vùng/tier bảo mật nếu yêu cầu cô lập cao hơn mức Project cho phép.

**37. Phân tích trade-off giữa một ArgoCD instance trung tâm quản lý mọi cluster so với mỗi cluster có ArgoCD instance riêng.**
Instance trung tâm (hub-spoke) dễ quan sát tổng thể toàn bộ hạ tầng từ một nơi, giảm chi phí vận hành nhiều instance, nhưng tạo single point of failure (nếu ArgoCD hub down, mất khả năng deploy tới mọi cluster dù cluster đích vẫn chạy bình thường) và cần quản lý credentials truy cập nhiều cluster tập trung (rủi ro bảo mật cao hơn nếu hub bị compromise). Instance riêng theo từng cluster tăng độ cô lập lỗi và bảo mật (mỗi ArgoCD chỉ có quyền trên cluster của chính nó, thường in-cluster không cần lưu external credentials), nhưng khó có cái nhìn tổng thể và tăng chi phí vận hành nhiều instance.

**38. Giải thích chiến lược xử lý Sync Wave khi có phụ thuộc phức tạp qua nhiều Application (không chỉ trong một Application).**
Sync Wave chỉ hoạt động trong phạm vi một Application; để xử lý phụ thuộc giữa nhiều Application khác nhau (ví dụ Application B cần Application A đã Healthy trước), thường dùng App of Apps pattern kết hợp Sync Wave ở cấp Application cha (annotation sync-wave trên chính manifest Application con), hoặc dùng ApplicationSet với `syncPolicy` kết hợp cơ chế chờ health check thủ công qua PreSync hook gọi API kiểm tra trạng thái Application khác trước khi tiếp tục.

**39. Thiết kế cơ chế Disaster Recovery cho chính ArgoCD (nếu ArgoCD control plane bị mất hoàn toàn).**
Vì ArgoCD hoạt động theo GitOps, may mắn là Git repo vẫn giữ toàn bộ trạng thái mong muốn của mọi Application — khôi phục bằng cách cài lại ArgoCD (qua Helm chart/manifest lưu sẵn), restore lại các Secret cluster credentials và AppProject/RBAC config (nên backup riêng vì đây là config nội bộ ArgoCD, không nằm trong Git chứa manifest ứng dụng), rồi trỏ lại Git repo — cluster ứng dụng đích vẫn tiếp tục chạy bình thường trong lúc ArgoCD down (chỉ mất khả năng deploy/tự sửa drift tạm thời), sau khi ArgoCD được khôi phục nó sẽ tự động reconcile lại đúng trạng thái.

**40. Phân tích cách xử lý khi một Application liên tục dao động giữa Synced và OutOfSync (flapping) dù không ai thay đổi gì thủ công.**
Nguyên nhân thường gặp: một field trong manifest bị một Admission Webhook hoặc Controller khác (ví dụ service mesh sidecar injector, HPA, hoặc default value tự động của K8s) liên tục thay đổi ngay sau khi ArgoCD sync xong, khiến vòng lặp diff/health check tiếp theo phát hiện lệch và (nếu Self-Heal bật) sync lại, rồi lại bị thay đổi tiếp — vòng lặp vô hạn. Khắc phục bằng cách xác định chính xác field nào gây flapping (qua `argocd app diff` lặp lại nhiều lần) và thêm vào `ignoreDifferences` cho field đó.

**41. Giải thích cách tích hợp ArgoCD với chính sách bảo mật OPA/Gatekeeper hoặc Kyverno để chặn Application vi phạm compliance trước khi sync.**
Có thể tích hợp theo 2 cách: (1) Admission Controller (Kyverno/Gatekeeper) chạy trong cluster đích tự động chặn resource vi phạm policy ngay tại API Server — khi đó ArgoCD sync sẽ fail và hiển thị lỗi rõ ràng trên UI; (2) kiểm tra "shift-left" hơn bằng cách chạy policy check (`conftest`, `kyverno cli`) ngay trong pipeline CI trước khi commit manifest vào Git — giảm khả năng Application bị reject ở giai đoạn sync, phát hiện lỗi sớm hơn trong vòng đời phát triển.

**42. Thiết kế quy trình Promotion (dev → staging → prod) an toàn dùng ArgoCD theo mô hình GitOps thuần (không có bước "click nút deploy" thủ công trong CI).**
Cấu trúc Git theo từng môi trường (thư mục hoặc branch riêng cho dev/staging/prod, hoặc dùng Kustomize overlay); pipeline CI chỉ cập nhật tag image trong thư mục môi trường tương ứng qua PR (không tự động merge thẳng vào prod). Promotion từ staging lên prod thực hiện bằng PR merge thủ công (hoặc tự động sau khi staging pass smoke test/soak time) copy giá trị tag đã verify từ thư mục staging sang thư mục prod — toàn bộ quyết định "deploy gì, khi nào" nằm ở việc merge PR vào Git, còn ArgoCD chỉ đơn thuần phản ánh trung thực trạng thái Git vào cluster, giữ đúng triết lý GitOps.

**43. Giải thích cách audit đầy đủ "ai đã deploy gì, khi nào" trong một hệ thống dùng ArgoCD.**
Vì mọi thay đổi đều qua Git, lịch sử Git commit (kèm PR review, tác giả, thời gian) tự nhiên trở thành audit trail đầy đủ cho câu hỏi "ai thay đổi gì, khi nào, được ai duyệt". Bổ sung thêm: ArgoCD lưu lịch sử sync (kèm Git revision) có thể truy vấn qua `argocd app history`, ArgoCD Notifications gửi log sự kiện sync ra hệ thống log tập trung, và bật audit log ở tầng Kubernetes API Server để ghi lại chính xác request nào (kể cả từ ArgoCD service account) đã thay đổi resource.

**44. Phân tích rủi ro bảo mật nếu ArgoCD Application được cấu hình sai để có thể deploy tới namespace `kube-system` hoặc chạy Cluster-scoped resource không mong muốn.**
Nếu AppProject không giới hạn rõ namespace/cluster-scoped resource được phép, một Application (đặc biệt từ team không đáng tin cậy hoàn toàn hoặc bị compromise qua Git) có thể vô tình hoặc cố ý deploy resource vào namespace hệ thống nhạy cảm (`kube-system`) hoặc tạo ClusterRole/ClusterRoleBinding cấp quyền quá rộng — ảnh hưởng tới toàn bộ cluster chứ không chỉ ứng dụng của họ. Phòng ngừa bằng cách cấu hình chặt `AppProject.spec.destinations` (whitelist namespace cụ thể) và `AppProject.spec.clusterResourceWhitelist` (mặc định nên để trống/rất hạn chế, chỉ mở khi thực sự cần).

**45. Giải thích cách xử lý Large-Scale Sync (hàng nghìn Application) mà không làm quá tải ArgoCD Repo Server/Application Controller.**
Cần: scale ngang Repo Server (nhiều replica xử lý song song việc render manifest từ Git — đây thường là bottleneck chính do tốn CPU/memory khi render Helm/Kustomize) và Application Controller (hỗ trợ sharding theo cluster để chia tải giữa nhiều Controller instance), tăng interval reconciliation hợp lý (không cần quá thường xuyên cho Application ít thay đổi), bật caching manifest render đã tính toán, và tách nhiều ArgoCD instance theo shard team/cluster nếu một instance duy nhất không còn scale nổi dù đã tối ưu các bước trên.

**46. Giải thích cách xử lý trường hợp cần rollback khẩn cấp (emergency rollback) khi Git revert bị chặn bởi CI pipeline đang gặp sự cố.**
Trong tình huống khẩn cấp cần rollback ngay mà không thể chờ CI pipeline (đang down hoặc queue dài), có thể dùng `argocd app rollback` trực tiếp để ArgoCD sync về Git revision cũ đã biết là ổn định (bỏ qua toàn bộ pipeline CI vì ArgoCD tự thao tác dựa trên lịch sử Git đã lưu) — đây là ngoại lệ chấp nhận được cho tình huống khẩn cấp, nhưng cần đảm bảo sau đó Git repo cũng được cập nhật/revert tương ứng (qua commit sau đó) để tránh commit tiếp theo vô tình ghi đè lại trạng thái lỗi đã rollback.

**47. Phân tích cách thiết kế Custom Health Check (Lua script) cho một Custom Resource không có health check mặc định.**
Định nghĩa trong ConfigMap `argocd-cm` dưới key `resource.customizations.health.<group_kind>`, viết một đoạn Lua script nhận `obj` (đại diện resource) làm input, đọc các field trong `obj.status` để quyết định trả về `hs.status` (Healthy/Progressing/Degraded/Suspended) kèm `hs.message` mô tả — ví dụ với một CRD Database tuỳ chỉnh, có thể kiểm tra `obj.status.phase == "Running"` để trả Healthy, giúp ArgoCD hiển thị đúng tình trạng thực tế thay vì luôn "Unknown".

**48. Một Application ở trạng thái "Synced" và "Healthy" nhưng người dùng thực tế vẫn gặp lỗi khi truy cập ứng dụng — bạn sẽ debug theo hướng nào?**
Điều này cho thấy vấn đề nằm ngoài phạm vi mà ArgoCD kiểm tra (ArgoCD chỉ đảm bảo config khớp Git và resource ở trạng thái K8s-level healthy, không đảm bảo ứng dụng logic bên trong hoạt động đúng). Cần kiểm tra thêm: log ứng dụng thực tế (lỗi runtime, kết nối downstream service/database), cấu hình network layer ngoài phạm vi Application đó (DNS, Ingress Controller, CDN, cert TLS hết hạn), hoặc vấn đề ở tầng dữ liệu (migration chưa chạy đúng dù Job Sync Hook báo thành công) — tức là mở rộng phạm vi debug ra ngoài ArgoCD, dùng công cụ observability (log/metric/trace) của chính ứng dụng.

**49. Giải thích cách thiết kế Application Set Progressive Rollout (deploy tuần tự qua nhiều cluster thay vì đồng loạt) để giảm rủi ro khi update ảnh hưởng toàn bộ fleet.**
ApplicationSet hỗ trợ `strategy: RollingSync` với các bước (steps) định nghĩa nhóm cluster theo thứ tự triển khai (dựa trên label matchExpressions, ví dụ nhóm "canary" trước, rồi "region-us", cuối cùng "region-eu"), kèm giới hạn số lượng/tỷ lệ Application được phép sync đồng thời trong mỗi bước — nếu nhóm trước gặp lỗi (Application không đạt Healthy), ApplicationSet sẽ dừng lại không tiếp tục sang nhóm sau, giảm đáng kể rủi ro một thay đổi lỗi ảnh hưởng đồng loạt toàn bộ fleet cluster.

**50. Thiết kế toàn diện chiến lược GitOps Security Hardening cho ArgoCD ở môi trường Enterprise.**
Bao gồm: (1) Bật RBAC chi tiết theo AppProject, tích hợp SSO/OIDC, không dùng chung admin account; (2) Giới hạn chặt `destinations` và `clusterResourceWhitelist` theo từng Project; (3) Không lưu Secret plaintext trong Git — dùng Sealed Secrets/SOPS/External Secrets Operator; (4) Bật TLS cho toàn bộ giao tiếp ArgoCD (API Server, Repo Server, Dex); (5) Giới hạn quyền của ArgoCD Service Account trong mỗi cluster đích theo least privilege (không cần cluster-admin toàn bộ nếu phạm vi quản lý rõ ràng); (6) Bật audit log và Notifications cảnh báo mọi thay đổi bất thường; (7) Định kỳ review Application/Project không còn dùng để dọn dẹp; (8) Áp dụng policy engine (Kyverno/OPA) kiểm tra compliance trước khi cho phép sync vào production.

---

## PHẦN 5: Terraform (50 câu)

### Nhóm Cơ bản (1–17)

**1. Terraform là gì và IaC (Infrastructure as Code) giải quyết vấn đề gì?**
Terraform là công cụ IaC của HashiCorp, cho phép định nghĩa và quản lý hạ tầng (cloud resource) dưới dạng file code (HCL) thay vì thao tác thủ công qua console/CLI. IaC giải quyết vấn đề: đảm bảo tính nhất quán và tái lập giữa các môi trường, cho phép version control và review thay đổi hạ tầng như code, tự động hoá provisioning giảm lỗi thủ công, và dễ dàng audit lịch sử thay đổi qua Git.

**2. Provider trong Terraform là gì?**
Provider là plugin cho phép Terraform giao tiếp với API của một nền tảng cụ thể (AWS, Azure, GCP, Kubernetes...), chịu trách nhiệm dịch cấu hình HCL thành lời gọi API thực tế để tạo/sửa/xoá resource. Mỗi provider cần được khai báo trong block `provider` và có thể cấu hình version cụ thể để đảm bảo tính ổn định.

**3. Resource và Data Source khác nhau như thế nào?**
`resource` định nghĩa một đối tượng hạ tầng mà Terraform sẽ tạo, quản lý, và có thể xoá (nằm trong vòng đời quản lý của Terraform). `data` (data source) chỉ đọc thông tin về một tài nguyên đã tồn tại sẵn (do Terraform khác hoặc thủ công tạo trước đó) để tham chiếu giá trị, không tạo/sửa/xoá gì cả — ví dụ dùng `data "aws_ami"` để lấy ID AMI mới nhất thay vì hardcode.

**4. State file trong Terraform là gì và tại sao quan trọng?**
State file (`terraform.tfstate`) là nơi Terraform lưu ánh xạ giữa resource khai báo trong code và resource thực tế đã tạo trên cloud provider (kèm metadata, dependency), giúp Terraform biết được resource nào đã tồn tại, cần tạo mới, sửa, hay xoá khi chạy `plan`/`apply` — mất hoặc hỏng state file có thể khiến Terraform "quên" resource đã quản lý, dẫn tới việc cố tạo trùng hoặc không thể quản lý resource đó nữa.

**5. Local Backend và Remote Backend khác nhau ra sao?**
Local Backend lưu state file ngay trên máy chạy Terraform (mặc định) — không phù hợp làm việc nhóm vì dễ xung đột, mất file, không có locking. Remote Backend (S3, Terraform Cloud, Azure Storage...) lưu state tập trung ở nơi chia sẻ được, hỗ trợ locking (tránh 2 người chạy apply cùng lúc gây corrupt state), và thường mã hoá dữ liệu tại nơi lưu trữ.

**6. `terraform plan` và `terraform apply` khác nhau như thế nào?**
`terraform plan` tính toán và hiển thị trước những thay đổi sẽ được thực hiện (tạo/sửa/xoá resource nào) mà không thực sự thay đổi gì trên hạ tầng thực — dùng để review trước khi áp dụng. `terraform apply` thực sự thực thi các thay đổi đó (thường hiển thị lại plan và yêu cầu xác nhận `yes` trước khi tiến hành, trừ khi dùng `-auto-approve`).

**7. Variable và Output trong Terraform dùng để làm gì?**
Variable (`variable` block) cho phép tham số hoá giá trị đầu vào (ví dụ instance type, region) để tái sử dụng code cho nhiều môi trường mà không cần sửa trực tiếp trong resource. Output (`output` block) hiển thị/trả về giá trị sau khi apply xong (ví dụ IP của instance vừa tạo) — có thể dùng để hiển thị cho người dùng hoặc truyền sang module/state khác.

**8. Module trong Terraform là gì?**
Module là một tập hợp file `.tf` được đóng gói lại thành một đơn vị tái sử dụng, có thể nhận input (variable) và trả output riêng — giúp tránh lặp lại code khi cần tạo cùng một pattern hạ tầng (ví dụ VPC chuẩn) nhiều lần ở nhiều nơi/môi trường khác nhau, chỉ cần gọi `module "vpc" { source = "..." }` với tham số khác nhau.

**9. `terraform init` dùng để làm gì?**
`terraform init` khởi tạo thư mục làm việc: tải về provider plugin cần thiết theo version khai báo, cấu hình backend lưu state (kết nối tới remote backend nếu có), và tải module (nếu tham chiếu module từ registry/Git) — là lệnh bắt buộc chạy đầu tiên trước khi có thể `plan`/`apply` trong một thư mục Terraform mới hoặc sau khi thay đổi cấu hình backend/provider.

**10. `terraform destroy` dùng để làm gì và rủi ro cần lưu ý?**
`terraform destroy` xoá toàn bộ resource đang được quản lý trong state hiện tại — cực kỳ nguy hiểm nếu chạy nhầm môi trường (đặc biệt production), nên luôn review kỹ output của `terraform plan -destroy` trước, và cân nhắc dùng `prevent_destroy` lifecycle rule cho resource quan trọng để chặn xoá nhầm.

**11. Terraform Workspace là gì?**
Workspace cho phép quản lý nhiều state file riêng biệt từ cùng một bộ code Terraform (ví dụ workspace `dev`, `staging`, `prod`), mỗi workspace có state độc lập — tuy nhiên đây không phải cách ly hoàn toàn về cấu hình (vẫn dùng chung code), nên với môi trường khác biệt lớn, nhiều team ưu tiên tách thư mục/repo riêng thay vì chỉ dùng Workspace.

**12. `count` và `for_each` trong Terraform dùng khi nào?**
`count` tạo nhiều instance của một resource dựa trên số nguyên (ví dụ tạo 3 EC2 giống nhau), truy cập qua index (`aws_instance.example[0]`) — nhược điểm là nếu xoá phần tử ở giữa danh sách, Terraform có thể tính toán lại index gây thay đổi ngoài ý muốn. `for_each` tạo resource dựa trên map hoặc set (mỗi resource có key riêng biệt cố định thay vì index số), tránh được vấn đề dịch chuyển index khi thêm/xoá phần tử — thường được khuyến nghị hơn `count` khi danh sách có thể thay đổi.

**13. Giải thích `.tfvars` file dùng để làm gì.**
File `.tfvars` (ví dụ `dev.tfvars`, `prod.tfvars`) chứa giá trị cụ thể để gán cho các `variable` đã khai báo, cho phép tách biệt giá trị cấu hình theo môi trường mà không cần sửa code chính — chạy qua `terraform apply -var-file="dev.tfvars"` để áp dụng giá trị tương ứng.

**14. Terraform State Locking dùng để làm gì?**
State Locking ngăn nhiều người/tiến trình chạy `apply` đồng thời lên cùng state file, tránh race condition gây corrupt state — thường triển khai qua DynamoDB table (khi dùng S3 backend) hoặc cơ chế lock tích hợp sẵn của Terraform Cloud, đảm bảo chỉ một tiến trình được thao tác ghi vào state tại một thời điểm.

**15. Provisioner trong Terraform là gì và khi nào nên dùng?**
Provisioner (`local-exec`, `remote-exec`) cho phép chạy script/command sau khi resource được tạo (ví dụ chạy script cài đặt trên EC2 vừa tạo) — tuy nhiên HashiCorp khuyến nghị hạn chế dùng Provisioner vì nó nằm ngoài mô hình declarative thuần của Terraform (khó dự đoán, không idempotent tự nhiên), nên ưu tiên dùng cơ chế native khác (user_data, cloud-init, hoặc công cụ configuration management chuyên dụng như Ansible) khi có thể.

**16. Sensitive variable trong Terraform dùng để làm gì?**
Khai báo `sensitive = true` cho variable/output khiến Terraform tự động ẩn giá trị đó khỏi output console (thay bằng `(sensitive value)`) khi chạy plan/apply, giảm rủi ro rò rỉ dữ liệu nhạy cảm (password, key) ra log CI/CD — tuy nhiên giá trị vẫn được lưu plaintext trong state file, nên cần bảo vệ thêm ở tầng lưu trữ state (mã hoá S3, giới hạn quyền truy cập).

**17. `terraform fmt` và `terraform validate` dùng để làm gì?**
`terraform fmt` tự động format lại code theo chuẩn style chính thức của HCL (căn chỉnh khoảng trắng, thụt lề), giúp code nhất quán giữa các thành viên. `terraform validate` kiểm tra cú pháp và tính hợp lệ cơ bản của cấu hình (không cần kết nối tới provider thực) — cả hai thường được chạy như bước kiểm tra tự động trong CI trước khi cho phép merge.

### Nhóm Trung cấp (18–35)

**18. Giải thích Dependency Graph trong Terraform và cách Terraform quyết định thứ tự tạo resource.**
Terraform tự động xây dựng một dependency graph dựa trên tham chiếu giữa các resource (ví dụ resource B tham chiếu output của resource A qua `resource_a.id` sẽ tạo cạnh phụ thuộc A→B), từ đó tính toán thứ tự tạo/sửa/xoá hợp lý và có thể xử lý song song những resource không phụ thuộc nhau để tăng tốc độ apply — có thể xem trực quan qua `terraform graph`.

**19. `depends_on` dùng khi nào nếu Terraform đã tự suy luận dependency?**
`depends_on` dùng khi có phụ thuộc ẩn không thể suy luận tự động qua tham chiếu attribute trực tiếp (ví dụ resource A cần IAM permission từ resource B được tạo xong trước, nhưng code không tham chiếu attribute nào của B) — khai báo thủ công để đảm bảo Terraform tạo đúng thứ tự, tránh race condition khi apply.

**20. Giải thích Lifecycle Meta-Argument (`create_before_destroy`, `prevent_destroy`, `ignore_changes`).**
`create_before_destroy = true` khiến Terraform tạo resource mới trước khi xoá resource cũ (thay vì mặc định xoá trước tạo sau), giảm downtime khi thay thế resource (ví dụ Launch Template mới). `prevent_destroy = true` chặn hoàn toàn việc Terraform xoá resource đó (kể cả vô tình qua destroy), bảo vệ resource quan trọng. `ignore_changes` bảo Terraform bỏ qua thay đổi ở một số field cụ thể khi tính diff (hữu ích khi field đó bị hệ thống bên ngoài tự động sửa, ví dụ auto-scaling thay đổi desired_capacity).

**21. Giải thích `terraform import` dùng để làm gì và hạn chế của nó.**
`terraform import` cho phép đưa một resource đã tồn tại sẵn trên cloud (tạo thủ công trước đó, không qua Terraform) vào state để Terraform bắt đầu quản lý — hạn chế: chỉ import state, không tự sinh ra code HCL tương ứng (phải tự viết code khớp chính xác cấu hình hiện tại, nếu không lần `plan` tiếp theo sẽ hiển thị thay đổi không mong muốn); từ Terraform 1.5+ có thêm block `import` khai báo trong code giúp quy trình này declarative và dễ review hơn.

**22. Giải thích `terraform state mv` và `terraform state rm` dùng khi nào.**
`terraform state mv` dùng để đổi tên/di chuyển resource trong state mà không xoá/tạo lại resource thực tế trên hạ tầng (ví dụ khi refactor code, đổi tên resource hoặc chuyển vào module) — tránh Terraform hiểu nhầm là cần destroy resource cũ và tạo resource mới. `terraform state rm` xoá resource khỏi state (Terraform "quên" quản lý nó) nhưng KHÔNG xoá resource thực tế trên cloud — dùng khi muốn ngừng để Terraform quản lý một resource cụ thể mà vẫn giữ nguyên nó chạy.

**23. Giải thích khái niệm Drift Detection trong Terraform.**
Drift là hiện tượng trạng thái thực tế trên cloud khác với trạng thái được ghi trong state (do ai đó sửa thủ công qua console/CLI ngoài Terraform). Terraform phát hiện drift khi chạy `plan` (nó refresh state bằng cách gọi API kiểm tra thực tế rồi so sánh với code) — hiển thị thay đổi cần "sửa lại" để khớp code, giúp phát hiện thay đổi ngoài ý muốn trước khi nó gây hậu quả.

**24. Giải thích Terraform Cloud/Enterprise mang lại lợi ích gì so với chạy Terraform CLI thủ công/CI tự viết.**
Terraform Cloud cung cấp remote state management tích hợp sẵn (không cần tự dựng S3+DynamoDB), remote execution (chạy plan/apply trên môi trường chuẩn hoá thay vì máy local/CI runner khác nhau), Sentinel/OPA policy-as-code để enforce rule trước khi apply, cơ chế approval workflow (yêu cầu phê duyệt trước khi apply plan), và quản lý version/variable tập trung qua UI — giảm công sức tự xây dựng các thành phần này thủ công.

**25. Giải thích cách tổ chức cấu trúc thư mục Terraform cho một dự án nhiều môi trường (dev/staging/prod).**
Cách phổ biến: tách thư mục theo môi trường (`environments/dev`, `environments/staging`, `environments/prod`), mỗi thư mục có file `.tfvars` riêng và gọi chung các `module` tái sử dụng đặt trong thư mục `modules/` — đảm bảo logic hạ tầng (module) dùng chung nhất quán giữa các môi trường, trong khi giá trị cấu hình (kích thước instance, số lượng, region) khác nhau theo từng môi trường qua biến.

**26. Giải thích Remote State Data Source dùng để làm gì.**
`terraform_remote_state` data source cho phép một cấu hình Terraform đọc output từ state của một cấu hình Terraform khác (ví dụ layer network tách riêng với layer application) — dùng để chia nhỏ hạ tầng lớn thành nhiều state độc lập (giảm blast radius khi apply, tăng tốc plan/apply vì mỗi state nhỏ hơn) trong khi vẫn có thể tham chiếu giá trị cần thiết giữa các layer (như VPC ID từ layer network để layer application dùng).

**27. Giải thích Dynamic Block trong Terraform dùng khi nào.**
Dynamic Block cho phép sinh ra nhiều block lồng nhau (nested block, ví dụ nhiều `ingress` rule trong Security Group) một cách động dựa trên một list/map biến đầu vào, thay vì phải viết cứng từng block riêng lẻ — hữu ích khi số lượng block con thay đổi tuỳ theo môi trường/tham số truyền vào.

**28. Giải thích sự khác biệt giữa Terraform Registry Module chính thức (HashiCorp Verified) và Module tự viết nội bộ.**
Module chính thức trên Terraform Registry (được HashiCorp verify hoặc cộng đồng đóng góp nhiều sao) thường đã qua kiểm thử rộng rãi, hỗ trợ nhiều edge case, và được duy trì cập nhật theo API provider mới — phù hợp dùng nhanh cho pattern phổ biến (VPC, EKS...). Module tự viết nội bộ cho phép tuỳ chỉnh chính xác theo chuẩn/policy riêng của tổ chức (naming convention, tagging bắt buộc, compliance riêng) nhưng cần tự bảo trì và kiểm thử.

**29. Giải thích cách Terraform xử lý Sensitive Data trong State File và rủi ro liên quan.**
Terraform lưu toàn bộ attribute của resource (kể cả giá trị nhạy cảm như password được tạo ra hoặc đọc từ output) dưới dạng plaintext trong state file mặc định — đây là rủi ro bảo mật lớn nếu state không được bảo vệ đúng cách. Giải pháp: luôn mã hoá state tại nơi lưu trữ (S3 server-side encryption + KMS), giới hạn quyền truy cập nghiêm ngặt (IAM policy chỉ cho phép người/hệ thống cần thiết đọc state), và cân nhắc dùng Terraform Cloud (tự động mã hoá và kiểm soát truy cập state tốt hơn tự quản lý S3 thủ công).

**30. Giải thích cách viết Custom Provider hoặc lý do khi nào một tổ chức cần làm việc này.**
Custom Provider cần thiết khi muốn Terraform quản lý một hệ thống nội bộ không có provider public sẵn có (ví dụ hệ thống provisioning nội bộ tự phát triển của công ty) — viết bằng Go, implement interface theo Terraform Plugin SDK/Framework định nghĩa CRUD logic (Create/Read/Update/Delete) tương ứng với API nội bộ đó, cho phép đội ngũ dùng chung cú pháp HCL quen thuộc để quản lý cả tài nguyên nội bộ lẫn tài nguyên cloud công khai trong cùng workflow.

**31. Giải thích cách test Terraform code (Terratest, `terraform plan` trong CI, policy check).**
Terratest (thư viện Go) cho phép viết test tự động: apply thực sự một cấu hình Terraform trong môi trường sandbox, kiểm tra kết quả (gọi API kiểm tra resource đã tạo đúng chưa), rồi tự động destroy sau test — kiểm thử ở mức tích hợp thực tế. Ngoài ra, CI thường chạy `terraform plan` và review output như một hình thức kiểm tra "sẽ thay đổi gì" trước khi merge, kết hợp policy-as-code (Sentinel, OPA/Conftest) để tự động chặn plan vi phạm quy tắc (ví dụ không được tạo Security Group mở toàn bộ 0.0.0.0/0).

**32. Giải thích khái niệm Terraform Plan File (`-out`) dùng để đảm bảo apply đúng những gì đã review.**
Chạy `terraform plan -out=plan.tfplan` lưu kết quả plan vào file nhị phân, sau đó `terraform apply plan.tfplan` sẽ áp dụng chính xác plan đã lưu đó (không tính toán lại) — đảm bảo những gì được review/approve (ví dụ trong pipeline CI/CD có bước approval) chính xác là những gì được apply, tránh trường hợp trạng thái hạ tầng thay đổi giữa lúc plan và lúc apply dẫn đến kết quả khác với review ban đầu.

**33. Giải thích cách quản lý version của Provider và Terraform Core để tránh breaking change.**
Khai báo `required_providers` với version constraint cụ thể (ví dụ `~> 5.0` cho phép patch/minor nhưng không major) trong block `terraform`, và `required_version` cho chính Terraform CLI — kết hợp file `.terraform.lock.hcl` (tự động sinh, nên commit vào Git) để pin chính xác version/checksum provider đã dùng, đảm bảo mọi thành viên/CI dùng đúng cùng version, tránh trường hợp behavior khác nhau do version provider khác nhau.

**34. Giải thích trường hợp Terraform Plan hiển thị thay đổi không mong muốn dù không sửa code (unexpected diff) và nguyên nhân phổ biến.**
Nguyên nhân thường gặp: provider version mới thay đổi default value hoặc cách tính toán một số attribute; giá trị computed do cloud provider tự sinh khác đi giữa các lần refresh (ví dụ một số tag hệ thống tự thêm); hoặc do thay đổi thủ công ngoài Terraform (drift thực sự). Cần đọc kỹ output diff (`~` là sửa, `-/+` là xoá tạo lại), kiểm tra changelog provider nếu vừa upgrade version, và xác nhận có ai thao tác thủ công ngoài Terraform gần đây không.

**35. Giải thích khái niệm Blast Radius trong thiết kế state Terraform và cách giảm thiểu.**
Blast Radius là phạm vi ảnh hưởng nếu một lệnh `apply` sai sót hoặc state bị lỗi — nếu toàn bộ hạ tầng (network, database, application) nằm trong một state file khổng lồ duy nhất, một lỗi nhỏ có thể ảnh hưởng toàn bộ hệ thống cùng lúc. Giảm thiểu bằng cách chia nhỏ state theo layer/lifecycle khác nhau (network ít thay đổi tách riêng khỏi application thay đổi thường xuyên), dùng `terraform_remote_state` để tham chiếu giữa các layer, giúp mỗi lần apply chỉ ảnh hưởng phạm vi nhỏ, dễ review và giảm rủi ro.

### Nhóm Nâng cao (36–50)

**36. Thiết kế chiến lược quản lý State Terraform cho một tổ chức lớn với hàng trăm hạ tầng khác nhau (nhiều team, nhiều môi trường).**
Chia state theo cả 2 chiều: theo layer (network, security baseline, shared services, application) và theo môi trường/team (mỗi team/môi trường có state riêng biệt hoàn toàn) — dùng remote backend tập trung (S3 + DynamoDB hoặc Terraform Cloud Workspace riêng cho từng tổ hợp), quy ước naming/path rõ ràng để dễ tìm kiếm, và dùng `terraform_remote_state`/module registry nội bộ để chia sẻ giá trị và pattern chuẩn giữa các state mà không tạo phụ thuộc chặt gây khó bảo trì.

**37. Phân tích trade-off giữa việc dùng nhiều State nhỏ (nhiều layer tách biệt) so với một State lớn duy nhất cho toàn bộ hạ tầng.**
State nhỏ giảm blast radius, tăng tốc độ `plan`/`apply` (ít resource hơn để tính toán), cho phép nhiều team làm việc song song không tranh chấp lock, nhưng tăng độ phức tạp quản lý phụ thuộc giữa các state (cần cẩn thận thứ tự apply và dùng remote state reference đúng cách, có thể phát sinh vấn đề nếu output cần thiết chưa tồn tại). State lớn đơn giản hơn về mặt quản lý phụ thuộc (mọi thứ trong cùng dependency graph, Terraform tự lo thứ tự) nhưng rủi ro cao (một lỗi ảnh hưởng toàn bộ), chậm khi hạ tầng lớn, và dễ xảy ra tranh chấp lock khi nhiều người cùng làm việc.

**38. Thiết kế quy trình CI/CD cho Terraform với approval workflow an toàn cho production.**
Pipeline: (1) PR trigger `terraform fmt -check` + `validate` + `plan`, output plan post comment vào PR để review; (2) chạy policy check tự động (Sentinel/Conftest) chặn vi phạm rule; (3) merge PR yêu cầu review từ người có thẩm quyền; (4) sau merge, job apply cho môi trường non-prod chạy tự động, còn môi trường prod yêu cầu thêm bước approval thủ công (qua Environment Protection Rule của GitHub Actions hoặc built-in approval của Terraform Cloud) trước khi thực thi `apply plan.tfplan` (dùng đúng plan file đã được review, không tính toán lại) để đảm bảo tính nhất quán giữa review và thực thi.

**39. Giải thích cách xử lý khi cần refactor một module lớn (tách module cũ thành nhiều module nhỏ) mà không muốn Terraform destroy/recreate resource.**
Dùng kết hợp `terraform state mv` (di chuyển resource giữa các address trong state cũ sang đường dẫn mới tương ứng với cấu trúc module mới) hoặc từ Terraform 1.1+ dùng block `moved` khai báo trực tiếp trong code (declarative hơn, tự động ghi vào state khi apply, không cần chạy lệnh thủ công riêng) — quan trọng nhất là đảm bảo cấu hình mới sinh ra plan không có diff (`plan` sạch, không show create/destroy) sau khi refactor, xác nhận resource thực tế không bị động tới.

**40. Phân tích chiến lược quản lý Secrets trong Terraform (tránh lưu plaintext trong code/state) tích hợp với Vault/AWS Secrets Manager.**
Không hardcode secret trong file `.tf`/`.tfvars` (dễ commit nhầm vào Git); thay vào đó dùng `data` source đọc secret động từ Vault (`vault_generic_secret`) hoặc AWS Secrets Manager (`aws_secretsmanager_secret_version`) tại thời điểm apply — secret vẫn sẽ nằm trong state (vì Terraform cần biết giá trị để cấu hình resource), nên bắt buộc phải mã hoá state ở nơi lưu trữ và giới hạn quyền truy cập state nghiêm ngặt; với secret cực kỳ nhạy cảm, cân nhắc để hệ thống ngoài Terraform (ví dụ Vault Agent Injector trong runtime) xử lý thay vì để Terraform biết giá trị thật.

**41. Thiết kế cách xử lý Zero-Downtime Infrastructure Change (ví dụ thay đổi Launch Template của ASG) bằng Terraform.**
Kết hợp `create_before_destroy = true` trong lifecycle của resource cần thay thế (đảm bảo instance/resource mới được tạo và sẵn sàng trước khi resource cũ bị xoá), cấu hình `min_elb_capacity` hoặc health check chờ instance mới healthy trước khi tiếp tục, và với ASG cụ thể thường dùng thêm cơ chế Instance Refresh (tính năng native của AWS ASG được Terraform hỗ trợ qua `instance_refresh` block) để rolling replace instance theo tỷ lệ kiểm soát được (ví dụ 10% mỗi lần) thay vì thay thế toàn bộ đồng loạt.

**42. Giải thích cách xử lý xung đột khi hai pipeline CI/CD khác nhau cùng cố gắng `apply` vào cùng một state trong cùng thời điểm.**
Nếu dùng remote backend hỗ trợ locking (S3+DynamoDB hoặc Terraform Cloud), lệnh `apply` thứ hai sẽ tự động chờ hoặc báo lỗi "state locked" cho tới khi lệnh đầu hoàn thành và giải phóng lock — đảm bảo không có 2 tiến trình ghi state đồng thời gây corrupt. Về mặt quy trình, nên thiết kế pipeline với concurrency control (ví dụ GitHub Actions `concurrency` group theo tên state/môi trường) để tránh queue nhiều job apply cùng lúc gây chờ đợi không cần thiết hoặc timeout.

**43. Phân tích cách thiết kế Terraform Module Versioning Strategy cho một tổ chức có nhiều team cùng dùng chung module nội bộ.**
Publish module nội bộ lên private registry (Terraform Cloud private registry, hoặc Git repo với tag semantic version), mỗi team tham chiếu module qua version cụ thể (`source = "git::...?ref=v1.2.0"`) thay vì `main`/`master` để tránh thay đổi đột ngột ảnh hưởng ngoài ý muốn — đội ngũ phát triển module tuân theo semantic versioning nghiêm ngặt (breaking change tăng major version), duy trì CHANGELOG rõ ràng, và cân nhắc cơ chế thông báo/deprecation timeline khi cần ép các team upgrade lên version mới.

**44. Giải thích cách xử lý một tình huống thực tế: state file bị corrupt hoặc mất hoàn toàn, hạ tầng thực tế vẫn đang chạy — quy trình khôi phục ra sao?**
Quy trình: (1) Nếu dùng remote backend có versioning (S3 versioning), thử khôi phục version trước đó của state file; (2) Nếu không còn bản backup nào, cần liệt kê toàn bộ resource thực tế đang tồn tại (qua console/CLI/API hoặc công cụ như `terraformer` để tự động generate lại code + import resource hiện có); (3) Dùng `terraform import` (hoặc block `import` declarative) lần lượt đưa từng resource vào state mới, đối chiếu kỹ với code hiện tại để đảm bảo không có diff bất ngờ sau khi import xong; (4) Rút kinh nghiệm bật versioning + backup định kỳ cho backend để tránh lặp lại.

**45. Thiết kế chiến lược Compliance as Code cho Terraform (đảm bảo mọi hạ tầng tuân thủ chuẩn bảo mật trước khi apply).**
T�ch hợp Sentinel (Terraform Cloud/Enterprise) hoặc Open Policy Agent/Conftest (mã nguồn mở, chạy trong CI) để viết policy dạng code kiểm tra plan output (dạng JSON qua `terraform show -json`) trước khi cho phép apply — ví dụ chặn tạo S3 bucket public, bắt buộc tag `Owner`/`CostCenter`, giới hạn instance type được phép theo ngân sách team. Policy chạy tự động trong pipeline, fail sớm ngay ở bước plan nếu vi phạm, đảm bảo compliance được enforce nhất quán mà không phụ thuộc vào việc con người tự nhớ kiểm tra thủ công.

**46. Giải thích cách tối ưu hiệu năng khi Terraform quản lý một hạ tầng cực lớn (hàng nghìn resource) khiến `plan`/`apply` chậm.**
Chia nhỏ state theo layer/domain (giảm số resource mỗi state phải tính toán dependency graph), tăng `-parallelism` (mặc định 10, có thể tăng nếu provider API cho phép nhiều request đồng thời hơn mà không bị rate limit), giảm số lượng `data` source gọi API không cần thiết (mỗi data source là một API call làm chậm refresh), và với resource ổn định ít thay đổi, cân nhắc dùng `-target` một cách thận trọng cho các tình huống cần apply nhanh một phần cụ thể (dù HashiCorp khuyến cáo hạn chế lạm dụng vì có thể gây state không nhất quán nếu dùng sai cách).

**47. Phân tích rủi ro và cách giảm thiểu khi dùng `-target` flag trong Terraform.**
`-target` cho phép chỉ apply/plan một resource cụ thể thay vì toàn bộ, hữu ích khi cần sửa gấp một phần nhỏ mà không muốn tính toán/ảnh hưởng resource khác — nhưng rủi ro là dependency graph không được xem xét đầy đủ, có thể dẫn tới trạng thái state không nhất quán với thực tế (resource phụ thuộc không được cập nhật theo), hoặc dẫn tới hành vi bất ngờ ở lần `apply` đầy đủ tiếp theo. Nên chỉ dùng `-target` như biện pháp tạm thời khẩn cấp, luôn theo sau bằng một lần `apply` đầy đủ (không target) để đảm bảo toàn bộ state nhất quán trở lại.

**48. Một `terraform apply` bị gián đoạn giữa chừng (mất kết nối mạng/máy CI bị kill) — trạng thái state và hạ tầng thực tế lúc này ra sao và xử lý thế nào?**
Nếu apply bị gián đoạn giữa chừng, một số resource có thể đã được tạo/sửa thành công trên cloud nhưng state chưa kịp ghi nhận đầy đủ (Terraform thường ghi state ngay sau mỗi resource hoàn thành, không phải chỉ ở cuối, nên rủi ro mất mát thường không quá nghiêm trọng nhưng vẫn có thể xảy ra). Xử lý: chạy lại `terraform plan` để Terraform tự refresh và so sánh thực tế với state hiện có, xem xét kỹ những gì plan đề xuất (có thể có resource "phantom" cần import lại nếu state chưa ghi nhận resource đã tạo thành công) trước khi apply tiếp — luôn kiểm tra kỹ thay vì chạy `apply -auto-approve` ngay lập tức trong tình huống này.

**49. Thiết kế chiến lược Multi-Cloud với Terraform (ví dụ vừa quản lý AWS vừa GCP trong cùng tổ chức) cần lưu ý gì?**
Có thể khai báo nhiều `provider` khác nhau (AWS, Google, Azure...) trong cùng codebase, dùng `alias` nếu cần nhiều cấu hình cùng một provider (ví dụ nhiều account/region AWS). Cần lưu ý: mỗi cloud có model resource/networking khác biệt đáng kể (không thể dùng chung module 1:1 giữa các cloud), nên thiết kế module trừu tượng hoá ở mức phù hợp (interface chung nhưng implementation riêng theo cloud) nếu cần hỗ trợ multi-cloud thực sự, và cân nhắc kỹ về độ phức tạp tăng thêm này có thực sự cần thiết hay chỉ nên chọn một cloud chính và dùng cloud khác cho mục đích cụ thể (DR, dịch vụ đặc thù).

**50. Một hạ tầng production dùng Terraform đột nhiên có báo cáo hàng loạt resource "sẽ bị destroy và recreate" trong lần plan định kỳ dù không ai chủ động sửa code — bạn phân tích và xử lý theo trình tự nào?**
Trình tự: (1) Kiểm tra Git log xem có commit nào gần đây thay đổi resource liên quan (kể cả thay đổi tưởng như nhỏ ở argument bắt buộc phải recreate, ví dụ đổi `availability_zone` của EBS volume); (2) Kiểm tra xem có nâng cấp version provider gần đây không (changelog provider có thể thay đổi cách một số attribute được xử lý, coi là force-new dù trước đó không phải); (3) Kiểm tra CHANGELOG của module nếu dùng module bên ngoài vừa update version; (4) Nếu xác định do thay đổi hợp lệ trong code, đánh giá kỹ tác động thực tế của việc recreate (mất dữ liệu nếu là stateful resource như RDS/EBS không có snapshot) trước khi apply, cân nhắc thêm `create_before_destroy` hoặc tách bước migrate dữ liệu an toàn trước; (5) Nếu là false positive do lỗi provider, có thể tạm thời pin lại version provider cũ trong khi chờ xác nhận/fix từ nhà phát triển provider.

---

## PHẦN 6: Monitoring & Logging (50 câu)

### Nhóm Cơ bản (1–17)

**1. Phân biệt Metrics, Logs, và Traces (3 trụ cột của Observability).**
Metrics là dữ liệu số theo thời gian (time-series) đo lường trạng thái hệ thống (CPU%, request count, latency), nhẹ, dễ tổng hợp/alert, phù hợp phát hiện "có vấn đề gì không". Logs là bản ghi sự kiện chi tiết dạng văn bản (thường có timestamp), giúp trả lời "chuyện gì đã xảy ra" cụ thể. Traces theo dõi hành trình một request đi qua nhiều service (distributed tracing), giúp trả lời "vấn đề nằm ở đâu trong chuỗi service" — cả ba bổ trợ nhau tạo thành bức tranh quan sát hệ thống đầy đủ.

**2. Monitoring và Observability khác nhau như thế nào?**
Monitoring theo dõi các chỉ số/trạng thái đã biết trước (known-unknowns) qua dashboard/alert được định nghĩa sẵn, trả lời "hệ thống có đang khoẻ không". Observability là khả năng đặt câu hỏi mới bất kỳ về hệ thống (unknown-unknowns) và tìm ra câu trả lời từ dữ liệu telemetry sẵn có (metrics/logs/traces) mà không cần deploy thêm code mới để debug — Observability là khái niệm rộng hơn, monitoring là một phần của nó.

**3. Prometheus là gì và hoạt động theo mô hình nào?**
Prometheus là hệ thống monitoring/time-series database mã nguồn mở, hoạt động theo mô hình pull (kéo): Prometheus server định kỳ (scrape interval) gửi HTTP request tới endpoint `/metrics` của các target để lấy dữ liệu, khác với nhiều hệ thống khác theo mô hình push (ứng dụng chủ động đẩy dữ liệu đi).

**4. Exporter trong hệ sinh thái Prometheus là gì?**
Exporter là một tiến trình nhỏ expose metric ở định dạng Prometheus (`/metrics` endpoint) cho các hệ thống không tự hỗ trợ Prometheus format sẵn — ví dụ Node Exporter (metric phần cứng/OS của server), MySQL Exporter (metric database), Blackbox Exporter (kiểm tra khả năng truy cập của endpoint từ bên ngoài).

**5. PromQL là gì?**
PromQL (Prometheus Query Language) là ngôn ngữ truy vấn chuyên dụng để lấy và tính toán trên dữ liệu time-series lưu trong Prometheus, hỗ trợ các hàm tổng hợp (`rate()`, `sum()`, `avg()`, `histogram_quantile()`...) để tạo ra biểu đồ dashboard hoặc điều kiện alert.

**6. Grafana là gì và vai trò của nó khác Prometheus ra sao?**
Grafana là công cụ trực quan hoá dữ liệu (visualization), không tự lưu trữ dữ liệu mà kết nối tới nhiều nguồn dữ liệu khác nhau (Prometheus, Loki, Elasticsearch, CloudWatch...) để hiển thị dashboard, biểu đồ tuỳ chỉnh — Prometheus chịu trách nhiệm thu thập/lưu trữ/query dữ liệu, Grafana chịu trách nhiệm hiển thị đẹp mắt và dễ hiểu cho con người.

**7. Alertmanager trong hệ sinh thái Prometheus dùng để làm gì?**
Alertmanager nhận alert được trigger từ Prometheus (khi một rule điều kiện đúng), sau đó xử lý logic nâng cao: gom nhóm (grouping) các alert liên quan thành một thông báo, khử trùng lặp (deduplication), tạm ẩn (silencing) theo lịch bảo trì, định tuyến (routing) tới đúng kênh/team phù hợp (Slack, PagerDuty, email) dựa trên label của alert.

**8. ELK Stack là gì (Elasticsearch, Logstash, Kibana)?**
ELK Stack là bộ công cụ phổ biến cho log aggregation: Logstash (hoặc Fluentd/Filebeat) thu thập và xử lý log từ nhiều nguồn, Elasticsearch lưu trữ và index log để tìm kiếm nhanh (full-text search), Kibana cung cấp giao diện trực quan để tìm kiếm, lọc, và tạo dashboard từ dữ liệu log trong Elasticsearch.

**9. Loki là gì và khác Elasticsearch ra sao trong việc lưu trữ log?**
Loki (Grafana Labs) là hệ thống log aggregation được thiết kế nhẹ hơn Elasticsearch: chỉ index metadata (label) của log thay vì index toàn bộ nội dung log (full-text index), giúp giảm đáng kể chi phí lưu trữ và tài nguyên vận hành — đánh đổi là tìm kiếm theo nội dung log (grep-style) chậm hơn so với Elasticsearch cho tập dữ liệu cực lớn, nhưng đủ dùng cho phần lớn nhu cầu và tích hợp liền mạch với Grafana.

**10. Log Level (DEBUG, INFO, WARN, ERROR, FATAL) dùng để làm gì?**
Log Level phân loại mức độ nghiêm trọng của một dòng log, giúp lọc và ưu tiên xử lý: DEBUG (chi tiết kỹ thuật để debug), INFO (sự kiện bình thường đáng ghi nhận), WARN (bất thường nhưng chưa gây lỗi), ERROR (lỗi xảy ra ảnh hưởng chức năng), FATAL/CRITICAL (lỗi nghiêm trọng khiến ứng dụng không thể tiếp tục). Cấu hình đúng log level giúp giảm nhiễu (noise) trong production trong khi vẫn giữ đủ thông tin cần thiết.

**11. Structured Logging là gì và tại sao nên dùng thay vì plain text log?**
Structured Logging ghi log theo định dạng có cấu trúc (thường là JSON) với các field rõ ràng (timestamp, level, service, trace_id, message...) thay vì chuỗi văn bản tự do — giúp hệ thống log aggregation dễ dàng parse, lọc, và truy vấn chính xác theo field (ví dụ lọc tất cả log của một `user_id` cụ thể) mà không cần regex phức tạp như với plain text log.

**12. Health Check Endpoint (`/health`, `/readyz`) dùng để làm gì trong monitoring?**
Health Check Endpoint là một API đơn giản ứng dụng expose để hệ thống bên ngoài (load balancer, Kubernetes probe, monitoring system) kiểm tra nhanh tình trạng hoạt động — thường trả về 200 OK nếu ứng dụng khoẻ, hoặc kiểm tra sâu hơn (liveness kiểm tra tiến trình còn chạy, readiness kiểm tra đã sẵn sàng nhận traffic, ví dụ đã kết nối được database chưa).

**13. Dashboard trong Grafana/monitoring dùng để làm gì và cách thiết kế dashboard hiệu quả?**
Dashboard tổng hợp trực quan nhiều metric quan trọng vào một màn hình để nhanh chóng nắm bắt tình trạng hệ thống. Thiết kế hiệu quả nên ưu tiên hiển thị theo "Golden Signals" (latency, traffic, errors, saturation) ở vị trí dễ thấy nhất, nhóm theo ngữ cảnh logic (theo service, theo tầng hạ tầng), tránh nhồi nhét quá nhiều biểu đồ không liên quan gây khó đọc khi cần phản ứng nhanh lúc sự cố.

**14. Alert Fatigue là gì và tại sao là vấn đề cần tránh?**
Alert Fatigue là hiện tượng đội ngũ nhận quá nhiều cảnh báo (đặc biệt false positive hoặc alert không thực sự cần hành động), dẫn tới việc dần "phớt lờ" hoặc phản ứng chậm với alert — kể cả alert thực sự quan trọng, gây rủi ro bỏ sót sự cố nghiêm trọng. Cần thiết kế alert có ý nghĩa hành động (actionable), đúng ngưỡng, và giảm nhiễu bằng cách gom nhóm/deduplicate hợp lý.

**15. Retention Policy trong hệ thống log/metric dùng để làm gì?**
Retention Policy quy định thời gian dữ liệu (log/metric) được giữ lại trước khi tự động xoá — cân bằng giữa nhu cầu điều tra sự cố/tuân thủ quy định (cần giữ đủ lâu) và chi phí lưu trữ (giữ càng lâu càng tốn kém) — thường áp dụng tier khác nhau (dữ liệu gần đây lưu chi tiết/nhanh truy cập, dữ liệu cũ hơn nén lại hoặc chuyển sang lưu trữ rẻ hơn).

**16. Uptime Monitoring (Synthetic Monitoring) là gì?**
Uptime/Synthetic Monitoring chủ động gửi request định kỳ (từ bên ngoài, nhiều vị trí địa lý) tới endpoint public của ứng dụng để kiểm tra khả năng truy cập và thời gian phản hồi thực tế từ góc nhìn người dùng cuối, độc lập với hệ thống monitoring nội bộ — giúp phát hiện sự cố ngay cả khi vấn đề nằm ở tầng network/DNS bên ngoài mà monitoring nội bộ không thấy được.

**17. CloudWatch (AWS) đóng vai trò gì trong hệ sinh thái monitoring của AWS?**
CloudWatch là dịch vụ monitoring/logging tích hợp sẵn của AWS, tự động thu thập metric cơ bản từ hầu hết dịch vụ AWS (EC2, RDS, Lambda...), cho phép custom metric, lưu trữ log tập trung (CloudWatch Logs), và tạo alarm/dashboard — là lựa chọn mặc định tiện lợi cho hệ thống chủ yếu chạy trên AWS, dù nhiều tổ chức vẫn kết hợp thêm Prometheus/Grafana cho khả năng tuỳ biến sâu hơn.

### Nhóm Trung cấp (18–35)

**18. Giải thích khái niệm Golden Signals (Latency, Traffic, Errors, Saturation) trong SRE.**
Golden Signals (từ Google SRE Book) là 4 chỉ số cốt lõi nên giám sát cho bất kỳ service nào: Latency (thời gian phản hồi request, cần phân biệt request thành công và lỗi vì có thể khác nhau đáng kể), Traffic (lượng request/nhu cầu hệ thống đang xử lý), Errors (tỷ lệ request thất bại), Saturation (mức độ "đầy" của tài nguyên hệ thống, ví dụ CPU/memory/queue gần đạt giới hạn) — theo dõi đủ 4 chỉ số này cho một cái nhìn tổng quan đủ tốt về sức khoẻ service mà không cần quá nhiều dashboard phức tạp.

**19. Giải thích SLI, SLO, SLA khác nhau như thế nào.**
SLI (Service Level Indicator) là chỉ số đo lường thực tế (ví dụ % request thành công trong 5 phút qua). SLO (Service Level Objective) là mục tiêu nội bộ đặt ra cho SLI đó (ví dụ 99.9% request thành công trong 30 ngày) để định hướng vận hành. SLA (Service Level Agreement) là cam kết chính thức với khách hàng (thường kèm điều khoản bồi thường nếu không đạt) — SLA thường có ngưỡng "dễ thở" hơn SLO nội bộ để có buffer an toàn trước khi vi phạm cam kết với khách hàng.

**20. Giải thích khái niệm Error Budget trong SRE.**
Error Budget là "ngân sách lỗi cho phép" tính từ SLO (ví dụ SLO 99.9% nghĩa là error budget 0.1% thời gian được phép lỗi trong chu kỳ) — khi error budget còn dư, team có thể tự tin release tính năng mới nhanh hơn; khi error budget gần cạn, team nên ưu tiên ổn định hệ thống (freeze deploy rủi ro) thay vì tiếp tục release nhanh — đây là cơ chế cân bằng giữa tốc độ phát triển và độ ổn định một cách định lượng, khách quan.

**21. Giải thích cơ chế Histogram và Percentile (P50, P95, P99) trong đo lường latency.**
Trung bình (average) latency dễ bị che khuất bởi outlier (một vài request rất chậm không phản ánh trải nghiệm đa số), nên thường dùng Percentile: P50 (trung vị, 50% request nhanh hơn giá trị này), P95/P99 (95%/99% request nhanh hơn giá trị này, phản ánh trải nghiệm của nhóm người dùng gặp độ trễ tệ nhất). Prometheus lưu dữ liệu dạng Histogram (bucket theo khoảng giá trị) để tính percentile qua hàm `histogram_quantile()`, khác với Summary tính percentile trực tiếp tại client nhưng không thể tổng hợp (aggregate) chính xác giữa nhiều instance.

**22. Giải thích Cardinality trong Prometheus và tại sao High Cardinality là vấn đề.**
Cardinality là số lượng tổ hợp label duy nhất của một metric (ví dụ metric có label `user_id` sẽ tạo ra một time-series riêng biệt cho MỖI user, dẫn tới hàng triệu time-series nếu có nhiều user) — High Cardinality gây quá tải bộ nhớ/storage của Prometheus, làm chậm truy vấn nghiêm trọng. Nguyên tắc: tránh dùng label có giá trị không giới hạn (unbounded, như user_id, request_id, IP) làm label của metric, những giá trị đó nên đưa vào log/trace thay vì metric.

**23. Giải thích Distributed Tracing và OpenTelemetry là gì.**
Distributed Tracing theo dõi hành trình một request khi nó đi qua nhiều microservice, ghi lại từng "span" (một đơn vị công việc, ví dụ một lần gọi database) kèm thời gian và mối quan hệ cha-con giữa các span, tổng hợp thành một "trace" hoàn chỉnh — giúp xác định chính xác service/bước nào gây chậm trong một chuỗi request phức tạp. OpenTelemetry là chuẩn mã nguồn mở (do CNCF duy trì) thống nhất cách instrument code để sinh ra metrics/logs/traces, giúp không bị khoá chặt (vendor lock-in) vào một công cụ tracing cụ thể (Jaeger, Zipkin, Datadog...).

**24. Giải thích khái niệm Log Aggregation và tại sao cần thiết trong kiến trúc microservices/container.**
Log Aggregation là việc thu thập log từ nhiều nguồn phân tán (nhiều container/instance/service) về một nơi tập trung để tìm kiếm/phân tích — cực kỳ cần thiết trong container vì log trong container thường mất khi container bị xoá/restart (ephemeral filesystem), và với hàng chục/hàng trăm instance, việc SSH vào từng máy xem log thủ công là bất khả thi.

**25. Giải thích cơ chế Sidecar Pattern trong việc thu thập log (ví dụ Fluent Bit sidecar) so với DaemonSet Agent.**
Sidecar Pattern chạy một container thu thập log riêng trong cùng Pod với ứng dụng chính, phù hợp khi cần xử lý log đặc thù riêng cho từng ứng dụng (định dạng khác nhau, cần transform riêng). DaemonSet Agent (một agent duy nhất trên mỗi Node, thu thập log của toàn bộ Pod trên Node đó) hiệu quả tài nguyên hơn (không nhân bản agent theo từng Pod), là cách tiếp cận phổ biến hơn cho hầu hết trường hợp trong Kubernetes trừ khi có nhu cầu xử lý đặc thù theo từng ứng dụng.

**26. Giải thích cách thiết kế Alert Rule tốt (Actionable Alert) để tránh Alert Fatigue.**
Alert tốt cần: gắn với triệu chứng ảnh hưởng trực tiếp người dùng (symptom-based, ví dụ error rate tăng) hơn là nguyên nhân kỹ thuật chi tiết (cause-based, ví dụ CPU tăng — có thể không ảnh hưởng gì nếu vẫn còn dư tài nguyên), có ngưỡng và thời gian đủ (`for: 5m`) để tránh trigger vì dao động tạm thời (flapping), kèm runbook link rõ ràng hướng dẫn hành động cần làm, và luôn tự hỏi "nếu alert này kêu lúc 3h sáng, người trực có cần thức dậy xử lý ngay không?" — nếu câu trả lời là không, nên hạ mức độ ưu tiên hoặc bỏ hẳn alert đó.

**27. Giải thích Runbook trong vận hành on-call là gì?**
Runbook là tài liệu hướng dẫn từng bước cụ thể để xử lý một loại sự cố/alert nhất định (kiểm tra gì trước, lệnh nào để chẩn đoán, cách khắc phục tạm thời/escalate khi nào) — giúp người trực (kể cả người mới, chưa quen sâu hệ thống) có thể phản ứng nhất quán và nhanh chóng thay vì phải tự mò mẫm mỗi lần, giảm đáng kể MTTR (Mean Time To Resolve).

**28. Giải thích khái niệm MTTR, MTTD, MTBF trong vận hành hệ thống.**
MTTD (Mean Time To Detect) là thời gian trung bình từ khi sự cố xảy ra tới khi được phát hiện. MTTR (Mean Time To Resolve/Recover) là thời gian trung bình từ khi phát hiện tới khi khắc phục xong. MTBF (Mean Time Between Failures) là thời gian trung bình giữa hai lần sự cố liên tiếp — các chỉ số này giúp đánh giá khách quan hiệu quả của hệ thống monitoring/alerting (MTTD thấp) và quy trình xử lý sự cố (MTTR thấp).

**29. Giải thích Log Sampling dùng để làm gì trong hệ thống có traffic cực lớn.**
Log Sampling chỉ ghi lại một tỷ lệ (ví dụ 10%) trong số các sự kiện log (thường áp dụng cho log không quan trọng như access log thành công), giảm đáng kể chi phí lưu trữ/xử lý khi traffic cực lớn, trong khi vẫn giữ lại 100% log cho các sự kiện quan trọng (error, request chậm bất thường) — cần thiết kế logic sampling thông minh (ví dụ luôn giữ log của request lỗi dù có sampling) để không mất thông tin quan trọng khi cần điều tra sự cố.

**30. Giải thích cách tích hợp Alerting với on-call tool (PagerDuty/Opsgenie) và Escalation Policy hoạt động ra sao.**
Alertmanager/monitoring system gửi alert tới PagerDuty/Opsgenie qua webhook/integration, các công cụ này quản lý Escalation Policy: nếu người trực đầu tiên (theo lịch on-call rotation) không phản hồi (acknowledge) trong khoảng thời gian quy định, tự động escalate lên người/nhóm tiếp theo (ví dụ team lead, hoặc secondary on-call) — đảm bảo sự cố nghiêm trọng luôn được ai đó xử lý dù người trực chính không kịp phản hồi (ngủ quên, mất kết nối...).

**31. Giải thích khái niệm Correlation ID (Trace ID) trong việc liên kết log giữa các microservice.**
Correlation ID (thường gọi Trace ID/Request ID) là một mã định danh duy nhất được sinh ra ngay khi request bắt đầu (thường ở API Gateway hoặc service đầu tiên nhận request) và được truyền xuyên suốt qua tất cả các service downstream (qua HTTP header) — mọi dòng log liên quan tới request đó ở bất kỳ service nào đều ghi kèm Trace ID này, cho phép truy vấn và ghép lại toàn bộ hành trình của một request cụ thể qua nhiều service khác nhau.

**32. Giải thích cơ chế Push Gateway trong Prometheus dùng khi nào.**
Push Gateway là thành phần trung gian cho phép các job ngắn hạn (batch job, cron job kết thúc nhanh trước khi Prometheus kịp scrape theo chu kỳ pull thông thường) đẩy (push) metric của chúng vào Push Gateway, sau đó Prometheus scrape từ Push Gateway như một target bình thường — chỉ nên dùng cho trường hợp đặc biệt này (job ngắn hạn), không nên dùng Push Gateway để thay thế mô hình pull thông thường cho service dài hạn.

**33. Giải thích cách xử lý vấn đề Log Volume quá lớn gây tốn kém chi phí lưu trữ trong hệ thống lớn.**
Chiến lược: giảm log level ở production (loại bỏ DEBUG), áp dụng sampling cho log không quan trọng, nén log trước khi lưu trữ dài hạn, thiết lập tiered storage (hot storage cho vài ngày gần nhất tìm kiếm nhanh, cold storage rẻ hơn cho log cũ hơn ít truy vấn), dùng structured logging để giảm dung lượng dư thừa (loại bỏ field trùng lặp), và định kỳ review xem có log nào không còn giá trị (không ai từng query tới) để loại bỏ hoàn toàn.

**34. Giải thích cách Prometheus xử lý High Availability (chạy nhiều Prometheus instance song song).**
Prometheus mặc định không có clustering/replication tích hợp sẵn — cách phổ biến để đạt HA là chạy 2 (hoặc nhiều) instance Prometheus giống hệt nhau, cùng scrape các target giống nhau độc lập (mỗi instance có dữ liệu riêng, không đồng bộ với nhau), Alertmanager cấu hình nhận alert từ cả hai và tự khử trùng lặp — với nhu cầu lưu trữ lâu dài/truy vấn tổng hợp từ nhiều instance, thường bổ sung thêm giải pháp như Thanos hoặc Cortex/Mimir.

**35. Giải thích Thanos hoặc Cortex/Mimir giải quyết vấn đề gì cho Prometheus ở quy mô lớn.**
Thanos/Cortex/Mimir mở rộng Prometheus để giải quyết các giới hạn native: lưu trữ dài hạn (long-term storage) bằng cách đẩy dữ liệu ra object storage (S3) thay vì chỉ giữ local disk có giới hạn, truy vấn hợp nhất (global query view) dữ liệu từ nhiều Prometheus instance/cluster khác nhau như một nguồn duy nhất, và hỗ trợ khả năng scale ngang thực sự cho việc lưu trữ/truy vấn time-series ở quy mô hàng triệu series mà một Prometheus instance đơn lẻ không kham nổi.

### Nhóm Nâng cao (36–50)

**36. Thiết kế kiến trúc Observability hoàn chỉnh cho một hệ thống microservices quy mô lớn (50+ service).**
Kiến trúc gồm: Metrics qua Prometheus (mỗi service expose `/metrics`) + Thanos/Mimir cho long-term storage và global view, Logging qua Fluent Bit (DaemonSet) đẩy log về Loki hoặc Elasticsearch với structured JSON logging bắt buộc kèm Trace ID, Distributed Tracing qua OpenTelemetry SDK instrument tất cả service, xuất trace về Jaeger/Tempo, tất cả visualize thống nhất qua Grafana (data source Prometheus + Loki + Tempo, hỗ trợ liên kết chéo giữa metric/log/trace ngay trên UI), và Alertmanager tích hợp PagerDuty với Escalation Policy rõ ràng theo từng team sở hữu service.

**37. Phân tích trade-off giữa self-hosted monitoring stack (Prometheus/Grafana/Loki tự vận hành) và SaaS observability platform (Datadog/New Relic).**
Self-hosted cho phép kiểm soát hoàn toàn dữ liệu (quan trọng với compliance/dữ liệu nhạy cảm), chi phí có thể thấp hơn ở quy mô cực lớn (chỉ trả tài nguyên hạ tầng), nhưng tốn công vận hành đáng kể (scaling, HA, upgrade, tích hợp nhiều công cụ với nhau). SaaS platform giảm gánh nặng vận hành gần như hoàn toàn (vendor tự lo scaling/HA), tích hợp sẵn nhiều tính năng nâng cao (anomaly detection, APM tự động instrument), triển khai nhanh, nhưng chi phí tăng nhanh theo khối lượng dữ liệu (đặc biệt cardinality/log volume cao) và có rủi ro vendor lock-in.

**38. Thiết kế chiến lược giảm chi phí Observability mà không giảm khả năng phát hiện sự cố (Cost-Effective Observability).**
Áp dụng: Tail-based Sampling cho tracing (chỉ giữ lại 100% trace có lỗi hoặc latency bất thường, sample thấp cho trace bình thường, quyết định sau khi trace hoàn tất thay vì random ngay từ đầu), giảm cardinality metric triệt để (loại bỏ label không cần thiết), log level phù hợp theo môi trường (production chỉ WARN/ERROR trở lên trừ khi đang debug tạm thời), tiered storage cho dữ liệu cũ, và định kỳ audit dashboard/alert không còn ai dùng để tắt hẳn nguồn thu thập dữ liệu tương ứng không cần thiết.

**39. Giải thích cách xây dựng hệ thống Anomaly Detection tự động (không chỉ threshold tĩnh) cho metric quan trọng.**
Thay vì ngưỡng cố định (ví dụ CPU > 80%) vốn không phù hợp với pattern traffic biến động theo giờ/ngày/mùa, dùng phương pháp thống kê như so sánh với baseline lịch sử cùng khung giờ (ví dụ so với cùng giờ tuần trước ± độ lệch chuẩn), hoặc áp dụng thuật toán machine learning cho time-series (như Facebook Prophet, hoặc tính năng anomaly detection tích hợp sẵn trong một số SaaS platform) để tự động học pattern bình thường và cảnh báo khi có độ lệch bất thường thực sự, giảm đáng kể false positive so với ngưỡng tĩnh cứng nhắc.

**40. Phân tích cách thiết kế Multi-Tenant Observability Platform (một stack quan sát chung phục vụ nhiều team/khách hàng) đảm bảo cô lập dữ liệu.**
Cần: giới hạn quyền truy cập theo label/tenant ID ở tầng data source (Prometheus với remote-write có thể gắn thêm label tenant, hoặc dùng multi-tenant native của Mimir/Loki hỗ trợ tenant ID qua header `X-Scope-OrgID`), RBAC trong Grafana giới hạn dashboard/data source theo team, và đặc biệt cẩn trọng với Cardinality/resource quota theo tenant để một tenant có traffic/log lớn bất thường không làm ảnh hưởng hiệu năng của tenant khác dùng chung hạ tầng.

**41. Thiết kế cách giám sát chính hệ thống Monitoring (Monitoring the Monitoring, "who watches the watchers").**
Cần một hệ thống giám sát độc lập (thường đơn giản hơn, nằm ngoài phạm vi hạ tầng chính) để theo dõi chính Prometheus/Alertmanager/Grafana có đang hoạt động không — ví dụ Dead Man's Switch: một alert luôn trigger định kỳ (heartbeat) gửi tới một dịch vụ bên ngoài hoàn toàn độc lập (như healthchecks.io), nếu dịch vụ đó KHÔNG nhận được heartbeat trong khoảng thời gian dự kiến, nó tự gửi cảnh báo riêng — đảm bảo phát hiện được ngay cả khi toàn bộ hệ thống monitoring chính bị sập hoàn toàn (mất khả năng tự cảnh báo về chính nó).

**42. Giải thích cách xử lý vấn đề Clock Skew (lệch đồng hồ) giữa các server ảnh hưởng đến độ chính xác của Distributed Tracing/Log Correlation như thế nào.**
Nếu đồng hồ giữa các server lệch nhau (dù chỉ vài trăm ms), thứ tự log/span hiển thị có thể sai lệch so với thứ tự thực tế xảy ra, gây khó khăn khi debug trace phức tạp (span con hiển thị bắt đầu trước span cha). Giải pháp: bắt buộc đồng bộ NTP chính xác trên toàn bộ hạ tầng, một số hệ thống tracing dùng logic bù trừ (clock skew adjustment) dựa trên round-trip time ước lượng, và ưu tiên dùng monotonic clock cho việc đo khoảng thời gian (duration) thay vì chỉ dựa vào wall-clock timestamp tuyệt đối để tránh sai lệch do NTP điều chỉnh đột ngột giữa chừng.

**43. Phân tích chiến lược Capacity Planning dựa trên dữ liệu Monitoring lịch sử.**
Dùng dữ liệu metric lịch sử (ít nhất vài tháng để thấy được pattern theo mùa/sự kiện đặc biệt) để dự đoán xu hướng tăng trưởng traffic/tài nguyên (qua phân tích trend, không chỉ nhìn giá trị đỉnh gần nhất), tính toán buffer an toàn (thường 20-30% headroom trên đỉnh dự đoán để chịu được spike bất ngờ), kết hợp với kế hoạch kinh doanh (sự kiện marketing lớn, mùa cao điểm) để chủ động scale trước thay vì phản ứng bị động khi hệ thống đã quá tải.

**44. Giải thích cách thiết kế hệ thống Log-based Metrics (sinh metric từ log thay vì chỉ dùng metric gốc) và khi nào cần dùng.**
Log-based Metrics (ví dụ đếm số dòng log chứa từ khóa "error" trong một khung thời gian để tạo thành một metric error rate) hữu ích khi ứng dụng không tự expose metric sẵn (legacy application không thể sửa code dễ dàng) nhưng có log chi tiết — công cụ như Loki (qua LogQL) hoặc Elasticsearch có thể tính toán dạng này. Tuy nhiên cách này tốn tài nguyên hơn nhiều so với metric gốc (phải parse/xử lý toàn bộ log thay vì chỉ tăng một counter đơn giản), nên chỉ nên dùng như giải pháp tạm thời/bổ sung, ưu tiên lâu dài là instrument metric trực tiếp trong code khi có thể.

**45. Thiết kế chiến lược On-Call Rotation và Incident Management tích hợp với hệ thống Monitoring.**
Kết hợp: lịch rotation công bằng (dùng công cụ như PagerDuty/Opsgenie tự động xoay ca, tránh một người bị gọi quá thường xuyên), phân loại mức độ nghiêm trọng alert rõ ràng (P1 gọi điện ngay lập tức, P3 chỉ cần ticket xử lý giờ hành chính), quy trình Incident Management chuẩn (declare incident, chỉ định Incident Commander, kênh giao tiếp riêng cho từng sự cố lớn), và bắt buộc Post-Incident Review (blameless postmortem) sau mỗi sự cố nghiêm trọng để rút ra hành động cải thiện cụ thể (bao gồm cả cải thiện chính hệ thống monitoring/alerting nếu phát hiện thiếu sót trong lúc xử lý).

**46. Giải thích cách xử lý False Positive và False Negative trong hệ thống Alerting, đâu là vấn đề nghiêm trọng hơn?**
False Positive (alert kêu nhưng thực tế không có vấn đề) gây Alert Fatigue, làm giảm độ tin cậy vào hệ thống alert theo thời gian. False Negative (có sự cố thực sự nhưng không có alert nào kêu) nghiêm trọng hơn về mặt hậu quả trực tiếp (sự cố không được phát hiện kịp thời, ảnh hưởng người dùng kéo dài) — tuy nhiên về lâu dài, quá nhiều False Positive gián tiếp dẫn tới nhiều False Negative hơn (vì con người bắt đầu phớt lờ/tắt bớt alert), nên cần cân bằng cả hai, ưu tiên giảm False Positive trước để duy trì độ tin cậy của hệ thống, đồng thời định kỳ review incident đã xảy ra mà không có alert tương ứng để bổ sung coverage còn thiếu.

**47. Phân tích cách tích hợp Security Monitoring (SIEM) với hệ thống Observability thông thường có gì khác biệt.**
SIEM (Security Information and Event Management) tập trung vào phát hiện hành vi bất thường liên quan bảo mật (đăng nhập bất thường, truy cập trái phép, pattern tấn công) thường cần giữ log lâu hơn nhiều (yêu cầu compliance/forensic, có thể 1 năm+) và cần khả năng correlation phức tạp giữa nhiều nguồn log khác nhau (network, application, IAM) theo rule bảo mật chuyên biệt — khác với Observability thông thường tập trung vào hiệu năng/độ tin cậy ứng dụng với retention ngắn hơn. Nhiều tổ chức duy trì 2 pipeline log riêng biệt (hoặc dùng chung nguồn nhưng route tới 2 hệ thống lưu trữ/phân tích khác nhau) để đáp ứng cả hai nhu cầu mà không ảnh hưởng chi phí/hiệu năng lẫn nhau.

**48. Một dashboard Grafana hiển thị metric bình thường nhưng người dùng thực tế báo cáo hệ thống chậm — bạn phân tích nguyên nhân gap này như thế nào?**
Đây là dấu hiệu cổ điển của "Monitoring Blind Spot" — nguyên nhân có thể: metric đang đo trung bình (average) che khuất vấn đề chỉ ảnh hưởng một phân khúc người dùng cụ thể (ví dụ chỉ region/khách hàng cụ thể bị chậm, nhưng trung bình toàn hệ thống vẫn ổn); vấn đề nằm ở tầng client-side (frontend rendering, network người dùng) mà monitoring backend không đo được — cần bổ sung Real User Monitoring (RUM); hoặc metric đo sai điểm (đo tại tầng service nhưng vấn đề thực tế nằm ở một downstream dependency không được instrument, như một API bên thứ ba chậm mà không có trace/metric riêng theo dõi) — cần rà soát lại toàn bộ request path để tìm điểm mù chưa được quan sát.

**49. Thiết kế chiến lược đảm bảo tính nhất quán của Trace ID/Correlation ID xuyên suốt một hệ thống có cả service đồng bộ (HTTP) và bất đồng bộ (message queue).**
Với giao tiếp đồng bộ (HTTP), Trace ID truyền qua HTTP header (chuẩn W3C Trace Context) khá đơn giản qua các middleware tự động propagate. Với giao tiếp bất đồng bộ qua message queue (SQS/Kafka), cần chủ động nhúng Trace ID vào metadata/attribute của message khi publish (không tự động như HTTP), và consumer phải chủ động đọc lại Trace ID đó để tiếp tục "nối" trace khi xử lý message — cần chuẩn hoá convention rõ ràng (ví dụ luôn dùng field `trace_id` trong message envelope) và đảm bảo mọi producer/consumer trong tổ chức tuân thủ nhất quán để trace không bị "đứt gãy" giữa các đoạn đồng bộ và bất đồng bộ.

**50. Một hệ thống production gặp sự cố nghiêm trọng (outage) nhưng đội ngũ mất 45 phút mới xác định được nguyên nhân gốc dù có đầy đủ Prometheus/Grafana/ELK — bạn sẽ đề xuất cải thiện gì cho lần sau?**
Phân tích và cải thiện theo nhiều hướng: (1) Xem lại liệu Golden Signals có được đặt ở dashboard "first responder" dễ tìm nhất không, hay bị chôn giữa hàng chục dashboard không liên quan; (2) Kiểm tra có Trace ID/Correlation ID xuyên suốt đủ để nhanh chóng khoanh vùng service gây lỗi trong request chain hay không, nếu thiếu cần bổ sung distributed tracing; (3) Rà soát Runbook cho loại sự cố này đã tồn tại và đủ chi tiết chưa, nếu chưa cần viết bổ sung ngay sau sự cố; (4) Đánh giá liệu alert ban đầu có đủ ngữ cảnh (đúng service, đúng thời điểm bắt đầu) hay chỉ báo chung chung khiến đội ngũ phải tự mò; (5) Tổ chức buổi Post-Incident Review (blameless) ghi nhận timeline chi tiết (khi nào phát hiện, khi nào từng giả thuyết được kiểm tra và loại bỏ) để xác định chính xác bước nào tốn thời gian nhất và cải thiện cụ thể bước đó cho lần sau — mục tiêu giảm cả MTTD lẫn MTTR một cách có hệ thống, không chỉ dựa vào việc "cẩn thận hơn" ở lần tới.

---

## Tổng kết & Gợi ý sử dụng

- **Với ứng viên Junior/Mid-level**: tập trung vào nhóm câu hỏi Cơ bản và Trung cấp của từng mảng để đánh giá nền tảng kiến thức và khả năng vận hành thực tế hàng ngày.
- **Với ứng viên Senior/Lead**: ưu tiên nhóm câu hỏi Nâng cao — đây là các câu hỏi tình huống/thiết kế hệ thống giúp đánh giá tư duy kiến trúc, khả năng xử lý sự cố thực chiến, và kinh nghiệm đưa ra trade-off hợp lý chứ không chỉ thuộc lòng khái niệm.
- **Gợi ý phỏng vấn**: nên chọn ngẫu nhiên 8-10 câu mỗi mảng (ưu tiên trải đều 3 mức độ) thay vì hỏi hết 50 câu trong một buổi, đồng thời khuyến khích ứng viên kể lại tình huống thực tế đã gặp thay vì chỉ trả lời lý thuyết thuần tuý — đặc biệt với nhóm câu hỏi Nâng cao, câu trả lời kèm ví dụ thực chiến cụ thể (dù không hoàn hảo) thường có giá trị đánh giá cao hơn một câu trả lời lý thuyết hoàn hảo nhưng thiếu trải nghiệm thực tế.
