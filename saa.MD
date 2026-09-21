# 📘 Tài liệu ôn thi AWS Certified Solutions Architect – Associate (SAA-C03)

![AWS](https://img.shields.io/badge/AWS-SAA--C03-FF9900?logo=amazonaws&logoColor=white) ![Language](https://img.shields.io/badge/Ngôn%20ngữ-Tiếng%20Việt-red) ![Docs](https://img.shields.io/badge/Nội%20dung-16%20phần-blue)

Cẩm nang tổng hợp kiến thức chi tiết để ôn tập và sẵn sàng cho kỳ thi **AWS Certified Solutions Architect – Associate (SAA-C03)**: lý thuyết cốt lõi, bảng so sánh dịch vụ, các con số cần thuộc, mẫu kiến trúc điển hình, ngân hàng câu hỏi có giải thích và chiến lược làm bài.

| | |
|---|---|
| **Mã kỳ thi** | SAA-C03 |
| **Số câu hỏi** | 65 câu (50 tính điểm) |
| **Thời gian** | 130 phút |
| **Điểm đạt** | 720 / 1000 |
| **Lệ phí** | 150 USD |
| **Hiệu lực** | 3 năm |

### 🗺️ Lộ trình ôn tập gợi ý

| Tuần | Nội dung | Phần |
|---|---|---|
| 1 | Nền tảng: hạ tầng toàn cầu, IAM, bảo mật | I – III |
| 2 | Compute và Storage | IV – V |
| 3 | Database và Networking | VI – VII |
| 4 | Tích hợp ứng dụng, phân tích, vận hành | VIII – X |
| 5 | Kiến trúc, DR, chi phí | XI – XIII |
| 6 | Luyện đề + ôn bảng so sánh & con số | XIV – XVI |

---

<details>
<summary><b>📚 MỤC LỤC — bấm để mở</b></summary>

- [PHẦN I. TỔNG QUAN KỲ THI SAA-C03](#phần-i-tổng-quan-kỳ-thi-saa-c03)
  - [1.1. Thông tin kỳ thi](#11-thông-tin-kỳ-thi)
  - [1.2. Bốn miền kiến thức (Domains) và trọng số](#12-bốn-miền-kiến-thức-domains-và-trọng-số)
  - [1.3. Chiến lược làm bài](#13-chiến-lược-làm-bài)
  - [1.4. Các "từ khóa – đáp án" hay xuất hiện](#14-các-từ-khóa--đáp-án-hay-xuất-hiện)
- [PHẦN II. HẠ TẦNG TOÀN CẦU VÀ NỀN TẢNG AWS](#phần-ii-hạ-tầng-toàn-cầu-và-nền-tảng-aws)
  - [2.1. Kiến trúc hạ tầng toàn cầu](#21-kiến-trúc-hạ-tầng-toàn-cầu)
  - [2.2. Mô hình trách nhiệm chung (Shared Responsibility Model)](#22-mô-hình-trách-nhiệm-chung-shared-responsibility-model)
- [PHẦN III. BẢO MẬT, DANH TÍNH VÀ QUẢN TRỊ](#phần-iii-bảo-mật-danh-tính-và-quản-trị)
  - [3.1. IAM – Identity and Access Management](#31-iam--identity-and-access-management)
    - [3.1.1. Các thành phần cốt lõi](#311-các-thành-phần-cốt-lõi)
    - [3.1.2. Cấu trúc một IAM Policy](#312-cấu-trúc-một-iam-policy)
    - [3.1.3. Thuật toán đánh giá quyền (rất hay thi)](#313-thuật-toán-đánh-giá-quyền-rất-hay-thi)
    - [3.1.4. Identity-based vs Resource-based policy](#314-identity-based-vs-resource-based-policy)
    - [3.1.5. Permission Boundary vs SCP](#315-permission-boundary-vs-scp)
    - [3.1.6. IAM Best Practices (rất hay ra thi)](#316-iam-best-practices-rất-hay-ra-thi)
  - [3.2. AWS STS – Security Token Service](#32-aws-sts--security-token-service)
  - [3.3. AWS Organizations](#33-aws-organizations)
  - [3.4. AWS Control Tower](#34-aws-control-tower)
  - [3.5. AWS IAM Identity Center (AWS SSO)](#35-aws-iam-identity-center-aws-sso)
  - [3.6. Amazon Cognito](#36-amazon-cognito)
  - [3.7. AWS Directory Service](#37-aws-directory-service)
  - [3.8. AWS KMS – Key Management Service](#38-aws-kms--key-management-service)
  - [3.9. Quản lý bí mật và tham số](#39-quản-lý-bí-mật-và-tham-số)
  - [3.10. Bảo vệ tầng ứng dụng và mạng](#310-bảo-vệ-tầng-ứng-dụng-và-mạng)
  - [3.11. Phát hiện mối đe dọa và tuân thủ](#311-phát-hiện-mối-đe-dọa-và-tuân-thủ)
    - [Chi tiết cần nhớ về CloudTrail](#chi-tiết-cần-nhớ-về-cloudtrail)
- [PHẦN IV. ĐIỆN TOÁN (COMPUTE)](#phần-iv-điện-toán-compute)
  - [4.1. Amazon EC2 – Elastic Compute Cloud](#41-amazon-ec2--elastic-compute-cloud)
    - [4.1.1. Họ instance và cách đọc tên](#411-họ-instance-và-cách-đọc-tên)
    - [4.1.2. Các mô hình mua (Purchase Options) – trọng tâm Domain 4](#412-các-mô-hình-mua-purchase-options--trọng-tâm-domain-4)
    - [4.1.3. Placement Groups](#413-placement-groups)
    - [4.1.4. AMI, User Data, Instance Metadata](#414-ami-user-data-instance-metadata)
    - [4.1.5. Vòng đời và trạng thái](#415-vòng-đời-và-trạng-thái)
    - [4.1.6. Elastic IP và ENI](#416-elastic-ip-và-eni)
  - [4.2. Auto Scaling Group (ASG)](#42-auto-scaling-group-asg)
    - [4.2.1. Thành phần](#421-thành-phần)
    - [4.2.2. Các chính sách scaling](#422-các-chính-sách-scaling)
    - [4.2.3. Lifecycle Hooks và Termination Policy](#423-lifecycle-hooks-và-termination-policy)
  - [4.3. Elastic Load Balancing (ELB)](#43-elastic-load-balancing-elb)
    - [4.3.1. So sánh 4 loại Load Balancer](#431-so-sánh-4-loại-load-balancer)
    - [4.3.2. Các khái niệm quan trọng](#432-các-khái-niệm-quan-trọng)
  - [4.4. AWS Lambda](#44-aws-lambda)
    - [4.4.1. Đặc điểm và giới hạn (nhớ con số!)](#441-đặc-điểm-và-giới-hạn-nhớ-con-số)
    - [4.4.2. Các khái niệm cốt lõi](#442-các-khái-niệm-cốt-lõi)
  - [4.5. Container trên AWS](#45-container-trên-aws)
    - [4.5.1. Amazon ECS – Elastic Container Service](#451-amazon-ecs--elastic-container-service)
    - [4.5.2. Amazon EKS – Elastic Kubernetes Service](#452-amazon-eks--elastic-kubernetes-service)
    - [4.5.3. Amazon ECR – Elastic Container Registry](#453-amazon-ecr--elastic-container-registry)
    - [4.5.4. Các lựa chọn compute khác](#454-các-lựa-chọn-compute-khác)
    - [4.5.5. Chiến lược triển khai (Deployment Strategies)](#455-chiến-lược-triển-khai-deployment-strategies)
- [PHẦN V. LƯU TRỮ (STORAGE)](#phần-v-lưu-trữ-storage)
  - [5.1. Ba loại hình lưu trữ](#51-ba-loại-hình-lưu-trữ)
  - [5.2. Amazon EBS – Elastic Block Store](#52-amazon-ebs--elastic-block-store)
    - [5.2.1. Đặc tính chung](#521-đặc-tính-chung)
    - [5.2.2. Các loại EBS Volume (bảng cần thuộc)](#522-các-loại-ebs-volume-bảng-cần-thuộc)
    - [5.2.3. EBS Snapshot](#523-ebs-snapshot)
    - [5.2.4. Mã hóa EBS](#524-mã-hóa-ebs)
    - [5.2.5. Instance Store](#525-instance-store)
  - [5.3. Amazon EFS – Elastic File System](#53-amazon-efs--elastic-file-system)
  - [5.4. Amazon FSx](#54-amazon-fsx)
  - [5.5. Amazon S3 – Simple Storage Service](#55-amazon-s3--simple-storage-service)
    - [5.5.1. Nguyên tắc nền tảng](#551-nguyên-tắc-nền-tảng)
    - [5.5.2. Các lớp lưu trữ (Storage Classes) – bảng phải thuộc](#552-các-lớp-lưu-trữ-storage-classes--bảng-phải-thuộc)
    - [5.5.3. Lifecycle Policy](#553-lifecycle-policy)
    - [5.5.4. Versioning, MFA Delete, Object Lock](#554-versioning-mfa-delete-object-lock)
    - [5.5.5. Kiểm soát truy cập S3](#555-kiểm-soát-truy-cập-s3)
    - [5.5.6. Mã hóa S3](#556-mã-hóa-s3)
    - [5.5.7. Replication (nhân bản)](#557-replication-nhân-bản)
    - [5.5.8. Các tính năng S3 khác hay ra thi](#558-các-tính-năng-s3-khác-hay-ra-thi)
  - [5.6. Di chuyển và chuyển dữ liệu](#56-di-chuyển-và-chuyển-dữ-liệu)
    - [Storage Gateway – bốn loại](#storage-gateway--bốn-loại)
  - [5.7. AWS Backup](#57-aws-backup)
- [PHẦN VI. CƠ SỞ DỮ LIỆU](#phần-vi-cơ-sở-dữ-liệu)
  - [6.1. Bản đồ chọn cơ sở dữ liệu (rất quan trọng)](#61-bản-đồ-chọn-cơ-sở-dữ-liệu-rất-quan-trọng)
  - [6.2. Amazon RDS](#62-amazon-rds)
    - [6.2.1. Multi-AZ vs Read Replica – so sánh kinh điển](#621-multi-az-vs-read-replica--so-sánh-kinh-điển)
    - [6.2.2. Backup và phục hồi](#622-backup-và-phục-hồi)
    - [6.2.3. Bảo mật RDS](#623-bảo-mật-rds)
    - [6.2.4. RDS Proxy](#624-rds-proxy)
    - [6.2.5. Giám sát và bảo trì](#625-giám-sát-và-bảo-trì)
  - [6.3. Amazon Aurora](#63-amazon-aurora)
    - [6.3.1. Kiến trúc](#631-kiến-trúc)
    - [6.3.2. Các Endpoint của Aurora](#632-các-endpoint-của-aurora)
    - [6.3.3. Các biến thể Aurora](#633-các-biến-thể-aurora)
  - [6.4. Amazon DynamoDB](#64-amazon-dynamodb)
    - [6.4.1. Nền tảng](#641-nền-tảng)
    - [6.4.2. Chế độ dung lượng (Capacity Mode)](#642-chế-độ-dung-lượng-capacity-mode)
    - [6.4.3. Index](#643-index)
    - [6.4.4. Các tính năng quan trọng khác](#644-các-tính-năng-quan-trọng-khác)
  - [6.5. Amazon ElastiCache](#65-amazon-elasticache)
  - [6.6. Amazon Redshift](#66-amazon-redshift)
  - [6.7. Các cơ sở dữ liệu chuyên biệt](#67-các-cơ-sở-dữ-liệu-chuyên-biệt)
  - [6.8. Di chuyển cơ sở dữ liệu](#68-di-chuyển-cơ-sở-dữ-liệu)
- [PHẦN VII. MẠNG VÀ PHÂN PHỐI NỘI DUNG](#phần-vii-mạng-và-phân-phối-nội-dung)
  - [7.1. Amazon VPC – Virtual Private Cloud](#71-amazon-vpc--virtual-private-cloud)
    - [7.1.1. Các thành phần nền tảng](#711-các-thành-phần-nền-tảng)
    - [7.1.2. Security Group vs Network ACL – so sánh phải thuộc](#712-security-group-vs-network-acl--so-sánh-phải-thuộc)
    - [7.1.3. VPC Endpoints – truy cập riêng tư tới dịch vụ AWS](#713-vpc-endpoints--truy-cập-riêng-tư-tới-dịch-vụ-aws)
    - [7.1.4. Kết nối giữa các VPC](#714-kết-nối-giữa-các-vpc)
    - [7.1.5. Kết nối lai (Hybrid)](#715-kết-nối-lai-hybrid)
    - [7.1.6. DNS trong VPC](#716-dns-trong-vpc)
    - [7.1.7. Giám sát mạng](#717-giám-sát-mạng)
    - [7.1.8. Chi phí mạng cần nhớ](#718-chi-phí-mạng-cần-nhớ)
  - [7.2. Amazon Route 53](#72-amazon-route-53)
    - [7.2.1. Tổng quan](#721-tổng-quan)
    - [7.2.2. Record Types và Alias Record](#722-record-types-và-alias-record)
    - [7.2.3. Bảy chính sách định tuyến (Routing Policies)](#723-bảy-chính-sách-định-tuyến-routing-policies)
    - [7.2.4. Health Checks](#724-health-checks)
  - [7.3. Amazon CloudFront](#73-amazon-cloudfront)
    - [7.3.1. Cơ chế](#731-cơ-chế)
    - [7.3.2. Bảo mật CloudFront](#732-bảo-mật-cloudfront)
    - [7.3.3. Caching](#733-caching)
    - [7.3.4. CloudFront Functions vs Lambda@Edge](#734-cloudfront-functions-vs-lambdaedge)
  - [7.4. AWS Global Accelerator](#74-aws-global-accelerator)
- [PHẦN VIII. TÍCH HỢP ỨNG DỤNG VÀ KIẾN TRÚC PHI TẬP TRUNG](#phần-viii-tích-hợp-ứng-dụng-và-kiến-trúc-phi-tập-trung)
  - [8.1. Amazon SQS – Simple Queue Service](#81-amazon-sqs--simple-queue-service)
    - [8.1.1. Hai loại hàng đợi](#811-hai-loại-hàng-đợi)
    - [8.1.2. Các tham số phải nhớ](#812-các-tham-số-phải-nhớ)
    - [8.1.3. Dead Letter Queue (DLQ)](#813-dead-letter-queue-dlq)
    - [8.1.4. Mẫu kiến trúc với SQS](#814-mẫu-kiến-trúc-với-sqs)
  - [8.2. Amazon SNS – Simple Notification Service](#82-amazon-sns--simple-notification-service)
  - [8.3. Amazon EventBridge (trước là CloudWatch Events)](#83-amazon-eventbridge-trước-là-cloudwatch-events)
  - [8.4. AWS Step Functions](#84-aws-step-functions)
  - [8.5. Amazon API Gateway](#85-amazon-api-gateway)
  - [8.6. Streaming dữ liệu – Amazon Kinesis](#86-streaming-dữ-liệu--amazon-kinesis)
  - [8.7. Amazon MQ và AppSync](#87-amazon-mq-và-appsync)
- [PHẦN IX. PHÂN TÍCH DỮ LIỆU](#phần-ix-phân-tích-dữ-liệu)
- [PHẦN X. GIÁM SÁT, VẬN HÀNH VÀ TỰ ĐỘNG HÓA](#phần-x-giám-sát-vận-hành-và-tự-động-hóa)
  - [10.1. Amazon CloudWatch](#101-amazon-cloudwatch)
    - [10.1.1. Metrics](#1011-metrics)
    - [10.1.2. Logs](#1012-logs)
    - [10.1.3. Alarms](#1013-alarms)
    - [10.1.4. Các thành phần khác](#1014-các-thành-phần-khác)
  - [10.2. AWS X-Ray](#102-aws-x-ray)
  - [10.3. AWS Systems Manager (SSM)](#103-aws-systems-manager-ssm)
  - [10.4. Infrastructure as Code](#104-infrastructure-as-code)
  - [10.5. CI/CD trên AWS](#105-cicd-trên-aws)
  - [10.6. Hỗ trợ và tối ưu](#106-hỗ-trợ-và-tối-ưu)
- [PHẦN XI. WELL-ARCHITECTED FRAMEWORK VÀ THIẾT KẾ KIẾN TRÚC](#phần-xi-well-architected-framework-và-thiết-kế-kiến-trúc)
  - [11.1. Sáu trụ cột (Six Pillars)](#111-sáu-trụ-cột-six-pillars)
  - [11.2. Nguyên tắc thiết kế cần khắc cốt ghi tâm](#112-nguyên-tắc-thiết-kế-cần-khắc-cốt-ghi-tâm)
  - [11.3. Chiến lược Disaster Recovery – RTO và RPO](#113-chiến-lược-disaster-recovery--rto-và-rpo)
- [PHẦN XII. TỐI ƯU CHI PHÍ](#phần-xii-tối-ưu-chi-phí)
  - [12.1. Công cụ quản lý chi phí](#121-công-cụ-quản-lý-chi-phí)
  - [12.2. Danh sách kiểm tra tối ưu chi phí (áp dụng trong đề thi)](#122-danh-sách-kiểm-tra-tối-ưu-chi-phí-áp-dụng-trong-đề-thi)
- [PHẦN XIII. CÁC MẪU KIẾN TRÚC ĐIỂN HÌNH TRONG ĐỀ THI](#phần-xiii-các-mẫu-kiến-trúc-điển-hình-trong-đề-thi)
  - [13.1. Web application 3 tầng có tính sẵn sàng cao](#131-web-application-3-tầng-có-tính-sẵn-sàng-cao)
  - [13.2. Kiến trúc serverless](#132-kiến-trúc-serverless)
  - [13.3. Xử lý ảnh/video khi upload](#133-xử-lý-ảnhvideo-khi-upload)
  - [13.4. Phân tích log tập trung](#134-phân-tích-log-tập-trung)
  - [13.5. Hybrid – mở rộng data center lên AWS](#135-hybrid--mở-rộng-data-center-lên-aws)
  - [13.6. Ứng dụng toàn cầu độ trễ thấp](#136-ứng-dụng-toàn-cầu-độ-trễ-thấp)
- [PHẦN XIV. NGÂN HÀNG CÂU HỎI ÔN TẬP CÓ GIẢI THÍCH](#phần-xiv-ngân-hàng-câu-hỏi-ôn-tập-có-giải-thích)
- [PHẦN XV. BẢNG SO SÁNH NHANH – ÔN TRƯỚC GIỜ THI](#phần-xv-bảng-so-sánh-nhanh--ôn-trước-giờ-thi)
  - [15.1. Các cặp dễ nhầm](#151-các-cặp-dễ-nhầm)
  - [15.2. Những con số phải nhớ](#152-những-con-số-phải-nhớ)
- [PHẦN XVI. CHECKLIST ÔN TẬP CUỐI CÙNG](#phần-xvi-checklist-ôn-tập-cuối-cùng)
  - [16.1. Tuần cuối trước khi thi](#161-tuần-cuối-trước-khi-thi)
  - [16.2. Kiểm tra mức độ sẵn sàng – bạn có tự tin trả lời được không?](#162-kiểm-tra-mức-độ-sẵn-sàng--bạn-có-tự-tin-trả-lời-được-không)
  - [16.3. Trong phòng thi](#163-trong-phòng-thi)
  - [16.4. Tài nguyên ôn tập gợi ý](#164-tài-nguyên-ôn-tập-gợi-ý)

</details>

---

# PHẦN I. TỔNG QUAN KỲ THI SAA-C03

## 1.1. Thông tin kỳ thi

| Hạng mục | Chi tiết |
|---|---|
| Mã kỳ thi | SAA-C03 (AWS Certified Solutions Architect – Associate) |
| Số câu hỏi | 65 câu (50 câu tính điểm + 15 câu thử nghiệm không tính điểm) |
| Thời gian | 130 phút (có thể xin thêm 30 phút nếu tiếng Anh không phải ngôn ngữ mẹ đẻ – đăng ký **trước** khi book lịch) |
| Dạng câu hỏi | Multiple choice (1 đáp án đúng / 4 lựa chọn) và Multiple response (2+ đáp án đúng / 5-6 lựa chọn) |
| Điểm đạt | 720 / 1000 (thang điểm scaled) |
| Lệ phí | 150 USD |
| Hiệu lực | 3 năm |
| Hình thức | Pearson VUE test center hoặc OnVUE online proctored |

## 1.2. Bốn miền kiến thức (Domains) và trọng số

| Domain | Tên miền | Trọng số |
|---|---|---|
| Domain 1 | Design Secure Architectures – Thiết kế kiến trúc bảo mật | 30% |
| Domain 2 | Design Resilient Architectures – Thiết kế kiến trúc bền bỉ (HA/DR) | 26% |
| Domain 3 | Design High-Performing Architectures – Thiết kế kiến trúc hiệu năng cao | 24% |
| Domain 4 | Design Cost-Optimized Architectures – Thiết kế kiến trúc tối ưu chi phí | 20% |

**Nhận xét quan trọng:** Đề thi SAA-C03 không kiểm tra bạn có thuộc lòng cú pháp CLI hay không. Nó kiểm tra khả năng **chọn dịch vụ phù hợp nhất** cho một tình huống kinh doanh có ràng buộc. Với mỗi câu hỏi, hãy xác định ràng buộc chủ đạo: *bảo mật – độ sẵn sàng – hiệu năng – chi phí – ít thao tác vận hành nhất (least operational overhead)*.

## 1.3. Chiến lược làm bài

1. **Đọc câu hỏi từ dưới lên.** Câu cuối cùng (câu hỏi thật sự) thường chứa từ khóa quyết định: "MOST cost-effective", "LEAST operational overhead", "MOST secure", "HIGHEST availability".
2. **Loại trừ trước, chọn sau.** Thường có 2 đáp án sai hiển nhiên (dịch vụ không tồn tại, dịch vụ dùng sai mục đích) và 2 đáp án "đúng về kỹ thuật" nhưng chỉ 1 đáp án tối ưu theo tiêu chí câu hỏi.
3. **Ưu tiên managed service.** Nếu câu hỏi nói "least operational overhead" / "minimal management", hãy ưu tiên serverless và managed: Lambda > EC2, Aurora Serverless > RDS tự quản, Fargate > EC2 launch type, S3 > EBS tự dựng file server.
4. **Cẩn thận với "cost-effective".** Đáp án rẻ nhất phải vẫn đáp ứng yêu cầu kỹ thuật. Nếu yêu cầu là truy xuất trong mili-giây thì Glacier bị loại dù rẻ hơn.
5. **Quản lý thời gian:** 130 phút / 65 câu = **2 phút/câu**. Nếu bí quá 90 giây, hãy đánh dấu (flag) và quay lại sau. Dành 15-20 phút cuối để rà soát các câu đã flag.
6. **Không bỏ trống câu nào.** Không bị trừ điểm khi trả lời sai.

## 1.4. Các "từ khóa – đáp án" hay xuất hiện

| Từ khóa trong đề | Dịch vụ / giải pháp thường đúng |
|---|---|
| Decouple, buffer, spike traffic | SQS |
| Fan-out, pub/sub, thông báo nhiều nơi | SNS (+ SQS fanout) |
| Event-driven, filter theo pattern, schedule | EventBridge |
| Orchestration, workflow nhiều bước, retry logic | Step Functions |
| Real-time streaming, ordering theo shard, replay | Kinesis Data Streams |
| Nạp streaming vào S3/Redshift/OpenSearch, near real-time | Kinesis Data Firehose |
| Truy vấn SQL ad-hoc trực tiếp trên S3 | Athena |
| Data warehouse, OLAP, BI, petabyte | Redshift |
| Cache DB, giảm latency đọc, session store | ElastiCache (Redis) |
| Cache DynamoDB microsecond | DAX |
| Cache nội dung tĩnh cho người dùng toàn cầu | CloudFront |
| Tăng tốc TCP/UDP, IP tĩnh, non-HTTP | Global Accelerator |
| Chia sẻ file NFS giữa nhiều EC2 Linux | EFS |
| Chia sẻ file SMB cho Windows, AD | FSx for Windows File Server |
| HPC, machine learning, throughput cực cao | FSx for Lustre |
| Kết nối on-premises băng thông ổn định, riêng tư | Direct Connect |
| Kết nối on-premises nhanh, rẻ, qua Internet | Site-to-Site VPN |
| Hàng trăm VPC kết nối nhau | Transit Gateway |
| Truy cập S3/DynamoDB không qua Internet | VPC Gateway Endpoint |
| Truy cập dịch vụ AWS/bên thứ ba riêng tư | Interface Endpoint (PrivateLink) |
| Di chuyển petabyte dữ liệu offline | Snowball / Snowmobile |
| Đồng bộ on-prem ↔ AWS qua mạng | DataSync |
| Mở rộng dung lượng on-prem lên cloud | Storage Gateway |
| Xoay vòng (rotate) mật khẩu DB tự động | Secrets Manager |
| Lưu tham số cấu hình, miễn phí | SSM Parameter Store |
| Chống DDoS lớp 7, SQLi, XSS | AWS WAF |
| Chống DDoS lớp 3/4 nâng cao + hỗ trợ 24/7 | Shield Advanced |
| Phát hiện hành vi bất thường từ log | GuardDuty |
| Quét lỗ hổng CVE trên EC2/ECR/Lambda | Inspector |
| Tìm dữ liệu nhạy cảm (PII) trong S3 | Macie |
| Kiểm tra tuân thủ cấu hình tài nguyên | AWS Config |
| Ai đã làm gì trên tài khoản (audit API) | CloudTrail |

# PHẦN II. HẠ TẦNG TOÀN CẦU VÀ NỀN TẢNG AWS

## 2.1. Kiến trúc hạ tầng toàn cầu

- **Region (Vùng):** Một khu vực địa lý riêng biệt (ví dụ `ap-southeast-1` Singapore, `us-east-1` N. Virginia). Mỗi Region gồm **tối thiểu 3 Availability Zone**. Region hoàn toàn độc lập – dữ liệu **không** tự động rời khỏi Region trừ khi bạn cấu hình (replication). Tiêu chí chọn Region: **tuân thủ pháp lý (data sovereignty) → độ trễ tới người dùng → dịch vụ khả dụng → giá**.
- **Availability Zone (AZ):** Một hoặc nhiều data center rời nhau, có nguồn điện, làm mát, mạng riêng. Các AZ trong cùng Region cách nhau hàng chục km nhưng nối bằng đường truyền riêng, độ trễ **dưới 10ms (single-digit millisecond)**. Đây là đơn vị cô lập lỗi cơ bản: **Muốn HA thì phải triển khai qua nhiều AZ**.
- **Local Zones:** Mở rộng của Region đặt gần các thành phố lớn, cho độ trễ **một chữ số mili-giây** cho ứng dụng nhạy độ trễ (game, media rendering).
- **Wavelength Zones:** Nhúng hạ tầng AWS vào mạng 5G của nhà mạng, phục vụ ứng dụng di động độ trễ cực thấp.
- **Outposts:** Rack phần cứng AWS đặt tại data center của khách hàng – dùng khi có yêu cầu **dữ liệu phải nằm tại chỗ (data residency)** nhưng vẫn muốn API AWS.
- **Edge Locations / Points of Presence (PoP):** Hơn 400 điểm trên toàn cầu, phục vụ CloudFront, Route 53, Global Accelerator, S3 Transfer Acceleration.

**Điểm thi thường gặp:** Sự cố "mất một AZ" → kiến trúc Multi-AZ vẫn sống. Sự cố "mất cả Region" → cần Multi-Region (DR). Yêu cầu "dữ liệu không được rời khỏi quốc gia" → chọn Region trong nước hoặc Outposts/Local Zone.

## 2.2. Mô hình trách nhiệm chung (Shared Responsibility Model)

| AWS chịu trách nhiệm ("Security **OF** the Cloud") | Khách hàng chịu trách nhiệm ("Security **IN** the Cloud") |
|---|---|
| Phần cứng, data center, điện, làm cool | Dữ liệu của khách hàng |
| Hạ tầng mạng vật lý, ảo hóa (hypervisor) | Phân quyền IAM, mật khẩu, MFA |
| Vá lỗi hệ điều hành của **managed service** (RDS, Lambda, DynamoDB) | Vá lỗi hệ điều hành **EC2**, cài đặt phần mềm |
| Mã hóa hạ tầng, độ bền của S3 (11 số 9) | Cấu hình mã hóa dữ liệu, phân loại dữ liệu |
| Vận hành phần cứng lưu trữ | Cấu hình Security Group, NACL, firewall |

Nguyên tắc rút gọn: **Càng "managed" thì AWS gánh càng nhiều.** EC2 (IaaS) → khách hàng lo hầu hết OS trở lên. Lambda/S3 (Serverless/SaaS) → khách hàng chỉ lo dữ liệu và quyền truy cập.

# PHẦN III. BẢO MẬT, DANH TÍNH VÀ QUẢN TRỊ

## 3.1. IAM – Identity and Access Management

IAM là dịch vụ **global** (không gắn Region), **miễn phí**, dùng để kiểm soát *ai* được làm *gì* trên *tài nguyên nào*.

### 3.1.1. Các thành phần cốt lõi

- **Root user:** Tài khoản gốc tạo ra khi mở AWS account. Có toàn quyền tuyệt đối, **không thể giới hạn bằng IAM policy**. Thực hành tốt: bật MFA, xóa access key của root, khóa lại và **không dùng cho công việc hằng ngày**. Chỉ một số tác vụ bắt buộc root: đổi tên/đóng tài khoản, đổi gói hỗ trợ, khôi phục quyền IAM bị mất, đăng ký làm reseller, khôi phục S3 bucket policy bị khóa nhầm, bật MFA delete trên S3.
- **IAM User:** Danh tính lâu dài cho con người hoặc ứng dụng cũ. Có thể có mật khẩu (console) và/hoặc **access key ID + secret access key** (API/CLI). Tối đa **2 access key** mỗi user (để rotate). Giới hạn 5.000 user/account.
- **IAM Group:** Tập hợp user để gán policy chung. **Group không thể lồng nhau**, và **group không phải là identity** (không thể gán group vào Resource-based policy, group không có credentials).
- **IAM Role:** Danh tính **tạm thời**, không có credentials cố định – được "assume" để nhận **temporary security credentials** từ STS. Đây là cách được khuyến nghị cho: EC2 truy cập S3, Lambda truy cập DynamoDB, cross-account access, liên kết danh tính (federation).
- **Policy:** Tài liệu JSON định nghĩa quyền.

### 3.1.2. Cấu trúc một IAM Policy

Một policy JSON gồm các phần tử: `Version` (luôn dùng `2012-10-17`), `Statement` (mảng), và trong mỗi statement có `Sid`, `Effect` (Allow/Deny), `Principal` (chỉ có trong resource-based policy), `Action`, `Resource`, `Condition`.

Các điều kiện (`Condition`) thường gặp trong đề thi: `aws:SourceIp` (giới hạn dải IP), `aws:RequestedRegion`, `aws:MultiFactorAuthPresent` (bắt buộc MFA), `aws:PrincipalOrgID` (chỉ cho phép account trong Organization), `s3:x-amz-server-side-encryption` (bắt buộc upload có mã hóa), `aws:SecureTransport` (bắt buộc HTTPS), `aws:PrincipalTag` / `aws:ResourceTag` (ABAC).

### 3.1.3. Thuật toán đánh giá quyền (rất hay thi)

Thứ tự đánh giá khi một request tới AWS:

1. **Explicit DENY** ở bất kỳ đâu → **Từ chối ngay lập tức**, không cần xét tiếp. Deny luôn thắng.
2. **Organizations SCP** – nếu action không được SCP cho phép → từ chối.
3. **Resource-based policy** – Allow ở đây có thể đủ (cùng account).
4. **IAM permission boundary** – nếu vượt boundary → từ chối.
5. **Session policy** (nếu assume role có session policy).
6. **Identity-based policy** – Allow.
7. Mặc định: **Implicit DENY** (không có Allow nào → từ chối).

Ghi nhớ: **Mặc định là từ chối; cần một Allow rõ ràng; một Deny rõ ràng ghi đè mọi Allow.**

### 3.1.4. Identity-based vs Resource-based policy

| Tiêu chí | Identity-based policy | Resource-based policy |
|---|---|---|
| Gắn vào | User, Group, Role | Tài nguyên (S3 bucket, SQS queue, SNS topic, KMS key, Lambda, ECR, API GW, Secrets Manager) |
| Có `Principal` | Không | **Có** (bắt buộc) |
| Cross-account | Cần assume role (đổi danh tính) | Cho phép trực tiếp, **principal giữ nguyên danh tính** |
| Ví dụ dùng | Cho phép nhóm Dev đọc S3 | Cho bucket A chấp nhận account B ghi vào |

**Cross-account với S3:** Cần Allow ở **cả hai phía** – bucket policy của account chủ bucket **và** IAM policy của user bên account kia.

### 3.1.5. Permission Boundary vs SCP

- **Permission Boundary:** Gán cho **một IAM user/role**, đặt trần quyền tối đa. Quyền thực tế = giao của (Identity policy ∩ Boundary). Dùng để **ủy quyền tạo user cho developer mà không sợ họ tự nâng quyền** (privilege escalation).
- **SCP (Service Control Policy):** Gán cho **OU hoặc account** trong AWS Organizations. SCP **không cấp quyền**, chỉ đặt trần. SCP **áp dụng cả cho root user** của member account (đây là cách duy nhất giới hạn root). SCP **không áp dụng** cho management account, service-linked roles.

### 3.1.6. IAM Best Practices (rất hay ra thi)

- Bật MFA cho root và cho mọi user có quyền cao.
- **Least privilege** – chỉ cấp quyền tối thiểu cần thiết. Dùng **IAM Access Analyzer** để tìm quyền thừa và tài nguyên bị chia sẻ ra ngoài; dùng **Last Accessed information** để cắt bớt quyền không dùng.
- **Dùng Role thay vì access key** cho EC2/Lambda/ECS. Không bao giờ hardcode access key trong code hoặc AMI.
- Rotate credentials định kỳ; dùng **IAM Credential Report** (CSV toàn bộ user, trạng thái MFA, tuổi access key).
- Ưu tiên **IAM Identity Center (AWS SSO)** cho tổ chức nhiều tài khoản, thay vì tạo IAM user rải rác.

## 3.2. AWS STS – Security Token Service

Cung cấp credentials tạm thời (từ 15 phút tới 12 giờ; role chaining tối đa 1 giờ). Các API chính:

- `AssumeRole` – assume role trong cùng account hoặc cross-account.
- `AssumeRoleWithSAML` – liên kết với AD/ADFS qua SAML 2.0.
- `AssumeRoleWithWebIdentity` – liên kết với Google/Facebook/Cognito/OIDC. **AWS khuyến nghị dùng Cognito Identity Pools thay vì gọi trực tiếp.**
- `GetSessionToken` – dùng cho MFA.

**Confused deputy problem:** Khi cấp quyền cho bên thứ ba assume role vào account của bạn, hãy dùng **ExternalId** trong trust policy để chống việc bên thứ ba bị lợi dụng truy cập nhầm tài khoản khác.

## 3.3. AWS Organizations

- Quản lý tập trung nhiều AWS account theo cây: **Root → OU (Organizational Unit) → Account**.
- **Consolidated Billing:** Một hóa đơn duy nhất; **gộp mức sử dụng để hưởng volume discount**; **chia sẻ Reserved Instances và Savings Plans** giữa các account trong tổ chức (có thể tắt tính năng chia sẻ này).
- **Hai chế độ:** *All features* (bắt buộc để dùng SCP) và *Consolidated billing only*.
- **Tag Policies** – chuẩn hóa tag; **Backup Policies** – áp chính sách backup tập trung; **AI services opt-out policies**.
- Mô hình đa tài khoản khuyến nghị: tách account theo môi trường (Prod/Dev/Test), theo phòng ban, có account riêng cho **Log Archive** và **Security/Audit**.

## 3.4. AWS Control Tower

Thiết lập môi trường multi-account "landing zone" theo best practice chỉ với vài cú click: tự dựng Organizations, Identity Center, tài khoản Log Archive và Audit, áp **Guardrails** (preventive – dùng SCP; detective – dùng AWS Config). **Account Factory** để cấp phát tài khoản mới chuẩn hóa.

## 3.5. AWS IAM Identity Center (AWS SSO)

Một điểm đăng nhập duy nhất cho tất cả AWS account trong Organization và cho các ứng dụng SaaS (Salesforce, Microsoft 365, Box). Nguồn danh tính (identity source) có thể là: thư mục nội bộ của Identity Center, Active Directory (qua AWS Managed Microsoft AD / AD Connector), hoặc IdP bên ngoài qua SAML 2.0 (Okta, Azure AD). Dùng **Permission Sets** để gán quyền cho nhóm người dùng vào từng account.

## 3.6. Amazon Cognito

Dành cho **người dùng cuối của ứng dụng** (hàng triệu user), khác với IAM dành cho người vận hành AWS.

- **User Pools:** Thư mục người dùng – đăng ký, đăng nhập, xác minh email/SĐT, MFA, quên mật khẩu, social login (Google, Facebook, Apple), SAML/OIDC. Trả về **JWT token**. Tích hợp trực tiếp với **API Gateway (Cognito Authorizer)** và **ALB**.
- **Identity Pools (Federated Identities):** Đổi token (từ User Pool hoặc IdP khác) lấy **AWS credentials tạm thời** để truy cập trực tiếp S3, DynamoDB… Hỗ trợ **guest/unauthenticated access**.

**Mẫu đề:** "Ứng dụng di động cần cho người dùng đăng nhập bằng Facebook và tải ảnh lên S3 vào thư mục riêng của từng người" → **Cognito Identity Pool + IAM policy dùng biến `${cognito-identity.amazonaws.com:sub}`**.

## 3.7. AWS Directory Service

| Dịch vụ | Mô tả | Khi nào dùng |
|---|---|---|
| **AWS Managed Microsoft AD** | AD thật (Windows Server) chạy trên AWS, hỗ trợ trust hai chiều với AD on-prem | Cần AD đầy đủ trên cloud, ứng dụng Windows, FSx, RDS SQL Server Windows Auth |
| **AD Connector** | Proxy chuyển tiếp xác thực về AD on-premises, **không lưu dữ liệu** | Muốn dùng AD on-prem, không muốn nhân bản dữ liệu lên cloud |
| **Simple AD** | Thư mục tương thích Samba, rẻ, tính năng hạn chế | Nhu cầu cơ bản (<5.000 user), không cần trust, không cần MFA |

## 3.8. AWS KMS – Key Management Service

- Quản lý khóa mã hóa, tích hợp với hầu hết dịch vụ AWS. Khóa **gắn với Region** (không tự động dùng chéo Region – muốn replicate phải dùng **Multi-Region Keys**).
- **Các loại KMS Key:**
  - **AWS Owned Key:** AWS sở hữu, bạn không thấy, miễn phí (mặc định SSE-S3).
  - **AWS Managed Key** (`aws/s3`, `aws/ebs`, `aws/rds`): Miễn phí, AWS tự xoay khóa **mỗi năm**, bạn không kiểm soát policy.
  - **Customer Managed Key (CMK):** ~1 USD/tháng + phí API. Bạn kiểm soát **key policy**, bật/tắt, **xoay khóa tự động hằng năm**, audit qua CloudTrail. Dùng khi cần kiểm soát chi tiết hoặc chia sẻ cross-account.
  - **Imported key material (BYOK):** Bạn tự đưa key material lên; AWS **không tự rotate** loại này.
- **Envelope Encryption:** KMS chỉ mã hóa trực tiếp dữ liệu **≤ 4 KB**. Với dữ liệu lớn hơn, dùng `GenerateDataKey` để lấy **data key** (plaintext + encrypted), mã hóa dữ liệu tại client bằng data key, rồi hủy bản plaintext và lưu bản mã hóa của data key cùng dữ liệu. **AWS Encryption SDK** hỗ trợ việc này, kèm **Data Key Caching** để giảm số lần gọi KMS.
- **Key Policy là bắt buộc.** Không có key policy cho phép thì kể cả admin cũng không dùng được khóa. Default key policy cho phép root account toàn quyền.
- **Xóa khóa:** Chỉ có thể **schedule deletion** với thời gian chờ **7–30 ngày** (mặc định 30). Có thể hủy trong thời gian chờ.
- **CloudHSM:** Phần cứng HSM dành riêng, **FIPS 140-2 Level 3**, khách hàng tự quản khóa hoàn toàn (AWS không truy cập được). Dùng khi có yêu cầu tuân thủ bắt buộc single-tenant HSM.

## 3.9. Quản lý bí mật và tham số

| | **AWS Secrets Manager** | **SSM Parameter Store** |
|---|---|---|
| Chi phí | Có phí (~0,40 USD/secret/tháng) | Standard **miễn phí**; Advanced có phí |
| Rotation tự động | **Có** (Lambda dựng sẵn cho RDS/Redshift/DocumentDB) | Không có sẵn |
| Tích hợp RDS | Sâu, tạo & xoay mật khẩu tự động | Không |
| Mã hóa | KMS (bắt buộc) | Tùy chọn (SecureString + KMS) |
| Cross-account | Có (resource policy) | Advanced tier |
| Dùng cho | Mật khẩu DB, API key nhạy cảm | Config, endpoint, AMI ID, biến môi trường |

## 3.10. Bảo vệ tầng ứng dụng và mạng

- **AWS WAF (Web Application Firewall):** Lọc HTTP/HTTPS ở **Layer 7**. Gắn được vào **CloudFront, ALB, API Gateway, AppSync, Cognito User Pool, App Runner**. **Không gắn được vào NLB** (vì NLB là Layer 4). Bảo vệ khỏi **SQL Injection, XSS**, chặn theo IP set, theo quốc gia (geo-match), theo **rate-based rule** (chống brute force / HTTP flood), dùng **Managed Rule Groups** của AWS và bên thứ ba.
- **AWS Shield Standard:** Miễn phí, tự động, chống DDoS Layer 3/4 phổ biến (SYN flood, UDP reflection).
- **AWS Shield Advanced:** 3.000 USD/tháng/tổ chức + phí data transfer. Bảo vệ nâng cao, **DDoS Response Team (DRT/SRT) hỗ trợ 24/7**, **hoàn tiền chi phí tăng đột biến do DDoS (cost protection)**, WAF miễn phí, tự động tạo rule giảm thiểu Layer 7.
- **AWS Firewall Manager:** Quản lý tập trung WAF rules, Shield Advanced, Security Group, Network Firewall, Route 53 Resolver DNS Firewall trên **toàn bộ account trong Organization**, tự áp cho tài nguyên mới tạo.
- **AWS Network Firewall:** Firewall có trạng thái (stateful) cho toàn bộ VPC, lọc Layer 3–7, hỗ trợ IPS/IDS, filter theo domain name.

## 3.11. Phát hiện mối đe dọa và tuân thủ

| Dịch vụ | Chức năng cốt lõi | Nguồn dữ liệu |
|---|---|---|
| **GuardDuty** | Phát hiện mối đe dọa bằng ML, không cần cài agent | VPC Flow Logs, CloudTrail (management + S3 data events), DNS Logs, EKS audit logs, EBS volume, RDS login, Lambda network |
| **Inspector** | Quét lỗ hổng (CVE) tự động, liên tục | EC2 (qua SSM Agent), container image trong ECR, Lambda function |
| **Macie** | Phát hiện dữ liệu nhạy cảm (PII, thẻ tín dụng) bằng ML | Chỉ **Amazon S3** |
| **Security Hub** | Bảng điều khiển tổng hợp findings, chấm điểm theo chuẩn (CIS, PCI DSS, AWS FSBP) | Tổng hợp từ GuardDuty, Inspector, Macie, Config, IAM Access Analyzer |
| **Detective** | Điều tra nguyên nhân gốc rễ của finding, dựng đồ thị hành vi | GuardDuty, CloudTrail, VPC Flow Logs |
| **AWS Config** | Ghi lại cấu hình tài nguyên theo thời gian, đánh giá tuân thủ theo rule, **auto-remediation** qua SSM | Cấu hình tài nguyên |
| **CloudTrail** | Nhật ký mọi lời gọi API (ai, khi nào, từ IP nào) | API calls |
| **Audit Manager** | Tự động thu thập bằng chứng cho các cuộc audit | Nhiều nguồn |

**Phân biệt kinh điển:** *CloudTrail = "Ai đã làm gì?"* — *Config = "Cấu hình đã thay đổi như thế nào và có tuân thủ không?"* — *CloudWatch = "Hệ thống đang chạy ra sao?"* — *GuardDuty = "Có ai đó đang tấn công tôi không?"*

### Chi tiết cần nhớ về CloudTrail

- Mặc định bật sẵn, lưu **90 ngày** trong Event History. Muốn lưu lâu hơn phải tạo **Trail** và trỏ về S3 (kèm tùy chọn gửi sang CloudWatch Logs để tạo alarm).
- **Management Events** (mặc định ghi): thao tác lên tài nguyên (tạo user, tạo subnet).
- **Data Events** (mặc định **KHÔNG** ghi, phải bật, có phí): S3 object-level (GetObject/PutObject), Lambda Invoke, DynamoDB item-level.
- **Insights Events:** Phát hiện hoạt động API bất thường (spike đột biến).
- **Organization Trail:** Một trail thu log của toàn bộ account con vào một bucket trung tâm.
- Đảm bảo toàn vẹn log: bật **log file validation** (dùng SHA-256 và ký số).

# PHẦN IV. ĐIỆN TOÁN (COMPUTE)

## 4.1. Amazon EC2 – Elastic Compute Cloud

### 4.1.1. Họ instance và cách đọc tên

Cú pháp: `m5a.2xlarge` = **m** (họ) + **5** (thế hệ) + **a** (biến thể: a=AMD, g=Graviton/ARM, n=network tăng cường, d=NVMe local, i=Intel) + `2xlarge` (kích cỡ).

| Họ | Tối ưu cho | Ví dụ ứng dụng |
|---|---|---|
| **T** (t3, t4g) | Burstable – hiệu năng cơ sở thấp, tích lũy CPU credit | Web server nhỏ, dev/test, tải không đều |
| **M** (m5, m6i, m7g) | Cân bằng CPU/RAM/mạng | App server, backend đa dụng |
| **C** (c5, c6g, c7g) | Compute optimized – nhiều CPU | Batch, HPC, mã hóa, game server, media transcoding |
| **R** (r5, r6g), **X**, **z1d** | Memory optimized – nhiều RAM | In-memory DB, SAP HANA, ElastiCache, Redis lớn |
| **I** (i3, i4i), **D**, **H** | Storage optimized – NVMe local IOPS cao | NoSQL, data warehouse, Elasticsearch |
| **P** (p3, p4), **G**, **Inf**, **Trn** | Accelerated computing – GPU/AI chip | ML training/inference, đồ họa, video |

**T-instances (burstable) – hay ra thi:** Tích lũy CPU credit khi chạy dưới baseline, tiêu credit khi vượt baseline. Hết credit → bị **throttle** xuống baseline. Bật **T2/T3 Unlimited** để vượt baseline không giới hạn (trả thêm phí). Nếu đề nói "ứng dụng thỉnh thoảng chậm bất thường, CPU bị giới hạn" → hết CPU credit.

### 4.1.2. Các mô hình mua (Purchase Options) – trọng tâm Domain 4

| Mô hình | Giảm giá | Cam kết | Trường hợp dùng |
|---|---|---|---|
| **On-Demand** | 0% | Không | Tải ngắn hạn, không dự đoán được, workload mới |
| **Reserved Instance (RI)** 1 hoặc 3 năm | Tới **72%** | Loại instance, Region (và AZ nếu Zonal) | Tải ổn định, dài hạn (DB, app server chạy 24/7) |
| **Convertible RI** | Tới ~54% | Có thể đổi họ/OS/tenancy | Tải ổn định nhưng có thể đổi loại máy |
| **Savings Plans – Compute** | Tới **66%** | Cam kết **$/giờ**, linh hoạt mọi họ/Region, áp cả **Lambda & Fargate** | Muốn giảm giá nhưng giữ linh hoạt tối đa |
| **Savings Plans – EC2 Instance** | Tới **72%** | Cam kết $/giờ cho một họ instance trong một Region | Tải ổn định, biết trước họ instance |
| **Spot Instances** | Tới **90%** | Không, **bị thu hồi với thông báo trước 2 phút** | Batch, CI/CD, xử lý ảnh/video, big data, workload chịu lỗi và **stateless** |
| **Dedicated Instance** | – | Phần cứng không chia sẻ với khách khác | Yêu cầu tuân thủ về cô lập |
| **Dedicated Host** | – | Thuê nguyên máy chủ vật lý, thấy socket/core | **BYOL license theo core/socket** (Oracle, Windows Server), tuân thủ nghiêm ngặt |
| **Capacity Reservation** | Không giảm giá (trả như On-Demand) | Giữ chỗ dung lượng trong một AZ cụ thể | Đảm bảo có máy cho sự kiện quan trọng, DR |

**Mẹo phân biệt:** *Dedicated Instance* = phần cứng riêng nhưng AWS quản lý vị trí. *Dedicated Host* = bạn thấy và kiểm soát máy vật lý → **chỉ Dedicated Host mới hỗ trợ BYOL theo socket/core**.

**Spot Fleet / EC2 Fleet:** Kết hợp Spot + On-Demand + RI theo nhiều loại instance và AZ để đạt dung lượng mục tiêu với chi phí thấp nhất. Chiến lược phân bổ: `capacityOptimized` (ít bị gián đoạn nhất – **khuyến nghị**), `lowestPrice`, `diversified`, `priceCapacityOptimized`.

### 4.1.3. Placement Groups

| Loại | Cách đặt máy | Ưu điểm | Rủi ro / Hạn chế |
|---|---|---|---|
| **Cluster** | Cùng một rack, cùng một AZ | Độ trễ thấp nhất, throughput **10 Gbps – 100 Gbps** giữa các node | Nếu rack hỏng → mất toàn bộ. Không HA |
| **Spread** | Mỗi instance trên một phần cứng riêng biệt, có thể trải nhiều AZ | Cô lập lỗi tối đa | Tối đa **7 instance mỗi AZ mỗi group** |
| **Partition** | Chia thành các partition, mỗi partition trên một tập rack riêng | Tới **7 partition/AZ**, hàng trăm instance | Ứng dụng phải nhận biết partition |

**Ứng dụng:** Cluster → HPC, tính toán chặt chẽ. Spread → một số ít instance quan trọng (domain controller). Partition → HDFS, HBase, Cassandra, Kafka.

### 4.1.4. AMI, User Data, Instance Metadata

- **AMI (Amazon Machine Image):** Khuôn mẫu để khởi tạo EC2 (OS + phần mềm + cấu hình). AMI **gắn theo Region** – muốn dùng ở Region khác phải **copy AMI**. Dùng **golden AMI** để rút ngắn thời gian khởi động trong Auto Scaling. **EC2 Image Builder** tự động hóa quá trình build – test – phân phối AMI theo lịch.
- **User Data:** Script chạy **một lần duy nhất khi instance khởi động lần đầu** với quyền root. Dùng để cài phần mềm, tải cấu hình.
- **Instance Metadata Service (IMDS):** Truy cập tại `http://169.254.169.254/latest/meta-data/`. **IMDSv2** dùng token (PUT rồi GET) để chống SSRF – **AWS khuyến nghị bắt buộc IMDSv2**. Lấy được instance-id, IP, và **credentials tạm thời của IAM role** đang gắn.

### 4.1.5. Vòng đời và trạng thái

`pending → running → stopping → stopped → terminated`. Ngoài ra có **hibernate** (lưu RAM vào EBS root, khởi động lại nhanh – yêu cầu EBS root mã hóa, RAM ≤ 150 GB, không dùng được với instance store).

**Ghi nhớ về chi phí:** Khi **stopped**, bạn **không trả tiền compute** nhưng **vẫn trả tiền EBS volume** và **Elastic IP nếu chưa gắn vào instance đang chạy**. Instance store **mất sạch dữ liệu** khi stop/terminate.

### 4.1.6. Elastic IP và ENI

- **Elastic IP (EIP):** IPv4 tĩnh, public. Mặc định giới hạn **5 EIP/Region**. **Tính phí khi không được sử dụng** (và từ 2024 AWS tính phí cho **mọi** public IPv4). Best practice: **tránh dùng EIP**; thay bằng DNS + Load Balancer.
- **ENI (Elastic Network Interface):** Card mạng ảo, gắn với một subnet (một AZ). Có private IP, có thể có EIP, gắn security group. Có thể **tách và gắn sang instance khác trong cùng AZ** → dùng cho failover thủ công.
- **Enhanced Networking (ENA / SR-IOV)** cho throughput cao; **EFA (Elastic Fabric Adapter)** cho HPC/MPI với độ trễ cực thấp.

## 4.2. Auto Scaling Group (ASG)

### 4.2.1. Thành phần

- **Launch Template** (khuyến nghị, thay cho Launch Configuration đã lỗi thời): định nghĩa AMI, instance type, key pair, security group, IAM instance profile, user data, EBS. Hỗ trợ **versioning** và **mix instance types + Spot/On-Demand**.
- **Min / Desired / Max capacity**: ASG luôn cố giữ số instance = Desired, trong khoảng Min–Max.
- **Health Check:** Kiểu **EC2** (kiểm tra trạng thái instance) hoặc **ELB** (kiểm tra ứng dụng qua health check của load balancer – **nên bật để phát hiện app treo dù OS vẫn sống**). Có **health check grace period** (mặc định 300s) để ứng dụng kịp khởi động.

### 4.2.2. Các chính sách scaling

| Loại | Mô tả | Ví dụ |
|---|---|---|
| **Target Tracking** | Đơn giản nhất, giữ một metric ở mức mục tiêu. **Khuyến nghị mặc định** | Giữ CPU trung bình ở 50%, giữ `ALBRequestCountPerTarget` = 1000 |
| **Step Scaling** | Thêm/bớt N instance theo từng bậc vượt ngưỡng CloudWatch alarm | CPU > 70% thêm 2 máy; CPU > 85% thêm 4 máy |
| **Simple Scaling** | Một hành động cho một alarm, có cooldown | (Cách cũ, ít dùng) |
| **Scheduled Scaling** | Theo lịch cố định, biết trước | Tăng lên 20 máy lúc 8h sáng thứ Hai, giảm còn 4 máy 22h |
| **Predictive Scaling** | ML phân tích lịch sử, dự báo và **scale trước** khi tải tăng | Tải theo chu kỳ tuần/ngày rõ rệt |

**Scaling Cooldown:** Mặc định 300 giây – trong thời gian này ASG không thực hiện thêm hành động scaling để chỉ số kịp ổn định. Dùng AMI đã cài sẵn phần mềm để rút ngắn thời gian khởi động và có thể giảm cooldown.

### 4.2.3. Lifecycle Hooks và Termination Policy

- **Lifecycle Hooks:** Tạm dừng instance ở trạng thái `Pending:Wait` (trước khi vào service – để cài đặt/nạp dữ liệu) hoặc `Terminating:Wait` (trước khi hủy – để **rút log, drain connection**). Kết hợp với EventBridge/SNS/Lambda.
- **Thứ tự hủy mặc định:** Chọn AZ có nhiều instance nhất → chọn instance dùng **Launch Template/Configuration cũ nhất** → chọn instance **gần với mốc tính giờ tiếp theo nhất**. Có thể bật **Instance Protection** để bảo vệ một instance khỏi bị scale-in.
- **Warm Pools:** Giữ sẵn một nhóm instance đã khởi tạo (stopped/hibernated) để scale-out cực nhanh với ứng dụng khởi động chậm.

## 4.3. Elastic Load Balancing (ELB)

### 4.3.1. So sánh 4 loại Load Balancer

| Tiêu chí | **ALB** (Application) | **NLB** (Network) | **GWLB** (Gateway) | **CLB** (Classic – legacy) |
|---|---|---|---|---|
| Tầng OSI | Layer 7 (HTTP/HTTPS/gRPC/WebSocket) | Layer 4 (TCP/UDP/TLS) | Layer 3 (GENEVE, port 6081) | Layer 4 & 7 |
| Hiệu năng | Cao | **Cực cao, hàng triệu req/s, độ trễ ~100µs** | Cao | Trung bình |
| Static IP | Không (dùng DNS name) | **Có – 1 Elastic IP mỗi AZ** | – | Không |
| Định tuyến theo | Path, hostname, HTTP header, query string, HTTP method, source IP | Port/Protocol | – | Cơ bản |
| Target | Instance, IP, **Lambda**, ALB | Instance, IP, **ALB** | Appliance ảo | Instance |
| Bảo toàn IP nguồn | Không (dùng `X-Forwarded-For`) | **Có** | Có | Không |
| Hỗ trợ WAF | **Có** | Không | Không | Không |
| Dùng khi | Web app, microservices, container | TCP/UDP, IP tĩnh, độ trễ cực thấp, gaming, IoT | Chèn firewall/IDS của bên thứ ba | Không dùng cho hệ thống mới |

### 4.3.2. Các khái niệm quan trọng

- **Target Group:** Nhóm đích + health check riêng. Một ALB có nhiều rule trỏ tới nhiều target group.
- **Sticky Sessions (Session Affinity):** Giữ một client đi về cùng một target. ALB dùng cookie `AWSALB` (duration-based) hoặc cookie ứng dụng. **Nhược điểm:** gây mất cân bằng tải. Giải pháp tốt hơn: lưu session ra **ElastiCache/DynamoDB** để ứng dụng thành **stateless**.
- **Cross-Zone Load Balancing:** Phân phối đều tới mọi target ở mọi AZ. **ALB: luôn bật, miễn phí. NLB & GWLB: mặc định TẮT, bật thì bị tính phí data transfer liên AZ. CLB: mặc định tắt, bật thì miễn phí.**
- **Connection Draining / Deregistration Delay:** Cho phép request đang xử lý hoàn tất trước khi gỡ target (mặc định 300s, đặt được 0–3600s).
- **SSL/TLS:** Chứng chỉ quản lý bằng **ACM (AWS Certificate Manager)** – miễn phí cho chứng chỉ public dùng với ELB/CloudFront/API GW, tự động gia hạn. **SNI (Server Name Indication)** cho phép **nhiều chứng chỉ trên một listener** (chỉ ALB/NLB, CLB không hỗ trợ). Có thể **TLS termination** ở ELB hoặc **TLS passthrough** (chỉ NLB) để đầu cuối tự giải mã.
- **Deletion Protection:** Bật để tránh xóa nhầm.

## 4.4. AWS Lambda

### 4.4.1. Đặc điểm và giới hạn (nhớ con số!)

| Thông số | Giá trị |
|---|---|
| Thời gian chạy tối đa | **15 phút (900 giây)** |
| Bộ nhớ | 128 MB – **10.240 MB** (CPU tăng tỉ lệ thuận với RAM) |
| Dung lượng `/tmp` | 512 MB – **10 GB** |
| Kích thước deployment package | 50 MB (zip nén, upload trực tiếp), 250 MB (giải nén), **10 GB nếu dùng container image** |
| Payload đồng bộ (synchronous) | **6 MB** request/response |
| Payload bất đồng bộ | 256 KB |
| Biến môi trường | 4 KB |
| Concurrency mặc định | **1.000 đồng thời/Region** (có thể xin tăng) |

### 4.4.2. Các khái niệm cốt lõi

- **Ba kiểu gọi:** *Synchronous* (API Gateway, ALB, CLI – trả kết quả ngay), *Asynchronous* (S3, SNS, EventBridge – Lambda tự retry **2 lần**, nên cấu hình **DLQ** hoặc **Lambda Destinations**), *Event Source Mapping* (Lambda **poll** từ SQS, Kinesis, DynamoDB Streams, MSK).
- **Cold Start:** Độ trễ khi khởi tạo môi trường lần đầu. Giảm bằng: **Provisioned Concurrency** (giữ sẵn môi trường – có phí), **SnapStart** (cho Java, giảm tới 10 lần), giảm kích thước package, tránh khởi tạo nặng trong handler.
- **Reserved Concurrency:** Đặt trần đồng thời cho một function → bảo vệ các function khác (tránh một function ăn hết 1.000 slot), đồng thời bảo vệ backend DB khỏi quá tải.
- **Lambda Layers:** Chia sẻ thư viện/dependency giữa nhiều function (tối đa 5 layer).
- **Lambda trong VPC:** Mặc định Lambda chạy ngoài VPC của bạn (có Internet). Khi đặt Lambda **vào VPC** để truy cập RDS private, Lambda **mất Internet** trừ khi có **NAT Gateway** + route; muốn gọi S3/DynamoDB thì dùng **VPC Endpoint**.
- **RDS Proxy:** Bắt buộc cân nhắc khi Lambda kết nối RDS ở quy mô lớn – gộp connection pool, tránh làm cạn kiệt kết nối DB.
- **Lambda@Edge vs CloudFront Functions:** xem phần CloudFront.

**Khi nào KHÔNG dùng Lambda:** tác vụ chạy > 15 phút, cần trạng thái lâu dài, cần kiểm soát OS, workload liên tục cường độ cao (EC2/Fargate rẻ hơn).

## 4.5. Container trên AWS

### 4.5.1. Amazon ECS – Elastic Container Service

- Orchestrator container riêng của AWS, đơn giản, tích hợp sâu (ALB, IAM, CloudWatch).
- **Hai launch type:**
  - **EC2 launch type:** Bạn quản lý cụm EC2 (ECS Agent chạy trên đó). Rẻ hơn khi tải ổn định, kiểm soát nhiều hơn, dùng được GPU/instance đặc thù.
  - **Fargate launch type:** **Serverless** – không quản lý máy chủ, trả theo vCPU/RAM và thời gian chạy. Chọn khi đề nói "**least operational overhead**".
- **Task Definition:** Bản thiết kế container (image, CPU, RAM, port, biến môi trường, volume, log config).
- **Hai loại IAM Role rất hay thi:**
  - **Task Execution Role:** Quyền cho **ECS Agent** kéo image từ ECR và ghi log lên CloudWatch.
  - **Task Role:** Quyền cho **chính ứng dụng trong container** gọi S3, DynamoDB…
- **Service Auto Scaling:** dùng Application Auto Scaling theo CPU/Memory/ALBRequestCountPerTarget. Với EC2 launch type, dùng **ECS Capacity Provider** để scale cả cụm EC2.
- **Dynamic Port Mapping** với ALB cho phép chạy nhiều task cùng container port trên một host.

### 4.5.2. Amazon EKS – Elastic Kubernetes Service

Kubernetes được quản lý. Chọn EKS khi: tổ chức **đã dùng Kubernetes**, cần tính di động đa đám mây (hybrid/multi-cloud), cần hệ sinh thái K8s (Helm, Istio). Node group có thể là **Managed Node Group, Self-managed, hoặc Fargate**.

### 4.5.3. Amazon ECR – Elastic Container Registry

Registry lưu Docker image, tích hợp IAM, hỗ trợ **image scanning** (basic & enhanced qua Inspector), **lifecycle policy** xóa image cũ, **cross-region replication**. Lưu trữ thực tế nằm trên S3.

### 4.5.4. Các lựa chọn compute khác

| Dịch vụ | Mô tả | Khi nào chọn |
|---|---|---|
| **AWS Batch** | Chạy job hàng loạt theo hàng đợi, tự cấp phát EC2/Spot/Fargate | Xử lý batch quy mô lớn, job > 15 phút (Lambda không đáp ứng) |
| **Elastic Beanstalk** | PaaS – upload code, AWS lo EC2/ASG/ELB/RDS. Có deployment: All-at-once, Rolling, Rolling with batch, Immutable, Blue/Green (swap URL) | Dev muốn deploy nhanh nhưng vẫn thấy tài nguyên bên dưới |
| **AWS App Runner** | Từ source code/container image → dịch vụ web chạy ngay, tự scale, tự HTTPS | Web app/API đơn giản, không muốn học ECS |
| **Amazon Lightsail** | VPS trọn gói giá cố định | Dự án nhỏ, người mới, blog WordPress |
| **AWS Fargate** | Compute engine serverless cho ECS/EKS | Không muốn quản lý máy chủ |
| **AWS Outposts** | Hạ tầng AWS đặt tại on-premises | Data residency, độ trễ cực thấp với hệ thống nội bộ |

### 4.5.5. Chiến lược triển khai (Deployment Strategies)

| Chiến lược | Cách hoạt động | Downtime | Chi phí |
|---|---|---|---|
| **All-at-once** | Cập nhật tất cả cùng lúc | **Có** | Thấp nhất |
| **Rolling** | Cập nhật từng nhóm nhỏ | Không, nhưng giảm dung lượng | Thấp |
| **Rolling with additional batch** | Thêm máy mới trước khi gỡ máy cũ | Không, giữ nguyên dung lượng | Trung bình |
| **Immutable** | Tạo hoàn toàn ASG mới, chuyển traffic khi OK | Không | Cao (nhân đôi tạm thời) |
| **Blue/Green** | Hai môi trường song song, đổi DNS/target group | Không, **rollback tức thì** | Cao |
| **Canary** | Chuyển dần % traffic (ví dụ 10% → 50% → 100%) | Không, **giảm rủi ro tối đa** | Trung bình |

# PHẦN V. LƯU TRỮ (STORAGE)

## 5.1. Ba loại hình lưu trữ

| Loại | Dịch vụ AWS | Đặc điểm | Dùng cho |
|---|---|---|---|
| **Block Storage** | EBS, Instance Store | Ổ đĩa gắn vào một máy, thao tác theo block | OS, database, ứng dụng cần IOPS thấp độ trễ |
| **File Storage** | EFS, FSx | Hệ thống file phân cấp, nhiều máy cùng mount | Chia sẻ file, CMS, home directory, HPC |
| **Object Storage** | S3, Glacier | Đối tượng phẳng + metadata, truy cập qua API HTTP | Ảnh, video, backup, data lake, static website |

## 5.2. Amazon EBS – Elastic Block Store

### 5.2.1. Đặc tính chung

- Ổ đĩa mạng gắn vào EC2. **Gắn theo AZ** – không thể gắn volume ở AZ này vào instance ở AZ khác (muốn chuyển AZ: tạo snapshot → tạo volume từ snapshot ở AZ mới).
- Mặc định một volume gắn vào **một** instance tại một thời điểm; **EBS Multi-Attach** (chỉ io1/io2) cho phép gắn tối đa **16 instance trong cùng AZ** – ứng dụng phải tự quản lý ghi đồng thời (cluster-aware file system).
- **Dữ liệu tồn tại độc lập với vòng đời instance** (khác Instance Store). Root volume mặc định có `DeleteOnTermination = true`, volume gắn thêm mặc định `false`.
- Có thể **thay đổi kích thước, loại volume và IOPS khi đang chạy** (Elastic Volumes) – nhưng chỉ **tăng**, không giảm.

### 5.2.2. Các loại EBS Volume (bảng cần thuộc)

| Loại | Nhóm | Dung lượng | IOPS tối đa | Throughput tối đa | Boot volume | Dùng cho |
|---|---|---|---|---|---|---|
| **gp3** | SSD | 1 GiB – 16 TiB | **16.000** (baseline 3.000, độc lập dung lượng) | 1.000 MB/s | Có | **Mặc định khuyến nghị** – rẻ hơn gp2 ~20%, cấu hình IOPS/throughput độc lập |
| **gp2** | SSD | 1 GiB – 16 TiB | 16.000 (3 IOPS/GiB, burst 3.000) | 250 MB/s | Có | Thế hệ cũ, IOPS gắn với dung lượng |
| **io1** | SSD | 4 GiB – 16 TiB | 64.000 (50 IOPS/GiB) | 1.000 MB/s | Có | IOPS cao, DB quan trọng |
| **io2 / io2 Block Express** | SSD | 4 GiB – 64 TiB | **256.000** (io2 BE) | 4.000 MB/s | Có | Độ bền **99,999%**, IOPS cực cao, SAP HANA, Oracle, SQL Server |
| **st1** | HDD | 125 GiB – 16 TiB | 500 | **500 MB/s** | **Không** | Throughput cao, truy cập tuần tự: big data, log processing, data warehouse |
| **sc1** | HDD | 125 GiB – 16 TiB | 250 | 250 MB/s | **Không** | **Rẻ nhất**, dữ liệu truy cập ít, lưu trữ lạnh |

**Bẫy thường gặp:** HDD (st1, sc1) **không dùng làm boot volume**. Cần IOPS > 16.000 hoặc Multi-Attach → phải là io1/io2.

### 5.2.3. EBS Snapshot

- Sao lưu **tăng dần (incremental)** lưu trên **S3** (do AWS quản lý, bạn không thấy bucket). Snapshot mang tính **Region**, có thể **copy sang Region khác** (cho DR) và **chia sẻ với account khác** (nếu mã hóa bằng CMK thì phải chia sẻ cả key).
- **EBS Snapshot Archive:** Rẻ hơn tới 75%, nhưng khôi phục mất **24–72 giờ**.
- **Recycle Bin:** Đặt rule giữ snapshot đã xóa từ 1 ngày đến 1 năm để chống xóa nhầm.
- **Fast Snapshot Restore (FSR):** Loại bỏ độ trễ "lazy loading" khi tạo volume từ snapshot (có phí).
- **Data Lifecycle Manager (DLM)** hoặc **AWS Backup**: tự động hóa tạo/xóa snapshot theo lịch.

### 5.2.4. Mã hóa EBS

- Dùng **KMS (AES-256)**. Mã hóa cả dữ liệu at-rest, dữ liệu truyền giữa instance và volume, snapshot và mọi volume tạo từ snapshot đó.
- Ảnh hưởng hiệu năng **không đáng kể**, và mã hóa **trong suốt** với ứng dụng.
- **Không thể mã hóa một volume chưa mã hóa trực tiếp.** Quy trình: tạo snapshot → **copy snapshot với tùy chọn encrypt** → tạo volume mới từ snapshot đã mã hóa → gắn vào instance.
- Có thể bật **Encryption by default** ở cấp Region.

### 5.2.5. Instance Store

Ổ đĩa **vật lý gắn trực tiếp** vào máy chủ host → **IOPS cực cao** (hàng triệu). Nhưng là **ephemeral**: mất dữ liệu khi instance **stop, hibernate hoặc terminate** (không mất khi reboot). Dùng cho: buffer, cache, scratch data, dữ liệu tạm có thể tái tạo. **Đề thi:** "cần IOPS cao nhất có thể, dữ liệu tạm" → Instance Store.

## 5.3. Amazon EFS – Elastic File System

- Hệ thống file **NFS v4.1** cho **Linux**, có thể mount đồng thời từ **hàng nghìn EC2 trên nhiều AZ** (và cả on-premises qua Direct Connect/VPN).
- **Tự động co giãn** tới petabyte, không cần cấp phát trước. Trả tiền theo dung lượng thực dùng (đắt hơn EBS ~3 lần).
- **Chế độ khả dụng:** *Regional (Multi-AZ)* – khuyến nghị cho production; *One Zone* – rẻ hơn ~47%, chỉ một AZ.
- **Performance Mode:** *General Purpose* (mặc định, độ trễ thấp – web, CMS) và *Max I/O* (throughput cao hơn, độ trễ cao hơn – big data, xử lý song song lớn).
- **Throughput Mode:** *Bursting* (theo dung lượng), *Provisioned* (đặt throughput độc lập với dung lượng), *Elastic* (tự động, khuyến nghị cho tải khó đoán).
- **Storage Classes + Lifecycle Management:** Standard → **Infrequent Access (IA)** → **Archive**, tự chuyển sau N ngày không truy cập (7–365 ngày), tiết kiệm tới 92%.
- **Bảo mật:** Mount target trong mỗi AZ, dùng Security Group; mã hóa at-rest bằng KMS và in-transit bằng TLS; **EFS Access Points** để phân tách quyền và ép POSIX user cho từng ứng dụng.

**Đề thi kinh điển:** "Nhiều EC2 Linux cần đọc/ghi cùng một bộ file" → **EFS** (không phải EBS, vì EBS chỉ gắn 1 máy).

## 5.4. Amazon FSx

| Loại FSx | Giao thức | Đặc điểm nổi bật | Trường hợp dùng |
|---|---|---|---|
| **FSx for Windows File Server** | **SMB**, NTFS | Tích hợp **Active Directory**, ACL Windows, DFS Namespaces, Multi-AZ | Ứng dụng Windows, home directory, thay thế file server on-prem |
| **FSx for Lustre** | Lustre (POSIX) | Throughput **hàng trăm GB/s**, độ trễ **sub-millisecond**, **liên kết trực tiếp với S3** (lazy load & export) | HPC, machine learning, phân tích tài chính, xử lý gen |
| **FSx for NetApp ONTAP** | **NFS, SMB, iSCSI** cùng lúc | Tính năng doanh nghiệp: snapshot, clone, dedup, compression, **data tiering** tự động | Di chuyển workload NetApp on-prem lên AWS, cần đa giao thức |
| **FSx for OpenZFS** | NFS | Snapshot tức thì, độ trễ thấp, tới 1 triệu IOPS | Di chuyển ZFS/Linux file server |

**FSx for Lustre hai kiểu triển khai:** *Scratch* (tạm, hiệu năng cao, không nhân bản – dùng cho xử lý ngắn hạn) và *Persistent* (nhân bản trong AZ, tự thay phần cứng hỏng – dùng cho dữ liệu lâu dài).

## 5.5. Amazon S3 – Simple Storage Service

### 5.5.1. Nguyên tắc nền tảng

- Lưu trữ đối tượng, dung lượng **không giới hạn**; mỗi object tối đa **5 TB**. Upload > 5 GB **bắt buộc dùng Multipart Upload** (khuyến nghị dùng từ 100 MB).
- **Tên bucket là duy nhất toàn cầu**, gồm chữ thường/số/dấu gạch ngang, 3–63 ký tự, không viết hoa, không gạch dưới.
- Bucket **nằm trong một Region** (dù namespace là global).
- **Độ bền 99,999999999% (11 số 9)**, khả dụng 99,99% với S3 Standard.
- **Strong read-after-write consistency** cho mọi thao tác (từ tháng 12/2020) – kể cả LIST.
- **Hiệu năng:** Mỗi prefix hỗ trợ **3.500 PUT/COPY/POST/DELETE và 5.500 GET/HEAD mỗi giây**. Dùng nhiều prefix để nhân số này lên.

### 5.5.2. Các lớp lưu trữ (Storage Classes) – bảng phải thuộc

| Storage Class | Độ khả dụng | Số AZ | Phí truy xuất | Thời gian lưu tối thiểu | Kích thước tính phí tối thiểu | Trường hợp dùng |
|---|---|---|---|---|---|---|
| **S3 Standard** | 99,99% | ≥3 | Không | Không | Không | Dữ liệu truy cập thường xuyên |
| **S3 Intelligent-Tiering** | 99,9% | ≥3 | Không (chỉ phí monitoring nhỏ/object) | Không | 128 KB (để tự chuyển tầng) | **Mẫu truy cập không đoán trước** |
| **S3 Standard-IA** | 99,9% | ≥3 | **Có** | **30 ngày** | 128 KB | Backup, DR, truy cập ít nhưng cần nhanh |
| **S3 One Zone-IA** | 99,5% | **1** | Có | 30 ngày | 128 KB | Dữ liệu tái tạo được, bản sao thứ cấp |
| **S3 Glacier Instant Retrieval** | 99,9% | ≥3 | Có | **90 ngày** | 128 KB | Archive nhưng cần truy xuất **mili-giây** (ảnh y tế, media) |
| **S3 Glacier Flexible Retrieval** | 99,99% | ≥3 | Có | **90 ngày** | 40 KB | Archive; truy xuất **Expedited 1–5 phút / Standard 3–5 giờ / Bulk 5–12 giờ (miễn phí)** |
| **S3 Glacier Deep Archive** | 99,99% | ≥3 | Có | **180 ngày** | 40 KB | Lưu trữ dài hạn **rẻ nhất**; **Standard 12 giờ / Bulk 48 giờ** |
| **S3 Express One Zone** | 99,95% | 1 (một AZ, chọn được) | Có | Không | 512 KB | Độ trễ **single-digit millisecond**, nhanh hơn Standard 10x |

**Mẹo nhớ ngưỡng phạt:** Standard-IA & One Zone-IA = **30 ngày**; Glacier IR & Flexible = **90 ngày**; Deep Archive = **180 ngày**. Xóa sớm hơn vẫn bị tính đủ.

### 5.5.3. Lifecycle Policy

Hai loại hành động: **Transition** (chuyển sang lớp rẻ hơn) và **Expiration** (xóa object, xóa version cũ, xóa **incomplete multipart upload** – nên luôn bật rule này để khỏi trả tiền rác). Có thể lọc theo prefix, tag, kích thước object.

Chiều chuyển tầng phải đi **từ nóng xuống lạnh**: Standard → Standard-IA → Intelligent-Tiering → Glacier IR → Glacier Flexible → Deep Archive. **Không chuyển ngược lại được bằng lifecycle** (phải copy/restore).

Dùng **S3 Storage Class Analysis** để phân tích mẫu truy cập và đề xuất lúc nào nên chuyển tầng.

### 5.5.4. Versioning, MFA Delete, Object Lock

- **Versioning:** Bật ở cấp bucket, chống ghi đè và xóa nhầm. Khi bật rồi chỉ có thể **suspend**, **không thể tắt hẳn**. Object có trước khi bật sẽ có version ID là `null`. Xóa object chỉ tạo **delete marker** – khôi phục bằng cách xóa delete marker.
- **MFA Delete:** Yêu cầu MFA khi xóa vĩnh viễn một version hoặc thay đổi trạng thái versioning. **Chỉ root user bật được**, và bắt buộc dùng CLI.
- **S3 Object Lock (WORM – Write Once Read Many):** Yêu cầu bật versioning.
  - **Governance Mode:** Người dùng có quyền đặc biệt (`s3:BypassGovernanceRetention`) vẫn ghi đè/xóa được.
  - **Compliance Mode:** **Không ai** – kể cả root – có thể xóa hay rút ngắn thời hạn. Dùng cho yêu cầu pháp lý (SEC 17a-4, FINRA).
  - **Legal Hold:** Khóa vô thời hạn, độc lập với retention period, gỡ bằng quyền `s3:PutObjectLegalHold`.
- **S3 Glacier Vault Lock:** Chính sách WORM cho Glacier vault, một khi khóa là vĩnh viễn.

### 5.5.5. Kiểm soát truy cập S3

Thứ tự áp dụng: **IAM policy (user) + Bucket Policy (resource) + ACL (cũ, nên tắt) + Block Public Access**.

- **Block Public Access:** 4 tùy chọn, **bật mặc định** cho bucket mới. Đây là "cầu dao tổng" – nếu bật, mọi bucket policy công khai đều vô hiệu. Đề thi hỏi "ngăn dữ liệu bị lộ công khai" → **bật Block Public Access ở cấp account**.
- **Bucket Policy:** JSON có `Principal`. Ví dụ hay ra thi: buộc HTTPS bằng `Condition: {"Bool": {"aws:SecureTransport": "false"}}` với `Effect: Deny`; buộc mã hóa khi upload bằng điều kiện `s3:x-amz-server-side-encryption`.
- **ACL:** Cơ chế cũ. AWS khuyến nghị **Object Ownership = Bucket owner enforced** để **vô hiệu hóa ACL**.
- **S3 Access Points:** Tạo nhiều endpoint riêng cho từng ứng dụng/nhóm với policy riêng → thay vì một bucket policy khổng lồ. Có thể tạo **VPC-only access point** (chỉ truy cập từ VPC).
- **Object Lambda Access Point:** Biến đổi dữ liệu on-the-fly khi đọc (che PII, resize ảnh, đổi định dạng) mà không cần lưu nhiều bản.
- **Presigned URL:** Cấp quyền truy cập tạm thời cho object private (thời hạn tuỳ chọn). Người nhận có quyền y hệt người tạo URL. Dùng cho "cho phép khách tải file private trong 1 giờ".

### 5.5.6. Mã hóa S3

| Phương pháp | Ai giữ khóa | Ai mã hóa | Ghi chú |
|---|---|---|---|
| **SSE-S3** | AWS (AES-256) | AWS | **Bật mặc định cho mọi bucket từ 2023**, miễn phí. Header `x-amz-server-side-encryption: AES256` |
| **SSE-KMS** | KMS (CMK của bạn) | AWS | Kiểm soát & audit qua CloudTrail; **bị giới hạn KMS quota** (nghẽn nếu lưu lượng lớn) → dùng **S3 Bucket Keys** để giảm 99% lời gọi KMS |
| **DSSE-KMS** | KMS | AWS (mã hóa **hai lớp**) | Yêu cầu tuân thủ đặc biệt |
| **SSE-C** | **Khách hàng** cung cấp khóa mỗi request | AWS | AWS không lưu khóa; **bắt buộc HTTPS** |
| **Client-Side Encryption** | Khách hàng | **Khách hàng** (trước khi upload) | Kiểm soát tuyệt đối |

**In-transit:** dùng HTTPS/TLS. Bắt buộc bằng bucket policy `aws:SecureTransport`.

### 5.5.7. Replication (nhân bản)

- **CRR (Cross-Region Replication):** Tuân thủ, giảm độ trễ cho người dùng ở xa, DR đa Region.
- **SRR (Same-Region Replication):** Gộp log, sao chép giữa môi trường Prod và Test.
- **Bắt buộc bật Versioning ở cả bucket nguồn và đích** + IAM role cho S3.
- **Chỉ nhân bản object tạo MỚI sau khi bật rule.** Muốn nhân bản object cũ dùng **S3 Batch Replication**.
- **Không có replication dây chuyền (chaining):** nếu A→B và B→C thì object từ A **không** tự tới C.
- Delete marker replication là **tùy chọn**; xóa theo version ID thì **không** được nhân bản (chống xóa độc hại).
- **S3 RTC (Replication Time Control):** Cam kết SLA nhân bản **99,99% object trong 15 phút**, có metric giám sát. Dùng khi đề nói "yêu cầu nhân bản trong thời gian xác định".

### 5.5.8. Các tính năng S3 khác hay ra thi

- **S3 Transfer Acceleration:** Upload qua **Edge Location** rồi đi đường trục AWS tới bucket → nhanh hơn đáng kể khi người dùng ở xa Region. Kết hợp tốt với Multipart Upload.
- **S3 Static Website Hosting:** Host web tĩnh, endpoint dạng `bucket-name.s3-website-region.amazonaws.com`. Muốn HTTPS và domain riêng → đặt **CloudFront** phía trước.
- **S3 Select / Glacier Select:** Dùng SQL đơn giản lấy **một phần** object (CSV/JSON/Parquet) → giảm tới 80% dữ liệu truyền và chi phí. (Với truy vấn phức tạp nhiều file → dùng **Athena**.)
- **S3 Event Notifications:** Kích hoạt **SNS, SQS, Lambda, EventBridge** khi có `s3:ObjectCreated:*`, `s3:ObjectRemoved:*`… Gửi sang **EventBridge** để lọc nâng cao và fan-out tới 20+ đích.
- **S3 Batch Operations:** Thực hiện thao tác hàng loạt trên hàng tỷ object (copy, thay ACL, gọi Lambda, restore từ Glacier, áp Object Lock).
- **S3 Inventory:** Báo cáo CSV/ORC/Parquet định kỳ liệt kê object và metadata (kích thước, storage class, trạng thái mã hóa/replication).
- **S3 Storage Lens:** Bảng điều khiển phân tích sử dụng và chi phí trên toàn tổ chức, đề xuất tối ưu.
- **Requester Pays:** Người tải trả phí data transfer thay vì chủ bucket – dùng khi chia sẻ dataset lớn.
- **Multi-Region Access Points:** Một endpoint toàn cầu tự định tuyến tới bucket gần nhất, hỗ trợ failover.
- **CORS:** Cần cấu hình trên bucket đích khi web ở origin khác gọi tới.

## 5.6. Di chuyển và chuyển dữ liệu

| Dịch vụ | Mô tả | Khi nào dùng |
|---|---|---|
| **AWS DataSync** | Đồng bộ dữ liệu online, tốc độ cao, có agent; hỗ trợ NFS, SMB, HDFS, S3, EFS, FSx; giữ metadata & permission; chạy theo lịch | Di chuyển/đồng bộ định kỳ qua mạng (Internet/DX) |
| **AWS Storage Gateway** | Hybrid – mở rộng lưu trữ on-prem lên AWS | Xem chi tiết bên dưới |
| **AWS Transfer Family** | Endpoint được quản lý cho **SFTP, FTPS, FTP, AS2** ghi thẳng vào S3/EFS | Đối tác vẫn dùng giao thức FTP truyền thống |
| **AWS Snowcone** | ~8 TB (HDD) / 14 TB (SSD), nhỏ gọn, chịu môi trường khắc nghiệt | Edge, thu thập dữ liệu tại hiện trường |
| **AWS Snowball Edge** | Storage Optimized ~80 TB / Compute Optimized; có thể chạy EC2 & Lambda tại chỗ | Petabyte, nơi băng thông kém |
| **AWS Snowmobile** | Xe container 100 PB | Exabyte-scale, di dời cả data center |

**Quy tắc chọn:** Ước lượng thời gian truyền qua mạng. Nếu truyền qua Internet mất **hơn một tuần** → dùng Snow Family. Nếu có đường truyền tốt và cần đồng bộ liên tục → DataSync.

### Storage Gateway – bốn loại

| Loại | Giao thức | Cơ chế | Dùng khi |
|---|---|---|---|
| **S3 File Gateway** | NFS, SMB | File hiện lên như thư mục, thực chất lưu thành object trên S3; cache dữ liệu nóng tại chỗ | Mở rộng file share lên S3, tận dụng lifecycle |
| **FSx File Gateway** | SMB | Cache cục bộ cho FSx for Windows | Truy cập FSx với độ trễ thấp từ on-prem |
| **Volume Gateway** | iSCSI | *Cached* (dữ liệu chính trên S3, cache nóng tại chỗ) hoặc *Stored* (dữ liệu chính tại chỗ, backup async lên S3 dạng EBS snapshot) | Backup volume, DR ứng dụng block |
| **Tape Gateway** | iSCSI VTL | Giả lập thư viện băng từ, ghi vào S3/Glacier | Thay thế hệ thống backup băng từ mà không đổi phần mềm |

## 5.7. AWS Backup

Dịch vụ **quản lý sao lưu tập trung** cho EC2, EBS, RDS, Aurora, DynamoDB, EFS, FSx, Storage Gateway, S3, DocumentDB, Neptune. Tính năng: **Backup Plan** theo lịch, **lifecycle** chuyển sang cold storage, **cross-region & cross-account copy**, **Backup Vault Lock** (WORM – không ai xóa được backup, kể cả root) và báo cáo tuân thủ. Đề nói "quản lý backup tập trung, tuân thủ, ít thao tác vận hành" → **AWS Backup**.

# PHẦN VI. CƠ SỞ DỮ LIỆU

## 6.1. Bản đồ chọn cơ sở dữ liệu (rất quan trọng)

| Nhu cầu trong đề bài | Dịch vụ |
|---|---|
| SQL quan hệ, OLTP, ít thao tác vận hành | **Amazon RDS** |
| SQL quan hệ, hiệu năng cao, HA mạnh, tự scale storage | **Amazon Aurora** |
| Serverless SQL, tải bất định hoặc gián đoạn | **Aurora Serverless v2** |
| NoSQL key-value, độ trễ **mili-giây một chữ số**, scale vô hạn, serverless | **DynamoDB** |
| Cache in-memory, giảm tải đọc cho DB, session store, leaderboard | **ElastiCache (Redis)** |
| Cache DynamoDB độ trễ **micro-giây** | **DAX** |
| Data warehouse, OLAP, BI, truy vấn phân tích petabyte | **Redshift** |
| Đồ thị, mạng xã hội, phát hiện gian lận, recommendation engine | **Neptune** |
| MongoDB-compatible, tài liệu JSON | **DocumentDB** |
| Cassandra-compatible | **Keyspaces** |
| Dữ liệu chuỗi thời gian, IoT, telemetry | **Timestream** |
| Sổ cái bất biến, có thể kiểm chứng bằng mật mã | **QLDB** |
| Redis bền vững (durable), dùng làm DB chính | **MemoryDB for Redis** |
| Tìm kiếm toàn văn, phân tích log, dashboard log | **OpenSearch Service** |

## 6.2. Amazon RDS

Hỗ trợ 6 engine: **MySQL, PostgreSQL, MariaDB, Oracle, SQL Server, Amazon Aurora**. AWS quản lý: cấp phát, vá lỗi OS & engine, backup, phục hồi, giám sát, failover. **Bạn KHÔNG có quyền SSH vào máy chủ RDS** (trừ RDS Custom cho Oracle/SQL Server).

### 6.2.1. Multi-AZ vs Read Replica – so sánh kinh điển

| Tiêu chí | **Multi-AZ Deployment** | **Read Replica** |
|---|---|---|
| Mục đích chính | **Tính sẵn sàng cao (HA) & DR** | **Mở rộng hiệu năng đọc (scale read)** |
| Cơ chế nhân bản | **Đồng bộ (synchronous)** | **Bất đồng bộ (asynchronous)** – có độ trễ (replica lag) |
| Standby có phục vụ traffic không | **Không** (chế độ Multi-AZ 1 standby); chế độ **Multi-AZ DB Cluster** (2 standby) thì **có đọc được** | **Có**, dùng endpoint riêng để đọc |
| Failover | **Tự động** (thường 60–120 giây), DNS endpoint tự trỏ sang standby | **Thủ công** – phải promote replica thành DB độc lập |
| Phạm vi | Trong cùng Region, khác AZ | **Cùng AZ, khác AZ, hoặc khác Region** |
| Số lượng | 1 standby (hoặc 2 với DB Cluster) | Tối đa **5** (15 với Aurora) |
| Chi phí | Nhân đôi chi phí instance | Trả tiền theo từng replica |
| Backup ảnh hưởng | Backup lấy từ standby → **không ảnh hưởng hiệu năng primary** | – |

**Ghi nhớ:** *Multi-AZ = độ sẵn sàng. Read Replica = hiệu năng.* Nếu đề vừa muốn HA vừa muốn scale read → **dùng cả hai**. Nếu đề nói "cần DR sang Region khác cho RDS (không phải Aurora)" → **Cross-Region Read Replica**.

**Lưu ý chi phí:** Nhân bản trong cùng Region **miễn phí data transfer**; nhân bản cross-Region **tính phí data transfer**.

### 6.2.2. Backup và phục hồi

- **Automated Backups:** Bật mặc định, giữ **1–35 ngày** (0 = tắt). Bao gồm snapshot hằng ngày trong **backup window** + **transaction logs mỗi 5 phút** → cho phép **Point-in-Time Recovery (PITR)** tới bất kỳ thời điểm nào trong khoảng giữ.
- **Manual DB Snapshots:** Giữ **vô thời hạn** cho tới khi bạn xóa. Có thể copy cross-region, chia sẻ cho account khác.
- **Khôi phục luôn tạo ra một DB instance MỚI với endpoint mới** – ứng dụng phải cập nhật connection string (hoặc dùng Route 53 CNAME).
- Xóa DB instance: nên bật **Deletion Protection** và tạo **final snapshot**.

### 6.2.3. Bảo mật RDS

- **Mã hóa at-rest** bằng KMS – **phải bật khi tạo instance**. Muốn mã hóa DB chưa mã hóa: snapshot → copy snapshot có encrypt → restore. Read Replica của DB đã mã hóa cũng được mã hóa.
- **Mã hóa in-transit** bằng SSL/TLS (tải AWS RDS CA cert); có thể **bắt buộc SSL** qua parameter group (`rds.force_ssl=1` với PostgreSQL, `require_secure_transport=ON` với MySQL).
- **IAM Database Authentication** (MySQL, PostgreSQL, MariaDB): Đăng nhập bằng **token IAM có hiệu lực 15 phút** thay vì mật khẩu → không lưu mật khẩu, quản lý tập trung, bắt buộc SSL.
- Mạng: đặt RDS trong **private subnet**, kiểm soát bằng **Security Group**; **DB Subnet Group** phải phủ ≥ 2 AZ.

### 6.2.4. RDS Proxy

Proxy **fully managed, serverless, tự HA** đặt giữa ứng dụng và RDS/Aurora:
- **Gộp và tái sử dụng connection** → giải quyết vấn đề **Lambda tạo quá nhiều kết nối**.
- **Giảm thời gian failover tới 66%**.
- Cho phép ép **IAM authentication** và lấy credentials từ **Secrets Manager**.
- **Chỉ truy cập được từ trong VPC** (không public).

### 6.2.5. Giám sát và bảo trì

- **RDS Enhanced Monitoring:** Metric OS ở mức chi tiết tới 1 giây (khác CloudWatch metric lấy từ hypervisor).
- **Performance Insights:** Phân tích tải DB, tìm câu truy vấn/wait event gây nghẽn.
- **Maintenance Window:** AWS vá lỗi; với Multi-AZ, AWS vá standby trước rồi failover → giảm downtime.
- **Storage Auto Scaling:** Tự tăng dung lượng khi sắp đầy.

## 6.3. Amazon Aurora

### 6.3.1. Kiến trúc

- Tương thích **MySQL** và **PostgreSQL**; nhanh hơn MySQL tiêu chuẩn tới **5 lần**, PostgreSQL tới **3 lần**; giá khoảng **20% cao hơn RDS** nhưng hiệu quả hơn nhiều ở quy mô lớn.
- **Lưu trữ tách rời khỏi compute:** Một **shared storage volume** tự động tăng từ 10 GB tới **128 TB**, tự **nhân bản 6 bản sao trên 3 AZ**. Ghi cần 4/6 bản, đọc cần 3/6 → chịu được mất **2 bản sao mà không mất khả năng ghi**, mất **3 bản** vẫn đọc được. Tự sửa lỗi (self-healing).
- Tối đa **15 Aurora Replica**, failover **dưới 30 giây**.

### 6.3.2. Các Endpoint của Aurora

- **Cluster (Writer) Endpoint:** Luôn trỏ tới instance chính (primary). Sau failover tự trỏ sang instance mới.
- **Reader Endpoint:** Tự **cân bằng tải** giữa các replica đọc.
- **Custom Endpoint:** Nhóm một tập replica cụ thể (ví dụ replica cấu hình mạnh dành cho báo cáo).

### 6.3.3. Các biến thể Aurora

| Biến thể | Mô tả | Trường hợp dùng |
|---|---|---|
| **Aurora Serverless v2** | Tự động scale **theo từng bước nhỏ (ACU)** trong vài giây, không gián đoạn; hỗ trợ Multi-AZ, replica, Global Database | Tải khó đoán, tải gián đoạn, dev/test, ứng dụng SaaS đa khách hàng |
| **Aurora Global Database** | 1 Region chính + tối đa **5 Region phụ**; nhân bản **dưới 1 giây**; **RPO ~1 giây, RTO < 1 phút** khi promote | DR đa Region, đọc toàn cầu độ trễ thấp |
| **Aurora Multi-Master** | Nhiều node ghi cùng lúc | HA cho tác vụ ghi liên tục |
| **Aurora Machine Learning** | Gọi SageMaker/Comprehend từ câu SQL | Dự đoán, phân tích cảm xúc trong DB |
| **Babelfish for Aurora PostgreSQL** | Hiểu giao thức T-SQL của SQL Server | Di chuyển ứng dụng SQL Server, ít sửa code |

**Tính năng khác:** **Backtrack** (tua ngược DB tới thời điểm trong quá khứ tới 72 giờ **không cần restore từ backup**), **Fast Database Cloning** (nhân bản cluster gần như tức thì bằng copy-on-write – lý tưởng cho môi trường test), **Aurora Auto Scaling** cho replica đọc.

## 6.4. Amazon DynamoDB

### 6.4.1. Nền tảng

- NoSQL **key-value & document**, **serverless**, độ trễ **mili-giây một chữ số** ở mọi quy mô, tự nhân bản trên **3 AZ**, HA sẵn có.
- **Primary Key** gồm: chỉ **Partition Key** (đơn giản) hoặc **Partition Key + Sort Key** (composite). Partition key quyết định phân tán dữ liệu → chọn key có **độ phân tán cao (high cardinality)** để tránh **hot partition**.
- Kích thước item tối đa **400 KB** → lưu file lớn trên S3, chỉ lưu URL trong DynamoDB.

### 6.4.2. Chế độ dung lượng (Capacity Mode)

| | **Provisioned** | **On-Demand** |
|---|---|---|
| Cách tính | Đặt trước RCU/WCU, có thể bật **Auto Scaling** | Trả theo request thực tế |
| Chi phí | Rẻ hơn khi tải ổn định, dự đoán được | Đắt hơn ~5–7 lần mỗi request nhưng **không lãng phí** |
| Dùng khi | Tải đều, biết trước | Tải bất định, spike đột ngột, ứng dụng mới, tải rất thấp |
| Burst | Có **Burst Capacity**; vượt quá → `ProvisionedThroughputExceededException` (khắc phục bằng **exponential backoff**) | Tự thích ứng |

**Công thức cần nhớ:**
- **1 WCU** = 1 ghi/giây cho item **1 KB** (ghi transactional tốn **2x**).
- **1 RCU** = 1 đọc **strongly consistent**/giây cho item **4 KB**, hoặc **2 đọc eventually consistent**/giây (transactional đọc tốn 2x). Đọc eventually consistent là **mặc định** và **rẻ bằng một nửa**.

### 6.4.3. Index

| | **LSI (Local Secondary Index)** | **GSI (Global Secondary Index)** |
|---|---|---|
| Partition key | **Giống bảng gốc** | **Khác được** |
| Sort key | Khác | Khác |
| Thời điểm tạo | **Chỉ khi tạo bảng**, không thêm/xóa sau | Tạo/xóa **bất cứ lúc nào** |
| Số lượng | 5 | 20 (mặc định, có thể tăng) |
| Dung lượng | Dùng chung RCU/WCU của bảng | **Có RCU/WCU riêng** (nếu GSI bị throttle sẽ **throttle cả bảng chính** – bẫy hay thi) |
| Tính nhất quán | Hỗ trợ strongly consistent | **Chỉ eventually consistent** |
| Giới hạn | Tổng dữ liệu mỗi partition key ≤ 10 GB | Không |

### 6.4.4. Các tính năng quan trọng khác

- **DynamoDB Accelerator (DAX):** Cache in-memory chuyên biệt, giảm độ trễ đọc xuống **micro-giây**, **không cần sửa code** (API tương thích). Khác ElastiCache: DAX chỉ dùng cho DynamoDB, tự động quản lý cache invalidation.
- **DynamoDB Streams:** Ghi lại thay đổi item (INSERT/MODIFY/REMOVE), giữ **24 giờ**, kích hoạt Lambda → dùng cho cập nhật index tìm kiếm, gửi thông báo, cập nhật bảng tổng hợp. (**Kinesis Data Streams for DynamoDB** giữ tới 1 năm.)
- **Global Tables:** Bảng đa Region **active-active** (đọc và ghi ở mọi Region), nhân bản dưới 1 giây, yêu cầu bật **DynamoDB Streams**. Giải quyết xung đột theo "**last writer wins**".
- **TTL (Time To Live):** Tự xóa item hết hạn dựa trên thuộc tính timestamp (epoch) – **miễn phí**, dùng để dọn session, log tạm, tuân thủ chính sách lưu trữ.
- **Backup:** *PITR* (khôi phục tới bất kỳ giây nào trong **35 ngày**) và *On-demand backup* (giữ vô hạn, không ảnh hưởng hiệu năng).
- **DynamoDB Transactions:** ACID trên tối đa 100 item / 4 MB.
- **Đọc dữ liệu:** `GetItem` (theo key), `Query` (theo partition key + điều kiện sort key – **hiệu quả**), `Scan` (đọc **toàn bộ bảng** – **tốn kém, nên tránh**; nếu buộc dùng thì dùng **Parallel Scan** và giới hạn bằng `ProjectionExpression`).
- **Export to S3** không tiêu tốn RCU → dùng để phân tích bằng Athena.

## 6.5. Amazon ElastiCache

| Tiêu chí | **Redis (và Valkey)** | **Memcached** |
|---|---|---|
| Cấu trúc dữ liệu | Phong phú: string, list, set, sorted set, hash, hyperloglog, geospatial | Chỉ **key-value đơn giản** |
| Nhân bản & HA | **Có** – Read Replica, **Multi-AZ với auto-failover** | **Không** |
| Bền vững (persistence) | **Có** – snapshot, AOF | **Không** (thuần cache) |
| Multi-threaded | Không (Redis chuẩn) | **Có** |
| Sharding | Cluster Mode Enabled | Sharding phía client |
| Pub/Sub, transaction, Lua script | **Có** | Không |
| Dùng khi | Leaderboard, session store, hàng đợi, cần HA & bền vững | Cache thuần, đơn giản, cần scale ngang bằng cách thêm/bớt node |

**Chiến lược caching:** *Lazy Loading / Cache-Aside* (chỉ nạp khi cache miss – tiết kiệm bộ nhớ nhưng dữ liệu có thể cũ) và *Write-Through* (ghi vào cache mỗi lần ghi DB – dữ liệu luôn mới nhưng ghi chậm hơn và tốn bộ nhớ). Kết hợp cả hai + đặt **TTL** là thực hành tốt.

**Đề thi phổ biến:** "Ứng dụng chạy trên nhiều EC2 sau ALB, người dùng bị đăng xuất khi chuyển máy" → lưu **session vào ElastiCache Redis** (hoặc DynamoDB), thay vì bật sticky session.

## 6.6. Amazon Redshift

- **Data warehouse** cột (columnar), **MPP (Massively Parallel Processing)**, dùng SQL chuẩn, tích hợp BI (QuickSight, Tableau).
- Kiến trúc: **Leader node** (nhận truy vấn, lập kế hoạch) + **Compute nodes** (thực thi song song, chia thành slice).
- **Redshift Spectrum:** Truy vấn trực tiếp dữ liệu trên **S3** mà không cần nạp vào cluster.
- **Redshift Serverless:** Không quản lý cluster, tự scale theo tải.
- **Concurrency Scaling:** Tự thêm cluster tạm khi có nhiều truy vấn đồng thời.
- **Distribution Style** (KEY, EVEN, ALL, AUTO) và **Sort Key** quyết định hiệu năng.
- **Snapshot** tự động lưu trên S3, hỗ trợ **cross-region copy**; **Redshift Multi-AZ** cho HA.
- **AQUA**, **materialized views**, **Data Sharing** giữa các cluster.
- **Không phải OLTP.** Nếu đề nói giao dịch nhiều, ghi nhỏ liên tục → RDS/Aurora/DynamoDB, không phải Redshift.

## 6.7. Các cơ sở dữ liệu chuyên biệt

| Dịch vụ | Mô hình | Đặc trưng |
|---|---|---|
| **Amazon Neptune** | Graph (Gremlin, openCypher, SPARQL) | Mạng xã hội, knowledge graph, phát hiện gian lận, gợi ý; 15 read replica, 3 AZ |
| **Amazon DocumentDB** | Document (tương thích MongoDB) | Lưu JSON, scale storage tự động tới 64 TB, nhân bản 6 bản/3 AZ |
| **Amazon Keyspaces** | Wide-column (tương thích Cassandra) | Serverless, scale tự động, dùng CQL |
| **Amazon Timestream** | Time-series | IoT, metric ứng dụng; tự tiering nóng/lạnh; nhanh hơn DB quan hệ 1.000 lần với chi phí 1/10 |
| **Amazon QLDB** | Ledger | Nhật ký bất biến, có **journal chỉ ghi thêm (append-only)**, xác thực bằng mật mã (SHA-256). Khác blockchain: **tập trung, không phi tập trung** |
| **Amazon Managed Blockchain** | Blockchain | Hyperledger Fabric, Ethereum – **phi tập trung**, nhiều bên |
| **Amazon MemoryDB for Redis** | In-memory **bền vững** | Redis nhưng **durable** (multi-AZ transaction log) → dùng làm **DB chính**, không chỉ cache |

## 6.8. Di chuyển cơ sở dữ liệu

- **AWS DMS (Database Migration Service):** Di chuyển DB với **downtime tối thiểu** – DB nguồn vẫn hoạt động trong lúc chuyển. Hỗ trợ **homogeneous** (Oracle → Oracle) và **heterogeneous** (Oracle → Aurora PostgreSQL, cần **AWS SCT – Schema Conversion Tool** để chuyển đổi schema/stored procedure trước). Có chế độ **CDC (Change Data Capture)** để đồng bộ liên tục.
- **AWS Application Migration Service (MGN):** "Lift-and-shift" máy chủ vật lý/ảo lên EC2 (thay thế SMS).
- **AWS Migration Hub:** Theo dõi tiến độ di chuyển tập trung.
- **AWS Application Discovery Service:** Thu thập thông tin hạ tầng on-prem để lập kế hoạch.
- **7R chiến lược di chuyển:** Rehost (lift & shift), Replatform (lift, tinker & shift), Repurchase (đổi sang SaaS), Refactor/Re-architect, Retire, Retain, Relocate (VMware Cloud on AWS).

# PHẦN VII. MẠNG VÀ PHÂN PHỐI NỘI DUNG

## 7.1. Amazon VPC – Virtual Private Cloud

### 7.1.1. Các thành phần nền tảng

- **VPC:** Mạng ảo riêng trong một Region, xác định bởi dải **CIDR** (từ `/16` = 65.536 IP tới `/28` = 16 IP). Dùng dải private RFC 1918: `10.0.0.0/8`, `172.16.0.0/12`, `192.168.0.0/16`. **CIDR không thể thay đổi sau khi tạo**, nhưng có thể **thêm CIDR phụ (secondary CIDR)**. Mặc định 5 VPC/Region.
- **Subnet:** Nằm trong **một AZ duy nhất**. AWS **giữ lại 5 địa chỉ IP** trong mỗi subnet: `.0` (network), `.1` (VPC router), `.2` (DNS), `.3` (dự phòng tương lai), và địa chỉ cuối cùng (broadcast). Vậy subnet `/24` chỉ có **251 IP khả dụng**.
  - **Public subnet** = subnet có route table trỏ `0.0.0.0/0` tới **Internet Gateway**.
  - **Private subnet** = không có route trực tiếp ra IGW.
- **Route Table:** Quy tắc định tuyến. Mỗi subnet liên kết đúng một route table (nếu không gán thì dùng **main route table**). Route được chọn theo nguyên tắc **longest prefix match** (route cụ thể hơn thắng).
- **Internet Gateway (IGW):** Cổng ra Internet, **HA và scale tự động**, gắn 1-1 với VPC. Instance cần **public IP hoặc EIP** mới ra Internet được qua IGW.
- **NAT Gateway:** Cho phép instance ở **private subnet** khởi tạo kết nối ra Internet (tải bản vá, gọi API) nhưng **chặn kết nối từ Internet vào**. Đặt ở **public subnet**, cần **Elastic IP**. **NAT Gateway nằm trong một AZ** → muốn HA phải **đặt một NAT Gateway ở mỗi AZ** và cấu hình route table riêng cho từng AZ. Băng thông tự scale tới 45 Gbps. **Tính phí theo giờ + theo GB xử lý** (một trong những nguồn chi phí ẩn lớn nhất).
- **NAT Instance:** Cách cũ, tự quản lý EC2 – rẻ hơn nhưng phải tự lo HA, phải **tắt Source/Destination Check**. Đề thi hỏi "quản trị tối thiểu" → NAT Gateway; "chi phí thấp nhất, chấp nhận tự quản" → NAT Instance.
- **Egress-Only Internet Gateway:** Bản NAT dành cho **IPv6** (IPv6 luôn là public nên cần cơ chế riêng để chỉ cho phép ra).

### 7.1.2. Security Group vs Network ACL – so sánh phải thuộc

| Tiêu chí | **Security Group (SG)** | **Network ACL (NACL)** |
|---|---|---|
| Cấp áp dụng | **Instance / ENI** | **Subnet** |
| Có trạng thái | **Stateful** – traffic vào được phép thì trả về tự động được phép | **Stateless** – phải mở **cả inbound và outbound** |
| Loại rule | **Chỉ ALLOW** | **ALLOW và DENY** |
| Thứ tự đánh giá | Đánh giá **tất cả rule** rồi quyết định | Theo **số thứ tự rule từ nhỏ đến lớn**, dừng ở rule khớp đầu tiên |
| Mặc định | Từ chối mọi inbound, cho phép mọi outbound | Default NACL cho phép tất cả; NACL tự tạo **từ chối tất cả** |
| Tham chiếu SG khác | **Có** (rất hữu ích: cho phép SG-web gọi SG-db) | Không, chỉ dùng CIDR |
| Số lượng gắn | Một ENI có thể gắn nhiều SG | Một subnet chỉ có **một** NACL |

**Ứng dụng thực tế:** Dùng SG cho phần lớn nhu cầu; dùng NACL khi cần **chặn (DENY) một dải IP cụ thể** – vì SG không có rule DENY.

**Ephemeral ports:** Với NACL, phải mở outbound cổng **1024–65535** để phản hồi đi ra được.

### 7.1.3. VPC Endpoints – truy cập riêng tư tới dịch vụ AWS

| Loại | Dịch vụ hỗ trợ | Cơ chế | Chi phí |
|---|---|---|---|
| **Gateway Endpoint** | **Chỉ S3 và DynamoDB** | Thêm một **route** vào route table (prefix list) | **Miễn phí** |
| **Interface Endpoint (PrivateLink)** | Hầu hết dịch vụ AWS + dịch vụ SaaS của bên thứ ba | Tạo **ENI có private IP** trong subnet, gắn Security Group | Tính phí theo giờ + theo GB |

**Ghi nhớ:** Đề hỏi "truy cập S3 từ private subnet **mà không đi qua Internet, chi phí thấp nhất**" → **Gateway Endpoint**. Nếu yêu cầu truy cập **từ on-premises qua Direct Connect/VPN** → phải dùng **Interface Endpoint** (Gateway Endpoint không dùng được từ ngoài VPC).

**AWS PrivateLink:** Cho phép expose dịch vụ của bạn (đứng sau **NLB** hoặc **GWLB**) cho hàng nghìn VPC khác mà không cần peering, không cần IGW, không lo trùng CIDR. Đây là cách mở rộng tốt nhất khi có rất nhiều consumer.

### 7.1.4. Kết nối giữa các VPC

| Giải pháp | Đặc điểm | Hạn chế |
|---|---|---|
| **VPC Peering** | Kết nối 1-1, mạng riêng, băng thông cao, không tính phí giờ (chỉ phí data transfer) | **Không transitive** (A–B, B–C thì A không nói chuyện được với C); **CIDR không được chồng lấn**; phải cập nhật route table ở cả hai phía; số lượng peering tăng theo cấp số nhân (n(n-1)/2) |
| **Transit Gateway (TGW)** | Hub trung tâm, kết nối **hàng nghìn VPC + VPN + Direct Connect**, hỗ trợ **transitive routing**, có **route table riêng** để phân đoạn mạng, hỗ trợ **multicast**, **cross-region peering** | Có phí theo giờ mỗi attachment + phí data processing |
| **VPC Sharing (AWS RAM)** | Chia sẻ subnet của một VPC cho các account khác trong Organization | Cần cùng Organization |

**Đề thi:** "Công ty có 50 VPC và 3 kết nối on-prem, cần đơn giản hóa quản lý mạng" → **Transit Gateway**.

### 7.1.5. Kết nối lai (Hybrid)

| Giải pháp | Băng thông | Độ trễ | Bảo mật | Thời gian triển khai | Chi phí |
|---|---|---|---|---|---|
| **Site-to-Site VPN** | Tới **1,25 Gbps mỗi tunnel** (dùng ECMP với TGW để gộp nhiều tunnel) | Biến thiên (qua Internet) | IPsec mã hóa | **Vài phút** | Thấp |
| **Direct Connect (DX)** | 50 Mbps – **100 Gbps** chuyên dụng | **Ổn định, thấp** | Riêng tư nhưng **không mã hóa mặc định** | **Vài tuần đến vài tháng** | Cao (nhưng giảm mạnh phí data transfer out) |
| **DX + VPN** | – | – | Riêng tư **và** mã hóa IPsec | – | Cao nhất |

- **Site-to-Site VPN** gồm **Virtual Private Gateway (VGW)** phía AWS và **Customer Gateway (CGW)** phía bạn. Mỗi kết nối có **2 tunnel ở 2 AZ khác nhau** để dự phòng.
- **Direct Connect:** Có **Dedicated Connection** (cổng vật lý riêng) và **Hosted Connection** (qua đối tác, băng thông linh hoạt).
  - **Private VIF** → truy cập VPC (private IP).
  - **Public VIF** → truy cập dịch vụ public của AWS (S3, DynamoDB) qua đường DX.
  - **Transit VIF** → kết nối tới Transit Gateway.
  - **Direct Connect Gateway:** Cho phép một DX kết nối tới **nhiều VPC ở nhiều Region** (trừ Trung Quốc).
  - **DR cho DX:** Chạy **hai kết nối DX ở hai vị trí khác nhau** (đắt nhất, tốt nhất) hoặc **DX chính + VPN dự phòng** (tiết kiệm hơn).
- **MACsec** cung cấp mã hóa lớp 2 trên DX 10/100 Gbps.

### 7.1.6. DNS trong VPC

- Hai thuộc tính VPC: `enableDnsSupport` (dùng Amazon DNS resolver tại `VPC-CIDR + 2`) và `enableDnsHostnames` (cấp DNS hostname public cho instance).
- **Route 53 Resolver Endpoints:** *Inbound endpoint* cho on-prem truy vấn DNS của AWS; *Outbound endpoint* + **Resolver Rules** để AWS chuyển tiếp truy vấn về DNS on-prem. Đây là câu trả lời cho "phân giải tên miền hai chiều giữa on-prem và VPC".
- **Route 53 Resolver DNS Firewall:** Chặn truy vấn DNS tới domain độc hại (chống data exfiltration qua DNS).

### 7.1.7. Giám sát mạng

- **VPC Flow Logs:** Ghi metadata luồng IP (không ghi nội dung payload) ở cấp **VPC / Subnet / ENI**. Xuất ra **CloudWatch Logs, S3, hoặc Kinesis Data Firehose**. Trường quan trọng: srcaddr, dstaddr, srcport, dstport, protocol, packets, bytes, **action (ACCEPT/REJECT)**, log-status. Dùng để gỡ rối kết nối (SG/NACL chặn?), phát hiện bất thường; phân tích bằng **Athena**.
  - **Bẫy:** Flow Logs **không ghi** traffic tới Amazon DNS server, DHCP, metadata service `169.254.169.254`, license activation, và traffic giữa ENI với endpoint của NLB.
- **VPC Traffic Mirroring:** Sao chép **toàn bộ gói tin** (payload) tới thiết bị phân tích – dùng cho IDS/IPS chuyên sâu.
- **VPC Reachability Analyzer:** Kiểm tra tĩnh xem gói tin có đi được từ nguồn tới đích không, chỉ ra thành phần chặn.
- **Network Access Analyzer:** Xác minh không có đường truy cập ngoài ý muốn.

### 7.1.8. Chi phí mạng cần nhớ

- Dữ liệu **vào (inbound) AWS: miễn phí**.
- Dữ liệu **ra Internet (egress): tính phí** (đây là khoản lớn nhất).
- Trong **cùng AZ, dùng private IP: miễn phí**. Dùng public IP/Elastic IP: **tính phí**.
- Giữa các **AZ trong cùng Region**: tính phí **cả hai chiều** (~0,01 USD/GB mỗi chiều).
- Giữa các **Region**: tính phí cao hơn.
- **CloudFront → Internet rẻ hơn EC2 → Internet**, và **AWS → CloudFront miễn phí** → dùng CloudFront là cách giảm chi phí egress hiệu quả.

## 7.2. Amazon Route 53

### 7.2.1. Tổng quan

DNS được quản lý, SLA khả dụng **100%** – dịch vụ duy nhất của AWS có SLA này. Đóng vai trò: đăng ký domain, phân giải DNS (authoritative), health check, và định tuyến traffic.

**Hosted Zone:** *Public* (phân giải trên Internet, 0,50 USD/tháng) và *Private* (chỉ phân giải trong VPC được liên kết).

### 7.2.2. Record Types và Alias Record

Các loại record: **A** (IPv4), **AAAA** (IPv6), **CNAME**, **NS**, **MX**, **TXT**, **PTR**, **SOA**, **SRV**, **CAA**.

**Alias vs CNAME – hay ra thi:**

| | **Alias Record** | **CNAME Record** |
|---|---|---|
| Dùng cho zone apex (`example.com`) | **Được** | **KHÔNG được** |
| Chi phí truy vấn | **Miễn phí** | Tính phí |
| Trỏ tới | Tài nguyên AWS: ELB, CloudFront, S3 static website, API Gateway, Global Accelerator, VPC endpoint, hosted zone khác cùng account | Bất kỳ tên miền nào |
| Trỏ tới EC2 DNS name | **Không** | Có |
| TTL | Do AWS quản lý | Tự đặt |
| Health check | Có thể đánh giá tình trạng target | Không |

### 7.2.3. Bảy chính sách định tuyến (Routing Policies)

| Chính sách | Cách hoạt động | Trường hợp dùng |
|---|---|---|
| **Simple** | Trả về một hoặc nhiều giá trị (client chọn ngẫu nhiên). **Không hỗ trợ health check** | Một endpoint duy nhất |
| **Weighted** | Chia traffic theo tỉ lệ trọng số (weight 0 = ngừng gửi) | **A/B testing, canary deployment**, chuyển dần traffic sang Region mới |
| **Latency-based** | Trả về Region có **độ trễ mạng thấp nhất** với người dùng | Ứng dụng đa Region cần hiệu năng tốt nhất |
| **Failover (Active-Passive)** | Trỏ về primary; khi health check fail thì chuyển sang secondary | **DR** |
| **Geolocation** | Định tuyến theo **vị trí địa lý của người dùng** (châu lục/quốc gia/bang) | Nội dung theo ngôn ngữ, hạn chế phân phối theo bản quyền, tuân thủ pháp lý |
| **Geoproximity** | Theo khoảng cách địa lý, có **bias** để mở rộng/thu hẹp vùng phục vụ | Điều chỉnh tỉ lệ traffic giữa các Region theo địa lý (cần Traffic Flow) |
| **Multivalue Answer** | Trả về tối đa 8 record khỏe mạnh, ngẫu nhiên | "Load balancing kiểu client-side" giá rẻ, **không thay thế được ELB** |
| **IP-based** | Định tuyến theo dải CIDR của client | Tối ưu theo nhà mạng (ISP) |

**Phân biệt kinh điển:** *Latency* = tối ưu **tốc độ**; *Geolocation* = tối ưu **tuân thủ/nội dung theo vùng**.

### 7.2.4. Health Checks

Ba loại: (1) giám sát **endpoint** (IP/domain), (2) giám sát **health check khác** (calculated health check), (3) giám sát **CloudWatch Alarm** (dùng cho tài nguyên private vì health checker của Route 53 nằm ngoài Internet, không truy cập được private endpoint).

Health checker đặt ở nhiều vị trí toàn cầu; endpoint bị coi là unhealthy nếu **dưới 18% checker báo khỏe**. Cần mở firewall cho dải IP của health checker.

**TTL và DNS:** TTL thấp giúp failover nhanh nhưng tăng số truy vấn (tăng chi phí). Trước khi migrate, nên hạ TTL xuống trước vài ngày.

## 7.3. Amazon CloudFront

### 7.3.1. Cơ chế

CDN với hơn **400+ Edge Location** và **Regional Edge Cache**. Cache nội dung gần người dùng → giảm độ trễ, giảm tải origin, giảm chi phí egress.

**Origin hỗ trợ:** S3 bucket (dùng **OAC**), S3 static website endpoint, ALB, EC2, MediaStore/MediaPackage, hoặc **bất kỳ HTTP server nào kể cả on-premises**.

### 7.3.2. Bảo mật CloudFront

- **Origin Access Control (OAC)** – thay thế **OAI (Origin Access Identity)** đã cũ: Cho phép **chỉ CloudFront** đọc được S3 bucket private; kết hợp bucket policy chặn truy cập trực tiếp. OAC hỗ trợ SSE-KMS và mọi Region, nên **luôn chọn OAC** trong đề thi mới.
- **Signed URL** (một file) và **Signed Cookies** (nhiều file) để phân phối nội dung trả phí có thời hạn. So với **S3 Presigned URL**: CloudFront Signed URL đi qua cache và hỗ trợ hạn chế theo IP/thời gian, dùng cho phân phối toàn cầu.
- **Geo Restriction:** Whitelist/blacklist theo quốc gia.
- **Tích hợp AWS WAF và Shield** ngay tại edge → chặn tấn công trước khi tới origin.
- **Field-Level Encryption:** Mã hóa trường nhạy cảm (số thẻ) ngay tại edge bằng public key, chỉ ứng dụng có private key giải mã được.
- **HTTPS:** Viewer protocol policy (redirect-to-https, https-only) và origin protocol policy. Chứng chỉ tùy chỉnh phải ở **ACM Region us-east-1**.

### 7.3.3. Caching

- **Cache key** mặc định gồm host + path. Có thể thêm **headers, cookies, query strings** vào cache key qua **Cache Policy** – nhưng thêm càng nhiều thì **cache hit ratio càng thấp**.
- **Origin Request Policy:** Quyết định thông tin nào **chuyển tiếp tới origin** mà **không** đưa vào cache key.
- **TTL:** Điều khiển bằng header `Cache-Control: max-age`, `Expires` từ origin, hoặc Minimum/Maximum/Default TTL trong CloudFront.
- **Invalidation:** Xóa cache thủ công theo path (`/images/*`) – 1.000 path miễn phí mỗi tháng. Thực hành tốt hơn: **versioned filenames** (`style.v2.css`).
- **Origin Groups:** Origin chính + origin dự phòng → **failover tự động** khi origin chính lỗi.
- **Price Class:** `All` (mọi edge), `200` (bỏ Nam Mỹ, Úc, NZ), `100` (chỉ Bắc Mỹ & châu Âu – **rẻ nhất**).

### 7.3.4. CloudFront Functions vs Lambda@Edge

| | **CloudFront Functions** | **Lambda@Edge** |
|---|---|---|
| Chạy tại | Edge Location (218+) | Regional Edge Cache |
| Ngôn ngữ | JavaScript (ECMAScript 5.1) | Node.js, Python |
| Thời gian chạy tối đa | **< 1 mili-giây** | 5 giây (viewer) / 30 giây (origin) |
| Bộ nhớ | 2 MB | Tới 10 GB |
| Truy cập mạng / hệ thống file | **Không** | **Có** |
| Trigger | Viewer request/response | Viewer request/response, **Origin request/response** |
| Chi phí | Rẻ hơn ~1/6 | Cao hơn |
| Dùng cho | Ghi/đổi header, redirect URL, kiểm tra token đơn giản, chuẩn hóa cache key | Gọi API bên ngoài, xử lý ảnh, truy vấn DB, logic phức tạp, A/B testing sâu |

## 7.4. AWS Global Accelerator

- Cấp **2 địa chỉ IP Anycast tĩnh** (hoặc mang IP của bạn – BYOIP), đưa traffic vào **mạng đường trục toàn cầu của AWS ngay tại edge gần nhất** → cải thiện hiệu năng **60%**, failover **dưới 1 phút** (không phụ thuộc TTL DNS).
- **Hoạt động ở Layer 4 (TCP/UDP)** – khác CloudFront (Layer 7, HTTP).
- Endpoint hỗ trợ: **ALB, NLB, EC2, Elastic IP**.
- Hỗ trợ **traffic dial** (chia % traffic theo endpoint group) và **weight** cho từng endpoint.

**So sánh với CloudFront:**

| | **CloudFront** | **Global Accelerator** |
|---|---|---|
| Bản chất | CDN – **cache nội dung** tại edge | **Tăng tốc mạng** – không cache |
| Giao thức | HTTP/HTTPS | **TCP và UDP** (mọi giao thức) |
| IP tĩnh | Không | **Có, 2 IP Anycast** |
| Tối ưu cho | Nội dung tĩnh & động qua HTTP | Game, VoIP, IoT, MQTT, ứng dụng non-HTTP, cần IP tĩnh, failover nhanh đa Region |

**Dùng cả hai:** CloudFront cho nội dung web, Global Accelerator cho API/game backend.

# PHẦN VIII. TÍCH HỢP ỨNG DỤNG VÀ KIẾN TRÚC PHI TẬP TRUNG

Nguyên tắc cốt lõi của kiến trúc bền bỉ: **decoupling (tách rời)**. Thay vì để thành phần A gọi trực tiếp thành phần B (tight coupling – B chết thì A chết theo), ta chèn một lớp trung gian (queue, topic, event bus, load balancer).

## 8.1. Amazon SQS – Simple Queue Service

### 8.1.1. Hai loại hàng đợi

| | **Standard Queue** | **FIFO Queue** |
|---|---|---|
| Throughput | **Gần như không giới hạn** | 300 msg/s (3.000 nếu batch 10); **high throughput mode**: tới 70.000 msg/s |
| Thứ tự | Cố gắng giữ thứ tự, **không đảm bảo** | **Đảm bảo thứ tự tuyệt đối** trong mỗi Message Group |
| Trùng lặp | **At-least-once** (có thể nhận trùng) → ứng dụng phải **idempotent** | **Exactly-once**, khử trùng lặp trong **5 phút** (dựa vào `MessageDeduplicationId` hoặc content-based hash) |
| Tên hàng đợi | Bất kỳ | Phải kết thúc bằng **`.fifo`** |

### 8.1.2. Các tham số phải nhớ

| Tham số | Mặc định | Khoảng giá trị | Ý nghĩa |
|---|---|---|---|
| **Visibility Timeout** | 30 giây | 0 giây – **12 giờ** | Thời gian message bị "ẩn" sau khi được nhận. Nếu consumer không xóa kịp → message xuất hiện lại → **xử lý trùng**. Gọi `ChangeMessageVisibility` để gia hạn |
| **Message Retention** | 4 ngày | 60 giây – **14 ngày** | Thời gian giữ message chưa xử lý |
| **Kích thước message** | – | Tối đa **256 KB** | Lớn hơn → dùng **SQS Extended Client Library** lưu payload trên **S3** |
| **Long Polling (`ReceiveMessageWaitTimeSeconds`)** | 0 (short polling) | 1 – **20 giây** | **Giảm số lời gọi API rỗng → giảm chi phí và độ trễ**. Luôn nên bật |
| **Delay Queue** | 0 | 0 – **15 phút** | Hoãn hiển thị message |
| **Message in flight** | – | 120.000 (standard) / 20.000 (FIFO) | Số message đang được xử lý |

### 8.1.3. Dead Letter Queue (DLQ)

Sau khi message bị nhận và trả lại quá **`maxReceiveCount`** lần (Redrive Policy), nó được chuyển sang DLQ để điều tra thay vì chặn hàng đợi. DLQ của Standard queue phải là Standard, của FIFO phải là FIFO. Có **Redrive to Source** để đưa message trở lại sau khi sửa lỗi. **Nên đặt retention của DLQ là 14 ngày.**

### 8.1.4. Mẫu kiến trúc với SQS

- **Buffer chống spike:** Web tier ghi vào SQS, worker tier (ASG) đọc ra xử lý → traffic đột biến không làm sập backend.
- **Scale worker theo độ sâu hàng đợi:** Dùng metric CloudWatch **`ApproximateNumberOfMessagesVisible`** hoặc tốt hơn là **backlog per instance** để kích hoạt Auto Scaling. Đây là đáp án chuẩn cho câu hỏi "scale worker tier như thế nào".
- **Bảo mật:** Mã hóa SSE bằng KMS, kiểm soát bằng SQS Access Policy (resource-based).

## 8.2. Amazon SNS – Simple Notification Service

- Mô hình **pub/sub**: một message publish tới **Topic** được gửi tới **tất cả subscriber** (tối đa 12,5 triệu subscription/topic).
- Subscriber: **SQS, Lambda, HTTP/HTTPS endpoint, Email, SMS, Mobile Push (APNs, FCM), Kinesis Data Firehose**.
- **Standard Topic** và **FIFO Topic** (chỉ subscriber là SQS FIFO).
- **Message Filtering:** Filter policy dựa trên message attribute → mỗi subscriber chỉ nhận loại message nó quan tâm.
- **Fan-out Pattern (rất hay ra thi):** SNS Topic → nhiều SQS Queue. Ưu điểm: mỗi hệ thống hạ nguồn xử lý theo tốc độ riêng, có persistence, có retry và DLQ. Ví dụ: một sự kiện "đơn hàng mới" đẩy đồng thời sang queue thanh toán, queue kho, queue phân tích.
- **Message Data Protection:** Phát hiện và che dữ liệu nhạy cảm trong message.

## 8.3. Amazon EventBridge (trước là CloudWatch Events)

- **Serverless event bus** – định tuyến sự kiện dựa trên **rule khớp nội dung sự kiện (event pattern JSON)**.
- Ba loại bus: **Default** (sự kiện AWS service), **Custom** (ứng dụng của bạn), **Partner** (SaaS: Datadog, Zendesk, Shopify, Auth0).
- **Schema Registry:** Tự khám phá cấu trúc sự kiện và sinh code binding.
- **Scheduled Rules:** cron/rate expression – thay thế cron server.
- **Archive & Replay:** Lưu sự kiện và phát lại để debug hoặc khôi phục.
- **Pipes:** Kết nối point-to-point nguồn → đích với filter và enrichment.
- Hơn 20 loại target: Lambda, SQS, SNS, Step Functions, ECS task, Kinesis, SSM, API destination (HTTP endpoint bên ngoài).

**SNS vs EventBridge:** SNS đơn giản, throughput cực cao, độ trễ thấp, filter theo attribute. EventBridge mạnh về **định tuyến theo nội dung, tích hợp SaaS, schema, archive/replay** nhưng throughput thấp hơn và độ trễ cao hơn một chút. Đề nói "event-driven, filter phức tạp, tích hợp bên thứ ba, ít vận hành" → **EventBridge**.

## 8.4. AWS Step Functions

Điều phối workflow bằng **state machine** định nghĩa bằng **ASL (Amazon States Language – JSON)**. Có giao diện đồ họa theo dõi từng bước.

- **State types:** Task, Choice (rẽ nhánh), Parallel, Map (lặp qua mảng), Wait, Pass, Succeed, Fail.
- **Tích hợp sẵn** với Lambda, ECS, Batch, DynamoDB, SNS, SQS, Glue, SageMaker, EMR, API Gateway.
- **Xử lý lỗi tập trung:** `Retry` (với backoff) và `Catch` – không cần viết logic retry trong code.
- **Hai loại workflow:**
  - **Standard:** Chạy tới **1 năm**, **exactly-once**, có lịch sử thực thi đầy đủ, trả tiền theo state transition. Dùng cho quy trình nghiệp vụ dài, cần audit.
  - **Express:** Chạy tối đa **5 phút**, **at-least-once**, throughput cực cao (100.000 lần khởi chạy/giây), trả tiền theo số lần chạy và thời lượng. Dùng cho xử lý stream, IoT, backend di động.
- **Callback pattern (`waitForTaskToken`)**: Chờ hệ thống ngoài hoặc **con người phê duyệt** rồi mới tiếp tục.

**Đề thi:** "Quy trình nhiều bước có điều kiện, cần retry, cần thấy trạng thái từng bước, phối hợp nhiều Lambda" → **Step Functions** (không phải chuỗi Lambda gọi nhau).

## 8.5. Amazon API Gateway

- Tạo, quản lý, bảo vệ API ở quy mô lớn. Ba loại: **REST API** (đầy đủ tính năng), **HTTP API** (rẻ hơn ~70%, độ trễ thấp hơn, ít tính năng hơn – dùng cho proxy Lambda đơn giản), **WebSocket API** (hai chiều, real-time chat/game).
- **Endpoint types:** *Edge-optimized* (qua CloudFront, cho người dùng toàn cầu), *Regional*, *Private* (chỉ truy cập trong VPC qua Interface Endpoint).
- **Xác thực/ủy quyền:** IAM (SigV4), **Cognito User Pools**, **Lambda Authorizer** (custom, hỗ trợ OAuth/JWT/bên thứ ba), API Keys (dùng kèm **Usage Plans** để giới hạn quota).
- **Throttling:** Mặc định **10.000 request/giây, burst 5.000** mỗi Region – bảo vệ backend. Vượt quá trả về **HTTP 429 Too Many Requests**.
- **Caching:** Cache response ở API Gateway (TTL mặc định 300s, 0–3600s) → giảm tải backend. Có thể invalidate bằng header `Cache-Control: max-age=0` (nên yêu cầu quyền IAM).
- **Stages** (dev/test/prod) + **Stage Variables**, **Canary Deployment** để chuyển dần % traffic.
- **Mapping Templates (VTL)** để chuyển đổi request/response (ví dụ XML ↔ JSON).
- **Integration types:** Lambda proxy, HTTP, AWS service (ví dụ đẩy thẳng vào SQS/Kinesis không cần Lambda), Mock, VPC Link (tới NLB trong private VPC).

## 8.6. Streaming dữ liệu – Amazon Kinesis

| Dịch vụ | Chức năng | Đặc điểm cần nhớ |
|---|---|---|
| **Kinesis Data Streams** | Thu nhận stream **real-time**, tự quản consumer | Chia **shard**: mỗi shard 1 MB/s ghi, 2 MB/s đọc, 1.000 record/s ghi. Giữ dữ liệu **24 giờ (mặc định) tới 365 ngày** → **có thể replay**. Đảm bảo thứ tự trong shard theo **partition key**. Hai chế độ: **Provisioned** và **On-Demand** |
| **Kinesis Data Firehose** | **Nạp (deliver)** stream vào đích, fully managed | **Near real-time** (buffer theo kích thước 1–128 MB hoặc thời gian 0–900 giây). Đích: **S3, Redshift, OpenSearch, Splunk, HTTP endpoint**. **Không lưu trữ, không replay**. Tự động scale, có thể chuyển đổi dữ liệu bằng Lambda và chuyển sang Parquet/ORC |
| **Managed Service for Apache Flink** (trước là Kinesis Data Analytics) | Phân tích stream bằng **SQL hoặc Apache Flink** theo thời gian thực | Tính toán cửa sổ trượt, phát hiện bất thường |
| **Kinesis Video Streams** | Stream video từ thiết bị lên AWS | Phân tích video bằng Rekognition |
| **Amazon MSK** | Apache Kafka được quản lý | Khi tổ chức **đã dùng Kafka**; message tới **1 MB mặc định, cấu hình được tới 10 MB** (Kinesis giới hạn 1 MB); lưu trữ **không giới hạn thời gian** |

**SQS vs Kinesis – phân biệt kinh điển:**

| | **SQS** | **Kinesis Data Streams** |
|---|---|---|
| Mô hình | Hàng đợi, **message bị xóa sau khi xử lý** | Stream, dữ liệu **giữ lại và có thể đọc lại nhiều lần** |
| Số consumer | Mỗi message cho **một** consumer | **Nhiều ứng dụng đọc song song cùng dữ liệu** |
| Replay | Không | **Có** |
| Thứ tự | Chỉ FIFO queue | Đảm bảo trong shard |
| Scale | Tự động, không cần nghĩ | Phải quản lý shard (trừ On-Demand) |
| Dùng khi | Decouple, buffer công việc | Analytics real-time, log/clickstream/IoT, nhiều consumer |

## 8.7. Amazon MQ và AppSync

- **Amazon MQ:** Message broker được quản lý cho **ActiveMQ / RabbitMQ**, hỗ trợ giao thức chuẩn **MQTT, AMQP, STOMP, JMS, OpenWire**. Dùng khi **di chuyển ứng dụng cũ on-premises** đã dùng các giao thức này mà không muốn viết lại code sang SQS/SNS. Không scale bằng SQS/SNS; chạy trên broker cụ thể, có Multi-AZ active/standby.
- **AWS AppSync:** GraphQL được quản lý, hợp nhất dữ liệu từ DynamoDB, Aurora, Lambda, OpenSearch, HTTP; hỗ trợ **real-time subscription** và **offline sync** cho ứng dụng di động.

# PHẦN IX. PHÂN TÍCH DỮ LIỆU

| Dịch vụ | Vai trò | Điểm cần nhớ cho kỳ thi |
|---|---|---|
| **Amazon Athena** | Truy vấn **SQL serverless** trực tiếp trên S3 | Trả tiền **theo lượng dữ liệu quét** (~5 USD/TB). Tối ưu chi phí: dùng định dạng **cột (Parquet/ORC)**, **nén**, **partition** dữ liệu → giảm tới 90% chi phí. Dùng cho phân tích ad-hoc, truy vấn log (VPC Flow Logs, CloudTrail, ALB logs) |
| **AWS Glue** | **ETL serverless** + **Data Catalog** | **Glue Crawler** tự quét S3 để suy ra schema và điền vào **Data Catalog** (metastore dùng chung cho Athena, Redshift Spectrum, EMR). **Glue DataBrew** cho làm sạch dữ liệu không cần code |
| **Amazon EMR** | Cụm Hadoop/Spark/Hive/Presto/HBase được quản lý | Dùng khi cần xử lý **big data phức tạp, tùy chỉnh sâu**; kết hợp **Spot Instance cho task node** để tiết kiệm. Có EMR Serverless |
| **Amazon OpenSearch Service** | Tìm kiếm toàn văn + phân tích & trực quan hóa log | Kết hợp Firehose để nạp log; có **OpenSearch Dashboards**; hỗ trợ **UltraWarm/Cold storage** cho log cũ để giảm chi phí |
| **Amazon QuickSight** | BI/dashboard serverless | **SPICE** – engine in-memory tăng tốc; hỗ trợ ML Insights, **row-level security**, nhúng dashboard |
| **AWS Lake Formation** | Xây dựng **data lake** trên S3, quản lý quyền tập trung | Phân quyền tới **cấp cột, hàng, ô** cho Athena/Redshift/EMR |
| **AWS Data Exchange** | Mua/đăng ký dữ liệu bên thứ ba | – |
| **Amazon Kinesis** | Xem Phần VIII | – |

**Kiến trúc data lake điển hình:** Nguồn dữ liệu → **Kinesis Firehose** → **S3 (raw zone)** → **Glue ETL** chuyển sang Parquet → **S3 (curated zone)** → **Glue Data Catalog** → **Athena / Redshift Spectrum** → **QuickSight**.

# PHẦN X. GIÁM SÁT, VẬN HÀNH VÀ TỰ ĐỘNG HÓA

## 10.1. Amazon CloudWatch

### 10.1.1. Metrics

- Metric mặc định lấy **mỗi 5 phút** (basic monitoring, miễn phí). Bật **Detailed Monitoring** để lấy **mỗi 1 phút** (có phí). **High-resolution custom metric** có thể tới **1 giây**.
- **Metric mà EC2 KHÔNG tự gửi (bẫy kinh điển):** **Memory (RAM) usage** và **Disk space utilization**. Phải cài **CloudWatch Agent** (hoặc unified agent) để thu thập. EC2 chỉ tự gửi: CPUUtilization, NetworkIn/Out, DiskReadOps/WriteOps (của instance store), StatusCheck.
- **Custom Metrics:** Gửi bằng `PutMetricData`.
- **Metric Math** để tính toán tổng hợp; **Anomaly Detection** dùng ML tạo dải kỳ vọng.

### 10.1.2. Logs

- **Log Group** → **Log Stream** → **Log Event**. Retention mặc định là **Never Expire** (nên đặt hạn để tiết kiệm chi phí).
- Nguồn: CloudWatch Agent, Lambda, ECS, API Gateway, VPC Flow Logs, Route 53, CloudTrail.
- **Metric Filters:** Biến pattern trong log thành metric → gắn alarm (ví dụ đếm số lần "ERROR" hoặc số lần đăng nhập root thất bại).
- **CloudWatch Logs Insights:** Ngôn ngữ truy vấn để phân tích log tương tác.
- **Subscription Filters:** Đẩy log real-time sang Kinesis Data Streams, Firehose, Lambda, OpenSearch.
- **Export to S3** cho lưu trữ dài hạn giá rẻ (chậm, tới 12 giờ).

### 10.1.3. Alarms

Ba trạng thái: `OK`, `ALARM`, `INSUFFICIENT_DATA`. Hành động: gửi **SNS**, kích hoạt **Auto Scaling**, **EC2 Action** (stop/terminate/reboot/recover).

**EC2 Auto Recovery:** Alarm trên `StatusCheckFailed_System` → tự khôi phục instance sang phần cứng mới, **giữ nguyên instance ID, private IP, Elastic IP, metadata**. Lưu ý phân biệt: `StatusCheckFailed_Instance` (lỗi phần mềm/OS – cần reboot), `StatusCheckFailed_System` (lỗi hạ tầng AWS – cần recover).

**Composite Alarms:** Kết hợp nhiều alarm bằng AND/OR → giảm nhiễu cảnh báo.

### 10.1.4. Các thành phần khác

- **CloudWatch Dashboards:** Bảng điều khiển tùy chỉnh, **cross-region và cross-account**.
- **CloudWatch Synthetics Canary:** Script mô phỏng người dùng gọi endpoint theo lịch → phát hiện lỗi trước khi người dùng thật gặp.
- **CloudWatch RUM:** Thu thập hiệu năng thực tế từ trình duyệt người dùng.
- **CloudWatch Contributor Insights:** Tìm "top talker" gây tải cao.
- **Amazon Managed Service for Prometheus / Grafana:** Cho môi trường container/K8s.

## 10.2. AWS X-Ray

**Distributed tracing** – theo dõi request đi qua nhiều dịch vụ (microservices), dựng **Service Map**, đo độ trễ từng đoạn, tìm nút thắt cổ chai và lỗi. Cần cài **X-Ray SDK** trong code và **X-Ray daemon** (hoặc bật tracing với Lambda/API Gateway). Đề nói "tìm xem microservice nào gây chậm" → **X-Ray**.

## 10.3. AWS Systems Manager (SSM)

Bộ công cụ vận hành tập trung cho cả EC2 và máy on-premises (cần cài **SSM Agent** + IAM role phù hợp).

| Tính năng | Công dụng |
|---|---|
| **Session Manager** | SSH/RDP vào instance **không cần mở port 22, không cần bastion host, không cần key pair**; có log phiên làm việc. **Đáp án chuẩn cho câu hỏi "truy cập an toàn nhất vào EC2 trong private subnet"** |
| **Patch Manager** | Tự động vá lỗi hàng loạt theo baseline và maintenance window |
| **Run Command** | Chạy lệnh/script trên hàng loạt instance, không cần SSH |
| **Parameter Store** | Lưu cấu hình và secret (SecureString) |
| **State Manager** | Duy trì trạng thái cấu hình mong muốn |
| **Automation** | Runbook tự động hóa tác vụ vận hành (tạo AMI, khởi động lại dịch vụ) |
| **Inventory** | Kiểm kê phần mềm cài trên instance |
| **Fleet Manager** | Quản lý toàn bộ đội máy chủ qua giao diện |
| **OpsCenter** | Tập trung xử lý sự cố vận hành |

## 10.4. Infrastructure as Code

- **AWS CloudFormation:** Định nghĩa hạ tầng bằng **YAML/JSON template**. Khái niệm: **Stack**, **Change Sets** (xem trước thay đổi), **StackSets** (triển khai một stack ra **nhiều account và nhiều Region** cùng lúc), **Drift Detection** (phát hiện thay đổi thủ công), **Nested Stacks**, **Rollback tự động** khi lỗi. Các section trong template: `Parameters`, `Mappings`, `Conditions`, `Resources` (bắt buộc), `Outputs`, `Transform`. **DeletionPolicy: Retain/Snapshot** để giữ dữ liệu khi xóa stack.
- **AWS CDK (Cloud Development Kit):** Viết hạ tầng bằng ngôn ngữ lập trình (TypeScript, Python, Java, C#) rồi biên dịch ra CloudFormation.
- **AWS SAM:** Framework tối giản cho ứng dụng serverless.
- **AWS Service Catalog:** Danh mục sản phẩm hạ tầng đã được phê duyệt để người dùng tự phục vụ trong khuôn khổ cho phép.
- **AWS Proton:** Quản lý template hạ tầng cho đội platform và developer.
- **AWS Amplify:** Nền tảng phát triển frontend/full-stack (hosting, auth, API, storage).

## 10.5. CI/CD trên AWS

**CodeCommit** (Git repo) → **CodeBuild** (build & test) → **CodeDeploy** (triển khai lên EC2/ECS/Lambda/on-prem, hỗ trợ **In-place** và **Blue/Green**) → **CodePipeline** (điều phối toàn bộ). **CodeArtifact** lưu package dependency. **CodeGuru** review code và hồ sơ hiệu năng bằng ML.

## 10.6. Hỗ trợ và tối ưu

- **AWS Trusted Advisor:** Kiểm tra theo 5 nhóm: **Cost Optimization, Performance, Security, Fault Tolerance, Service Limits**. Gói Basic/Developer chỉ có 7 kiểm tra cốt lõi; **Business/Enterprise mở khóa toàn bộ** + API + tích hợp EventBridge.
- **AWS Health Dashboard (Personal Health Dashboard):** Cảnh báo sự cố AWS **ảnh hưởng trực tiếp tới tài nguyên của bạn**, kèm lịch bảo trì.
- **AWS Compute Optimizer:** Dùng ML phân tích metric để đề xuất right-sizing cho EC2, ASG, EBS, Lambda.
- **AWS Well-Architected Tool:** Đánh giá kiến trúc theo 6 trụ cột và đưa ra khuyến nghị cải thiện.
- **AWS Resource Access Manager (RAM):** Chia sẻ tài nguyên (subnet, Transit Gateway, License, Route 53 Resolver Rules) giữa các account trong Organization.

# PHẦN XI. WELL-ARCHITECTED FRAMEWORK VÀ THIẾT KẾ KIẾN TRÚC

## 11.1. Sáu trụ cột (Six Pillars)

| Trụ cột | Câu hỏi trung tâm | Nguyên tắc thiết kế chính |
|---|---|---|
| **1. Operational Excellence** (Xuất sắc vận hành) | Chúng ta vận hành và cải tiến hệ thống như thế nào? | Hạ tầng dưới dạng code (IaC), thay đổi nhỏ và có thể đảo ngược, tự động hóa, rút kinh nghiệm từ sự cố, dự đoán lỗi |
| **2. Security** (Bảo mật) | Dữ liệu và hệ thống được bảo vệ ra sao? | Nền tảng danh tính vững chắc, khả năng truy vết (traceability), **bảo mật ở mọi tầng (defense in depth)**, tự động hóa bảo mật, mã hóa khi lưu và khi truyền, giảm thiểu truy cập trực tiếp của con người, chuẩn bị cho sự cố |
| **3. Reliability** (Độ tin cậy) | Hệ thống có phục hồi được sau lỗi không? | **Tự động khôi phục sau lỗi**, kiểm thử quy trình phục hồi, **scale ngang** để tăng khả dụng, ngừng đoán dung lượng, quản lý thay đổi bằng tự động hóa |
| **4. Performance Efficiency** (Hiệu quả hiệu năng) | Có dùng đúng tài nguyên cho đúng việc không? | Dân chủ hóa công nghệ tiên tiến, triển khai toàn cầu trong vài phút, dùng **serverless**, thử nghiệm thường xuyên, **mechanical sympathy** (chọn công nghệ hợp bản chất bài toán) |
| **5. Cost Optimization** (Tối ưu chi phí) | Có đang trả tiền thừa không? | Áp dụng mô hình tiêu dùng theo nhu cầu, đo lường hiệu quả tổng thể, ngừng chi cho việc nâng đỡ data center, phân tích và quy trách nhiệm chi tiêu, dùng dịch vụ managed |
| **6. Sustainability** (Bền vững) | Tác động môi trường ra sao? | Hiểu tác động, đặt mục tiêu, tối đa hóa hiệu suất sử dụng, dùng phần cứng hiệu quả hơn (Graviton), giảm dữ liệu và tài nguyên dư thừa |

## 11.2. Nguyên tắc thiết kế cần khắc cốt ghi tâm

1. **Design for failure – "Everything fails, all the time" (Werner Vogels).** Giả định mọi thành phần sẽ hỏng, và thiết kế để hệ thống vẫn hoạt động.
2. **Loose coupling.** Dùng SQS/SNS/EventBridge/ELB làm lớp trung gian.
3. **Stateless application tier.** Đẩy state ra ElastiCache/DynamoDB/S3 để có thể scale ngang và thay thế instance tự do.
4. **Multi-AZ là mặc định cho production.** Multi-Region khi có yêu cầu DR nghiêm ngặt hoặc phục vụ toàn cầu.
5. **Automate everything.** IaC, Auto Scaling, Auto Recovery, tự động vá lỗi.
6. **Elasticity thay vì over-provisioning.** Scale theo nhu cầu thật.
7. **Security in depth.** IAM least privilege + SG + NACL + WAF + mã hóa + audit.
8. **Loại bỏ single point of failure (SPOF).** Hỏi: "Nếu thành phần này chết, hệ thống có sống không?"

## 11.3. Chiến lược Disaster Recovery – RTO và RPO

- **RPO (Recovery Point Objective):** Chấp nhận **mất bao nhiêu dữ liệu** (đo ngược từ thời điểm sự cố về bản backup gần nhất). RPO nhỏ → phải backup/replicate thường xuyên hơn.
- **RTO (Recovery Time Objective):** Chấp nhận **hệ thống ngừng bao lâu** trước khi hoạt động trở lại.

| Chiến lược | RPO | RTO | Chi phí | Mô tả |
|---|---|---|---|---|
| **Backup & Restore** | Giờ | **24 giờ hoặc hơn** | **Thấp nhất** | Chỉ sao lưu dữ liệu sang Region khác (snapshot, S3 CRR, AWS Backup). Khi có sự cố mới dựng hạ tầng từ CloudFormation |
| **Pilot Light** | Phút | **Hàng chục phút – vài giờ** | Thấp | Chỉ chạy **các thành phần lõi** (thường là **database luôn được replicate**), tầng compute tắt hoặc chưa tạo. Khi có sự cố thì "bật đèn lên" và scale |
| **Warm Standby** | Giây | **Vài phút** | Trung bình–cao | Chạy **bản sao thu nhỏ nhưng đầy đủ chức năng** ở Region phụ. Khi sự cố thì **scale up** và chuyển DNS |
| **Multi-Site / Hot Standby (Active-Active)** | **Gần 0** | **Gần 0** | **Cao nhất** | Hai Region cùng phục vụ traffic thật. Dùng Route 53 latency/weighted, Aurora Global Database, DynamoDB Global Tables |

**Ghi nhớ quy tắc chọn:** RTO/RPO càng nhỏ → chi phí càng cao. Đề bài luôn cho biết ràng buộc; hãy chọn chiến lược **rẻ nhất vẫn đáp ứng** RTO/RPO đó.

Các công cụ hỗ trợ: **AWS Elastic Disaster Recovery (DRS)** – nhân bản liên tục máy chủ với RPO tính bằng giây, RTO tính bằng phút; **AWS Backup** cho cross-region/cross-account; **Route 53 Failover Routing** + health check để chuyển traffic.

# PHẦN XII. TỐI ƯU CHI PHÍ

## 12.1. Công cụ quản lý chi phí

| Công cụ | Công dụng |
|---|---|
| **AWS Cost Explorer** | Trực quan hóa chi phí quá khứ (13 tháng), **dự báo 12 tháng tới**, đưa ra khuyến nghị mua RI/Savings Plans, phân tích theo dịch vụ/tag/account |
| **AWS Budgets** | Đặt ngân sách (chi phí, mức sử dụng, RI/SP utilization & coverage) và **cảnh báo qua SNS/email** khi vượt hoặc **dự báo sẽ vượt**. Có **Budget Actions** để tự áp policy chặn chi tiêu |
| **AWS Cost and Usage Report (CUR)** | Báo cáo chi tiết nhất, xuất ra S3, phân tích bằng Athena/QuickSight/Redshift |
| **Cost Allocation Tags** | Gắn tag (AWS-generated hoặc user-defined) và **kích hoạt trong Billing console** để phân tách chi phí theo phòng ban/dự án |
| **AWS Pricing Calculator** | Ước tính chi phí trước khi triển khai |
| **AWS Compute Optimizer** | Đề xuất right-sizing dựa trên ML |
| **AWS Cost Anomaly Detection** | ML phát hiện chi tiêu bất thường và cảnh báo |
| **Savings Plans / Reserved Instances Reports** | Theo dõi mức tận dụng cam kết |
| **Billing Alarms (CloudWatch)** | Metric `EstimatedCharges` – **chỉ có ở Region us-east-1** |

## 12.2. Danh sách kiểm tra tối ưu chi phí (áp dụng trong đề thi)

**Compute:**
- Dùng **Savings Plans / RI** cho tải nền chạy 24/7; **Spot** cho batch/stateless; **On-Demand** cho phần biến động.
- **Right-sizing:** giảm kích cỡ instance thừa (Compute Optimizer).
- Chuyển sang **Graviton (ARM)** – hiệu năng/giá tốt hơn tới 40%.
- **Tắt tài nguyên dev/test ngoài giờ làm việc** (Instance Scheduler / EventBridge + Lambda).
- Chuyển sang **serverless (Lambda/Fargate)** nếu tải không liên tục.
- Xóa **EBS volume không gắn**, **snapshot cũ**, **Elastic IP không dùng**, **load balancer nhàn rỗi**.

**Storage:**
- Áp **S3 Lifecycle Policy** chuyển sang IA/Glacier; bật **Intelligent-Tiering** nếu mẫu truy cập khó đoán.
- Xóa **incomplete multipart uploads**.
- Chuyển **gp2 → gp3** (rẻ hơn ~20% với hiệu năng tốt hơn).
- Dùng **EFS Lifecycle Management** chuyển sang IA/Archive.
- Bật **S3 Storage Lens** để tìm điểm lãng phí.

**Mạng:**
- Dùng **CloudFront** giảm phí egress; dùng **VPC Gateway Endpoint (miễn phí)** thay vì NAT Gateway khi chỉ truy cập S3/DynamoDB.
- Gộp **NAT Gateway** hợp lý (nhưng cân nhắc phí liên AZ).
- Giữ traffic **trong cùng AZ** khi có thể.

**Database:**
- **Aurora Serverless v2** cho tải bất định; **DynamoDB On-Demand** cho tải không đoán trước.
- Dùng **Read Replica** thay vì scale up instance chính.
- Dùng **ElastiCache** để giảm số truy vấn tới DB.
- Xóa snapshot thủ công cũ.

**Tổ chức:**
- **Consolidated Billing** để hưởng volume discount và chia sẻ RI/SP.
- Chuẩn hóa **tagging** và bật cost allocation tags.
- Đặt **AWS Budgets** cho từng đội.

# PHẦN XIII. CÁC MẪU KIẾN TRÚC ĐIỂN HÌNH TRONG ĐỀ THI

## 13.1. Web application 3 tầng có tính sẵn sàng cao

Route 53 (alias) → CloudFront (cache tĩnh + WAF) → **ALB trải trên ≥ 2 AZ (public subnet)** → **Auto Scaling Group EC2/ECS trong private subnet ở ≥ 2 AZ** → **RDS Multi-AZ (private subnet, DB subnet group ≥ 2 AZ)** + **ElastiCache** cho session/cache + **S3** cho nội dung tĩnh và upload. NAT Gateway ở mỗi AZ cho private subnet ra Internet. Secrets Manager giữ mật khẩu DB. CloudWatch + X-Ray giám sát.

## 13.2. Kiến trúc serverless

CloudFront → **S3 (frontend tĩnh)**; API: **API Gateway → Lambda → DynamoDB**; xác thực bằng **Cognito**; xử lý bất đồng bộ qua **SQS/EventBridge → Lambda**; workflow bằng **Step Functions**; file upload dùng **S3 presigned URL** rồi kích hoạt Lambda qua **S3 Event Notification**.

## 13.3. Xử lý ảnh/video khi upload

Người dùng upload lên **S3** → **S3 Event Notification** → **SQS** (buffer, chống mất event) → **Lambda** hoặc **ECS/Fargate worker** (nếu > 15 phút thì dùng **AWS Batch** hoặc **MediaConvert**) → lưu kết quả về S3 → cập nhật metadata vào **DynamoDB** → phân phối qua **CloudFront**.

## 13.4. Phân tích log tập trung

Nguồn (EC2 agent, VPC Flow Logs, ALB logs, CloudTrail) → **CloudWatch Logs / Kinesis Data Firehose** → **S3** (partition + Parquet) → truy vấn bằng **Athena**, trực quan hóa bằng **QuickSight**; log cần tìm kiếm real-time thì đẩy sang **OpenSearch**.

## 13.5. Hybrid – mở rộng data center lên AWS

**Direct Connect** (chính) + **Site-to-Site VPN** (dự phòng) → **Transit Gateway** → nhiều VPC. **Route 53 Resolver Endpoints** cho DNS hai chiều. **Storage Gateway** mở rộng file share; **DataSync** đồng bộ dữ liệu; **AWS Managed Microsoft AD** hoặc **AD Connector** cho danh tính.

## 13.6. Ứng dụng toàn cầu độ trễ thấp

**Route 53 latency-based routing** → nhiều Region, mỗi Region có ALB + ASG; dữ liệu dùng **Aurora Global Database** (đọc cục bộ, ghi về Region chính) hoặc **DynamoDB Global Tables** (active-active); nội dung tĩnh qua **CloudFront**; API non-HTTP qua **Global Accelerator**; **S3 CRR** đồng bộ tài sản.

# PHẦN XIV. NGÂN HÀNG CÂU HỎI ÔN TẬP CÓ GIẢI THÍCH

**Câu 1.** Một công ty chạy ứng dụng web trên EC2 sau ALB. Người dùng phàn nàn bị đăng xuất ngẫu nhiên. Giải pháp nào ít thay đổi kiến trúc nhất mà vẫn khắc phục triệt để và cho phép scale?
*Đáp án:* Lưu session ra **ElastiCache for Redis** (hoặc DynamoDB) để tầng ứng dụng trở thành stateless. Sticky session chỉ là giải pháp tạm và gây mất cân bằng tải.

**Câu 2.** Ứng dụng cần chia sẻ một thư mục dữ liệu giữa 20 EC2 chạy Linux ở 3 AZ, dung lượng tăng không đoán trước.
*Đáp án:* **Amazon EFS** (Regional). EBS không gắn được nhiều instance khác AZ; FSx for Windows là SMB cho Windows.

**Câu 3.** Cần lưu trữ 500 TB log để tuân thủ trong 7 năm, hầu như không truy cập, nếu cần thì chấp nhận chờ 12 giờ. Rẻ nhất?
*Đáp án:* **S3 Glacier Deep Archive** (kèm Lifecycle Policy chuyển từ Standard, và **S3 Object Lock Compliance Mode** nếu đề yêu cầu chống xóa).

**Câu 4.** Lambda trong VPC cần gọi Amazon S3 nhưng không được đi qua Internet, và cần chi phí thấp nhất.
*Đáp án:* **VPC Gateway Endpoint cho S3** (miễn phí). NAT Gateway tốn phí; Interface Endpoint tốn phí hơn.

**Câu 5.** Đội bảo mật yêu cầu phát hiện hành vi bất thường như EC2 giao tiếp với IP mining tiền ảo, không muốn cài agent.
*Đáp án:* **Amazon GuardDuty**.

**Câu 6.** Ứng dụng gửi hàng nghìn tin nhắn cần được xử lý **đúng thứ tự** và **không trùng lặp**.
*Đáp án:* **SQS FIFO Queue** với Message Group ID phù hợp.

**Câu 7.** Cần chạy job phân tích 6 giờ mỗi đêm, chịu được gián đoạn, chi phí thấp nhất.
*Đáp án:* **AWS Batch trên Spot Instances** (Lambda không đủ 15 phút).

**Câu 8.** DB Aurora ở us-east-1, cần khôi phục sang eu-west-1 với **RPO dưới 1 phút, RTO vài phút**.
*Đáp án:* **Aurora Global Database**. Snapshot cross-region không đạt RPO này.

**Câu 9.** Ứng dụng game dùng UDP, cần IP tĩnh và failover nhanh giữa các Region.
*Đáp án:* **AWS Global Accelerator** (CloudFront chỉ hỗ trợ HTTP; Route 53 phụ thuộc TTL DNS).

**Câu 10.** Cần cho phép developer truy cập shell vào EC2 ở private subnet mà không mở port 22 và không dựng bastion.
*Đáp án:* **AWS Systems Manager Session Manager**.

**Câu 11.** Muốn ngăn mọi bucket S3 trong tổ chức bị đặt public, kể cả do người dùng vô ý.
*Đáp án:* Bật **S3 Block Public Access ở cấp account** + **SCP** chặn hành động gỡ nó.

**Câu 12.** Ứng dụng đọc DynamoDB rất nhiều, muốn giảm độ trễ xuống mức micro-giây mà không sửa code.
*Đáp án:* **DynamoDB Accelerator (DAX)**.

**Câu 13.** Traffic tăng đột biến mỗi khi có chiến dịch marketing, backend RDS bị quá tải và request bị mất.
*Đáp án:* Chèn **SQS** giữa tầng web và tầng xử lý, worker đọc theo tốc độ chịu được; scale worker theo `ApproximateNumberOfMessagesVisible`.

**Câu 14.** Cần đảm bảo mọi object upload lên S3 đều được mã hóa bằng KMS, nếu không thì từ chối.
*Đáp án:* **Bucket policy** với `Deny` khi `s3:x-amz-server-side-encryption` khác `aws:kms` (kết hợp bật default encryption).

**Câu 15.** Công ty muốn di chuyển 200 TB dữ liệu lên S3, đường truyền chỉ 100 Mbps.
*Đáp án:* **AWS Snowball Edge** (200 TB qua 100 Mbps mất khoảng 6 tháng – không khả thi).

**Câu 16.** Cần một giải pháp cho phép nhiều VPC ở nhiều account truy cập một dịch vụ nội bộ mà không phơi ra Internet, không lo trùng CIDR.
*Đáp án:* **AWS PrivateLink** (Interface Endpoint trỏ tới NLB của dịch vụ).

**Câu 17.** Ứng dụng cần đọc/ghi rất nhiều IOPS tạm thời (cache tính toán), dữ liệu mất cũng không sao, muốn IOPS cao nhất.
*Đáp án:* **Instance Store NVMe**.

**Câu 18.** Cần audit "ai đã xóa security group này lúc nào".
*Đáp án:* **AWS CloudTrail** (nếu cần biết cấu hình thay đổi thế nào theo thời gian và có tuân thủ không → **AWS Config**).

**Câu 19.** Ứng dụng web cần chống SQL Injection và giới hạn 2.000 request/5 phút mỗi IP.
*Đáp án:* **AWS WAF** (managed rule SQLi + rate-based rule) gắn vào ALB hoặc CloudFront.

**Câu 20.** Cần đảm bảo mật khẩu RDS được xoay tự động mỗi 30 ngày.
*Đáp án:* **AWS Secrets Manager** với rotation Lambda.

**Câu 21.** Cần triển khai một stack hạ tầng giống hệt nhau lên 30 account và 4 Region.
*Đáp án:* **CloudFormation StackSets**.

**Câu 22.** Website tĩnh trên S3 cần dùng domain riêng và HTTPS.
*Đáp án:* **CloudFront + ACM certificate (us-east-1) + Route 53 alias record**, S3 làm origin với **OAC**.

**Câu 23.** Cần phân tích 5 TB log ALB trên S3 chỉ vài lần mỗi tháng, không muốn duy trì hạ tầng.
*Đáp án:* **Amazon Athena** (chuyển sang Parquet và partition để giảm chi phí quét).

**Câu 24.** ASG liên tục khởi tạo rồi hủy instance (thrashing).
*Đáp án:* Tăng **health check grace period**, kiểm tra health check ELB, tăng **cooldown**, kiểm tra ngưỡng scaling quá sát nhau.

**Câu 25.** Cần giới hạn để dù developer có quyền IAM tạo role, họ cũng không tự nâng quyền lên admin.
*Đáp án:* **IAM Permissions Boundary**.

# PHẦN XV. BẢNG SO SÁNH NHANH – ÔN TRƯỚC GIỜ THI

## 15.1. Các cặp dễ nhầm

| Cặp | Khác biệt cốt lõi |
|---|---|
| **Multi-AZ vs Read Replica** | HA/failover **vs** scale đọc |
| **SG vs NACL** | Stateful, chỉ allow, cấp instance **vs** stateless, có deny, cấp subnet |
| **CloudFront vs Global Accelerator** | Cache HTTP tại edge **vs** tăng tốc TCP/UDP với IP tĩnh |
| **SQS vs SNS** | Pull, một consumer, lưu trữ **vs** push, nhiều subscriber, không lưu |
| **SQS vs Kinesis** | Xóa sau xử lý **vs** giữ lại, replay, nhiều consumer |
| **Kinesis Data Streams vs Firehose** | Real-time, tự quản shard, replay **vs** near real-time, tự động, nạp thẳng vào đích |
| **EBS vs EFS vs S3** | Block 1 máy 1 AZ **vs** file nhiều máy nhiều AZ **vs** object qua HTTP |
| **Gateway vs Interface Endpoint** | Chỉ S3/DynamoDB, route, miễn phí **vs** hầu hết dịch vụ, ENI, có phí, dùng được từ on-prem |
| **VPC Peering vs Transit Gateway** | 1-1, không transitive **vs** hub, transitive, hàng nghìn kết nối |
| **CloudTrail vs CloudWatch vs Config** | API audit **vs** metric/log hiệu năng **vs** cấu hình & tuân thủ |
| **GuardDuty vs Inspector vs Macie** | Mối đe dọa từ log **vs** lỗ hổng CVE trên workload **vs** dữ liệu nhạy cảm trong S3 |
| **Secrets Manager vs Parameter Store** | Có phí, tự rotate **vs** miễn phí (standard), không tự rotate |
| **Latency vs Geolocation routing** | Nhanh nhất **vs** đúng vùng địa lý |
| **Alias vs CNAME** | Zone apex được, miễn phí, chỉ AWS resource **vs** không dùng cho apex, có phí, trỏ mọi nơi |
| **CloudFront Functions vs Lambda@Edge** | Sub-millisecond, JS, không mạng **vs** vài giây, Node/Python, gọi API được |
| **ECS EC2 vs Fargate** | Tự quản máy, rẻ hơn khi ổn định **vs** serverless, ít vận hành |
| **Dedicated Instance vs Dedicated Host** | Phần cứng riêng **vs** thấy máy vật lý, BYOL theo socket/core |
| **Spot vs On-Demand vs RI vs Savings Plan** | Rẻ nhất nhưng bị thu hồi **vs** linh hoạt **vs** cam kết loại máy **vs** cam kết $/giờ linh hoạt |
| **Cluster vs Spread vs Partition PG** | Độ trễ thấp nhất **vs** cô lập lỗi tối đa (7/AZ) **vs** phân nhóm rack lớn |
| **Athena vs Redshift** | Ad-hoc trên S3, serverless **vs** data warehouse chuyên dụng, hiệu năng cao |
| **Step Functions vs SQS** | Điều phối workflow có trạng thái **vs** hàng đợi phi trạng thái |
| **EventBridge vs SNS** | Định tuyến theo nội dung, SaaS, replay **vs** fan-out đơn giản, throughput cao |
| **QLDB vs Managed Blockchain** | Sổ cái tập trung, một chủ sở hữu **vs** phi tập trung, nhiều bên |

## 15.2. Những con số phải nhớ

| Chủ đề | Con số |
|---|---|
| Lambda | 15 phút, 10.240 MB RAM, 10 GB `/tmp`, 6 MB payload sync, 1.000 concurrency |
| SQS | 256 KB message, 14 ngày retention, 12 giờ visibility timeout, 20 giây long polling |
| S3 | Object tối đa 5 TB, multipart bắt buộc từ 5 GB, 3.500 PUT & 5.500 GET mỗi prefix/giây |
| S3 storage class minimum | IA 30 ngày, Glacier IR/Flexible 90 ngày, Deep Archive 180 ngày |
| EBS | gp3 16.000 IOPS/1.000 MB/s, io2 Block Express 256.000 IOPS/64 TiB, Multi-Attach 16 instance |
| DynamoDB | Item 400 KB, 1 WCU = 1 KB/s ghi, 1 RCU = 4 KB/s đọc strong, LSI 5 / GSI 20, Streams 24 giờ, PITR 35 ngày |
| RDS | Backup 0–35 ngày, tối đa 5 read replica, transaction log mỗi 5 phút |
| Aurora | 6 bản sao / 3 AZ, 15 replica, tới 128 TB, Global DB 5 Region phụ, Backtrack 72 giờ |
| VPC | CIDR /16 tới /28, 5 IP bị AWS giữ mỗi subnet, 5 VPC/Region mặc định |
| Kinesis | Shard 1 MB/s in – 2 MB/s out, retention 24 giờ tới 365 ngày |
| Spot | Cảnh báo thu hồi trước **2 phút** |
| Placement group | Spread tối đa 7 instance/AZ; Partition tối đa 7 partition/AZ |
| KMS | Mã hóa trực tiếp ≤ 4 KB, xóa khóa chờ 7–30 ngày |
| API Gateway | 10.000 req/s, burst 5.000, cache TTL 300s |
| Route 53 | SLA 100%, health check unhealthy khi < 18% checker báo khỏe |
| ELB | Deregistration delay mặc định 300 giây |
| ASG | Cooldown mặc định 300 giây, health check grace period 300 giây |
| CloudTrail | Event History 90 ngày |
| Glacier retrieval | Expedited 1–5 phút, Standard 3–5 giờ, Bulk 5–12 giờ; Deep Archive: Standard 12 giờ, Bulk 48 giờ |

# PHẦN XVI. CHECKLIST ÔN TẬP CUỐI CÙNG

## 16.1. Tuần cuối trước khi thi

1. Làm **ít nhất 3–4 bộ đề mô phỏng đầy đủ 65 câu trong 130 phút** (Tutorials Dojo/Jon Bonso, Stephane Maarek, hoặc đề chính thức của AWS Skill Builder). Mục tiêu đạt **ổn định trên 80%** trước khi thi thật.
2. **Đọc kỹ phần giải thích của MỌI câu**, kể cả câu làm đúng – vì có thể bạn đúng do đoán.
3. Ghi lại **nhật ký lỗi sai** theo chủ đề để biết mảng nào còn yếu.
4. Ôn lại các **FAQ chính thức** của: EC2, S3, VPC, RDS, Aurora, DynamoDB, Route 53, CloudFront, SQS, Lambda, ELB.
5. Đọc lại whitepaper **AWS Well-Architected Framework** và **Disaster Recovery of Workloads on AWS**.
6. Ôn thuộc các **bảng so sánh và con số** ở Phần XV.

## 16.2. Kiểm tra mức độ sẵn sàng – bạn có tự tin trả lời được không?

- Khác biệt giữa Multi-AZ và Read Replica; khi nào dùng cả hai?
- Khi nào dùng EBS, khi nào EFS, khi nào FSx (loại nào), khi nào S3?
- Bảy chính sách routing của Route 53 và trường hợp dùng từng loại?
- Cách cho EC2 ở private subnet ra Internet và cách truy cập S3 riêng tư?
- Sáu lớp lưu trữ S3 chính, ngưỡng phạt tối thiểu và thời gian truy xuất?
- Sự khác biệt Security Group và NACL, và khi nào bắt buộc dùng NACL?
- Thứ tự đánh giá quyền IAM và tác động của SCP, permission boundary?
- Bốn chiến lược DR, RTO/RPO tương ứng?
- Khi nào SQS, khi nào SNS, khi nào EventBridge, khi nào Kinesis, khi nào Step Functions?
- Các mô hình mua EC2 và cách chọn theo tình huống?
- Cách bảo vệ một web app: WAF, Shield, CloudFront, ALB đặt ở đâu?
- Cách mã hóa dữ liệu at-rest và in-transit cho S3, EBS, RDS?

## 16.3. Trong phòng thi

- Đọc **câu hỏi cuối cùng trước**, sau đó mới đọc kịch bản để lọc thông tin nhiễu.
- Tìm các từ khóa ràng buộc: *MOST cost-effective, LEAST operational overhead, HIGHEST availability, MINIMAL downtime, MOST secure, real-time vs near real-time*.
- Loại ngay các đáp án chứa: dịch vụ **không tồn tại**, dịch vụ dùng **sai mục đích** (ví dụ Redshift cho OLTP), giải pháp **thủ công** khi đề đòi tự động, hoặc **quản lý server** khi đề đòi serverless.
- Nếu hai đáp án đều đúng kỹ thuật, chọn cái **managed hơn / ít thao tác hơn / rẻ hơn**, tùy tiêu chí câu hỏi.
- **Đừng đọc thêm giả định** không có trong đề.
- Với câu "Chọn 2" hay "Chọn 3": phải chọn đủ số lượng, không có điểm từng phần.
- Flag và quay lại, đừng để một câu khó làm hỏng nhịp độ.

## 16.4. Tài nguyên ôn tập gợi ý

- **AWS Skill Builder** – Exam Prep Official Practice Question Set (miễn phí 20 câu) và Official Practice Exam.
- **Exam Guide chính thức SAA-C03** – đọc để nắm danh sách dịch vụ trong phạm vi thi.
- **AWS Whitepapers:** Well-Architected Framework, Architecting for the Cloud – AWS Best Practices, Disaster Recovery of Workloads on AWS, Security Pillar, Cost Optimization Pillar.
- **AWS FAQs** của các dịch vụ trọng tâm.
- **AWS Documentation** – phần "Best Practices" của mỗi dịch vụ.
- **Đề luyện:** Tutorials Dojo (Jon Bonso), Stephane Maarek Practice Exams, Adrian Cantrill.

Chúc bạn ôn tập hiệu quả và thi đạt kết quả cao. Hãy nhớ nguyên tắc vàng của kỳ thi này: **AWS luôn ưu tiên giải pháp được quản lý, có tính sẵn sàng cao, bảo mật theo nguyên tắc đặc quyền tối thiểu, và tối ưu chi phí mà vẫn đáp ứng yêu cầu kỹ thuật.**

---

## 📄 Giấy phép

Nội dung phát hành theo giấy phép **CC BY 4.0** — tự do sử dụng, chia sẻ và chỉnh sửa, chỉ cần ghi nguồn.

<sub>Tài liệu mang tính chất tham khảo cá nhân, không liên kết chính thức với Amazon Web Services.</sub>
