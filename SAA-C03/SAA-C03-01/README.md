## Quiz

### Q10. 
A company runs multiple workloads on virtual machines (VMs) in an on-premises data center. The company is expanding rapidly. The on-premises data center is not able to scale fast enough to meet business needs. The company wants to migrate the workloads to AWS.

The migration is time sensitive. The company wants to use a lift-and-shift strategy for non-critical workloads.

Which combination of steps will meet these requirements? (Choose three.)?
- Use AWS Application Migration Service. Install the AWS Replication Agent on the VMs.
- Use the AWS Schema Conversion Tool (AWS SCT) to collect data about the VMs.
- Use AWS App2Container (A2C) to collect data about the VMs.
- Complete the initial replication of the VMs. Launch test instances to perform acceptance tests on the VMs.
- Stop all operations on the VMs. Launch a cutover instance.
- Use AWS Database Migration Service (AWS DMS) to migrate the VMs.

<details>
<summary>Answer</summary>
Tóm tắt đề: 

Company cần migrate nhiều workloads từ VMs ở phía on-premises lên AWS với chiến thuật
lift-and-shift strategy(di chuyển nguyên trạng).

✅ Đáp án đúng:

Applicaiton Migration Service (MGN) là service cho phép thực hiện migrate server từ on-premise lên AWS thông qua hình thức lift-and-shift. Cơ chế hoạt động của MGN thì như kiến trúc bên dưới.

1. Thực hiện cài AWS Replication Agent về các server ở phía on-premise. Các agent này sẽ đồng bộ toàn bộ data của server lên AWS.

2. Lúc này trên AWS sẽ xuất hiện server trung gian gọi là Replication server, sẽ chứa các data được đồng bộ từ phía on-premise lên. Sau khi đồng bộ xong thì có thể tạo ra EC2 gọi là Test Instance. Instance này sẽ chứa mọi data của server on-premise, có thể nói là giống hệt. Việc này cho phép ta test xem quá trình migrate có vấn đề gì hay không.

3. Sau khi test xong rồi thì có thể xoá server ở phía on-premise, thực hiện step Cutover - tạo ra instance cuối để kết thúc quá trình migrate.

Dựa vào kiến trúc ở trên, thì có 3 đáp án tương ứng với 3 step ở trên, lần lượt là

Use AWS Application Migration Service. Install the AWS Replication Agent on the VMs.

Complete the initial replication of the VMs. Launch test instances to perform acceptance tests on the VMs.

Stop all operations on the VMs. Launch a cutover instance.

❌ Các đáp án sai:
❌ Use the AWS Schema Conversion Tool (AWS SCT) to collect data about the VMs.

→ AWS SCT dùng cho use case migrate database, không dùng cho migrate server (VMWare)

❌ Use AWS App2Container (A2C) to collect data about the VMs.

→ A2C dùng để containerize applications (chuyển thành containers), không phải migration lift-and-shift

❌ Use AWS Database Migration Service (AWS DMS) to migrate the VMs.

→ DMS chuyên migrate databases, không dùng cho migrate server (VMWare)

🔑 Tips and tricks:

Nhắc đến việc migrate lift-and-shift hay migrate server nói chung thì thường sẽ nghĩ đến Application Migration Service (MGN)

📖 Reference:

https://docs.aws.amazon.com/mgn/latest/ug/what-is-application-migration-service.html
</details>

---

### Q11. 
A company runs an environment where data is stored in an Amazon S3 bucket. The objects are accessed frequently throughout the day. The company has strict data encryption requirements for data that is stored in the S3 bucket. The company currently uses AWS Key Management Service (AWS KMS) for encryption.

The company wants to optimize costs associated with encrypting S3 objects without making additional calls to AWS KMS.

Which solution will meet these requirements?
- Use server-side encryption with Amazon S3 managed keys (SSE-S3).
- Use server-side encryption with customer-provided keys (SSE-C) stored in AWS KMS.
- Use an S3 Bucket Key for server-side encryption with AWS KMS keys (SSE-KMS) on the new objects.
- Use client-side encryption with AWS KMS customer managed keys.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Dữ liệu lưu trong S3 bucket, được truy cập thường xuyên trong ngày (frequently throughout the day)

Hiện tại đang dùng AWS KMS để mã hóa

Muốn tối ưu chi phí mã hóa KMS (optimize costs) mà không gọi thêm API Call đến AWS KMS (without making additional calls to AWS KMS)

✅ Đáp án đúng:

Use an S3 Bucket Key for server-side encryption with AWS KMS keys (SSE-KMS) on the new objects.

S3 Bucket Key giảm chi phí AWS KMS bằng cách tạo một key duy nhất sử dụng chung cho toàn bộ bucket, thay vì tạo riêng cho từng object (hành vi mặc định).

Điều này giảm đáng kể số lượng API calls đến KMS (có thể giảm đến 99%), từ đó giảm chi phí mà vẫn đảm bảo mức độ bảo mật cao với KMS.

Các đáp án sai:

❌ Use server-side encryption with Amazon S3 managed keys (SSE-S3).

→ Mặc dù miễn phí nhưng không đáp ứng yêu cầu "strict encryption requirements" do không có khả năng kiểm soát key.

❌ Use client-side encryption with AWS KMS customer managed keys.

→ Vẫn phải gọi KMS cho mỗi thao tác trên S3, không giảm được chi phí. Hơn nữa còn phải quản lý encryption/decryption ở phía client.

❌ Use server-side encryption with customer-provided keys (SSE-C) stored in AWS KMS.

→ SSE-C yêu cầu cung cấp key trong mỗi request, không lưu key trong KMS được. Mâu thuẫn với thiết kế hiện tại dùng KMS.

📖 Reference:

https://docs.aws.amazon.com/AmazonS3/latest/userguide/bucket-key.html
</details>

---

### Q12. 
A company hosts a website analytics application on a single Amazon EC2 On-Demand Instance. The analytics application is highly resilient and is designed to run in stateless mode.

The company notices that the application is showing signs of performance degradation during busy times and is presenting 5xx errors. The company needs to make the application scale seamlessly.

Which solution will meet these requirements MOST cost-effectively?
- Create an Amazon Machine Image (AMI) of the web application. Use the AMI to launch a second EC2 On-Demand Instance. Use an Application Load Balancer to distribute the load across the two EC2 instances.
- Create an Amazon Machine Image (AMI) of the web application. Apply the AMI to a launch template. Create an Auto Scaling group that includes the launch template. Configure the launch template to use a Spot Fleet. Attach an Application Load Balancer to the Auto Scaling group.
- Create an Amazon Machine Image (AMI) of the web application. Use the AMI to launch a second EC2 On-Demand Instance. Use Amazon Route 53 weighted routing to distribute the load across the two EC2 instances
- Create an AWS Lambda function to stop the EC2 instance and change the instance type. Create an Amazon CloudWatch alarm to invoke the Lambda function when CPU utilization is more than 75%.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng analytics hiện tại chạy trên 1 EC2 On-Demand instance duy nhất

Ứng dụng stateless (tức là không chứa data) và có khả năng chịu lỗi cao (highly resilient)

Gặp vấn đề performance và 5xx errors khi cần xử lý nhiều

Cần cơ chế scale và tiết kiệm chi phí (cost-effective)

✅ Đáp án đúng:

Create an Amazon Machine Image (AMI) of the web application. Apply the AMI to a launch template. Create an Auto Scaling group that includes the launch template. Configure the launch template to use a Spot Fleet. Attach an Application Load Balancer to the Auto Scaling group.

Có thể thấy ngay application đang chạy trên 1 EC2 duy nhất, do đó để scale được thì cần sử dụng Auto Scaling group

Loại instance sẽ lựa chọn là Spot Fleet để giúp tiết kiệm chi phí đến 90% so với On-Demand, mặc dù có tính bất ổn, có thể bị thu hồi bất cứ lúc nào nhưng application stateless và có khả năng chịu lỗi cao (highly resilient) nên sẽ không ảnh hưởng nhiều

Sử dụng Application Load Balancer để phân phối traffic đều và health check

Các đáp án sai:

❌ Create an Amazon Machine Image (AMI) of the web application. Use the AMI to launch a second EC2 On-Demand Instance. Use an Application Load Balancer to distribute the load across the two EC2 instances.

Không scale được do chỉ fix cứng 2 instances

❌ Create an Amazon Machine Image (AMI) of the web application. Use the AMI to launch a second EC2 On-Demand Instance. Use Amazon Route 53 weighted routing to distribute the load across the two EC2 instances

Tương tự như trên, không scale được do chỉ fix cứng 2 instances

❌ Create an AWS Lambda function to stop the EC2 instance and change the instance type. Create an Amazon CloudWatch alarm to invoke the Lambda function when CPU utilization is more than 75%.

Dùng Lambda tăng thêm tính phức tạp không cần thiết cho việc scale EC2, do đó không phù hợp. Sử dụng Auto Scaling group sẽ vẫn là sự lựa chọn hợp lí hơn

🔑 Tips and tricks:

Với các application stateless và có khả năng chịu lỗi tốt mà cần cơ chế scale thì thường sẽ nghĩ đến Auto Scaling Group & Spot Instances
</details>

---

### Q13. 
A medical company wants to perform transformations on a large amount of clinical trial data that comes from several customers. The company must extract the data from a relational database that contains the customer data. Then the company will transform the data by using a series of complex rules. The company will load the data to Amazon S3 when the transformations are complete.

All data must be encrypted where it is processed before the company stores the data in Amazon S3. All data must be encrypted by using customer-specific keys.

Which solution will meet these requirements with the LEAST amount of operational effort?

- Create one AWS Glue job for each customer. Attach a security configuration to each job that uses client-side encryption with AWS KMS managed keys (CSE-KMS) to encrypt the data.
- Create one AWS Glue job for each customer. Attach a security configuration to each job that uses server-side encryption with Amazon S3 managed keys (SSE-S3) to encrypt the data.
- Create one Amazon EMR cluster for each customer. Attach a security configuration to each cluster that uses client-side encryption with a custom client-side root key (CSE-Custom) to encrypt the data.
- Create one Amazon EMR cluster for each customer. Attach a security configuration to each cluster that uses server-side encryption with AWS KMS keys (SSE-KMS) to encrypt the data.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty y tế cần xử lý dữ liệu thử nghiệm lâm sàng (clinical trial data) lớn đến từ nhiều khách hàng

Quy trình: Extract từ relational database → Transform bằng complex rules → Load vào Amazon S3

Yêu cầu: mã hóa tất cả dữ liệu trong quá trình xử lý trước khi lưu S3

Mã hóa bằng key do customer chỉ định (customer-specific keys)

Cần giải pháp tốn ít công sức nhất (LEAST operational effort)

✅ Đáp án đúng:

Create one AWS Glue job for each customer. Attach a security configuration to each job that uses client-side encryption with AWS KMS managed keys (CSE-KMS) to encrypt the data.

AWS Glue là dịch vụ ETL serverless, phù hợp cho việc extract-transform-load với ít effort nhất. Sử dụng Glue sẽ phù hợp cho bài toán vì có khả năng kéo data từ RDS về xử lý, chuyển đổi rồi sau đó nạp vào S3.

CSE-KMS cho phép sử dụng key do customer chỉ định (customer-specific keys) thông qua KMS, đáp ứng yêu cầu mã hóa riêng biệt cho từng khách hàng

Client-side encryption đảm bảo dữ liệu được mã hóa ngay trong quá trình xử lý

![img](https://static.cloudexam.pro/courses/5/1756914659523-53utru2r-CleanShot_2025-09-04_at_00.50.03.png)

Các đáp án sai:

❌ Create one AWS Glue job for each customer. Attach a security configuration to each job that uses server-side encryption with Amazon S3 managed keys (SSE-S3) to encrypt the data.

→ SSE-S3 chỉ mã hóa khi lưu trữ ở S3, không cho phép mã hóa trong quá trình nạp data và không sử dụng customer-specific keys được

❌ Create one Amazon EMR cluster for each customer. Attach a security configuration to each cluster that uses client-side encryption with a custom client-side root key (CSE-Custom) to encrypt the data.

→ EMR yêu cầu nhiều operational effort hơn Glue (do chạy trên EC2). Hơn nữa CSE-Custom phức tạp hơn CSE-KMS

❌ Create one Amazon EMR cluster for each customer. Attach a security configuration to each cluster that uses server-side encryption with AWS KMS keys (SSE-KMS) to encrypt the data.

→ EMR yêu cầu nhiều operational effort hơn Glue (do chạy trên EC2). Hơn nữa SSE-KMS chỉ mã hóa tại S3 lúc lưu trữ, không mã hóa trong quá trình nạp data vào S3

🔑 Tips and tricks:

Bài toán nhắc đến use case ETL (extract-transform-load) và yêu cầu tốn ít effort (LEAST operational effort) thì thường sẽ nghĩ đến AWS Glue

📖 Reference:

https://aws.amazon.com/blogs/storage/understanding-amazon-s3-client-side-encryption-options/#:~:text=Using%20client%2Dside%20encryption%20(CSE,keys%20used%20to%20encrypt%20objects.
</details>

---

### Q14. 
A company is creating a new web application for its subscribers. The application will consist of a static single page and a persistent database layer. The application will have millions of users for 4 hours in the morning, but the application will have only a few thousand users during the rest of the day. The company's data architects have requested the ability to rapidly evolve their schema.

Which solutions will meet these requirements and provide the MOST scalability? (Choose two.)
- Deploy the web servers for static content across a fleet of Amazon EC2 instances in Auto Scaling groups. Configure the instances to periodically refresh the content from an Amazon Elastic File System (Amazon EFS) volume.
- Deploy Amazon DynamoDB as the database solution. Provision on-demand capacity.
- Deploy Amazon Aurora as the database solution. Choose the serverless DB engine mode.
- Deploy Amazon DynamoDB as the database solution. Ensure that DynamoDB auto scaling is enabled.
- Deploy the static content into an Amazon S3 bucket. Provision an Amazon CloudFront distribution with the S3 bucket as the origin.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng web có host web tĩnh (static single page) và database

Lưu lượng truy cập không ổn định: hàng triệu users truy cập trong vòng 4h sáng, chỉ vài nghìn users truy cập thời gian còn lại

DB yêu cầu khả năng linh hoạt đáp ứng các thay đổi về schema (rapidly evolve schema)

Cần giải pháp có khả năng mở rộng tốt (scalability) cho việc host web tĩnh và database

✅ Đáp án đúng:

Đối với database thì sẽ lựa chọn đáp án:

Deploy Amazon DynamoDB as the database solution. Provision on-demand capacity.

On-demand capacity là cơ chế traffic của DynamoDB cho phép tự động scale theo traffic thực tế, phù hợp với pattern lưu lượng không đều (triệu users → vài nghìn users)

DynamoDB là NoSQL Databse nên sẽ hỗ trợ schema linh hoạt - có thể thay đổi schema nhanh chóng mà không phát sinh downtime

Đối với việc host web tĩnh thì sẽ lựa chọn đáp án:

Deploy the static content into an Amazon S3 bucket. Provision an Amazon CloudFront distribution with the S3 bucket as the origin.

S3 + CloudFront là giải pháp tối ưu cho static content, scale không giới hạn

CloudFront CDN đảm bảo performance tốt cho hàng triệu concurrent users

![img](https://static.cloudexam.pro/courses/5/1756974900934-cgvohw6i-image.png)

Các đáp án sai:

❌ Deploy Amazon Aurora as the database solution. Choose the serverless DB engine mode.

Tuy Aurora Serverless có thể auto-scale, nhưng Aurora là DB dạng quan hệ do đó không linh hoạt trong việc thay đổi schema so với NoSQL DynamoDB

❌ Deploy Amazon DynamoDB as the database solution. Ensure that DynamoDB auto scaling is enabled.

Auto Scaling sẽ cần chờ Cloudwatch Alarm kích hoạt khi traffic tăng đột biến, do đó không thể scale ngay lập tức được, sử dụng on-demand capacity hợp lý hơn

❌ Deploy the web servers for static content across a fleet of Amazon EC2 instances in Auto Scaling groups. Configure the instances to periodically refresh the content from an Amazon Elastic File System (Amazon EFS) volume.

Quá phức tạp và không cost-effective cho static content. S3 + CloudFront đơn giản và rẻ hơn nhiều

🔑 Tips and tricks:

Các câu hỏi liên quan đến use case host web tĩnh thì thường sẽ nghĩ đến CloudFront và S3

Đề bài nhắc đến DB có khả năng linh hoạt đáp ứng thay đổi schema (rapidly evolve schema) thì sẽ nghĩ đến DynamoDB
</details>

---

### Q15
A company tracks customer satisfaction by using surveys that the company hosts on its website. The surveys sometimes reach thousands of customers every hour. Survey results are currently sent in email messages to the company so company employees can manually review results and assess customer sentiment.

The company wants to automate the customer survey process. Survey results must be available for the previous 12 months.

Which solution will meet these requirements in the MOST scalable way?
- Send the survey results data to an Amazon API Gateway endpoint that is connected to an Amazon Simple Queue Service (Amazon SQS) queue. Create an AWS Lambda function to poll the SQS queue, call Amazon Comprehend for sentiment analysis, and save the results to an Amazon DynamoDB table. Set the TTL for all records to 365 days in the future.
- Send the survey results data to an API that is running on an Amazon EC2 instance. Configure the API to store the survey results as a new record in an Amazon DynamoDB table, call Amazon Comprehend for sentiment analysis, and save the results in a second DynamoDB table. Set the TTL for all records to 365 days in the future
- Write the survey results data to an Amazon S3 bucket. Use S3 Event Notifications to invoke an AWS Lambda function to read the data and call Amazon Rekognition for sentiment analysis. Store the sentiment analysis results in a second S3 bucket. Use S3 lifecycle policies on each bucket to expire objects after 365 days.
- Send the survey results data to an Amazon API Gateway endpoint that is connected to an Amazon Simple Queue Service (Amazon SQS) queue. Configure the SQS queue to invoke an AWS Lambda function that calls Amazon Lex for sentiment analysis and saves the results to an Amazon DynamoDB table. Set the TTL for all records to 365 days in the future.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty thu thập khảo sát khách hàng (customer surveys) từ website

Kết quả khảo sát hiện tại đang được nhân viên review thủ công

Cần solution có thể mở rộng nhất (MOST scalable) và đáp ứng yêu cầu:

Tự động hoá quá trình khảo sát

Phân tích thái độ người dùng (sentiment analysis) từ kết quả khảo sát

Data chỉ cần lưu trữ 365 ngày

✅ Đáp án đúng:

Send the survey results data to an Amazon API Gateway endpoint that is connected to an Amazon Simple Queue Service (Amazon SQS) queue. Create an AWS Lambda function to poll the SQS queue, call Amazon Comprehend for sentiment analysis, and save the results to an Amazon DynamoDB table. Set the TTL for all records to 365 days in the future.

API Gateway + SQS + Lambda + Comprehend + DynamoDB là kiến trúc serverless hoàn toàn, do đó sẽ scale tốt và đáp ứng yêu cầu:

API Gateway đóng vai trò là API tiếp nhận dữ liệu khảo sát khách hàng

SQS đảm bảo không mất dữ liệu khi có traffic tăng, cho phép decoupling giữa các thành phần để có thể scale độc lập

Lambda có thể gọi Amazon Comprehend là dịch vụ chuyên dụng cho sentiment analysis để thực hiện phân tích thái độ

Sau khi phân tích xong thì data lưu vào DynamoDB, với chức năng TTL thì có thể tự động xóa data sau 365 ngày

![img](https://static.cloudexam.pro/courses/5/1756915815849-hz16qitq-CleanShot_2025-09-04_at_01.09.45.png)

Các đáp án sai:

❌ Send the survey results data to an API that is running on an Amazon EC2 instance. Configure the API to store the survey results as a new record in an Amazon DynamoDB table, call Amazon Comprehend for sentiment analysis, and save the results in a second DynamoDB table. Set the TTL for all records to 365 days in the future

→ EC2 không tự động scale như serverless, cần quản lý infrastructure, kém scalable hơn

❌ Write the survey results data to an Amazon S3 bucket. Use S3 Event Notifications to invoke an AWS Lambda function to read the data and call Amazon Rekognition for sentiment analysis. Store the sentiment analysis results in a second S3 bucket. Use S3 lifecycle policies on each bucket to expire objects after 365 days.

→ Amazon Rekognition dùng cho phân tích image/video analysis, không phải sentiment analysis

❌ Send the survey results data to an Amazon API Gateway endpoint that is connected to an Amazon Simple Queue Service (Amazon SQS) queue. Configure the SQS queue to invoke an AWS Lambda function that calls Amazon Lex for sentiment analysis and saves the results to an Amazon DynamoDB table. Set the TTL for all records to 365 days in the future.

→ Amazon Lex dùng cho chatbot/conversational AI, không chuyên về sentiment analysis

🔑 Tips and tricks:

Phân tích thái độ người dùng (sentiment analysis) thì thường nghĩ ngay đến Comprehend
</details>

---
[
### Q16. 
A company hosts its enterprise resource planning (ERP) system in the us-east-1 Region. The system runs on Amazon EC2 instances. Customers use a public API that is hosted on the EC2 instances to exchange information with the ERP system. International customers report slow API response times from their data centers.

Which solution will improve response times for the international customers MOST cost-effectively?
- Set up an Amazon CloudFront distribution in front of the API. Configured CachingOptimized managed cache policy to improved cache efficiency
- Set up AWS Global Accelerator. Configure listeners for the necessary ports. Configure endpoint groups for the appropriate Regions to distribute traffic. Create an endpoint in the group for the API.
- Create an AWS Direct Connect connection that has a public virtual interface (VIF)
- Use AWS Site-to-Site VPN to establish dedicated VPN tunnels

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty có hệ thống ERP (enterprise resource planning) chạy trên EC2 ở us-east-1

Khách hàng sử dụng public API được host trên EC2 để trao đổi thông tin

Khách hàng quốc tế (international customers) báo cáo thời gian phản hồi chậm (slow API response times)

Cần giải pháp cải thiện response time mà hợp lí về mặt chi phí (cost-effectively) nhất

✅ Đáp án đúng:

Set up AWS Global Accelerator. Configure listeners for the necessary ports. Configure endpoint groups for the appropriate Regions to distribute traffic. Create an endpoint in the group for the API.

→ AWS Global Accelerator là giải pháp giúp tăng tốc, cải thiện hiệu suất cho phép các kết nối trên khắp thế giới đến hệ thống một cách nhanh nhất thông qua mạng lưới nội bộ toàn cầu của AWS. Do định tuyến traffic qua đường truyền tối ưu, giúp giảm độ trễ (latency) cho khách hàng quốc tế mà không cần thay đổi kiến trúc hiện tại.

![img](https://static.cloudexam.pro/courses/5/1756975990773-s5txd0h2-image.png)

Các đáp án sai:

❌ Create an AWS Direct Connect connection that has a public virtual interface (VIF)

→ Direct Connect yêu cầu physical connection tại mỗi data center của khách hàng, chi phí rất cao và phức tạp để triển khai cho nhiều khách hàng quốc tế.

❌ Set up an Amazon CloudFront distribution in front of the API
→ CloudFront chủ yếu tối ưu cho cache content tĩnh (static content caching) như là file ảnh, v.v.. API calls thường là dynamic content do đó không cache được hiệu quả, đặc biệt là ERP system cần real-time data.

❌ Use AWS Site-to-Site VPN to establish dedicated VPN tunnels

→ Site-to-Site VPN dùng để kết nối môi trường VPC với On-premise, không liên quan đến việc định tuyến traffic cho người dùng quốc tế ở đây.

🔑 Tips and tricks:

Các bài toán cần tăng tốc kết nối đến hệ thống thì thường nghĩ đến Global Accelerator
</details>

---

### Q17. 
A company runs its media rendering application on premises. The company wants to reduce storage costs and has moved all data to Amazon S3. The on-premises rendering application needs low-latency access to storage.

The company needs to design a storage solution for the application. The storage solution must maintain the desired application performance.

Which storage solution will meet these requirements in the MOST cost-effective way?
- Configure on-premises file server với S3 API. Configure the application to access the storage from the on-premises file server
- Copy data from Amazon S3 to Amazon FSx for Window File Server. Configure an Amazon FSx File Gateway to provide storage for the on-premises application
- Configure an Amazon S3 File Gateway to provide storage for the on-premises application.
- Use Mountpoint for Amazon S3 to access the data in Amazon S3

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty chạy ứng dụng ở phía on-premises

Đã chuyển tất cả data lên Amazon S3 để giảm chi phí storage

Ứng dụng on-premises cần truy cập đến storage với độ trễ thấp (low-latency)

Cần giải pháp chi phí thấp (cost-effective) nhất mà vẫn đảm bảo performance (maintain performance)

✅ Đáp án đúng:

Configure an Amazon S3 File Gateway to provide storage for the on-premises application.

→ S3 File Gateway là giải pháp tối ưu vì:

Cung cấp NFS/SMB interface để ứng dụng on-premises có thể truy cập S3 như là local file system

Có local cache để đảm bảo việc truy cập với độ trễ thấp (low-latency) cho data truy cập thường xuyên (frequently accessed data)

![img](https://static.cloudexam.pro/courses/5/1756976738146-tjifir74-image.png)

Các đáp án sai:

❌ Use Mountpoint for Amazon S3 to access the data in Amazon S3

Mountpoint là tool của AWS cho phép access đến S3 trực tiếp như là local file system. Tuy nhiên chỉ hỗ trợ Linux và vẫn chưa phát triển, vẫn còn nhiều hạn chế, do đó không phù hợp cho production-workload

Không có local caching như File Gateway → performance không tối ưu

❌ Copy data từ S3 sang Amazon FSx + FSx File Gateway

Tốn kém do phải để data ở cả 2 nơi, và phải trả thêm cho FSx storage, tăng tính phức tạp không cần thiết

❌ Configure on-premises file server với S3 API

Phức tạp, để làm được việc này thì cần dev một hệ thống riêng

Không có built-in caching và tối ưu hóa như File Gateway

🔑 Tips and tricks:

Cần solution về storage cho phía on-premise cho phép kết nối và đồng bộ lên s3 thì sẽ nghĩ đến S3 File Gateway
</details>

---

### Q18. 
An online gaming company is transitioning user data storage to Amazon DynamoDB to support the company's growing user base. The current architecture includes DynamoDB tables that contain user profiles, achievements, and in-game transactions.

The company needs to design a robust, continuously available, and resilient DynamoDB architecture to maintain a seamless gaming experience for users.

Which solution will meet these requirements MOST cost-effectively?
- A. MongoDB
- B. Redis
- C. MySQL
- Use DynamoDB global tables for automatic multi-Region replication. Deploy tables in multiple AWS Regions. Use provisioned capacity mode. Enable auto scaling.
- 
<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty game cần chuyển dữ liệu user sang DynamoDB để đáp ứng số lượng người chơi tăng lên

Cần thiết kế kiến trúc tối ưu, có tính khả dụng cao và khả năng chịu lỗi tốt (robust, continuously available, resilient)

Yêu cầu MOST cost-effectively (tối ưu hiệu quả chi phí nhất)

✅ Đáp án đúng:

Use DynamoDB global tables for automatic multi-Region replication. Deploy tables in multiple AWS Regions. Use provisioned capacity mode. Enable auto scaling.
→ Global tables cho phép chạy DynamoDB trên nhiều region, giúp đảm bảo tính khả dụng cao và khả năng chịu lỗi đồng thời có thể phục vụ được lượng người dùng trên khắp thế giới tốt hơn. Sử dụng kết hợp với Provisioned capacity với auto scaling tối ưu chi phí hơn on-demand khi có traffic ổn định và có thể dự đoán được.

Các đáp án sai:

❌ Create DynamoDB tables in a single AWS Region. Use on-demand capacity mode. Use global tables to replicate data across multiple Regions.

→ Sai logic. Không thể dùng global tables nếu chỉ tạo table ở single Region. Global tables yêu cầu tạo table ở nhiều Region.

❌ Use DynamoDB Accelerator (DAX) to cache frequently accessed data. Deploy tables in a single AWS Region and enable auto scaling. Configure Cross-Region Replication manually to additional Regions.

→ Sử dụng DAX sẽ làm tăng nhiều chi phí, hơn nữa việc chạy DynamoDB trên 1 region và replicate data thủ công sang các region khác sẽ không hiệu quả do không có tính tự động hóa.

❌ Create DynamoDB tables in multiple AWS Regions. Use on-demand capacity mode. Use DynamoDB Streams for Cross-Region Replication between Regions.

→ DynamoDB Streams + manual replication phức tạp hơn so với việc dùng trực tiếp global tables. Hơn nữa On-demand đắt hơn provisioned.

🔑 Tips and tricks:

Dynamo DB để có tính khả dụng liên tục, chịu lỗi tốt thì nghĩ đến Global Table & Multi-Region deployment
</details>

---

### Q19. 
A company operates a food delivery service. Because of recent growth, the company's order processing system is experiencing scaling problems during peak traffic hours. The current architecture includes Amazon EC2 instances in an Auto Scaling group that collect orders from an application. A second group of EC2 instances in an Auto Scaling group fulfills the orders.

The order collection process occurs quickly, but the order fulfillment process can take longer. Data must not be lost because of a scaling event.

A solutions architect must ensure that the order collection process and the order fulfillment process can both scale adequately during peak traffic hours.

Which solution will meet these requirements?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty giao đồ ăn gặp vấn đề về khả năng scale hệ thống trong giờ cao điểm

Kiến trúc hiện tại: có 2 nhóm EC2 Auto Scaling

Nhóm 1: thu thập đơn hàng (order collection) - xử lý nhanh

Nhóm 2: xử lý đơn hàng (order fulfillment) - xử lý chậm hơn

Yêu cầu: không được mất dữ liệu (data must not be lost) khi scaling

Cần giải pháp cho cả 2 process đều scale được trong peak traffic

✅ Đáp án đúng:

Provision two Amazon Simple Queue Service (Amazon SQS) queues. Use one SQS queue for order collection. Use the second SQS queue for order fulfillment. Configure the EC2 instances to poll their respective queues. Scale the Auto Scaling groups based on the number of messages in each queue.

Sử dụng SQS queue đóng vai trò buffer cho việc truyền data đến mỗi nhóm EC2, từ đó giúp đảm bảo không mất data khi EC2 scale.

Để có khả năng scale EC2 tự động khi traffic tăng thì sẽ dựa vào Metric số lượng messages trong queue thực tế đang là bao nhiêu, từ đó đặt Cloudwatch Alarm phù hợp.



Các đáp án sai:

❌ Use Amazon CloudWatch to monitor the CPUUtilization metric for each instance in both Auto Scaling groups. Configure each Auto Scaling group's minimum capacity to meet its peak workload value.

Chỉ đặt minimum capacity không giải quyết được vấn đề data loss khi instance terminate đột ngột. CPU metric cũng không phản ánh chính xác lượng workload hiện tại của queue.

❌ Use Amazon CloudWatch to monitor the CPUUtilization metric for each instance in both Auto Scaling groups. Configure a CloudWatch alarm to invoke an Amazon Simple Notification Service (Amazon SNS) topic to create additional Auto Scaling groups on demand.

Tạo thêm Auto Scaling group mới là phức tạp hóa và không cần thiết. Hơn nữa không dùng queue thì vẫn không giải quyết được vấn đề bị mất data.

❌ Provision two Amazon Simple Queue Service (Amazon SQS) queues. Use one SQS queue for order collection. Use the second SQS queue for order fulfillment. Configure the EC2 instances to poll their respective queues. Scale the Auto Scaling groups based on notifications that the queues send.

SQS không có cơ chế send notifications để scale, đáp án vô lý.

🔑 Tips and tricks:

Các bài toán liên quan đến xử lí order, cần tránh việc mất data đồng thời cho khả năng scale dựa theo số order thực tế thì sẽ nghĩ đến SQS
</details>

---

### Q20. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q21. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q22. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q23. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q24. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q25. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q2. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>