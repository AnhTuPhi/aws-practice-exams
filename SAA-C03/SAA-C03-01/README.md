## Bộ đề 1

---

### Q1. 
A company is developing an application in the AWS Cloud. The application's HTTP API contains critical information that is published in Amazon API Gateway. The critical information must be accessible from only a limited set of trusted IP addresses that belong to the company's internal network.

Which solution will meet these requirements?
- Set up an API Gateway private integration to restrict access to a predefined set of IP addresses.
- Create a resource policy for the API that denies access to any IP address that is not specifically allowed.
- Directly deploy the API in a private subnet. Create a network ACL. Set up rules to allow the traffic from specific IP addresses.
- Modify the security group that is attached to API Gateway to allow inbound traffic from only the trusted IP addresses.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Company phát triển application với HTTP API trong API Gateway

Chỉ muốn cho phép truy cập từ danh sách IP giới hạn (limited set of trusted IP addresses) thuộc mạng nội bộ công ty (company's internal network)

✅ Đáp án đúng:

Create a resource policy for the API that denies access to any IP address that is not specifically allowed.

![img](https://static.cloudexam.pro/courses/5/1756734464759-qbwgf5fe-CleanShot_2025-09-01_at_22.47.32.png)

→ Resource Policy cũng là một cách không mất tiền để định nghĩa quyền kiểm soát truy cập API Gateway theo IP. Policy có thể định nghĩa chính xác các IP được phép và từ chối tất cả IP khác. Đây là best practice cho IP whitelisting trong API Gateway.

Các đáp án sai:

❌ Set up an API Gateway private integration to restrict access to a predefined set of IP addresses.

→ Sai. Private integration chỉ là cơ chế cho phép API Gateway kết nối với backend services trong VPC, không liên quan đến việc kiểm soát client access theo IP.

❌ Directly deploy the API in a private subnet. Create a network ACL. Set up rules to allow the traffic from specific IP addresses.

→ Sai. API Gateway là managed service, về bản chất không thể deploy trực tiếp trong subnet của VPC. Do đó Network ACL cũng không áp dụng cho API Gateway.

❌ Modify the security group that is attached to API Gateway to allow inbound traffic from only the trusted IP addresses.

→ Sai. API Gateway không sử dụng security groups như EC2. Security groups không áp dụng cho managed services như API Gateway.

🔑 Tips and tricks:

API Gateway access control theo IP thì có thể sử dụng resource policy hoặc

API Gateway là managed service nên không thể đặt bên trong subnet VPC. Kể cả Private REST APIs thì cũng chỉ là cơ chế cho phép access đến API Gateway thông qua endpoint từ VPC, chứ không phải bản thân API GW được đặt trong VPC
</details>

---

### Q2. 
A company needs to give a globally distributed development team secure access to the company's AWS resources in a way that complies with security policies.

The company currently uses an on-premises Active Directory for internal authentication. The company uses AWS Organizations to manage multiple AWS accounts that support multiple projects.

The company needs a solution to integrate with the existing infrastructure to provide centralized identity management and access control.

Which solution will meet these requirements with the LEAST operational overhead?

- Set up AWS Directory Service to create an AWS managed Microsoft Active Directory on AWS. Establish a trust relationship with the on-premises Active Directory. Use IAM rotes that are assigned to Active Directory groups to access AWS resources within the company's AWS accounts.
- Create an IAM user for each developer. Manually manage permissions for each IAM user based on each user's involvement with each project. Enforce multi-factor authentication (MFA) as an additional layer of security.
- Use AD Connector in AWS Directory Service to connect to the on-premises Active Directory. Integrate AD Connector with AWS IAM Identity Center. Configure permissions sets to give each AD group access to specific AWS accounts and resources.
- Use Amazon Cognito to deploy an identity federation solution. Integrate the identity federation solution with the on-premises Active Directory. Use Amazon Cognito to provide access tokens for developers to access AWS accounts and resources.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty cần cấp quyền truy cập vào môi trường AWS cho team dev, cụ thể là các AWS accounts hiện đang được quản lí bởi AWS Organizations

Đã có on-premises Active Directory để xác thực hệ thống nội bộ

Cần tích hợp với hạ tầng hiện có (existing infrastructure), ở đây có nghĩa là sử dụng Active Directory có sẵn để quản lý xác thực tập trung

Yêu cầu: ít tốn công nhất (LEAST operational overhead)

✅ Đáp án đúng:

Use AD Connector in AWS Directory Service to connect to the on-premises Active Directory. Integrate AD Connector with AWS IAM Identity Center. Configure permissions sets to give each AD group access to specific AWS accounts and resources.

AD Connector cho phép kết nối trực tiếp với AD hiện có mà không cần tạo mới (integrate with the existing infrastructure)

IAM Identity Center (trước đây là AWS SSO) cung cấp quản lý quyền truy cập tập trung cho multiple AWS accounts

Permission sets cho phép gán quyền theo nhóm AD, phù hợp với Organizations structure

Đây là kiến trúc tốn ít operational overhead nhất vì tận dụng AD hiện có + tự động sync

Kiến trúc:
![img](https://static.cloudexam.pro/courses/5/1756737277149-vcqaesq6-CleanShot_2025-09-01_at_23.33.17.png)

Các đáp án sai:

❌ Set up AWS Directory Service to create an AWS managed Microsoft Active Directory on AWS. Establish a trust relationship with the on-premises Active Directory. Use IAM rotes that are assigned to Active Directory groups to access AWS resources within the company's AWS accounts.

→ Tạo thêm một AD mới trên AWS, tăng complexity và operational overhead không cần thiết, không tận dung được infrastructure hiện tại

❌ Create an IAM user for each developer. Manually manage permissions for each IAM user based on each user's involvement with each project. Enforce multi-factor authentication (MFA) as an additional layer of security.

→ Tạo thủ công từng IAM user cho mỗi người developmer sẽ bị operational overhead rất cao, không có khả năng scale hiệu quả

❌ Use Amazon Cognito to deploy an identity federation solution. Integrate the identity federation solution with the on-premises Active Directory. Use Amazon Cognito to provide access tokens for developers to access AWS accounts and resources.

→ Cognito chủ yếu sử dụng cho web applications, không phù hợp cho cấp quyền truy cập đến AWS Organizations

🔑 Tips and tricks:

Khi câu hỏi nhắc đến việc liên kết người dùng Active Directory để cho phép truy cập account trong Organization thì thường nghĩ ngay đến IAM Identity Center
</details>

---

### Q3. 
A company is hosting a website behind multiple Application Load Balancers. The company has different distribution rights for its content around the world. A solutions architect needs to ensure that users are served the correct content without violating distribution rights.

Which configuration should the solutions architect choose to meet these requirements?
- Configure Amazon CloudFront with AWS WAF
- Configure Amazon Route 53 with a geoproximity routing policy
- Configure Application Load Balancers with AWS WAF
- Configure Amazon Route 53 with a geolocation policy

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty sử dụng nhiều Application Load Balancers để host website

Công ty có quyền phân phối nội dung khác nhau (different distribution rights) theo từng vùng trên thế giới

Cần đảm bảo users được phục vụ nội dung đúng mà không vi phạm quyền phân phối

Tóm lại: Cần giới hạn quyền truy cập của user theo vùng hoặc quốc gia

✅ Đáp án đúng:

Configure Amazon Route 53 with a geolocation policy

Route 53 geolocation policy cho phép định tuyến traffic dựa trên vị trí địa lý của user

Có thể chỉ định chính xác user từ quốc gia/châu lục nào được truy cập vào endpoint nào

Từ đó giúp đảm bảo tuân thủ distribution rights bằng cách kiểm soát truy cập theo địa lý

Các đáp án sai:

❌ Configure Amazon CloudFront with AWS WAF

CloudFront là CDN để cache và tăng tốc, WAF là firewall bảo mật

Không có khả năng kiểm soát phân phối nội dung theo quyền địa lý cụ thể

Mặc dù WAF có khả năng hạn chế IP theo quốc gia, tuy nhiên bài toán có nhiều load balancer trên nhiều nơi khác nhau, do đó mỗi load balancer sẽ cần 1 WAF khác nhau để quản lí, từ đó tốn công sức và tốn thêm nhiều chi phí so với việc routing thông qua Route53

❌ Configure Application Load Balancers with AWS WAF

ALB + WAF chỉ có thể block/allow traffic dựa trên rules, không có khả năng routing theo địa lý

Không giải quyết được yêu cầu phân phối nội dung khác nhau theo vùng

❌ Configure Amazon Route 53 with a geoproximity routing policy

Geoproximity routing phức tạp hơn, chủ yếu dùng để điều chỉnh traffic đến server có khả năng xử lý thuận tiện hơn hơn bất kể vị trí của người dùng. Ví dụ có server to ở Mỹ, server nhỏ ở Châu Âu thì vẫn có một phần ngườ dùng Châu Âu sẽ được điều hướng sang Mỹ vì server bên đó có tải tốt hơn. Tức là sẽ routing theo vị trí của resource chứ không phải vị trí của người dùng.

Không kiểm soát được quyền truy cập theo địa lý như geolocation policy

🔑 Tips and tricks:

Hệ thống global cần hạn chế quyền truy cập theo khu vực hoặc quốc gia, có thể nghĩ đến Route53 Geolocation Routing Policy
</details>

---

### Q4. 
A company runs an application in a private subnet behind an Application Load Balancer (ALB) in a VPC. The VPC has a NAT gateway and an internet gateway. The application calls the Amazon S3 API to store objects.

According to the company's security policy, traffic from the application must not travel across the internet.

Which solution will meet these requirements MOST cost-effectively?
- Configure an S3 interface endpoint. Create a security group that allows outbound traffic to Amazon S3.
- Configure an S3 gateway endpoint. Update the VPC route table to use the endpoint.
- Configure an S3 bucket policy to allow traffic from the Elastic IP address that is assigned to the NAT gateway.
- Create a second NAT gateway in the same subnet where the legacy application is deployed. Update the VPC route table to use the second NAT gateway.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng chạy trong private subnet với ALB, hiện đang kết nối đến S3

VPC có NAT gateway và internet gateway

Ứng dụng cần gọi Amazon S3 API để lưu objects mà traffic không được đi qua internet

Yêu cầu: giải pháp cost-effective nhất

✅ Đáp án đúng:

Configure an S3 gateway endpoint. Update the VPC route table to use the endpoint.

Gateway endpoint cho S3 là dịch vụ miễn phí, cho phép kết nối đến S3 với traffic đi trực tiếp trong đường truyền nội bộ của AWS, không qua internet. Muốn cho các resource trong private subnet truy cập đến S3 thì chỉ cần cập nhật route table để điều hướng traffic S3 qua endpoint này là sẽ đạt được yêu cầu bài toán

![img](https://static.cloudexam.pro/courses/5/1756825332440-3ncwvmv3-image.png)

Các đáp án sai:

❌ Configure an S3 interface endpoint. Create a security group that allows outbound traffic to Amazon S3.

→ Solution hợp lệ, tuy nhiên sử dụng Interface endpoint sẽ bị tính phí theo giờ và phí data processing, đắt hơn gateway endpoint, do đó không cần thiết ở đây

❌ Configure an S3 bucket policy to allow traffic from the Elastic IP address that is assigned to the NAT gateway.

→ Traffic vẫn phải đi qua NAT gateway → internet, vi phạm security policy

❌ Create a second NAT gateway in the same subnet where the legacy application is deployed. Update the VPC route table to use the second NAT gateway.

→ Thêm chi phí NAT gateway không cần thiết và traffic vẫn đi qua internet

🔑 Tips and tricks:

Access đến các service từ bên trong VPC mà không đi qua internet, nghĩ ngay đến Gateway Endpoint (áp dụng cho S3, DynamoDB)
</details>

---

### Q5. 
A company's image-hosting website gives users around the world the ability to up load, view, and download images from their mobile devices. The company currently hosts the static website in an Amazon S3 bucket.

Because of the website's growing popularity, the website's performance has decreased. Users have reported latency issues when they upload and download images.

The company must improve the performance of the website.

Which solution will meet these requirements with the LEAST implementation effort?
- Configure an Amazon CloudFront distribution for the S3 bucket to improve the download performance. Enable S3 Transfer Acceleration to improve the upload performance.
- Configure Amazon EC2 instances of the right sizes in multiple AWS Regions. Migrate the application to the EC2 instances. Use an Application Load Balancer to distribute the website traffic equally among the EC2 instances. Configure AWS Global Accelerator to address global demand with low latency.
- Configure an Amazon CloudFront distribution that uses the S3 bucket as an origin to improve the download performance. Configure the application to use CloudFront to upload images to improve the upload performance. Create S3 buckets in multiple AWS Regions. Configure replication rules for the buckets to replicate users' data based on the users' location. Redirect downloads to the S3 bucket that is closest to each user's location.
- Configure AWS Global Accelerator for the S3 bucket to improve network performance. Create an endpoint for the application to use Global Accelerator instead of the S3 bucket.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty host web tĩnh (static website) trên S3 bucket

Users trên toàn thế giới (around the world) upload, view, download ảnh

Users báo cáo độ trễ (latency issues) khi upload và download

Cần cải thiện hiệu suất với ít effort nhất (LEAST implementation effort)

✅ Đáp án đúng:

Configure an Amazon CloudFront distribution for the S3 bucket to improve the download performance. Enable S3 Transfer Acceleration to improve the upload performance.

Bài toán đang gặp vấn đề ở độ trễ khi load và upload, để giải quyết thì sẽ cần:

CloudFront là CDN service, cho phép cache các loại data như content tĩnh ở điểm cung cấp dịch vụ gần với người dùng nhất (edge locations), từ đó giúp giảm độ trễ, giúp cho việc load các content này nhanh hơn

S3 Transfer Acceleration là một tính năng của S3 cho phép upload bằng cách route traffic đến edge location, từ đó đi qua mạng nội bộ của AWS, giúp giảm độ trễ

Ít effort nhất: cả 2 cái ở trên đều là Managed service / tính năng có sẵn dễ config để đạt được yêu cầu, tốn ít effort

![img](https://static.cloudexam.pro/courses/5/1756826475250-hh7xt4wx-image.png)


Các đáp án sai:

❌ Configure Amazon EC2 instances of the right sizes in multiple AWS Regions. Migrate the application to the EC2 instances. Use an Application Load Balancer to distribute the website traffic equally among the EC2 instances. Configure AWS Global Accelerator to address global demand with low latency.

→ Yêu cầu migrate toàn bộ application, effort rất lớn, không phù hợp yêu cầu "LEAST implementation effort"

❌ Configure an Amazon CloudFront distribution that uses the S3 bucket as an origin to improve the download performance. Configure the application to use CloudFront to upload images to improve the upload performance. Create S3 buckets in multiple AWS Regions. Configure replication rules for the buckets to replicate users' data based on the users' location. Redirect downloads to the S3 bucket that is closest to each user's location.

→ Solution với độ phức tạp quá mức không cần thiết: nhiều components, replication rules, redirect logic -> tốn effort

❌ Configure AWS Global Accelerator for the S3 bucket to improve network performance. Create an endpoint for the application to use Global Accelerator instead of the S3 bucket.

→ Global Accelerator mặc dù giúp giảm độ trễ, tăng tốc kết nối tuy nhiên không trực tiếp support S3, chủ yếu sử dụng cho ALB/NLB/EC2 endpoints

🔑 Tips and tricks:

Các câu hỏi liên quan đến web tĩnh, content tĩnh mà cần tối ưu hoá độ trễ hay tăng tốc upload thì thường sẽ nghĩ đến CloudFront, S3 Transfer Acceleration
</details>

---

### Q6. 
A digital image processing company wants to migrate its on-premises monolithic application to the AWS Cloud. The company processes thousands of images and generates large files as part of the processing workflow.

The company needs a solution to manage the growing number of image processing jobs. The solution must also reduce the manual tasks in the image processing workflow. The company does not want to manage the underlying infrastructure of the solution.

Which solution will meet these requirements with the LEAST operational overhead?
- Use Amazon Elastic Container Service (Amazon ECS) with Amazon EC2 Spot Instances to process the images. Configure Amazon Simple Queue Service (Amazon SQS) to orchestrate the workflow. Store the processed files in Amazon Elastic File System (Amazon EFS).
- Use AWS Lambda functions and Amazon EC2 Spot Instances to process the images. Store the processed files in Amazon FSx.
- Use AWS Batch jobs to process the images. Use AWS Step Functions to orchestrate the workflow. Store the processed files in an Amazon S3 bucket.
- Deploy a group of Amazon EC2 instances to process the images. Use AWS Step Functions to orchestrate the workflow. Store the processed files in an Amazon Elastic Block Store (Amazon EBS) volume.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty xử lý ảnh muốn di chuyển hệ thống từ on-premises lên AWS Cloud

Cần solution cho việc chạy và quản lý workflow bao gồm nhiều job xử lí ảnh sao cho ít thao tác thủ công nhất và không muốn quản lí infastructure

Yêu cầu: Giải pháp với chi phí vận hành thấp nhất

✅ Đáp án đúng:

Use AWS Batch jobs to process the images. Use AWS Step Functions to orchestrate the workflow. Store the processed files in an Amazon S3 bucket.

AWS Batch là managed service cho phép tự động hoá và scale các công việc chạy job trên AWS, cho phép developer tập trung vào việc thực thi job mà không phải quản lý toàn bộ cơ sở hạ tầng. Do đó AWS Batch sẽ phù hợp với job xử lí ảnh của bài toán ở đây.

Step Functions là serverless service giúp điều phối & tự động hoá, cho phép build các luồng công việc (workflow) phức tạp kết hợp nhiều service khác nhau, từ đó giúp giảm các tác vụ & thao tác thủ công. Trong bài toán hiện tại có thể sử dụng Step Functions để điều phối các job của AWS Batch, tạo ra một luồng xử lí tự động hoá hoàn toàn.

S3 là giải pháp lưu trữ giá rẻ, rất phù hợp để lưu trữ ảnh sau khi đã xử lí xong.

Tổng thể giải pháp này đem lại chi phí vận hành thấp nhất vì sử dụng hầu hết các service serverless.

Kiến trúc tham khảo:
![img](https://static.cloudexam.pro/courses/5/1756828230479-xd5expuo-image.png)

Các đáp án sai:

❌ Use Amazon Elastic Container Service (Amazon ECS) with Amazon EC2 Spot Instances to process the images. Configure Amazon Simple Queue Service (Amazon SQS) to orchestrate the workflow. Store the processed files in Amazon Elastic File System (Amazon EFS).

Solution sử dụng EC2, do đó vẫn tốn effort vận hành

❌ Use AWS Lambda functions and Amazon EC2 Spot Instances to process the images. Store the processed files in Amazon FSx.

Tương tự như trên, vẫn là solution sử dụng EC2, do đó vẫn tốn effort vận hành

❌ Deploy a group of Amazon EC2 instances to process the images. Use AWS Step Functions to orchestrate the workflow. Store the processed files in an Amazon Elastic Block Store (Amazon EBS) volume.

Tương tự như trên, vẫn là solution sử dụng EC2, do đó vẫn tốn effort vận hành

🔑 Tips and tricks:

Xuất hiện keyword Workflow thì thường sẽ nghĩ đến Step Functions

Solution cần ít effort vận hành thì các đáp án sẽ không ưu tiên chọn EC2

Các yêu cầu về chạy job mà tốn ít effort thì thường sẽ nghĩ đến các service như Lambda, ECS Fargate, AWS Batch
</details>

---

### Q7. 
A company wants to provide users with access to AWS resources. The company has 1,500 users and manages their access to on-premises resources through Active Directory user groups on the corporate network. However, the company does not want users to have to maintain another identity to access the resources. A solutions architect must manage user access to the AWS resources while preserving access to the on-premises resources.

What should the solutions architect do to meet these requirements?

- Configure Security Assertion Markup Language (SAML) 2.0-based federation. Create roles with the appropriate policies attached. Map the roles to the Active Directory groups.
- Create an IAM user for each user in the company. Attach the appropriate policies to each user.
- Use Amazon Cognito with an Active Directory user pool. Create roles with the appropriate policies attached.
- Define cross-account roles with the appropriate policies attached. Map the roles to the Active Directory groups.


<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty có 1,500 users đang dùng Active Directory để quản lý truy cập ở phía on-premises

Muốn cấp quyền cho các user này truy cập vào môi trường AWS

Không muốn dùng thêm solution về quản lí identity mới, mà vẫn phải đảm bảo quyền to on-premises resources

✅ Đáp án đúng:

Configure Security Assertion Markup Language (SAML) 2.0-based federation. Create roles with the appropriate policies attached. Map the roles to the Active Directory groups.

SAML federation cho phép các người dùng hiện tại sử dụng trực tiếp credentials của Active Directory để truy cập vào môi trường AWS mà không cần một cơ chế quản lí identity mới (chẳng hạn như IAM). Việc cấp quyền cho các người dùng này sẽ thông qua mapping roles với AD groups giúp quản lý permissions hiệu quả cho 1,500 users.

![img](https://static.cloudexam.pro/courses/5/1756906624538-wwje5hdn-image.png)

Các đáp án sai:

❌ Create an IAM user for each user in the company. Attach the appropriate policies to each user.

→ Tạo 1,500 IAM users riêng biệt vi phạm yêu cầu "không quản lý thêm identity" vì như thế sẽ phải quản lý thêm IAM User Credentials.

❌ Use Amazon Cognito with an Active Directory user pool. Create roles with the appropriate policies attached.
→ Cognito User Pool vừa tạo ra thêm một identity store riêng biệt, vi phạm yêu cầu "không quản lý thêm identity", hơn nữa không tương tác và liên kết trực tiếp với Active Directory hiện tại được.

❌ Define cross-account roles with the appropriate policies attached. Map the roles to the Active Directory groups.

→ Cross-account roles dùng cho việc access giữa các AWS accounts khác nhau, không giải quyết việc integrate với on-premises Active Directory.

🔑 Tips and tricks:

Để cho phép các user trong Active Directory có thể access môi trường aws của một account đơn lẻ, không phải AWS Organizations thì sẽ nghĩ đến liên kết SAML 2.0 & IAM Role

📖 Reference:

https://aws.amazon.com/blogs/security/enabling-federation-to-aws-using-windows-active-directory-adfs-and-saml-2-0/
</details>

---

### Q8. 
A company has a three-tier web application that processes orders from customers. The web tier consists of Amazon EC2 instances behind an Application Load Balancer. The processing tier consists of EC2 instances. The company decoupled the web tier and processing tier by using Amazon Simple Queue Service (Amazon SQS). The storage layer uses Amazon DynamoDB.

At peak times, some users report order processing delays and halls. The company has noticed that during these delays, the EC2 instances are running at 100% CPU usage, and the SQS queue fills up. The peak times are variable and unpredictable.

The company needs to improve the performance of the application.

Which solution will meet these requirements?
- Use scheduled scaling for Amazon EC2 Auto Scaling
- Use Amazon ElastiCache for Redis in front of the DynamoDB
- Add an Amazon CloudFront distribution to cache responses for the web tier
- Use an Amazon EC2 Auto Scaling target tracking policy to scale out the processing tier instances. Use the ApproximateNumberOfMessages attribute to determine when to scale.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Bài toán có hệ thống xử lý order bằng kiến trúc : Web tier (EC2 + ALB) → SQS queue → Processing tier (EC2) → DynamoDB

Hệ thống có vấn đề khi gặp traffic không dự đoán được (variable and unpredictable):

EC2 processing tier chạy 100% CPU

SQS queue fills up (nhiều message mà không xử lý hết)

Users nhận thấy việc xử lí order bị delays

Cần cải thiện performance

✅ Đáp án đúng:

Use an Amazon EC2 Auto Scaling target tracking policy to scale out the processing tier instances. Use the ApproximateNumberOfMessages attribute to determine when to scale.

Có thể thấy hệ thống hiện tại đang bị bottle neck ở việc EC2 Processing Tier không xử lí hết data trong queue do chưa có cơ chế scale, do đó sẽ cần implement Auto Scaling Group. Kết hợp setup Target tracking policy để có thể tự động scale EC2 dựa trên metric

ApproximateNumberOfMessages - metric thể hiện số lượng message đang có trong queue. Tức là càng nhiều message thì sẽ scale càng nhiều EC2. Do đó đây là đáp án hợp lý, có thể scale đáp ứng được cả traffic thất thường (unpredictable peaks). Có thể xem kiến trúc bên dứoi để thấy trước & sau khi cải thiện.

![img](https://static.cloudexam.pro/courses/5/1756908848631-bu3e2r5r-CleanShot_2025-09-03_at_23.13.19_2x.png)

Các đáp án sai:

❌ Use scheduled scaling for Amazon EC2 Auto Scaling

Do traffic thất thường "variable and unpredictable" nên sẽ không thể sử dụng schedule scaling (scale theo lịch cố định) được

❌ Use Amazon ElastiCache for Redis in front of the DynamoDB

Sai vì bottleneck không phải ở DynamoDB mà ở processing tier (EC2 100% CPU + SQS fills up)

❌ Add an Amazon CloudFront distribution to cache responses for the web tier

Sai vì vấn đề không phải ở web tier mà ở processing tier không kịp xử lý queue

🔑 Tips and tricks:

Khi bài toán có EC2 xử lí SQS mà gặp vấn đề bottleneck thì nghĩ đến việc sử dụng kết hợp Auto Scaling Group và scale dựa trên metric số lượng message có trong queue

Việc scale EC2 thì thường sẽ sử dụng Target Tracking policy vì có tính tự động hoá cao, giúp đáp ứng khi gặp traffic lớn hoặc thất thường

📖 Reference:

https://docs.aws.amazon.com/autoscaling/ec2/userguide/as-using-sqs-queue.html
</details>

---

### Q9. 
A company hosts an application in a private subnet behind an Application Load Balancer. The company has already integrated the application with Amazon Cognito. The company uses an Amazon Cognito user pool to authenticate users.

The company needs to modify the application so the authenticated users can securely store their documents in an Amazon S3 bucket via the application.

Which combination of steps will securely integrate Amazon S3 with the application? (Choose two.)
- Use the existing Amazon Cognito user pool to generate Amazon S3 access tokens for users when they successfully log in.
- Create a NAT gateway in the VPC where the company hosts the application. Assign a policy to the S3 bucket to deny any request that is not initiated from Amazon Cognito.
- Create an Amazon S3 VPC endpoint in the same VPC where the company hosts the application.
- Attach a policy to the S3 bucket that allows access only from the users' IP addresses.
- Create an Amazon Cognito identity pool to generate secure Amazon S3 access tokens for users when they successfully log in.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Hệ thống host trong private subnet

Có sử dụng Amazon Cognito user pool để xác thực users

Hệ thống cần cho phép người dùng đã xác thực có thể kết nối đến S3 một cách an toàn thông qua application

✅ Đáp án đúng:

Create an Amazon Cognito identity pool to generate secure Amazon S3 access tokens for users when they successfully log in.

Identity pool là service giúp tạo credentials tạm thời cho các user bên ngoài có thể truy cập vào môi trường AWS, có thể sử dụng để truy cập đến S3

Create an Amazon S3 VPC endpoint in the same VPC where the company hosts the application.

Do người dùng access đến S3 thông qua application, do đó application sẽ cần cơ chế để access đến S3 một cách an toàn, do đó sẽ sử dụng S3 VPC Endpoint - service cho phép application từ bên trong VPC có thể access đến S3 thông qua đường truyền nội bộ của AWS mà không đi qua internet, do đó sẽ có tính an toàn

Các đáp án sai:

❌ Use the existing Amazon Cognito user pool to generate Amazon S3 access tokens for users when they successfully log in.

User pool chỉ dùng để xác thực, không thể tạo AWS access tokens để truy cập S3

❌ Create a NAT gateway in the VPC where the company hosts the application. Assign a policy to the S3 bucket to deny any request that is not initiated from Amazon Cognito.

NAT gateway không cần thiết khi có sử dụng VPC endpoint, hơn nữa traffic sẽ đi ra internet nên sẽ không an toàn (not secured)

❌ Attach a policy to the S3 bucket that allows access only from the users' IP addresses.

IP addresses của users có thể thay đổi và không thể biết trước được, do đó đây không phải giải pháp có tính mở rộng (scalable) và an toàn (secure)

🔑 Tips and tricks:

Để cấp quyền cho các user bên ngoài hệ thống có thể truy cập các AWS Service thì nghĩ đến Cognito Identity Pool

Cho phép application trong VPC access đến các service khác một cách an toàn thì nghĩ đến VPC Endpoint
</details>

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
- Provision two Amazon Simple Queue Service (Amazon SQS) queues. Use one SQS queue for order collection. Use the second SQS queue for order fulfillment. Configure the EC2 instances to poll their respective queues. Scale the Auto Scaling groups based on the number of messages in each queue.
- Use Amazon CloudWatch to monitor the CPUUtilization metric for each instance in both Auto Scaling groups. Configure each Auto Scaling group's minimum capacity to meet its peak workload value.
- Use Amazon CloudWatch to monitor the CPUUtilization metric for each instance in both Auto Scaling groups. Configure a CloudWatch alarm to invoke an Amazon Simple Notification Service (Amazon SNS) topic to create additional Auto Scaling groups on demand.
- Provision two Amazon Simple Queue Service (Amazon SQS) queues. Use one SQS queue for order collection. Use the second SQS queue for order fulfillment. Configure the EC2 instances to poll their respective queues. Scale the Auto Scaling groups based on notifications that the queues send.

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

![img](https://static.cloudexam.pro/courses/5/1756977911129-d841wucm-image.png)

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

### Q20. 

A company currently stores 5 TB of data in on-premises block storage systems. The company's current storage solution provides limited space for additional data. The company runs applications on premises that must be able to retrieve frequently accessed data with low latency. The company requires a cloud-based storage solution.

Which solution will meet these requirements with the MOST operational efficiency?
- Use Amazon S3 File Gateway with SMB file system
- Use Volume Gateway with stored volumes
- Use an AWS Storage Gateway Volume Gateway with cached volumes as iSCSI targets.
- Use Tape Gateway with virtual tapes

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty lưu dữ liệu trên block storage ở phía on-premises

Không gian lưu trữ hạn chế nên cần giải pháp lưu trữ cloud-based với hiệu quả vận hành cao nhất (MOST operational efficiency)

Ứng dụng on-premises cần truy xuất dữ liệu thường xuyên truy cập (frequently accessed) với độ trễ thấp (low latency)

✅ Đáp án đúng:

Use an AWS Storage Gateway Volume Gateway with cached volumes as iSCSI targets.

Cached volumes lưu dữ liệu thường xuyên truy cập tại local cache → đảm bảo low latency

Các dữ liệu còn lại sẽ được đồng bộ và lưu trữ tại Amazon S3, do đó có khả năng mở rộng không giới hạn

Tương thích với block storage hiện tại (iSCSI protocol)

Có độ hiệu quả vận hành (operational efficiency) cao vì tự động hoá việc quản lí cache tại local

![img](https://static.cloudexam.pro/courses/5/1756991188130-0brb4wc2-CleanShot_2025-09-04_at_22.05.39.png)

Các đáp án sai:

❌ Use Amazon S3 File Gateway with SMB file system

Đề yêu cầu block storage, không phải file storage. SMB là file-based protocol, không tương thích với ứng dụng block storage hiện tại.

❌ Use Volume Gateway with stored volumes

Sai vì stored volumes mặc dù có đồng bộ lên S3 tuy nhiên vẫn lưu toàn bộ dữ liệu tại on-premises → không giải quyết vấn đề limited space.

❌ Use Tape Gateway with virtual tapes

Sai vì Tape Gateway dùng cho use case là archival/backup, không phải cho data hay được truy cập với độ trễ thấp.

🔑 Tips and tricks:

Khi cần solution storage cho local mà support dạng block storage thì sẽ nghĩ đến Volume Gateway. Keyword: iSCSI, Block Storage, Volume
</details>

---

### Q21. 
A company is creating a prototype of an ecommerce website on AWS. The website consists of an Application Load Balancer, an Auto Scaling group of Amazon EC2 instances for web servers, and an Amazon RDS for MySQL DB instance that runs with the Single-AZ configuration.

The website is slow to respond during searches of the product catalog. The product catalog is a group of tables in the MySQL database that the company does not update frequently. A solutions architect has determined that the CPU utilization on the DB instance is high when product catalog searches occur.

What should the solutions architect recommend to improve the performance of the website during searches of the product catalog?
- Implement an Amazon ElastiCache for Redis cluster to cache the product catalog. Use lazy loading to populate the cache.
- Migrate the product catalog to an Amazon Redshift database. Use the COPY command to load the product catalog tables.
- Add an additional scaling policy to the Auto Scaling group to launch additional EC2 instances when database response is slow.
- Turn on the Multi-AZ configuration for the DB instance. Configure the EC2 instances to throttle the product catalog queries that are sent to the database.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Website gồm ALB + Auto Scaling EC2 + RDS MySQL Single-AZ

Tìm kiếm bảng product catalog trong db bị chậm dẫn đến CPU cao

Product catalog là nhóm bảng không update thường xuyên trong MySQL

Cần cải thiện performance khi search catalog

✅ Đáp án đúng:

Implement an Amazon ElastiCache for Redis cluster to cache the product catalog. Use lazy loading to populate the cache.

Có thể thấy được bảng product catalog mỗi khi query sẽ gây ảnh hưởng performance của DB. Để tránh việc này thì có thể cache data hay được query ra cache riêng biệt. Hơn nữa bảng này không update thường xuyên nên việc sử dụng cache sẽ càng hợp lý vì data sẽ lâu bị outdated.

Giải pháp cache dùng cho RDS & Aurora chính là ElastiCache, giúp tối ưu cho dữ liệu đọc nhiều, ghi ít như product catalog.

ElastiCache Redis giảm tải CPU cho RDS bằng cách trả về data query từ memory thay vì phải query trực tiếp vào database.

Lazy Caching là hình thức sẽ ưu tiên query data từ cache, nếu không có thì sẽ vào DB để lấy sau đó đưa lại vào cache, để từ các request lần sau sẽ có data sẵn sàng trong cache để trả về luôn.

![img](https://static.cloudexam.pro/courses/5/1756992142856-eq3tglsx-image.png)


Các đáp án sai:

❌ Migrate the product catalog to an Amazon Redshift database. Use the COPY command to load the product catalog tables.

→ Redshift dành cho data warehousing/analytics & bigdata, không phù hợp cho use case ở đây.

❌ Add an additional scaling policy to the Auto Scaling group to launch additional EC2 instances when database response is slow.
→ Vấn đề nằm ở database bottleneck, không phải ở phía web server. Scale thêm EC2 chỉ tạo thêm connection đến database, làm tình trạng tệ hơn.

❌ Turn on the Multi-AZ configuration for the DB instance. Configure the EC2 instances to throttle the product catalog queries that are sent to the database.

→ Multi-AZ chỉ cải thiện tính khả dụng (availability) cho DB chứ không giúp cải thiện performance.

🔑 Tips and tricks:

Với các bài toán dùng DB dạng quan hệ mà data đọc nhiều, ghi ít thì có thể sử dụng ElastiCache để cache data
</details>

---

### Q22. 
A company runs its legacy web application on AWS. The web application server runs on an Amazon EC2 instance in the public subnet of a VPC. The web application server collects images from customers and stores the image files in a locally attached Amazon Elastic Block Store (Amazon EBS) volume. The image files are uploaded every night to an Amazon S3 bucket for backup.

A solutions architect discovers that the image files are being uploaded to Amazon S3 through the public endpoint. The solutions architect needs to ensure that traffic to Amazon S3 does not use the public endpoint.

Which solution will meet these requirements?
- Move the S3 bucket inside the VPC. Configure the subnet route table to access the S3 bucket through private IP addresses.
- Create an Amazon S3 access point for the Amazon EC2 instance inside the VPC. Configure the web application to upload by using the Amazon S3 access point.
- Configure an AWS Direct Connect connection between the VPC that has the Amazon EC2 instance and Amazon S3 to provide a dedicated network path.
- Create a gateway VPC endpoint for the S3 bucket that has the necessary permissions for the VPC. Configure the subnet route table to use the gateway VPC endpoint.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Web application chạy trên EC2 trong public subnet

Upload image files lên S3 bucket mỗi đêm để backup

Hiện tại traffic đến S3 đang đi qua public endpoint (tức là đi qua internet)

Yêu cầu: đảm bảo traffic đến S3 không sử dụng public endpoint

✅ Đáp án đúng:

Create a gateway VPC endpoint for the S3 bucket that has the necessary permissions for the VPC. Configure the subnet route table to use the gateway VPC endpoint.

Gateway VPC Endpoint cho S3 là giải pháp miễn phí để định tuyến traffic từ EC2 đến S3 qua mạng nội bộ AWS thay vì internet. Route table sẽ tự động định tuyến traffic S3 qua endpoint này.

![img](https://static.cloudexam.pro/courses/5/1756992628593-gn6vdx1v-image.png)

Các đáp án sai:

❌ Move the S3 bucket inside the VPC. Configure the subnet route table to access the S3 bucket through private IP addresses.

→ Vô lý vì S3 là managed service, không thể di chuyển vào trong VPC. S3 bucket luôn nằm ngoài VPC.

❌ Create an Amazon S3 access point for the Amazon EC2 instance inside the VPC. Configure the web application to upload by using the Amazon S3 access point.

→ S3 Access Point chỉ là cách thức để tạo ra các điểm truy cập đến S3 phục vụ các mục đích và quản lý quyền truy cập khác nhau, không giúp thay đổi đường đi của traffic. Traffic vẫn có thể đi qua public endpoint.

❌ Configure an AWS Direct Connect connection between the VPC that has the Amazon EC2 instance and Amazon S3 to provide a dedicated network path.

→ Sai và không cần thiết. Direct Connect dùng cho kết nối từ on-premises đến môi trường VPC, không phải cho traffic EC2-to-S3 trong cùng region.

🔑 Tips and tricks:

Cho phép application trong VPC access đến các service khác một cách an toàn, không đi qua internet thì nghĩ đến VPC Endpoint
</details>

---

### Q23. 
A company is launching a new application that requires a structured database to store user profiles, application settings, and transactional data. The database must be scalable with application traffic and must offer backups.

Which solution will meet these requirements MOST cost-effectively?
- Deploy a self-managed database on Amazon EC2 instances by using open source software. Use Spot Instances for cost optimization.
- Use Amazon RDS. Use on-demand capacity mode for the database with General Purpose SSD storage. Configure automatic backups with a retention period of 7 days.
- Use Amazon Aurora Serverless for the database. Use serverless capacity scaling. Configure automated backups to Amazon S3.
- Deploy a self-managed NoSQL database on Amazon EC2 instances. Use Reserved Instances for cost optimization. Configure automated backups directly to Amazon S3 Glacier Flexible Retrieval.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty cần cơ sở dữ liệu có cấu trúc (structured database) cho user profiles, settings, transactional data

Yêu cầu: khả năng scale theo traffic và có backup tự động

Mục tiêu: tối ưu chi phí nhất (MOST cost-effectively)

✅ Đáp án đúng:

Use Amazon Aurora Serverless for the database. Use serverless capacity scaling. Configure automated backups to Amazon S3.

Aurora Serverless là managed service cho phép tạo database dạng quan hệ (relational), có cấu trúc (structured) dưới dạng serverless, từ đó có khả năng scale tự động để đáp ứng traffic, có cơ chế backup định kì.

Với use case phổ thông thì sử dụng General Purpose SSD để tiết kiệm chi phí.

Các đáp án sai:

❌ Deploy a self-managed database on Amazon EC2 instances by using open source software. Use Spot Instances for cost optimization.

→ Deploy DB trên EC2 làm tăng operational overhead, hơn nữa Spot Instances không ổn định cho việc chạy database production.

❌ Use Amazon RDS. Use on-demand capacity mode for the database with General Purpose SSD storage. Configure automatic backups with a retention period of 7 days.

→ RDS on-demand luôn chạy và tính phí 24/7, hơn nữa không có khả năng scale để đáp ứng traffic.

❌ Deploy a self-managed NoSQL database on Amazon EC2 instances. Use Reserved Instances for cost optimization. Configure automated backups directly to Amazon S3 Glacier Flexible Retrieval.

→ Đề yêu cầu DB dạng data có cấu trúc structured database (SQL), không phải NoSQL.

🔑 Tips and tricks:

DB dạng quan hệ có khả năng tự scale để đáp ứng traffic thì nghĩ đến Aurora Serverless
</details>

---

### Q24. 
A company runs an on-premises application on a Kubernetes cluster. The company recently added millions of new customers. The company's existing on-premises infrastructure is unable to handle the large number of new customers. The company needs to migrate the on-premises application to the AWS Cloud.

The company will migrate to an Amazon Elastic Kubernetes Service (Amazon EKS) cluster. The company does not want to manage the underlying compute infrastructure for the new architecture on AWS.

Which solution will meet these requirements with the LEAST operational overhead?
- Use a self-managed node to supply compute capacity
- Use managed node groups to supply compute capacity
- Use managed node groups with Karpenter to supply compute capacity
- Use AWS Fargate to supply compute capacity. Create a Fargate profile. Use the Fargate profile to deploy the application.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty chạy ứng dụng Kubernetes cluster on-premises

Thêm hàng triệu khách hàng mới → hạ tầng hiện tại không đủ đáp ứng

Cần migrate sang Amazon EKS

Không muốn quản lý (does not want to manage) hạ tầng

Yêu cầu operational overhead thấp nhất (LEAST operational overhead)

✅ Đáp án đúng:

Use AWS Fargate to supply compute capacity. Create a Fargate profile. Use the Fargate profile to deploy the application.

Fargate là dịch vụ serverless compute, được quản lý và vận hành hoàn toàn bởi AWS. Không cần quản lý nodes, patching, scaling infrastructure.

Chỉ cần tạo Fargate profile và deploy pods, AWS sẽ lo toàn bộ việc vận hành kiến trúc bên dưới.

Các đáp án sai:

❌ Use a self-managed node to supply compute capacity

→ Self-managed nodes yêu cầu quản lý toàn bộ EC2 instances, patching OS, scaling manually → operational overhead cao nhất.

❌ Use managed node groups to supply compute capacity

→ Managed node groups vẫn cần quản lý node scaling, instance types, AMI updates → vẫn chạy trên nền EC2 nên có operational overhead.

❌ Use managed node groups with Karpenter to supply compute capacity

→ Karpenter giúp auto-scaling tốt hơn nhưng vẫn cần quản lý nodes và configure Karpenter → vẫn tốn operational overhead.

🔑 Tips and tricks:

Các câu hỏi yêu cầu operational overhead thấp nhất (LEAST operational overhead) thì thường sẽ nghĩ đến các service serverless, đối với container thì đó là AWS Fargate
</details>

---

### Q25. 
A company has migrated an application to Amazon EC2 Linux instances. One of these EC2 instances runs several 1-hour tasks on a schedule. These tasks were written by different teams and have no common programming language. The company is concerned about performance and scalability while these tasks run on a single instance. A solutions architect needs to implement a solution to resolve these concerns.

Which solution will meet these requirements with the LEAST operational overhead?
- Copy the tasks into AWS Lambda functions. Schedule the Lambda functions by using Amazon EventBridge (Amazon CloudWatch Events).
- Use AWS Batch to run the tasks as jobs. Schedule the jobs by using Amazon EventBridge (Amazon CloudWatch Events).
- Convert the EC2 instance to a container. Use AWS App Runner to create the container on demand to run the tasks as jobs.
- Create an Amazon Machine Image (AMI) of the EC2 instance that runs the tasks. Create an Auto Scaling group with the AMI to run multiple copies of the instance.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty cần chạy job xử lý video

Thời gian xử lý: lên đến 20 phút

Cần giải pháp scale tự động và chi phí tiết kiệm (cost-effective)

✅ Đáp án đúng:

Use AWS Batch to run the tasks as jobs. Schedule the jobs by using Amazon EventBridge (Amazon CloudWatch Events).

AWS Batch là managed service được thiết kế chuyên cho batch computing workloads

Tự động scale compute resources dựa trên job queue

Có support trigger với EventBridge để schedule lịch chạy job

LEAST operational overhead vì AWS Batch lo toàn bộ việc quản lý và scale các resource

Các đáp án sai:

❌ Copy the tasks into AWS Lambda functions. Schedule the Lambda functions by using Amazon EventBridge (Amazon CloudWatch Events).

Lambda có timeout tối đa 15 phút, ko đáp ứng yêu cầu task chạy 1 giờ

❌ Convert the EC2 instance to a container. Use AWS App Runner to create the container on demand to run the tasks as jobs.

App Runner chủ yếu dùng cho web applications và APIs (long-running services), không phù hợp cho việc chạy scheduled batch jobs

❌ Create an Amazon Machine Image (AMI) of the EC2 instance that runs the tasks. Create an Auto Scaling group with the AMI to run multiple copies of the instance.

Vẫn phải quản lý EC2 instances → operational overhead cao

🔑 Tips and tricks:

Đối với các câu hỏi về việc chạy job thì các solution thường nghĩ đến đó là AWS Lambda, ECS Fargate, Batch, EC2 Spot Instances.

Đầu tiên cần xem thời gian chạy job là bao lâu, nếu trên 15 phút sẽ loại ngay Lambda, ưu tiên chọn các solution managed, serverless như ECS Fargate, Batch.

Nếu thời gian dưới 15 phút thì thường sẽ lựa chọn Lambda
</details>

---

### Q26. 
A company hosts a multi-tier web application that uses an Amazon Aurora MySQL DB cluster for storage. The application tier is hosted on Amazon EC2 instances. The company's IT security guidelines mandate that the database credentials be encrypted and rotated every 14 days.

What should a solutions architect do to meet this requirement with the LEAST operational effort?
- Create a new AWS Key Management Service (AWS KMS) encryption key. Use AWS Secrets Manager to create a new secret that uses the KMS key with the appropriate credentials. Associate the secret with the Aurora DB cluster. Configure a custom rotation period of 14 days.
- Create two parameters in AWS Systems Manager Parameter Store: one for the user name as a string parameter and one that uses the SecureString type for the password. Select AWS Key Management Service (AWS KMS) encryption for the password parameter, and load these parameters in the application tier. Implement an AWS Lambda function that rotates the password every 14 days
- Store a file that contains the credentials in an AWS Key Management Service (AWS KMS) encrypted Amazon Elastic File System (Amazon EFS) file system. Mount the EFS file system in all EC2 instances of the application tier. Restrict the access to the file on the file system so that the application can read the file and that only super users can modify the file. Implement an AWS Lambda function that rotates the key in Aurora every 14 days and writes new credentials into the file
- Store a file that contains the credentials in an AWS Key Management Service (AWS KMS) encrypted Amazon S3 bucket that the application uses to load the credentials. Download the file to the application regularly to ensure that the correct credentials are used. Implement an AWS Lambda function that rotates the Aurora credentials every 14 days and uploads these credentials to the file in the S3 bucket

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Company có multi-tier web application sử dụng Aurora MySQL DB cluster

IT security yêu cầu về thông tin đăng nhập DB (credentials) như sau:

Phải có mã hóa (encrypted)

Phải được làm mới mỗi 14 ngày (rotated every 14 days)

Cần giải pháp với ít công sức vận hành nhất (LEAST operational effort)

✅ Đáp án đúng:

Create a new AWS Key Management Service (AWS KMS) encryption key. Use AWS Secrets Manager to create a new secret that uses the KMS key with the appropriate credentials. Associate the secret with the Aurora DB cluster. Configure a custom rotation period of 14 days.

AWS Secrets Manager là service giúp lưu trữ các thông tin quan trọng như db credentials, username, password, v.v.. một cách an toàn.

Có cơ chế làm mới (rotation) các thông tin này một cách định kì. Có tương tác trực tiếp với RDS & Aurora, có support mã hoá thông qua KMS để tăng tính security.

Khi liên kết với Aurora, Secrets Manager sẽ tự động xử lý việc làm mới định kỳ mà không cần viết code custom.

Đây là managed service hoàn toàn, đáp ứng đầy đủ yêu cầu mã hóa (KMS) và rotation tự động.

Các đáp án sai:

❌ Create two parameters in AWS Systems Manager Parameter Store: one for the user name as a string parameter and one that uses the SecureString type for the password. Select AWS Key Management Service (AWS KMS) encryption for the password parameter, and load these parameters in the application tier. Implement an AWS Lambda function that rotates the password every 14 days

→ Cần phải tự code Lambda function để tạo ra logic cho việc rotation tự động, làm tăng operational effort. Hơn nữa Parameter Store không có tương tác trực tiếp với Aurora.

❌ Store a file that contains the credentials in an AWS Key Management Service (AWS KMS) encrypted Amazon Elastic File System (Amazon EFS) file system. Mount the EFS file system in all EC2 instances of the application tier. Restrict the access to the file on the file system so that the application can read the file and that only super users can modify the file. Implement an AWS Lambda function that rotates the key in Aurora every 14 days and writes new credentials into the file

→ Phức tạp hoá vấn đề và không cần thiết sử dụng thêm EFS trên tất cả EC2, cón thêm cả effort quản lý file, và tự code Lambda rotation logic. Operational effort cao nhất.

❌ Store a file that contains the credentials in an AWS Key Management Service (AWS KMS) encrypted Amazon S3 bucket that the application uses to load the credentials. Download the file to the application regularly to ensure that the correct credentials are used. Implement an AWS Lambda function that rotates the Aurora credentials every 14 days and uploads these credentials to the file in the S3 bucket

→ Cần phải tự code Lambda function để tạo ra logic cho việc rotation tự động, làm tăng operational effort. Hơn nữa việc lưu thông tin đăng nhập DB vào S3 là không tốt về mặt security, aws không khuyến cáo.

🔑 Tips and tricks:

Sử dụng DB, lưu trữ các thông tin quan trọng như db credentials mà cần cơ chế rotation tự động thì nghĩ ngay đến Secrets Manager
</details>

---

### Q27. 
A solutions architect is creating an application that will handle batch processing of large amounts of data. The input data will be held in Amazon S3 and the output data will be stored in a different S3 bucket. For processing, the application will transfer the data over the network between multiple Amazon EC2 instances.

What should the solutions architect do to reduce the overall data transfer costs?
- Place all the EC2 instances in the same Availability Zone.
- Place all the EC2 instances in an Auto Scaling group.
- Place all the EC2 instances in the same AWS Region.
- Place all the EC2 instances in private subnets in multiple Availability Zones.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng sử dụng nhiều EC2 để xử lý batch processing với lượng dữ liệu lớn

Các EC2 instances này truyền tải data cho nhau thông qua network

Mục tiêu: giảm chi phí data transfer

✅ Đáp án đúng:

Place all the EC2 instances in the same Availability Zone.

Data transfer giữa các EC2 instances trong cùng một AZ là miễn phí, trong khi transfer giữa các AZ khác nhau sẽ có phí ($0.01/GB).

Với batch processing lượng dữ liệu lớn, việc đặt tất cả EC2 trong cùng AZ sẽ tiết kiệm đáng kể chi phí.

Các đáp án sai:

❌ Place all the EC2 instances in an Auto Scaling group.

→ Auto Scaling group không ảnh hưởng đến data transfer cost, chỉ giúp scale instances theo demand.

❌ Place all the EC2 instances in the same AWS Region.

→ Trong cùng region vẫn có thể ở khác AZ, vẫn phát sinh phí data transfer giữa các AZ.

❌ Place all the EC2 instances in private subnets in multiple Availability Zones.

→ Multiple AZ sẽ phát sinh data transfer cost giữa các AZ, ngược với mục tiêu giảm chi phí.

📖 Reference:

https://aws.amazon.com/blogs/architecture/overview-of-data-transfer-costs-for-common-architectures/
</details>

---

### Q28. 
A company's software development team needs an Amazon RDS Multi-AZ cluster. The RDS cluster will serve as a backend for a desktop client that is deployed on premises. The desktop client requires direct connectivity to the RDS cluster.

The company must give the development team the ability to connect to the cluster by using the client when the team is in the office.

Which solution provides the required connectivity MOST securely?

- Create a VPC and two public subnets. Create the RDS cluster in the public subnets. Use AWS Site-to-Site VPN with a customer gateway in the company's office.
- Create a VPC and two private subnets. Create the RDS cluster in the private subnets. Use RDS security groups to allow the company's office IP ranges to access the cluster.
- Create a VPC and two public subnets. Create the RDS cluster in the public subnets. Create a cluster user for each developer. Use RDS security groups to allow the users to access the cluster.
- Create a VPC and two private subnets. Create the RDS cluster in the private subnets. Use AWS Site-to-Site VPN with a customer gateway in the company's office.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty cần cho desktop client cần ở phía on-premise kết nối trực tiếp (direct connectivity) tới RDS cluster trên AWS

Yêu cầu giải pháp bảo mật nhất (MOST securely)

✅ Đáp án đúng:


Create a VPC and two private subnets. Create the RDS cluster in the private subnets. Use AWS Site-to-Site VPN with a customer gateway in the company's office.

Để cho phía on-premise có thể kết nối đến RDS được thì sẽ cần tạo một đường truyền có tính an toàn, ở đây có thể sử dụng AWS Site-to-Site VPN, kết nối này có support mã hoá trên đường truyền.

Hơn nữa có thể đặt RDS trong Private Subnet, giúp tăng tính bảo mật mà vẫn không ảnh hưởng đến việc kết nối từ phía on-premise.

Ở đây phải chỉ định 2 subnet là do quy định của aws, thực tế thì RDS có thể chỉ chạy trên 1 subnet cũng được.


![img](https://static.cloudexam.pro/courses/5/1756995744008-im7aohgm-CleanShot_2025-09-04_at_23.22.06.png)

Các đáp án sai:

❌ Create a VPC and two public subnets. Create the RDS cluster in the public subnets. Use AWS Site-to-Site VPN with a customer gateway in the company's office.

→ RDS trong public subnet không an toàn, kém bảo mật dù có VPN

❌ Create a VPC and two private subnets. Create the RDS cluster in the private subnets. Use RDS security groups to allow the company's office IP ranges to access the cluster.

→ RDS trong private subnet mà không có VPN hay Direct Connect thì sẽ không thể truy cập trực tiếp từ internet được dù có setting security group rules đi nữa

❌ Create a VPC and two public subnets. Create the RDS cluster in the public subnets. Create a cluster user for each developer. Use RDS security groups to allow the users to access the cluster.

→ RDS trong public subnet không an toàn

🔑 Tips and tricks:

Lưu ý RDS hầu như không bao giờ để trong Public subnet

Để kết nối on-premise đến VPC an toàn, có mã hoá trên đường truyền thì có thể sử dụng Site-to-Site VPN
</details>

---

### Q29. 
A company uses GPS trackers to document the migration patterns of thousands of sea turtles. The trackers check every 5 minutes to see if a turtle has moved more than 100 yards (91.4 meters). If a turtle has moved, its tracker sends the new coordinates to a web application running on three Amazon EC2 instances that are in multiple Availability Zones in one AWS Region.

Recently, the web application was overwhelmed while processing an unexpected volume of tracker data. Data was lost with no way to replay the events. A solutions architect must prevent this problem from happening again and needs a solution with the least operational overhead.

What should the solutions architect do to meet these requirements?
- Create an Amazon Simple Queue Service (Amazon SQS) queue to store the incoming data. Configure the application to poll for new messages for processing.
- Create an Amazon API Gateway endpoint to handle transmitted location coordinates. Use an AWS Lambda function to process each item concurrently.
- Create an Amazon S3 bucket to store the data. Configure the application to scan for new data in the bucket for processing.
- Create an Amazon DynamoDB table to store transmitted location coordinates. Configure the application to query the table for new data for processing. Use TTL to remove data that has been processed.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

GPS trackers gửi tọa độ của rùa biển đến web application chạy trên 3 EC2 instances

Application bị quá tải (overwhelmed) khi xử lý lượng dữ liệu bất ngờ

Mất dữ liệu và không thể replay các sự kiện đã gửi

Cần giải pháp ít overhead nhất để tránh tình trạng này

✅ Đáp án đúng:

Create an Amazon Simple Queue Service (Amazon SQS) queue to store the incoming data. Configure the application to poll for new messages for processing.

SQS queue là service hàng chờ sẽ đóng vai trò như buffer/decoupling layer giữa GPS trackers và application. Khi có trafic tăng lên đột ngột thì messages vẫn sẽ được lưu trong queue đầy đủ thay vì làm quá tải application.

Application có thể xử lý messages dần dần mà không sợ bị mất dữ liệu. Trong trường hợp xử lí bị fail thì message sẽ quay trở lại queue để EC2 khác xử lí lại, còn thành công thì có thể xoá message và xử lí message tiếp theo.


![img](https://static.cloudexam.pro/courses/5/1756996327318-896rkw4b-CleanShot_2025-09-04_at_23.31.40.png)

Các đáp án sai:

❌ Create an Amazon S3 bucket to store the data. Configure the application to scan for new data in the bucket for processing.

→ Quét S3 bucket để tìm data mới không hiệu quả và tốn cost. Không có cơ chế real-time notification và khó quản lý việc tracking được các data nào đã xử lý hay chưa xử lý.

❌ Create an Amazon API Gateway endpoint to handle transmitted location coordinates. Use an AWS Lambda function to process each item concurrently.

→ Không giải quyết vấn đề gốc rễ như SQS. API Gateway + Lambda vẫn có thể bị quá tải nếu traffic quá lớn.

❌ Create an Amazon DynamoDB table to store transmitted location coordinates. Configure the application to query the table for new data for processing. Use TTL to remove data that has been processed.

→ DynamoDB chỉ là service database, không có có cơ chế queue. Do đó vẫn có thể bị quá tải như thường.

🔑 Tips and tricks:

Đối với yêu cầu cần buffer request giúp giảm quá tải cho server / db thì có thể sử dụng SQS
</details>

---

### Q30. 

A company is planning to migrate a legacy application to AWS. The application currently uses NFS to communicate to an on-premises storage solution to store application data. The application cannot be modified to use any other communication protocols other than NFS for this purpose.

Which storage solution should a solutions architect recommend for use after the migration?
- Amazon Elastic Block Store (Amazon EBS)
- AWS DataSync
- Amazon Elastic File System (Amazon EFS)
- Amazon EMR File System (Amazon EMRFS)

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Application đang sử dụng NFS protocol để kết nối đến storage

Application không thể chuyển qua sử dụng protocol khác

Cần tìm storage solution tương thích khi migrate lên AWS

✅ Đáp án đúng:

Amazon Elastic File System (Amazon EFS)

EFS là fully managed NFS service, hỗ trợ NFS protocol native

Do đó sẽ tương thích với application hiện tại mà không cần thay đổi code

Các đáp án sai:

❌ AWS DataSync

Không phải là service về storage

Chỉ dùng để transfer data giữa các storage systems

❌ Amazon Elastic Block Store (Amazon EBS)

Block storage nên sẽ không support NFS protocol

❌ Amazon EMR File System (Amazon EMRFS)

Chỉ dành riêng cho Amazon EMR clusters

Không phải NFS storage solution cho application hiện tại

🔑 Tips and tricks:

Khi thấy từ khóa NFS trên AWS thì thường sẽ nghĩ đến EFS
</details>

---

### Q31. 
A company runs database workloads on AWS that are the backend for the company's customer portals. The company runs a Multi-AZ database cluster on Amazon RDS for PostgreSQL.

The company needs to implement a 30-day backup retention policy. The company currently has both automated RDS backups and manual RDS backups. The company wants to maintain both types of existing RDS backups that are less than 30 days old.

Which solution will meet these requirements MOST cost-effectively?
- Configure the RDS backup retention policy to 30 days for automated backups. Manually delete manual backups that are older than 30 days.
- Configure the RDS backup retention policy to 30 days for automated backups by using AWS Backup. Manually delete manual backups that are older than 30 days.
- Disable RDS automated backups. Delete automated backups and manual backups that are older than 30 days. Configure the RDS backup retention policy to 30 days for automated backups.
- Disable RDS automated backups. Delete automated backups and manual backups that are older than 30 days automatically by using AWS CloudFormation. Configure the RDS backup retention policy to 30 days for automated backups.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty chạy Multi-AZ PostgreSQL cluster trên RDS

Công ty quy định backup chỉ lưu trữ 30 ngày

Hiện tại có cả automated RDS backups và manual RDS backups

Yêu cầu giải pháp cost-effective nhất

✅ Đáp án đúng:

Configure the RDS backup retention policy to 30 days for automated backups. Manually delete manual backups that are older than 30 days.

Automated backup là cơ chế backup tự động của RDS, có thể lưu trữ tối đa 35 ngày. Đối với backup loại này thì chỉ cần thay đổi cấu hình retention policy thành 30 ngày.

Manual backup là backup thủ công nên sẽ lưu trữ vô thời hạn, do đó cần xoá thủ công những snapshot đã quá 30 ngày.

Các đáp án sai:

❌ Configure the RDS backup retention policy to 30 days for automated backups by using AWS Backup. Manually delete manual backups that are older than 30 days.

→ Không cần thiết sử dụng AWS Backup khi RDS đã có sẵn tính năng retention policy cho automated backups. AWS Backup tăng thêm chi phí và độ phức tạp.

❌ Disable RDS automated backups. Delete automated backups and manual backups that are older than 30 days. Configure the RDS backup retention policy to 30 days for automated backups.

→ Logic mâu thuẫn: disable automated backups rồi lại configure retention policy cho automated backups. Việc disable sẽ làm mất tính năng tự động backup này luôn rồi.

❌ Disable RDS automated backups. Delete automated backups and manual backups that are older than 30 days automatically by using AWS CloudFormation. Configure the RDS backup retention policy to 30 days for automated backups.

→ Tương tự đáp án trên, logic không nhất quán. CloudFormation cũng tăng độ phức tạp không cần thiết.
</details>

---

### Q32. 
A company is designing the architecture for a new mobile app that uses the AWS Cloud. The company uses organizational units (OUs) in AWS Organizations to manage its accounts. The company wants to tag Amazon EC2 instances with data sensitivity by using values of sensitive and nonsensitive. IAM identities must not be able to delete a tag or create instances without a tag.

Which combination of steps will meet these requirements? (Choose two.)
- In Organizations, create a new service control policy (SCP) that specifies the data sensitivity tag key and the required tag values. Enforce the tag values for the EC2 instances.
- Create a service control policy (SCP) to deny creating instances when a tag key is not specified. Create another SCP that prevents identities from deleting tags. Attach the SCPs to the appropriate OU.
- Create a tag policy to deny running instances when a tag key is not specified. Create another tag policy that prevents identities from deleting tags.
- Create an AWS Config rule to check if EC2 instances use the data sensitivity tag and the specified values. Configure an AWS Lambda function to delete the resource if a noncompliant resource is found.
- In Organizations, create a new tag policy that specifies the data sensitivity tag key and the required values. Enforce the tag values for the EC2 instances. Attach the tag policy to the appropriate OU.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty muốn gắn thẻ (tag) cho EC2 instances với giá trị "sensitive" hoặc "nonsensitive"

Yêu cầu bắt buộc: IAM identities không được phép xóa tag hoặc tạo instance không có tag

Sử dụng AWS Organizations với organizational units (OUs) để quản lý

✅ Đáp án đúng:

In Organizations, create a new tag policy that specifies the data sensitivity tag key and the required values. Enforce the tag values for the EC2 instances. Attach the tag policy to the appropriate OU.

Tag Policy được thiết kế chính xác cho mục đích này - kiểm soát việc gắn tag và bắt buộc gán giá trị tag cụ thể cho resources.

Create a service control policy (SCP) to deny creating instances when a tag key is not specified. Create another SCP that prevents identities from deleting tags. Attach the SCPs to the appropriate OU.

SCP có thể ngăn chặn các hành động cụ thể như tạo instance không có tag và xóa tag, đáp ứng yêu cầu kiểm soát quyền hạn.

Các đáp án sai:

❌ In Organizations, create a new service control policy (SCP) that specifies the data sensitivity tag key and the required tag values. Enforce the tag values for the EC2 instances.

→ SCP không có cơ chế bắt buộc gắn tag cho resource - chủ yếu để deny các actions trong Organzation. Việc bắt buộc gắn tag là chức năng của Tag Policy.

❌ Create a tag policy to deny running instances when a tag key is not specified. Create another tag policy that prevents identities from deleting tags.

→ Tag Policy không thể deny actions như ngăn tạo instance hay xóa tag. Tag Policy chỉ enforce tag compliance, không có quyền deny actions.

❌ Create an AWS Config rule to check if EC2 instances use the data sensitivity tag and the specified values. Configure an AWS Lambda function to delete the resource if a noncompliant resource is found.

→ Dùng thêm AWS Config và Lambda làm tăng tính phức tạp không cần thiết, hơn nữa config rule chỉ có thể kiểm tra resource có đảm bảo tuân thủ hay không, chứ không có cơ chế ngăn chặn việc tạo resource không tuân thủ.

🔑 Tips and tricks:

Cần bắt buộc việc gắn tag và gán giá trị cho tag trong Organization thì nghĩ đến Tag policy

Cần chặn một hành động (action) nào đó trong Organization thì sẽ nghĩ đến SCP
</details>

---

### Q33. 
A company recently launched a new application for its customers. The application runs on multiple Amazon EC2 instances across two Availability Zones. End users use TCP to communicate with the application.

The application must be highly available and must automatically scale as the number of users increases.

Which combination of steps will meet these requirements MOST cost-effectively? (Choose two.)
- Add a Network Load Balancer in front of the EC2 instances.
- Configure an Auto Scaling group for the EC2 instances.
- Add an Application Load Balancer in front of the EC2 instances.
- Manually add more EC2 instances for the application.
- Add a Gateway Load Balancer in front of the EC2 instances.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng chạy trên nhiều EC2 instances ở 2 Availability Zones

End users sử dụng TCP để giao tiếp với ứng dụng

Yêu cầu: hệ thống có tính khả dụng cao (high available) và scale tự động khi số user tăng

Tìm giải pháp cost-effective

✅ Đáp án đúng:

Add a Network Load Balancer in front of the EC2 instances.

TCP traffic → Network Load Balancer là lựa chọn tối ưu vì được thiết kế đặc biệt cho Layer 4 (TCP/UDP)

Có hiệu năng cao, độ trễ thấp cho TCP connections

Configure an Auto Scaling group for the EC2 instances.

Đáp ứng yêu cầu automatically scale khi số user tăng

Cost-effective vì chỉ trả tiền cho resources thực sự cần thiết

![img](https://static.cloudexam.pro/courses/5/1756998489546-c4ro7rrb-CleanShot_2025-09-05_at_00.07.53.png)

Các đáp án sai:

❌ Add an Application Load Balancer in front of the EC2 instances.

ALB hoạt động ở Layer 7 (HTTP/HTTPS), không support traffic TCP

❌ Manually add more EC2 instances for the application.

Không đáp ứng yêu cầu scale tự động "automatically scale"

❌ Add a Gateway Load Balancer in front of the EC2 instances.

GWLB dành cho việc kiểm tra traffic/security (firewalls, IDS/IPS)

Không phù hợp với application traffic thông thường, chi phí cao không cần thiết

🔑 Tips and tricks:

Traffic TCP thì sẽ nghĩ đến Network Load Balancer, Global Accelerator
</details>

---

### Q34. 

A company is testing an application that runs on an Amazon EC2 Linux instance. A single 500 GB Amazon Elastic Block Store (Amazon EBS) General Purpose SSO (gp2) volume is attached to the EC2 instance.

The company will deploy the application on multiple EC2 instances in an Auto Scaling group. All instances require access to the data that is stored in the EBS volume. The company needs a highly available and resilient solution that does not introduce significant changes to the application's code.

Which solution will meet these requirements?
- Provision an Amazon Elastic File System (Amazon EFS) file system. Configure the file system to use General Purpose performance mode.
- Provision an EC2 instance that uses NFS server software. Attach a single 500 GB gp2 EBS volume to the instance.
- Provision an Amazon FSx for Windows File Server file system. Configure the file system as an SMB file store within a single Availability Zone.
- Provision an EC2 instance with two 250 GB Provisioned IOPS SSD EBS volumes.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng chạy trên một EC2 Linux instance với data lưu trong EBS

Sẽ triển khai trên nhiều EC2 instances với Auto Scaling group

Tất cả instances cần truy cập cùng dữ liệu đang nằm trong EBS đó

Cần giải pháp highly available và resilient

Không thay đổi code ứng dụng nhiều

✅ Đáp án đúng:

Provision an Amazon Elastic File System (Amazon EFS) file system. Configure the file system to use General Purpose performance mode.

Bài toán cần hệ thống file share cho linux, thì đó chính là EFS. Đây là managed service cho phép các EC2 instances Linux truy cập cùng lúc vào cùng một file system thông qua giao thức native của Linux là NFS.

Ngoài ra EFS còn có tính khả dụng cao (Highly available across multiple AZs), scale tự động (automatically scales), và tương thích với Linux mà không cần thay đổi code nhiều.

![img](https://static.cloudexam.pro/courses/5/1756999090866-gnms2nvn-image.png)


Các đáp án sai:

❌ Provision an EC2 instance that uses NFS server software. Attach a single 500 GB gp2 EBS volume to the instance.

→ Chỉ có 1 EC2 instance đóng vai trò làm NFS serve thì không đáp ứng yêu cầu tính khả dụng cao highly available.

❌ Provision an Amazon FSx for Windows File Server file system. Configure the file system as an SMB file store within a single Availability Zone.

→ FSx for Windows dành cho Windows workloads và SMB protocol, không phù hợp với Linux instance. Chỉ trong single AZ cũng không highly available.

❌ Provision an EC2 instance with two 250 GB Provisioned IOPS SSD EBS volumes.

→ Vẫn là một EBS volumes gắn vào một instance, không giải quyết vấn đề nhiều instances cần truy cập chung data.

🔑 Tips and tricks:

Fileshare cho Linux instance thì thường sẽ nghĩ đến EFS
</details>

---

### Q35.
A company stores user data in AWS. The data is used continuously with peak usage during business hours. Access patterns vary, with some data not being used for months at a time. A solutions architect must choose a cost-effective solution that maintains the highest level of durability while maintaining high availability.

Which storage solution meets these requirements?
- Amazon S3 Standard
- Amazon S3 Intelligent-Tiering
- Amazon S3 Glacier Deep Archive
- Amazon S3 One Zone-IA

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty muốn lưu trữ dữ liệu người dùng trên AWS

Patterns truy cập thay đổi không biết trước được (access patterns vary) - một số dữ liệu không dùng trong nhiều tháng

Yêu cầu: tiết kiệm (cost-effective), khả dụng cao (highest durability, high availability)

✅ Đáp án đúng:

Đầu tiên với use case lưu trữ dữ liệu với chi phí thấp thì service phù hợp sẽ là S3.

Với tần suất truy cập không biết trước mà vẫn muốn tiết kiệm chi phí thì lựa chọn Amazon S3 Intelligent-Tiering vì có các đặc điểm sau:

Tự động chuyển đổi giữa các storage class dựa trên tần suất truy cập thực tế

Có tính khả dụng cao

Cost-effective vì chỉ trả phí theo usage thực tế

![img](https://static.cloudexam.pro/courses/5/1756999797220-42c3oess-image.png)

Các đáp án sai:

❌ Amazon S3 Standard

Không tiết kiệm chi phí cho data ít được access

Giá cao nhất trong các S3 storage class

❌ Amazon S3 Glacier Deep Archive

Đây là tầng lưu trữ thiết kế để lưu data lâu dài

Không phù hợp với tần suất truy cập không biết trước

❌ Amazon S3 One Zone-IA

Không đáp ứng yêu cầu "highest durability" vì chỉ lưu data trong 1 AZ

🔑 Tips and tricks:

Sử dụng S3 không biết trước tần suất, chỉ muốn tiết kiệm chi phí thì sẽ nghĩ đến Amazon S3 Intelligent-Tiering
</details>

---

### Q36. 
A company hosts its main public web application in one AWS Region across multiple Availability Zones. The application uses an Amazon EC2 Auto Scaling group and an Application Load Balancer (ALB).

A web development team needs a cost-optimized compute solution to improve the company’s ability to serve dynamic content globally to millions of customers.

Which solution will meet these requirements?
- Create an Amazon S3 bucket with public read access enabled. Migrate the web application to the S3 bucket.
- Use Amazon Route 53 to serve traffic to the ALB and EC2 instances based on the geographic location of each customer.
- Use AWS Direct Connect to directly serve content from the web application to the location of each customer.
- Create an Amazon CloudFront distribution. Configure the existing ALB as the origin.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty có web application chạy trên EC2 Auto Scaling + ALB

Cần giải pháp cost-optimized để cung cấp dynamic content trên toàn cầu cho hàng triệu người dùng

Yêu cầu: chi phí tối ưu

✅ Đáp án đúng:

Create an Amazon CloudFront distribution. Configure the existing ALB as the origin.

CloudFront là CDN service phù hợp để cung cấp content globally với chi phí tối ưu.

Tuy rằng CloudFront chủ yếu dùng để cache static content tại edge locations gần user, giúp giảm độ trễ nhưng nó cũng giúp cho việc delivery dyanmic content đến người dùng một cách nhanh hơn.

Tham khảo link của aws ở bên dưới.

Các đáp án sai:

❌ Use Amazon Route 53 to serve traffic to the ALB and EC2 instances based on the geographic location of each customer.

→ Route 53 chỉ là service DNS routing, không giải giúp cho việc phân phối content globally, không có caching tại edge location.

❌ Create an Amazon S3 bucket with public read access enabled. Migrate the web application to the S3 bucket.

→ S3 chỉ giúp host static content, không thể host dynamic web application. Đề bài yêu cầu serve dynamic content.

❌ Use AWS Direct Connect to directly serve content from the web application to the location of each customer.

→ Direct Connect là kết nối môi trường on-premise và VPC, không phải giải pháp CDN để cung cấp content đến người dùng toàn cầu. Chi phí rất cao.


📖 Reference:

https://aws.amazon.com/cloudfront/dynamic-content/
</details>

---

### Q37. 
A company hosts its core network services, including directory services and DNS, in its on-premises data center. The data center is connected to the AWS Cloud using AWS Direct Connect (DX). Additional AWS accounts are planned that will require quick, cost-effective, and consistent access to these network services.

What should a solutions architect implement to meet these requirements with the LEAST amount of operational overhead?
- Configure VPC endpoints in the DX VPC for all required services. Route the network traffic to the on-premises servers.
- Configure AWS Transit Gateway between the accounts. Assign DX to the transit gateway and route network traffic to the on-premises servers.
- Create a DX connection in each new account. Route the network traffic to the on-premises servers.
- Create a VPN connection between each new account and the DX VPC Route the network traffic to the on-premises servers.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty đã có data center kết nối với AWS qua AWS Direct Connect (DX)

Bây giờ khi tạo thêm AWS accounts mới cần cho truy cập một cách nhanh gọn, tiết kiệm qua đường truyền đến các network services phía on-premise

Yêu cầu: LEAST operational overhead

✅ Đáp án đúng:

Configure AWS Transit Gateway between the accounts. Assign DX to the transit gateway and route network traffic to the on-premises servers.

Transit Gateway cho phép quản lí kết nối tập trung giữa nhiều accounts và môi trường on-premises thông qua một cổng kết nối duy nhất.

DX connection được sẽ được gắn vào Transit Gateway, tất cả accounts mới cũng chỉ cần gắn với Transit Gateway thì sẽ truy cập được on-premises network services.

Giải pháp này có operational overhead thấp nhất vì chỉ cần setup một lần và dễ dàng scale khi thêm accounts mới.

![img](https://static.cloudexam.pro/courses/5/1757000856549-dt8m0dpx-CleanShot_2025-09-05_at_00.47.26.png)


Các đáp án sai:

❌ Create a DX connection in each new account. Route the network traffic to the on-premises servers.

→ Tạo DX cho mỗi account mới rất sẽ tốn kém (DX có monthly fee cao) và operational overhead lớn (phải quản lý nhiều DX connections).

❌ Configure VPC endpoints in the DX VPC for all required services. Route the network traffic to the on-premises servers.

→ VPC endpoints là service cho phép VPC kết nối private đến các AWS services, không thể sử dụng kết nối đến on-premises.

❌ Create a VPN connection between each new account and the DX VPC Route the network traffic to the on-premises servers.

→ Mặc dù chi phí thấp hơn DX, nhưng vẫn phải setup và quản lý nhiều VPN connections, operational overhead cao hơn Transit Gateway.

🔑 Tips and tricks:

Khi cần kết nối môi trường On-premise với nhiều AWS account thông qua DirectConnect thì nghĩ đến Transit Gateway
</details>

---

### Q38. 
A company has an Amazon S3 bucket that contains sensitive data files. The company has an application that runs on virtual machines in an on-premises data center. The company currently uses AWS IAM Identity Center.

The application requires temporary access to files in the S3 bucket. The company wants to grant the application secure access to the files in the S3 bucket.

Which solution will meet these requirements?
- Install the AWS CLI on the virtual machine. Configure the AWS CLI with access keys from an IAM user
- Use IAM Roles Anywhere to obtain security credentials in IAM Identity Center that grant access to the S3 bucket. Configure the virtual machines to assume the role by using the AWS CLI.
- Create an S3 bucket policy that permits access to the bucket from the public IP address range
- Create an IAM user and policy that grants access to the bucket. Store the access key and secret key for the IAM user in AWS Secrets Manager. Configure the application to retrieve the access key and secret key at startup.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty có S3 bucket chứa dữ liệu nhạy cảm (sensitive data)

Ứng dụng chạy trên máy ảo on-premises

Công ty đã sử dụng AWS IAM Identity Center

Ứng dụng cần truy cập tạm thời (temporary access) vào files trong S3

Yêu cầu: truy cập an toàn (secure access)

✅ Đáp án đúng:

Use IAM Roles Anywhere to obtain security credentials in IAM Identity Center that grant access to the S3 bucket. Configure the virtual machines to assume the role by using the AWS CLI.

IAM Roles Anywhere cho phép workload on-premises sử dụng credentials tạm thời thông qua certificate-based authentication. Giải pháp này cung cấp truy cập tạm thời (temporary access) như yêu cầu và tích hợp tốt với IAM Identity Center hiện có.

![img](https://static.cloudexam.pro/courses/5/1757001092698-zljsvg1t-image.png)

Các đáp án sai:

❌ Create an S3 bucket policy that permits access to the bucket from the public IP address range

→ Dựa vào IP address không an toàn vì IP có thể thay đổi và không cung cấp được việc cấp quyền tạm thời thực sự.

❌ Install the AWS CLI on the virtual machine. Configure the AWS CLI with access keys from an IAM user
→ Access keys from an IAM user là credentials lâu dài, không phải credentials tạm thời như yêu cầu và kém an toàn hơn.

❌ Create an IAM user and policy that grants access to the bucket. Store the access key and secret key for the IAM user in AWS Secrets Manager. Configure the application to retrieve the access key and secret key at startup.

→ Vẫn là credentials lâu dài, không phải credentials tạm thời.

📖 Reference:

https://aws.amazon.com/blogs/security/use-iam-roles-anywhere-to-help-you-improve-security-in-on-premises-container-workloads/
</details>

---

### Q39. 
A company is building a cloud-based application on AWS that will handle sensitive customer data. The application uses Amazon RDS for the database, Amazon S3 for object storage, and S3 Event Notifications that invoke AWS Lambda for serverless processing.

The company uses AWS IAM Identity Center to manage user credentials. The development, testing, and operations teams need secure access to Amazon RDS and Amazon S3 while ensuring the confidentiality of sensitive customer data. The solution must comply with the principle of least privilege.

Which solution meets these requirements with the LEAST operational overhead?
- Create individual IAM users for each member in all the teams with role-based permissions. Assign the IAM roles with predefined policies for RDS and S3 access to each user based on user needs. Implement IAM Access Analyzer for periodic credential evaluation.
- Enable IAM Identity Center with an Identity Center directory. Create and configure permission sets with granular access to Amazon RDS and Amazon S3. Assign all the teams to groups that have specific access with the permission sets.
- Use IAM roles with least privilege to grant all the teams access. Assign IAM roles to each team with customized IAM policies defining specific permission for Amazon RDS and S3 object access based on team responsibilities.
- Use AWS Organizations to create separate accounts for each team. Implement cross-account IAM roles with least privilege. Grant specific permission for RDS and S3 access based on team roles and responsibilities.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty xây dựng ứng dụng xử lý dữ liệu nhạy cảm khách hàng (sensitive customer data) với RDS, S3, Lambda

Đang dùng AWS IAM Identity Center để quản lý xác thực

Cần cấp quyền cho 3 teams (dev, test, ops) truy cập Amazon RDS và S3

Yêu cầu: tuân thủ nguyên tắc ít quyền nhất (least privilege) với ít operational overhead nhất

✅ Đáp án đúng:

Enable IAM Identity Center with an Identity Center directory. Create and configure permission sets with granular access to Amazon RDS and Amazon S3. Assign all the teams to groups that have specific access with the permission sets.

Đề bài đã nói rõ "company uses AWS IAM Identity Center", nên sử dụng luôn các chức năng của IAM Identity Center và quản lý tại chỗ là giải pháp chính là tối ưu nhất.

Permission sets cho phép quản lý quyền theo nhóm một cách tập trung, giảm operational overhead và dễ dàng implement least privilege.



Các đáp án sai:

❌ Use IAM roles with least privilege to grant all the teams access. Assign IAM roles to each team with customized IAM policies defining specific permission for Amazon RDS and S3 object access based on team responsibilities.

→ Không tận dụng IAM Identity Center đã có sẵn, tăng độ phức tạp không cần thiết khi phải quản lý nhiều IAM roles riêng lẻ.

❌ Create individual IAM users for each member in all the teams with role-based permissions. Assign the IAM roles with predefined policies for RDS and S3 access to each user based on user needs. Implement IAM Access Analyzer for periodic credential evaluation.

→Tạo individual IAM users cho từng thành viên tạo ra operational overhead rất cao, không cần thiết.

❌ Use AWS Organizations to create separate accounts for each team. Implement cross-account IAM roles with least privilege. Grant specific permission for RDS and S3 access based on team roles and responsibilities.

→Tách account cho mỗi team rất tốn công, tạo ra độ phức tạo và chi phí vận hành không cần thiết cho requirement này.
</details>

---

### Q40. 
A company hosts its application on several Amazon EC2 instances inside a VPC. The company creates a dedicated Amazon S3 bucket for each customer to store their relevant information in Amazon S3.

The company wants to ensure that the application running on EC2 instances can securely access only the S3 buckets that belong to the company’s AWS account.

Which solution will meet these requirements with the LEAST operational overhead?
- Create a NAT Gateway in a public subnet. Update route tables to use the NAT Gateway. Assign bucket policies for all buckets with a Deny action and the following condition key:

{
"StringNotEquals" : {
"s3:ResourceAccount" : [ "CompanyAWSAccountNumber" ]
}
}
- Create a gateway endpoint for Amazon S3 that is attached to the VPC. Update the IAM instance profile policy with a Deny action and the following condition key:

{
"StringNotEquals" : {
"s3:ResourceAccount" : [ "CompanyAWSAccountNumber" ]
}
}
- Create a NAT gateway in a public subnet with a security group that allows access to only Amazon S3. Update the route tables to use the NAT Gateway.
- Create a gateway endpoint for Amazon S3 that is attached to the VPC. Update the IAM instance profile policy to provide access to only the specific buckets that the application needs.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng chạy trên EC2 instances trong VPC

Có S3 Bucket quản lý theo từng customer

Yêu cầu: muốn cho EC2 chỉ truy cập được S3 buckets trong AWS account của công ty một cách an toàn

Tìm giải pháp với ít operational overhead nhất

✅ Đáp án đúng:

Create a gateway endpoint for Amazon S3 that is attached to the VPC. Update the IAM instance profile policy with a Deny action and the following condition key:

{
"StringNotEquals" : {
"s3:ResourceAccount" : [ "CompanyAWSAccountNumber" ]
}
}
VPC Gateway Endpoint là service cho phép các application bên trong VPC có thể kết nối đến S3 mà không cần đi qua internet, mà thay vào đó là đường nội bộ của AWS, do đó sẽ giúp tiết kiệm chi phí và tối ưu, đồng thời có tính an toàn.

Còn việc giới hạn EC2 chỉ cho truy cập đến S3 buckets trong AWS account của công ty thì có thể dùng câu lệnh điều kiện Condition trong Bucket policy để deny nếu account không phải là của công ty

Các đáp án sai:

❌ Create a gateway endpoint for Amazon S3 that is attached to the VPC. Update the IAM instance profile policy to provide access to only the specific buckets that the application needs.

→ Mặc dù thoáng qua có vẻ hợp lý, tuy nhiên mỗi lần có user & tạo bucket mới thì sẽ lại phải update policy. Hơn nữa độ dài policy sẽ có giới hạn, không thể mỗi lần như vậy cứ dài thêm được.

❌ Create a NAT gateway in a public subnet with a security group that allows access to only Amazon S3. Update the route tables to use the NAT Gateway.

→ NAT Gateway gây thêm tốn chi phí không cần thiết, trong khi VPC Gateway Endpoint là miễn phí. Hơn nữa traffic đi ra internet nên không an toàn.

❌ Create a NAT Gateway in a public subnet. Update route tables to use the NAT Gateway. Assign bucket policies for all buckets with a Deny action and the following condition key:

{
"StringNotEquals" : {
"s3:ResourceAccount" : [ "CompanyAWSAccountNumber" ]
}
}
→ Tương tự như đáp án sử dụng NAT gateway ở trên

🔑 Tips and tricks:

EC2 từ bên trong VPC muốn access đến S3, nghĩ ngay đến Gateway Endpoint (áp dụng cho S3, DynamoDB)
</details>

---

### Q41. 
A company is developing a new application that uses a relational database to store user data and application configurations. The company expects the application to have steady user growth. The company expects the database usage to be variable and read-heavy, with occasional writes.

The company wants to cost-optimize the database solution. The company wants to use an AWS managed database solution that will provide the necessary performance.

Which solution will meet these requirements MOST cost-effectively?
- Deploy the database on Amazon RDS. Use magnetic storage and use read replicas to accommodate the workload.
- Deploy the database on Amazon RDS. Use Provisioned IOPS SSD storage to ensure consistent performance for read and write operations.
- Deploy the database on Amazon Aurora Serverless to automatically scale the database capacity based on actual usage to accommodate the workload.
- Deploy the database on Amazon DynamoDB. Use on-demand capacity mode to automatically scale throughput to accommodate the workload.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty sử dụng cơ sở dữ liệu quan hệ (relational database)

Lưu lượng sử dụng database biến động (variable), không cân bằng (đọc nhiều, thỉnh thoảng mới ghi)

Cần AWS managed database với chi phí tối ưu (cost-optimize) với đảm bảo performance

✅ Đáp án đúng:

Deploy the database on Amazon Aurora Serverless to automatically scale the database capacity based on actual usage to accommodate the workload.

Aurora Serverless là managed service cho phép tạo cơ sở dữ liệu dạng quan hệ trên nền serverless, hoàn hảo cho workload biến động, bất ổn vì có khả năng tự động scale up/down để đáp ứng lượng truy cập thực tế.

Chi phí cũng tối ưu vì chỉ phải trả tiền khi sử dụng.

Các đáp án sai:

❌ Deploy the database on Amazon RDS. Use Provisioned IOPS SSD storage

→ RDS thông thường không có khả năng tự scale để đáp ứng traffic.

❌ Deploy the database on Amazon DynamoDB. Use on-demand capacity mode

→ DynamoDB là NoSQL, không phải relational database như yêu cầu đề bài.

❌ Deploy the database on Amazon RDS. Use magnetic storage and read replicas

→ RDS thông thường không có khả năng tự scale để đáp ứng traffic.

🔑 Tips and tricks:

DB dạng quan hệ có khả năng tự scale để đáp ứng traffic thì nghĩ đến Aurora Serverless
</details>

---

### Q42. 
A company is migrating its on-premises Oracle database to an Amazon RDS for Oracle database. The company needs to retain data for 90 days to meet regulatory requirements. The company must also be able to restore the database to a specific point in time for up to 14 days.

Which solution will meet these requirements with the LEAST operational overhead?
- Create Amazon RDS automated backups. Set the retention period to 90 days.
- Use the Amazon Aurora Clone feature for Oracle to create a point-in-time restore. Delete clones that are older than 90 days.
- Create a backup plan that has a retention period of 90 days by using AWS Backup for Amazon RDS with Point In Time Recovery enabled.
- Create an Amazon RDS manual snapshot every day. Delete manual snapshots that are older than 90 days.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty đang chuyển sang sử dụng Amazon RDS for Oracle

Có các yêu cầu sau về lưu trữ data cho db:

Data cần lưu trữ 90 ngày

Có khả năng phục hồi db về thời điểm cụ thể trong 14 ngày gần nhất

Yêu cầu: LEAST operational overhead (ít vận hành nhất)

✅ Đáp án đúng:

Create a backup plan that has a retention period of 90 days by using AWS Backup for Amazon RDS with Point In Time Recovery enabled.

AWS Backup là managed service chuyên cho việc quản lý backup tập trung, hỗ trợ backup tự động với thời gian lưu trữ có thể lên đến vô thời hạn, ở đây để đáp ứng yêu cầu thì sẽ set là 90 ngày.

Point-in-time recovery là cơ chế backup tăng tiến tự động của RDS, giúp có thể quay lại bất cứ thời điểm nào trong quá khứ (tối đa 35 ngày), do đó đáp ứng yêu cầu có khả năng phục hồi db về thời điểm cụ thể trong 14 ngày gần nhất.

AWS Backup giảm thiểu effort vận hành vì là managed service.

Các đáp án sai:

❌ Create Amazon RDS automated backups. Set the retention period to 90 days.

RDS automated backups chỉ hỗ trợ lưu trữ tối đa 35 ngày, không thể set 90 ngày.

❌ Create an Amazon RDS manual snapshot every day. Delete manual snapshots that are older than 90 days.

High operational overhead: cần script tự động tạo snapshot hàng ngày và cleanup. Manual process không optimal.

❌ Use the Amazon Aurora Clone feature for Oracle to create a point-in-time restore.

Aurora không support cho Oracle, chỉ support MySQL / PostgresQL
</details>

---

### Q43.
A financial services company plans to launch a new application on AWS to handle sensitive financial transactions. The company will deploy the application on Amazon EC2 instances behind an Application Load Balancer. The company will use Amazon RDS for MySQL as the database. The company’s security policies mandate that data must be encrypted at rest and in transit.

Which solution will meet these requirements with the LEAST operational overhead?
- Configure encryption at rest for Amazon RDS for MySQL by using AWS KMS managed keys. Configure AWS Certificate Manager (ACM) SSL/TLS certificates for encryption in transit.
- Implement third-party application-level data encryption before storing data in Amazon RDS for MySQL. Configure AWS Certificate Manager (ACM) SSL/TLS certificates for encryption in transit.
- Configure encryption at rest for Amazon RDS for MySQL by using AWS KMS managed keys. Configure a VPN connection to enable private connectivity to encrypt data in transit.
- Configure encryption at rest for Amazon RDS for MySQL by using AWS KMS managed keys. Configure IPsec tunnels for encryption in transit.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty triển khai ứng dụng trên EC2 và RDS MySQL

Yêu cầu: mã hóa dữ liệu đầu cuối (encryption at rest) và mã hoá trên đường truyền (encryption in transit)

Mục tiêu: giải pháp có ít overhead vận hành nhất (LEAST operational overhead)

✅ Đáp án đúng:

Configure encryption at rest for Amazon RDS for MySQL by using AWS KMS managed keys. Configure AWS Certificate Manager (ACM) SSL/TLS certificates for encryption in transit.

Đối với yêu cầu mã hóa đầu cuối (encryption at rest), thì solution thường thấy ngay đó là KMS. KMS là managed service giúp quản lý key để mã hoá đầu cuối cho các service như RDS, S3. Cách sử dụng cũng rất đơn giản, chỉ cần chỉ định key khi tạo RDS, việc mã hoá sẽ được tự động hoàn toàn, do đó không tốn effort vận hành

Đối với yêu cầu mã hoá trên đường truyền (encryption in transit) thì sẽ cần sử dụng giao thức an toàn có mã hoá như HTTPS TLS SSL. Đối với ALB, để enable HTTPS thì cần tạo certificate SSL/TLS trong ACM sau đó gắn vào ALB. Certificate cũng có cơ chế gia hạn và làm mới tự động, giúp giảm effort vận hành

Các đáp án sai:

❌ Configure encryption at rest for Amazon RDS for MySQL by using AWS KMS managed keys. Configure IPsec tunnels for encryption in transit.

IPsec tunnels cấu hình phức tạp, làm tăng effort về mặt vận hành, không hiệu quả bằng sử dụng ACM.

❌ Implement third-party application-level data encryption before storing data in Amazon RDS for MySQL. Configure AWS Certificate Manager (ACM) SSL/TLS certificates for encryption in transit.

Thực hiện mã hoá data trong xử lí của applitcation làm tăng độ phức tạp cho code, phải implement thêm logic mã hoá và giải mã, tốn effort.

❌ Configure encryption at rest for Amazon RDS for MySQL by using AWS KMS managed keys. Configure a VPN connection to enable private connectivity to encrypt data in transit.

VPN ở đây không cần thiết cho encryption in transit, chỉ cần ALB + ACM là đủ rồi

🔑 Tips and tricks:

Mã hoá đầu cuối thì thường nghĩ ngay đến KMS

Mã hoá trên đường truyền thì sẽ nghĩ ngay đến các giao thức an toàn HTTPS TLS SSL. Đối với ALB thì implement HTTPS thông qua việc tạo HTTPS listener và liên kết với certificate trong ACM
</details>

---

### Q44. 
A company hosts its web application on AWS using seven Amazon EC2 instances. The company requires that the IP addresses of all healthy EC2 instances be returned in response to DNS queries.

Which policy should be used to meet this requirement?
- Simple routing policy
- Geocation routing policy
- Latency routing policy
- Multivalue routing policy

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty chạy web application trên 7 EC2 instances

Yêu cầu: DNS queries phải trả về IP addresses của TẤT CẢ các EC2 instances đang healthy

Cần chọn Route 53 routing policy phù hợp

✅ Đáp án đúng:

Multivalue routing policy

Đây là policy duy nhất cho phép trả về nhiều IP addresses (up to 8) trong response

Hỗ trợ health checks → chỉ trả về IP của các instances healthy, tự động loại bỏ unhealthy instances

Đáp ứng yêu cầu cho use case này: trả về multiple healthy endpoints cho DNS queries

Ví dụ về setting của multi-answer values (có health check)

![img](https://static.cloudexam.pro/courses/5/1766849110510-jyaqaj2a-CleanShot_2025-12-28_at_00.21.53_2x.png)

Các đáp án sai:

❌ Simple routing policy

Mặc dù cho phép setting nhiều value tương tự như Multi-Answer nhưng không có healthcheck

Không thể phân biệt và loại bỏ unhealthy instances → có thể trả về IP của instances đã ko còn hoạt động nữa

❌ Latency routing policy

Routes traffic dựa trên lowest latency (độ trễ thấp nhất)

Chỉ trả về MỘT record (endpoint có latency thấp nhất), không phải tất cả IPs

Không đáp ứng yêu cầu "return all healthy IPs"

❌ Geolocation routing policy

Routes dựa trên geographic location của user

Chỉ trả về MỘT record phù hợp với location, không phải multiple IPs

Không đáp ứng yêu cầu "return all healthy IPs"

🔑 Tips and tricks:

"Return ALL IPs" + "Healthy instances" → Multivalue routing policy

"Return ONE IP based on latency" → Latency routing policy

"Return ONE IP based on location" → Geolocation routing policy

"Simple routing" + "No health check needed" → Simple routing policy

Reference:

What is the difference between a multivalue answer routing policy and a simple routing policy?
</details>

---

### Q45. 
A weather forecasting company collects temperature readings from various sensors on a continuous basis. An existing data ingestion process collects the readings and aggregates the readings into larger Apache Parquet files. Then the process encrypts the files by using client-side encryption with KMS managed keys (CSE-KMS). Finally, the process writes the files to an Amazon S3 bucket with separate prefixes for each calendar day.

The company wants to run occasional SQL queries on the data to take sample moving averages for a specific calendar day.

Which solution will meet these requirements MOST cost-effectively?
- Configure Amazon Redshift to read the encrypted files. Use Redshift Spectrum and Redshift query editor v2 to run SQL queries on the data directly in Amazon S3.
- Use Amazon S3 Select to run SQL queries on the data directly in Amazon S3.
- Configure Amazon EMR Serverless to read the encrypted files. Use Apache SparkSQL to run SQL queries on the data directly in Amazon S3.
- Configure Amazon Athena to read the encrypted files. Run SQL queries on the data directly in Amazon S3.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty dự báo thời tiết thu thập dữ liệu từ các cảm biến

Dữ liệu được tổng hợp thành file Apache Parquet

Mã hóa bằng CSE-KMS và lưu trữ trên S3 với prefix theo ngày (phân chia folder s3 theo ngày)

Thỉnh thoảng chạy truy vấn SQL (occasional SQL queries) trên các dữ liệu đó

Yêu cầu: giải pháp tiết kiệm chi phí nhất (MOST cost-effectively)

✅ Đáp án đúng:

Configure Amazon Athena to read the encrypted files. Run SQL queries on the data directly in Amazon S3.

Athena là dịch vụ serverless cho phép query data trực tiếp trên S3. Vì là serverless nên sẽ thích hợp cho việc truy vấn occasional (không thường xuyên).

Athena chỉ cần trả tiền theo lượng dữ liệu scan thực tế, không có chi phí cố định. Ngoài ra có performance cao tương thích với Parquet và có support mã hoá client CSE-KMS encryption.

Các đáp án sai:

❌ Use Amazon S3 Select to run SQL queries on the data directly in Amazon S3.

→ S3 Select không hỗ trợ Apache Parquet format và không hỗ trợ CSE-KMS encryption. Chỉ hỗ trợ CSV, JSON và unencrypted data.

❌ Configure Amazon Redshift to read the encrypted files. Use Redshift Spectrum and Redshift query editor v2 to run SQL queries on the data directly in Amazon S3.

→ Redshift không phải serverless, cần chạy liên tục nên cả khi không sử dụng sẽ vẫn bị tính phí, không phù hợp với use case thỉnh thoảng truy vấn.

❌ Configure Amazon EMR Serverless to read the encrypted files. Use Apache SparkSQL to run SQL queries on the data directly in Amazon S3.

→ EMR Serverless vẫn đắt hơn Athena trong use case thỉnh thoảng truy vấn. Hơn nữa setup cũng setup phức tạp, query đơn giản thì cũng không cần đến Apache Spark.

🔑 Tips and tricks:

Việc query data trên S3 thỉnh thoảng, không thường xuyên thì nghĩ ngay đến Amazon Athena
</details>

---

### Q46. 
A company hosts an application on AWS. The application gives users the ability to upload photos and store the photos in an Amazon S3 bucket. The company wants to use Amazon CloudFront and a custom domain name to upload the photo files to the S3 bucket in the eu-west-1 Region.

Which solution will meet these requirements? (Choose two.)
- Use AWS Certificate Manager (ACM) to create a public certificate in the us-east-1 Region. Use the certificate in CloudFront.
- Use AWS Certificate Manager (ACM) to create a public certificate in eu-west-1. Use the certificate in CloudFront.
- Configure Amazon S3 to allow uploads from CloudFront origin access control (OAC).
- Configure Amazon S3 to allow uploads from CloudFront. Configure S3 Transfer Acceleration.
- Configure Amazon S3 to allow uploads from CloudFront. Configure an Amazon S3 website endpoint.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty muốn dùng CloudFront kết hợp với S3 bucket

S3 bucket nằm ở eu-west-1 Region

Cần giải pháp cho phép sử dụng custom domain với CloudFront

✅ Đáp án đúng:

Use AWS Certificate Manager (ACM) to create a public certificate in the us-east-1 Region. Use the certificate in CloudFront.

Mặc dù S3 bucket nằm ở region eu-west-1 nhưng CloudFront vẫn yêu cầu SSL certificate phải được tạo ở us-east-1 (N. Virginia) để sử dụng custom domain. Đây là requirement bắt buộc của CloudFront.

Configure Amazon S3 to allow uploads from CloudFront origin access control (OAC).

OAC là cách thức được AWS khuyến nghị để giới hạn việc truy cập đến S3 bắt buộc phải thông qua CloudFront, từ đó giúp tăng tính security, tránh việc người dùng truy cập trực tiếp đến S3 mà bỏ qua CloudFront.


![img](https://static.cloudexam.pro/courses/5/1757088527679-102oagl7-CleanShot_2025-09-06_at_01.08.20.png)

Các đáp án sai:

❌ Use AWS Certificate Manager (ACM) to create a public certificate in eu-west-1. Use the certificate in CloudFront.

→ Sai region. CloudFront chỉ chấp nhận certificate từ us-east-1, không phải eu-west-1.

❌ Configure Amazon S3 to allow uploads from CloudFront. Configure S3 Transfer Acceleration.

→ Transfer Acceleration dùng cho việc tăng tốc upload trực tiếp lên S3, không cần thiết ở đây.

❌ Configure Amazon S3 to allow uploads from CloudFront. Configure an Amazon S3 website endpoint.

→ S3 đã liên kết với CloudFront rồi thì không cần enable host web trên S3 nữa. Như thế thì phải mở bucket public, không tốt về mặt security, hơn nữa cũng không liên quan đến việc setting custom domain ở đây.

🔑 Tips and tricks:

Lưu ý cerfiticate cho CloudFront bắt buộc phải tạo ở region us-east-1
</details>

---

### Q47. 
A company is designing a web application with an internet-facing Application Load Balancer (ALB).

The company needs the ALB to receive HTTPS web traffic from the public internet. The ALB must send only HTTPS traffic to the web application servers hosted on the Amazon EC2 instances on port 443. The ALB must perform a health check of the web application servers over HTTPS on port 8443.

Which combination of configurations of the security group that is associated with the ALB will meet these requirements? (Choose three.)
- Allow HTTPS outbound traffic to the web application instances for port 443.
- Allow HTTPS outbound traffic to the web application instances for the health check on port 8443.
- Allow HTTPS inbound traffic from the web application instances for the health check on port 8443.
- Allow all outbound traffic to 0.0.0.0/0 for port 443.
- Allow HTTPS inbound traffic from 0.0.0.0/0 for port 443.
- Allow HTTPS inbound traffic from the web application instances for port 443.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty thiết kế web app với internet-facing ALB

ALB nhận HTTPS traffic từ internet

ALB gửi HTTPS traffic đến EC2 trên port 443

ALB thực hiện health check qua HTTPS trên port 8443

Cần cấu hình security group cho ALB

✅ Đáp án đúng:

Allow HTTPS inbound traffic from 0.0.0.0/0 for port 443

→ ALB cần nhận traffic HTTPS từ internet, nên phải allow inbound từ anywhere (0.0.0.0/0) trên port 443

Allow HTTPS outbound traffic to the web application instances for port 443

→ ALB cần gửi HTTPS đến EC2 instances, nên cần outbound rule đến web servers port 443

Allow HTTPS outbound traffic to the web application instances for the health check on port 8443

→ ALB cần thực hiện health check qua HTTPS port 8443, nên cần outbound rule đến instances port 8443

![img](https://static.cloudexam.pro/courses/5/1757089119641-vg9y2rio-CleanShot_2025-09-06_at_01.18.23.png)

Các đáp án sai:

❌ Allow all outbound traffic to 0.0.0.0/0 for port 443

→ Sai. ALB chỉ cần gửi traffic đến web instances cụ thể, không phải toàn bộ internet (0.0.0.0/0)

❌ Allow HTTPS inbound traffic from the web application instances for port 443

→ Sai. ALB chỉ nhận inbound traffic từ internet, chứ không nhận inbound traffic từ EC2 (outbound đến Ec2 mới đúng)

❌ Allow HTTPS inbound traffic from the web application instances for the health check on port 8443

→ Sai. Tương tự như đáp án trên, Health check là ALB gửi request đến instances, chứ không phải instances gửi về ALB. Do đó không cần inbound cho ALB trên port 8443.
</details>

---

### Q48. 
An online photo-sharing company stores its photos in an Amazon S3 bucket that exists in the us-west-1 Region. The company needs to store a copy of all new photos in the us-east-1 Region.

Which solution will meet this requirement with the LEAST operational effort?
- Create a second S3 bucket in us-east-1. Configure S3 event notifications on object creation and update events to invoke an AWS Lambda function to copy photos from the existing S3 bucket to the second S3 bucket.
- Create a second S3 bucket in us-east-1. Use S3 Cross-Region Replication to copy photos from the existing S3 bucket to the second S3 bucket.
- Create a cross-origin resource sharing (CORS) configuration of the existing S3 bucket. Specify us-east-1 in the CORS rule's AllowedOrigin element.
- Create a second S3 bucket in us-east-1 across multiple Availability Zones. Create an S3 Lifecycle rule to save photos into the second S3 bucket.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty lưu trữ ảnh trong S3 bucket ở us-west-1

Cần sao chép tất cả ảnh mới sang us-east-1 Region

Yêu cầu: giải pháp với ít công sức vận hành nhất (LEAST operational effort)

✅ Đáp án đúng:

Create a second S3 bucket in us-east-1. Use S3 Cross-Region Replication to copy photos from the existing S3 bucket to the second S3 bucket.

S3 Cross-Region Replication (CRR) là tính năng managed service của AWS, tự động sao chép objects giữa các regions mà không cần can thiệp thủ công. Chỉ cần cấu hình một lần và AWS lo phần còn lại.

![img](https://static.cloudexam.pro/courses/5/1757089877774-aonlbim2-image.png)


Các đáp án sai:

❌ Create a cross-origin resource sharing (CORS) configuration of the existing S3 bucket. Specify us-east-1 in the CORS rule's AllowedOrigin element.

→ Sai. CORS chỉ dùng để cấu hình cho phép access các file trên s3 từ các Web domain khác, không liên quan đến việc sao chép dữ liệu giữa các region.

❌ Create a second S3 bucket in us-east-1 across multiple Availability Zones. Create an S3 Lifecycle rule to save photos into the second S3 bucket.

→ Sai. S3 Lifecycle chỉ dùng để chuyển đổi object qua lại giữa các storage class hoặc xóa objects, tất cả đều trong cùng một S3 bucket. Không thể copy objects sang region khác.

❌ Create a second S3 bucket in us-east-1. Configure S3 event notifications on object creation and update events to invoke an AWS Lambda function to copy photos from the existing S3 bucket to the second S3 bucket.

→ Sai về yêu cầu tốn ít operational effort do phải viết code Lambda và quản lý function. Tốn nhiều công sức hơn so với việc sử dụng chức năng Replication có sẵn của S3.

🔑 Tips and tricks:

S3 cần copy sang region khác thì nghĩ đến S3-cross region replication
</details>

---

### Q49. 
A company is planning to deploy a business-critical application in the AWS Cloud. The application requires durable storage with consistent, low-latency performance.

Which type of storage should a solutions architect recommend to meet these requirements?

- Instance store volume
- Amazon ElastiCache for Memcached cluster
- Provisioned IOPS SSD Amazon Elastic Block Store (Amazon EBS) volume
- Throughput Optimized HDD Amazon Elastic Block Store (Amazon EBS) volume

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng quan trọng cho doanh nghiệp (business-critical)

Cần solution storage có tính bền vững (durable), perfomance cao, độ trễ thấp

✅ Đáp án đúng:

Provisioned IOPS SSD Amazon Elastic Block Store (Amazon EBS) volume

Provisioned IOPS SSD EBS là block storage có độ ổn định tốt, performance cao, hiệu suất IOPS nhất quán, cung cấp độ trễ thấp cho các ứng dụng quan trọng do đó sẽ là lựa chọn phù hợp cho use case ở đây.

Các đáp án sai:

❌ Instance store volume

Không có tính lưu trữ bền - bị dữ liệu mất khi instance stop/terminate

Chỉ phù hợp cho lưu trữ data tạm, không phù hợp cho ứng dụng business-critical

❌ Amazon ElastiCache for Memcached cluster

Đây là caching service, không phải service chuyên để lưu trữ data

❌ Throughput Optimized HDD Amazon Elastic Block Store (Amazon EBS) volume

Băng thông kém hơn và độ trễ không tốt bằng Provisioned IOPS SSD EBS, do đó không phù hợp
</details>

---

### Q50. 
A company has deployed its newest product on AWS. The product runs in an Auto Scaling group behind a Network Load Balancer. The company stores the product’s objects in an Amazon S3 bucket.

The company recently experienced malicious attacks against its systems. The company needs a solution that continuously monitors for malicious activity in the AWS account, workloads, and access patterns to the S3 bucket. The solution must also report suspicious activity and display the information on a dashboard.

Which solution will meet these requirements?"

- Configure AWS Config to monitor and report findings to Amazon EventBridge.
- Configure Amazon GuardDuty to monitor and report findings to AWS Security Hub.
- Configure Amazon Inspector to monitor and report findings to AWS CloudTrail.
- Configure Amazon Macie to monitor and report findings to AWS Config.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty cần giám sát liên tục (continuously monitors) các hoạt động trong AWS account, workloads và việc truy cập tới S3 bucket

Yêu cầu báo cáo hoạt động đáng ngờ (report suspicious activity) và hiển thị các thông tin phát hiện được lên dashboard

✅ Đáp án đúng:

Configure Amazon GuardDuty to monitor and report findings to AWS Security Hub.

GuardDuty là threat detection service giúp phát hiện hoạt động bất thường trong account thông qua việc phân tích CloudTrail events, DNS logs, VPC Flow Logs.

Security Hub cung cấp dashboad trung tâm để tổng hợp và hiển thị các báo cáo về security từ nhiều service khác nhau.

Việc sử dụng kết hợp 2 service này sẽ thoả mãn yêu cầu đề bài.

Các đáp án sai:

❌ Configure Amazon Macie to monitor and report findings to AWS Config.

Macie là service AI giúp phát hiện các thông tin cá nhân của người dùng (PII) trên S3, không có khả năng phát hiện các hoạt động bất thường trong account.

❌ Configure Amazon Inspector to monitor and report findings to AWS CloudTrail.

Inspector chỉ giúp phát hiện các lỗ hổng bảo mật (vulnerabilities) trong EC2 instances và container images, không có khả năng giám sát và phát hiện các hoạt động bất thường.

❌ Configure AWS Config to monitor and report findings to Amazon EventBridge.

Config chỉ giúp ghi lại lịch sử thay đổi setting của resource và đặt ra quy định để đảm bảo tuần thủ, không có khả năng giám sát và phát hiện các hoạt động bất thường.

🔑 Tips and tricks:

Phát hiện hoạt động bất thường, đáng ngờ "malicious" thì nghĩ đến GuardDuty

Tổng hợp và quản lý các phát hiện security (security findings) thì nghĩ đến Security Hub
</details>

---

### Q51. 
A company currently runs an on-premises application that uses ASP.NET on Linux machines. The application is resource-intensive and serves customers directly.

The company wants to modernize the application to .NET. The company wants to run the application on containers and to scale based on Amazon CloudWatch metrics. The company also wants to reduce the time spent on operational maintenance activities.

Which solution will meet these requirements with the LEAST operational overhead?
- Use AWS App2Container to containerize the application. Use an AWS CloudFormation template to deploy the application to Amazon Elastic Container Service (Amazon ECS) on Amazon EC2 instances.
- Use AWS App2Container to containerize the application. Use an AWS CloudFormation template to deploy the application to Amazon Elastic Container Service (Amazon ECS) on AWS Fargate.
- Use AWS App Runner to containerize the application. Use App Runner to deploy the application to Amazon Elastic Container Service (Amazon ECS) on AWS Fargate.
- Use AWS App Runner to containerize the application. Use App Runner to deploy the application to Amazon Elastic Kubernetes Service (Amazon EKS) on Amazon EC2 instances.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty hiện đang chạy ứng dụng ASP.NET trên Linux và muốn chuyển qua chạy containers

Yêu cầu giảm thiểu operational overhead (effort vận hành)

✅ Đáp án đúng:

Use AWS App2Container to containerize the application. Use an AWS CloudFormation template to deploy the application to Amazon Elastic Container Service (Amazon ECS) on AWS Fargate.

App2Container là service chuyên dụng để container hoá các ứng dụng một cách tự động, giảm effort thủ công

ECS Fargate là serverless platform để chạy container mà không cần quản lý hạ tầng bên dưới, từ đó giảm thiểu operational overhead

CloudFormation là service IaC (Infrastructure as Code), cho phép triển khai kiến trúc tự động hoá hoàn toàn thông qua code, do đó cũng giúp giảm thiểu operational overhead

Các đáp án sai:

❌ Use AWS App2Container to containerize the application. Use an AWS CloudFormation template to deploy the application to Amazon Elastic Container Service (Amazon ECS) on Amazon EC2 instances.

→ Sai vì lựa chọn chạy ECS trên EC2 sẽ làm tăng operational overhead so với Fargate

❌ Use AWS App Runner to containerize the application. Use App Runner to deploy the application to Amazon Elastic Container Service (Amazon ECS) on AWS Fargate.
→ App Runner là service cung cấp nền tảng chạy container built-in và tự động hoá dễ dàng, mặc dù về tính customize ít hơn ECS. App Runner không có chức năng container hoá (containerize the application)

❌ Use AWS App Runner to containerize the application. Use App Runner to deploy the application to Amazon Elastic Kubernetes Service (Amazon EKS) on Amazon EC2 instances.

→ Sai vì lý do như câu trên trên, App Runner không có chức năng container hoá (containerize the application). Hơn nữa việc chạy EKS trên EC2 so với ECS sẽ tốn effort hơn nhiều.

🔑 Tips and tricks:

Solution cần ít effort vận hành thì các đáp án sẽ không ưu tiên chọn EC2

Đối với container thì sẽ ưu tiên sử dụng AWS Fargate
</details>

---

### Q52.
A finance company uses an on-premises search application to collect streaming data from various producers. The application provides real-time updates to search and visualization features.

The company is planning to migrate to AWS and wants to use an AWS native solution.

Which solution will meet these requirements?
- Use Amazon Kinesis Data Streams to ingest and process the data streams to Amazon OpenSearch Service. Use OpenSearch Service to search the data. Use Amazon QuickSight to create visualizations.
- Use Amazon Elastic Kubernetes Service (Amazon EKS) to ingest and process the data streams to Amazon DynamoDB for storage. Use Amazon CloudWatch to create graphical dashboards to search and visualize the data.
- Use Amazon EMR to ingest and process the data streams to Amazon Redshift for storage. Use Amazon Redshift Spectrum to search the data. Use Amazon QuickSight to create visualizations.
- Use Amazon EC2 instances to ingest and process the data streams to Amazon S3 buckets tor storage. Use Amazon Athena to search the data. Use Amazon Managed Grafana to create visualizations.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty có ứng dụng ở on-premises thu thập streaming data từ nhiều nguồn

Ứng dụng cung cấp các data real-time cho tính năng search và visualization

Muốn migrate lên AWS và ưu tiên sử dụng AWS native solution

✅ Đáp án đúng:

Use Amazon Kinesis Data Streams to ingest and process the data streams to Amazon OpenSearch Service. Use OpenSearch Service to search the data. Use Amazon QuickSight to create visualizations.

→ Có thể thấy bài toán đang có một pipeline cơ bản bao gồm các bước ingest (thu thập data) → process (xử lý) → search (tìm kiếm data) → visualize (hiển thị hoá) và cần AWS native service tương đương với các bước này, do đó các service bên dưới sẽ là solution phù hợp nhất:

Kinesis Data Streams: Service dành cho việc streaming data và processing data trong thời gian thực (real-time)

OpenSearch Service: Service chuyên cho việc search data, support search data real-time

QuickSight: AWS native service cho việc hiển thị hoá data, support liên kết với OpenSearch

Toàn bộ đều là AWS managed services, đáp ứng yêu cầu "AWS native solution"

Các đáp án sai:

❌ Use Amazon EC2 instances to ingest and process the data streams to Amazon S3 buckets for storage. Use Amazon Athena to search the data. Use Amazon Managed Grafana to create visualizations.

→ EC2 không phải service chuyên dụng cho data streaming, ngoài ra Athena không phù hợp cho real-time search trên streaming data (chủ yếu cho batch queries)

❌ Use Amazon EMR to ingest and process the data streams to Amazon Redshift for storage. Use Amazon Redshift Spectrum to search the data. Use Amazon QuickSight to create visualizations.

→ EMR + Redshift phù hợp cho data warehouse và batch processing, không phù hợp cho use case real-time streaming & search

❌ Use Amazon Elastic Kubernetes Service (Amazon EKS) to ingest and process the data streams to Amazon DynamoDB for storage. Use Amazon CloudWatch to create graphical dashboards to search and visualize the data.

→ EKS không phải service chuyên dụng cho data streaming, DynamoDB không phải là service đóng vai trò là search engine, CloudWatch không phải service dùng dể hiển thị hoá data.

🔑 Tips and tricks:

Keyword thời gian thực "real-time" thì thường sẽ nghĩ đến Kinesis Data Streams

Keyword "search, real-time update" thì thường sẽ nghĩ đến OpenSearch

Keyword hiển thị hoá "visualization" thì thường sẽ nghĩ đến QuickSight
</details>

---

### Q53. 
A solutions architect is designing an application that helps users fill out and submit registration forms. The solutions architect plans to use a two-tier architecture that includes a web application server tier and a worker tier.

The application needs to process submitted forms quickly. The application needs to process each form exactly once. The solution must ensure that no data is lost.

Which solution will meet these requirements?
- Use an Amazon Simple Queue Service (Amazon SQS) FIFO queue between the web application server tier and the worker tier to store and forward form data.
- Use an Amazon Simple Queue Service (Amazon SQS) standard queue between the web application server tier and the worker tier to store and forward form data.
- Use an AWS Step Functions workflow. Create a synchronous workflow between the web application server tier and the worker tier that stores and forwards form data.
- Use an Amazon API Gateway HTTP API between the web application server tier and the worker tier to store and forward form data.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng cho phép user điền và submit forms

Dự định build kiến trúc 2 tầng: web application server tier và worker tier

Yêu cầu: xử lý mỗi form đúng 1 lần (exactly once)

Yêu cầu: không được mất data (no data loss)

✅ Đáp án đúng:

Use an Amazon Simple Queue Service (Amazon SQS) FIFO queue between the web application server tier and the worker tier to store and forward form data.

Do yêu cầu xử lý không được mất data (no data loss) nên sẽ cần implement SQS queue vào giữa 2 tầng web application server tier và worker tier như kiến trúc bên dưới.

![img](https://static.cloudexam.pro/courses/5/1757170597193-nblsasnw-image.png)


Ở đây sẽ sử dụng SQS FIFO queue vì có cơ chế deduplication để tránh xử lý trùng lặp, từ đó đảm bảo được việc chỉ xử lý message đúng 1 lần duy nhất (exactly-once processing)

Các đáp án sai:

❌ Use an Amazon API Gateway HTTP API between the web application server tier and the worker tier to store and forward form data.

→ Như vậy thì API Gateway đóng vai trò là một api trung gian giữa 2 tầng, hoàn toàn không có cơ chế đảm bảo data không bị mất (no data loss) như SQS

❌ Use an Amazon Simple Queue Service (Amazon SQS) standard queue between the web application server tier and the worker tier to store and forward form data.

→ Standard queue chỉ đảm bảo cơ chế delivery message ít nhất 1 lần (at-least-once delivery), do đó có thể bị xử lý trùng lặp nhiều hơn 1 lần, không đáp ứng yêu cầu "exactly once"

❌ Use an AWS Step Functions workflow. Create a synchronous workflow between the web application server tier and the worker tier that stores and forwards form data.

→ Synchronous workflow bắt buộc web tier phải chờ cho đến khi worker xử lý xong, do đó sẽ bị bottleneck, 2 tier không thể scale độc lập và sẽ không đáp ứng yêu cầu xử lý nhanh

🔑 Tips and tricks:

Đảm bảo data không bị mất thì sẽ nghĩ đến SQS

Đảm bảo được việc chỉ xử lý message đúng 1 lần duy nhất (exactly-once processing) thì nghĩ đến SQS FIFO
</details>

---

### Q54. 
A company is building a data analysis platform on AWS by using AWS Lake Formation. The platform will ingest data from different sources such as Amazon S3 and Amazon RDS. The company needs a secure solution to prevent access to portions of the data that contain sensitive information.

Which solution will meet these requirements with the LEAST operational overhead?
- Create an IAM role that includes permissions to access Lake Formation tables.
- Create data filters to implement row-level security and cell-level security.
- Create an AWS Lambda function that periodically queries and removes sensitive information from Lake Formation tables.
- Create an AWS Lambda function that removes sensitive information before Lake Formation ingests the data.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty xây dựng nền tảng phân tích dữ liệu (data analysis platform) trên AWS với AWS Lake Formation

Dữ liệu từ nhiều nguồn: Amazon S3 và Amazon RDS

Yêu cầu: ngăn chặn truy cập (prevent access) vào các phần dữ liệu chứa thông tin nhạy cảm (sensitive information)

Ưu tiên: ít tác vụ vận hành nhất (LEAST operational overhead)

✅ Đáp án đúng:

Create data filters to implement row-level security and cell-level security.

→ Lake Formation cung cấp tính năng data filters tích hợp sẵn để kiểm soát truy cập ở mức hàng và ô một cách tự động. Đây là giải pháp native, không cần code thêm hay quản lý infrastructure.

Các đáp án sai:

❌ Create an IAM role that includes permissions to access Lake Formation tables.

→ IAM role chỉ kiểm soát quyền truy cập table-level, không thể ẩn sensitive data ở mức row/cell cụ thể.

❌ Create an AWS Lambda function that removes sensitive information before Lake Formation ingests the data.

→ Tốn effort code thêm lambda. Tăng operational overhead và độ phức tạp không cần thiết.

❌ Create an AWS Lambda function that periodically queries and removes sensitive information from Lake Formation tables.

→ Tốn effort code thêm lambda. Tăng operational overhead và độ phức tạp không cần thiết.

🔑 Tips and tricks:

Sử dụng Lake Formation mà muốn hạn chế quyền truy cập đến 1 phần data (theo hàng hoặc cột) thì sử dụng fine-grained access control với data filter - đây là chức năng built-in có sẵn.

📖 Reference:
https://docs.aws.amazon.com/lake-formation/latest/dg/access-control-fine-grained.html
</details>

---

### Q55. 

A company is migrating an application from an on-premises environment to AWS. The application will store sensitive data in Amazon S3. The company must encrypt the data before storing the data in Amazon S3.

Which solution will meet these requirements?
- Encrypt the data by using client-side encryption with Amazon S3 managed keys.
- Encrypt the data by using client-side encryption with customer managed keys.
- Encrypt the data by using server-side encryption with AWS KMS keys (SSE-KMS).
- Encrypt the data by using server-side encryption with customer-provided keys (SSE-C).

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty lưu data trong Amazon S3 và có yêu cầu data cần được mã hoá trước khi lưu vào S3

✅ Đáp án đúng:

Encrypt the data by using client-side encryption with customer managed keys.

Đề yêu cầu rõ ràng là "encrypt before storing" - tức là data phải được mã hóa trước khi gửi lên S3. Do đó sử dụng Client-side encryption để đảm bảo dữ liệu được mã hóa tại client trước khi upload.

Các đáp án sai:

❌ Encrypt the data by using server-side encryption with AWS KMS keys (SSE-KMS)

→ Sai vì đây là server-side encryption - data được mã hóa SAU KHI đã đến S3, không phải "before storing"

❌ Encrypt the data by using server-side encryption with customer-provided keys (SSE-C)

→ Sai vì cũng là server-side encryption.

❌ Encrypt the data by using client-side encryption with Amazon S3 managed keys

→ S3 managed keys không tồn tại cho client-side encryption. S3 managed keys chỉ dùng cho server-side encryption (SSE-S3)

🔑 Tips and tricks:

Mã hoá TRƯỚC khi lưu trữ thì nghĩ ngay đến Client Side Encryption
</details>

---

### Q56.
A company has several on-premises Internet Small Computer Systems Interface (ISCSI) network storage servers. The company wants to reduce the number of these servers by moving to the AWS Cloud. A solutions architect must provide low-latency access to frequently used data and reduce the dependency on on-premises servers with a minimal number of infrastructure changes.

Which solution will meet these requirements?
- Deploy an AWS Storage Gateway volume gateway that is configured with cached volumes.
- Deploy an Amazon S3 File Gateway.
- Deploy Amazon Elastic Block Store (Amazon EBS) storage with backups to Amazon S3.
- Deploy an AWS Storage Gateway volume gateway that is configured with stored volumes.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty có nhiều iSCSI storage servers on-premises

Muốn giảm số lượng servers bằng cách chuyển lên AWS

Yêu cầu: low-latency access cho dữ liệu thường xuyên sử dụng

Giảm phụ thuộc vào on-premises với ít thay đổi về kiến trúc (minimal infrastructure changes)

✅ Đáp án đúng:

Deploy an AWS Storage Gateway volume gateway that is configured with cached volumes.

Cached volumes lưu dữ liệu chính trên S3, cache dữ liệu hay được truy cập tại local → đảm bảo truy cập độ trễ thấp (low-latency) cho data hay được truy cập

AWS Storage Gateway volume gateway vẫn giữ nguyên iSCSI interface → do đó ít thay đổi về kiến trúc (minimal infrastructure changes)

Giảm dependency vào on-premises vì phần lớn storage sẽ được chuyển lên AWS S3

![img](https://static.cloudexam.pro/courses/5/1756991188130-0brb4wc2-CleanShot_2025-09-04_at_22.05.39.png)

Các đáp án sai:

❌ Deploy an Amazon S3 File Gateway

S3 File Gateway dành cho file-based access (NFS/SMB), không support iSCSI

❌ Deploy Amazon EBS storage with backups to Amazon S3

Vô lý vì EBS chỉ gắn được với EC2, không thể gắn trực tiếp từ phía on-premises qua iSCSI

❌ Deploy an AWS Storage Gateway volume gateway that is configured with stored volumes

Sai vì stored volumes lưu vẫn lưu toàn bộ data ở on-premises → không giảm được phụ thuộc storage ở local.

🔑 Tips and tricks:

Khi cần solution storage cho local mà support dạng block storage thì sẽ nghĩ đến Volume Gateway. Keyword: iSCSI, Block Storage, Volume
</details>

---

### Q57. 
A company will migrate 10 PB of data to Amazon S3 in 6 weeks. The current data center has a 500 Mbps uplink to the internet. Other on-premises applications share the uplink. The company can use 80% of the internet bandwidth for this one-time migration task.

Which solution will meet these requirements?
- Use the AWS CLI and multiple copy processes to send the data directly to Amazon S3.
- Order multiple AWS Snowball devices. Copy the data to the devices. Send the devices to AWS to copy the data to Amazon S3.
- Use rsync to transfer the data directly to Amazon S3.
- Configure AWS DataSync to migrate the data to Amazon S3 and to automatically verify the data.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Cần migrate 10 PB data trong 6 tuần lên S3

Băng thông hiện tại là 500 Mbps tuy nhiên chỉ có thể dùng 80% bandwidth (400 Mbps available)

Cần tìm giải pháp phù hợp cho large-scale migration

✅ Đáp án đúng:

Order multiple AWS Snowball devices. Copy the data to the devices. Send the devices to AWS to copy the data to Amazon S3.

10 PB là dung lượng cực lớn. Với 400 Mbps băng thông thì, việc transfer qua internet sẽ mất khoảng 208 ngày (tính toán: 10 PB = 80,000,000 Gb ÷ 400 Mbps ÷ 86,400 giây/ngày), vượt xa 6 tuần yêu cầu. Do đó sẽ cần solution migrate qua đường vật lý.

Snowball Edge cho phép việc migrate data lên S3 qua đường vật lý thông qua việc order và shipping thiết bị đến AWS. Mỗi thiết bị có thể transfer 210TB data, có thể order nhiều thiết bị cùng lúc để việc migration diễn ra nhanh chóng, với timeline 6 tuần thì hoàn toàn khả thi

Các đáp án sai:

❌ Configure AWS DataSync to migrate the data to Amazon S3

→ DataSync vẫn phải transfer qua internet với và bị giới hạn bởi băng thông mạng hiện có, không giải quyết được vấn đề bottleneck về thời gian.

❌ Use rsync to transfer the data directly to Amazon S3
→ Tương tự như trên, vẫn phải transfer qua internet và bị giới hạn bởi băng thông mạng hiện có.

❌ Use the AWS CLI and multiple copy processes → Tương tự như trên, vẫn phải transfer qua internet và bị giới hạn bởi băng thông mạng hiện có.

🔑 Tips and tricks:

Với lượng data hàng trăm TB hay hàng PB thì việc migrate hầu như sẽ thực hiện qua Snowball Edge
</details>

---

### Q58. 
A company is planning to migrate a TCP-based application into the company's VPC. The application is publicly accessible on a nonstandard TCP port through a hardware appliance in the company's data center. This public endpoint can process up to 3 million requests per second with low latency. The company requires the same level of performance for the new public endpoint in AWS.

What should a solutions architect recommend to meet this requirement?
- Deploy an Amazon API Gateway API that is configured with the TCP port that the application requires. Configure AWS Lambda functions with provisioned concurrency to process the requests.
- Deploy an Amazon CloudFront distribution that listens on the TCP port that the application requires. Use an Application Load Balancer as the origin.
- Deploy an Application Load Balancer (ALB). Configure the ALB to be publicly accessible over the TCP port that the application requires.
- Deploy a Network Load Balancer (NLB). Configure the NLB to be publicly accessible over the TCP port that the application requires.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Migrate ứng dụng chạy giao thức TCP vào VPC

Cần AWS service để cho phép truy cập vào app thông qua một public endpoint với port TCP

Yêu cầu phải đáp ứng performance cao: 3 triệu requests/giây với độ trễ thấp (low latency)

✅ Đáp án đúng:

Deploy a Network Load Balancer (NLB). Configure the NLB to be publicly accessible over the TCP port that the application requires.

NLB là service cân bằng tải traffic được thiết kế đặc biệt cho TCP / UDP traffic với ultra-high performance (hàng triệu requests/giây), độ trễ cực thấp (ultra-low latency) do đó sẽ đáp ứng yêu cầu đề bài.

Các đáp án sai:

❌ Deploy an Application Load Balancer (ALB). Configure the ALB to be publicly accessible over the TCP port that the application requires.

→ ALB chỉ hoạt động ở Layer 7 (HTTP/HTTPS), không support TCP traffic ở Layer 4.

❌ Deploy an Amazon CloudFront distribution that listens on the TCP port that the application requires. Use an Application Load Balancer as the origin.

→ CloudFront chỉ support HTTP/HTTPS, không support TCP traffic.

❌ Deploy an Amazon API Gateway API that is configured with the TCP port that the application requires. Configure AWS Lambda functions with provisioned concurrency to process the requests.

→ CloudFront chỉ support HTTP/HTTPS, không support TCP traffic.

🔑 Tips and tricks:

Traffic TCP/UDP thì sẽ nghĩ đến Network Load Balancer, Global Accelerator
</details>

---

### Q59. 
A company uses AWS to host its public ecommerce website. The website uses an AWS Global Accelerator accelerator for traffic from the internet. The Global Accelerator accelerator forwards the traffic to an Application Load Balancer (ALB) that is the entry point for an Auto Scaling group.

The company recently identified a DDoS attack on the website. The company needs a solution to mitigate future attacks.

Which solution will meet these requirements with the LEAST implementation effort?
- Configure an Amazon CloudFront distribution in front of the Global Accelerator accelerator
- Configure an AWS WAF web ACL for the Global Accelerator accelerator to block traffic by using rate-based rules
- Configure an AWS Lambda function to read the ALB metrics to block attacks by updating a VPC network ACL
- Configure an AWS WAF web ACL on the ALB to block traffic by using rate-based rules

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty sử dụng Global Accelerator forward traffic đến Application Load Balancer (ALB) và vừa bị tấn công DDoS

Cần giải pháp giảm thiểu các cuộc tấn công tương tự trong tương lai với ít effort nhất (LEAST implementation effort)

✅ Đáp án đúng:

Configure an AWS WAF web ACL on the ALB to block traffic by using rate-based rules

WAF là service tường lửa, cho phép tạo ra các Web ACL rule để giúp ngăn chặn các loại hình tấn công phổ biến. Trong đó có Rate-based rules có thể tự động block IP khi số request vượt quá mức cho phép trong một khoảng thời gian nhất định, từ đó giúp hạn chế tấn công DDoS.

Để ngăn chặn hoàn toàn DDoS thì Shield sẽ là service chuyên dụng hợp lý hơn. Tuy nhiên trong đáp án không có nên sẽ dùng WAF thay thế. Việc implementation cũng rất đơn giản chỉ cần gắn WAF Web ACL Rule vào ALB.

![img](https://static.cloudexam.pro/courses/5/1757174300861-cfec2ntq-CleanShot_2025-09-07_at_00.58.07.png)


Các đáp án sai:

❌ Configure an AWS WAF web ACL for the Global Accelerator accelerator to block traffic by using rate-based rules

→ Global Accelerator không hỗ trợ tích hợp trực tiếp với AWS WAF. Cần thêm CloudFront làm layer trung gian, tăng complexity.

❌ Configure an AWS Lambda function to read the ALB metrics to block attacks by updating a VPC network ACL

→ Solution phức tạp, cần viết code Lambda.

❌ Configure an Amazon CloudFront distribution in front of the Global Accelerator accelerator

→ Thêm một layer mới (CloudFront) vào architecture hiện tại, tăng complexity và cost, hơn nữa cũng không có khả năng hạn chế DDoS

🔑 Tips and tricks:

WAF có thể hạn chế tấn công DDoS thông qua Rate-based rule
</details>

---

### Q60. 
A consumer survey company has gathered data for several years from a specific geographic region. The company stores this data in an Amazon S3 bucket in an AWS Region.

The company has started to share this data with a marketing firm in a new geographic region. The company has granted the firm's AWS account access to the S3 bucket. The company wants to minimize the data transfer costs when the marketing firm requests data from the S3 bucket.

Which solution will meet these requirements?
- Configure the Requester Pays feature on the company’s S3 bucket.
- Configure S3 Cross-Region Replication (CRR) from the company’s S3 bucket to one of the marketing firm’s S3 buckets.
- Configure the company’s S3 bucket to use S3 Intelligent-Tiering Sync the S3 bucket to one of the marketing firm’s S3 buckets.
- Configure AWS Resource Access Manager to share the S3 bucket with the marketing firm AWS account.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty khảo sát có dữ liệu trong S3 bucket ở một AWS Region và muốn chia sẻ dữ liệu cho một công ty marketing firm ở region khác

Mục tiêu là giảm thiểu chi phí transfer data (minimize data transfer costs) khi marketing firm truy cập dữ liệu

✅ Đáp án đúng:

Configure the Requester Pays feature on the company's S3 bucket.

Requester Pays cho phép chuyển chi phí data transfer và phí request từ bucket owner sang người request data (mặc định người sở hữu S3 phải trả toàn bộ cả phí lưu trữ và phí data transfer)

Marketing firm sẽ trả phí khi họ download data từ S3 bucket thay vì công ty phải trả, từ đó đáp ứng yêu cầu giảm thiểu chi phí transfer data khi marketing firm truy cập dữ liệu

Mô hình trả phí cho S3 requester pay
![img](https://static.cloudexam.pro/courses/5/1766849822348-5j43fi4l-image.png)

Các đáp án sai:

❌ Configure S3 Cross-Region Replication (CRR) from the company's S3 bucket to one of the marketing firm's S3 buckets.

→ CRR copy data sang bucket bên region của công ty marketing firm -> giúp tiết kiệm chi phí data transfer, nhưng trường hợp này là cho bên công ty marketing firm (do request trong cùng region), không phải cho công ty cung cấp S3.

❌ Configure AWS Resource Access Manager to share the S3 bucket with the marketing firm AWS account.

→ RAM chỉ giúp chia sẻ quản lý quyền truy cập tài nguyên, không liên quan đến chi phí data transfer. Dữ liệu vẫn phải transfer cross-region với chi phí cao.

❌ Configure the company's S3 bucket to use S3 Intelligent-Tiering Sync the S3 bucket to one of the marketing firm's S3 buckets.

→ Intelligent-Tiering chỉ tối ưu chi phí storage S3 dựa trên tần suất truy cập, không giải quyết vấn đề cross-region transfer cost.

🔑 Tips and tricks:

Requester Pay sẽ giúp tiết kiệm chi phí request và data transfer cho người sở hữu S3

Tuỳ vào việc giảm chi phí transfer cho bên nào mà đôi khi cũng có thể cân nhắc sử dụng CRR

Reference:

Using Requester Pays general purpose buckets for storage transfers and usage
</details>

---

### Q61. 
A company has multiple Amazon RDS DB instances that run in a development AWS account. All the instances have tags to identify them as development resources. The company needs the development DB instances to run on a schedule only during business hours.

Which solution will meet these requirements with the LEAST operational overhead?
- Create an Amazon EventBridge rule that invokes AWS Lambda functions to start and stop the RDS instances.
- Create AWS Systems Manager State Manager associations to start and stop the RDS instances.
- Create an AWS Trusted Advisor report to identify RDS instances to be started and stopped. Create an AWS Lambda function to start and stop the RDS instances.
- Create an Amazon CloudWatch alarm to identify RDS instances that need to be stopped. Create an AWS Lambda function to start and stop the RDS instances.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Nhiều RDS DB instances chạy trong development account

Tất cả instances đều có gắn tags để nhận diện (development resources)

Muốn instance chỉ chạy theo lịch trong giờ hành chính

Yêu cầu LEAST operational overhead (ít phức tạp vận hành nhất)

✅ Đáp án đúng:

Create AWS Systems Manager State Manager associations to start and stop the RDS instances.

State Manager là chức năng của Systems Manager được thiết kế chuyên để tự động hoá quản lý trạng thái resource.

Chỉ cần tạo association với schedule expression, chọn target bằng tags, và State Manager sẽ tự động thực hiện start/stop theo thời gian định sẵn.

Ít operational overhead nhất vì không cần viết code Lambda hay setup monitoring phức tạp.

Các đáp án sai:

❌ Create an Amazon CloudWatch alarm to identify RDS instances that need to be stopped. Create an AWS Lambda function to start and stop the RDS instances.

→ CloudWatch alarm được thiết kế để giám sát & thực hiện action với metric khi vượt quá ngưỡng cho phép, không dùng cho mục đích đặt lịch vận hành RDS.

❌ Create an AWS Trusted Advisor report to identify RDS instances to be started and stopped. Create an AWS Lambda function to start and stop the RDS instances.

→ Trusted Advisor là service đưa ra các gợi ý về mặt vận hành, security trong account, không có chức năng vận hành RDS theo lịch.

❌ Create an Amazon EventBridge rule that invokes AWS Lambda functions to start and stop the RDS instances.

→ Solution hoàn toàn hợp lệ. Tuy nhiên operational overhead cao hơn vì cần viết code cho Lambda.

📖 Reference:
https://docs.aws.amazon.com/systems-manager/latest/userguide/scheduling-automations-state-manager-associations.html
</details>

---

### Q62. 
A global ecommerce company runs its critical workloads on AWS. The workloads use an Amazon RDS for PostgreSQL DB instance that is configured for a Multi-AZ deployment.

Customers have reported application timeouts when the company undergoes database failovers. The company needs a resilient solution to reduce failover time.

Which solution will meet these requirements?
- Enable Performance Insights. Monitor the CPU load to identify the timeouts.
- Create an Amazon RDS Proxy. Assign the proxy to the DB instance.
- Take regular automatic snapshots. Copy the automatic snapshots to multiple AWS Regions.
- Create a read replica for the DB instance. Move the read traffic to the read replica.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty chạy workload quan trọng trên AWS

Sử dụng Amazon RDS PostgreSQL với Multi-AZ deployment

Khách hàng báo cáo application timeouts khi xảy ra database failovers

Cần giải pháp để giảm thời gian failover

✅ Đáp án đúng:

Create an Amazon RDS Proxy. Assign the proxy to the DB instance.

RDS Proxy hoạt động như connection pooling layer, duy trì connection pool sẵn sàng. Khi failover xảy ra, RDS Proxy chuyển hướng traffic đến standby instance mà không cần tạo lại connection từ đầu, giảm đáng kể thời gian failover và application timeout.

Tham khảo link bên dưới do aws công bố về hiệu quả giảm thời gian failover khi sử dụng RDS Proxy.

Các đáp án sai:

❌ Create a read replica for the DB instance. Move the read traffic to the read replica.

→ Read replica chỉ giải quyết vấn đề liên quan đến performance cho read traffic, không giải quyết vấn đề failover time của primary database.

❌ Enable Performance Insights. Monitor the CPU load to identify the timeouts.

→ Performance Insights chỉ là monitoring tool, không cải thiện failover time hay giảm timeout.

❌ Take regular automatic snapshots. Copy the automatic snapshots to multiple AWS Regions.
→Backup strategy không liên quan đến việc giảm failover time, chỉ dùng cho use case disaster recovery.

🔑 Tips and tricks:

Giảm thời gian failover cho RDS hoặc Aurora thì nghĩ đến RDS Proxy

📖 Reference:
https://aws.amazon.com/blogs/database/improving-application-availability-with-amazon-rds-proxy/
</details>

---

### Q63. 
A company needs to design a hybrid network architecture. The company's workloads are currently stored in the AWS Cloud and in on-premises data centers. The workloads require single-digit latencies to communicate. The company uses an AWS Transit Gateway transit gateway to connect multiple VPCs.

Which combination of steps will meet these requirements MOST cost-effectively? (Choose two.)
- Associate an AWS Direct Connect gateway with the transit gateway that is attached to the VPCs.
- Establish an AWS Site-to-Site VPN connection to an AWS Direct Connect gateway.
- Associate AWS Site-to-Site VPN connections with the transit gateway that is attached to the VPCs.
- Establish an AWS Direct Connect connection. Create a transit virtual interface (VIF) to a Direct Connect gateway.
- Establish an AWS Site-to-Site VPN connection to each VPC.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty cần thiết kế kiến trúc mạng hybrid (AWS Cloud + on-premises)

Yêu cầu độ trễ hàng mili giây (single-digit latency) để giao tiếp

Hiện tại đã dùng AWS Transit Gateway kết nối nhiều VPC

Cần giải pháp cost-effective nhất

✅ Đáp án đúng:

Associate an AWS Direct Connect gateway with the transit gateway that is attached to the VPCs.

Direct Connect Gateway cho phép Transit Gateway kết nối với on-premises qua Direct Connect

Establish an AWS Direct Connect connection. Create a transit virtual interface (VIF) to a Direct Connect gateway.

Direct Connect là đường truyền vật lý chuyên dụng với độ trễ thấp nhất, Đáp ứng yêu cầu độ trễ single-digit latency

Transit VIF kết nối Direct Connect với Direct Connect Gateway

Dựa vào kết hợp của 2 đáp án trên sẽ ra được kiến trúc như bên dưới, đáp ứng toàn bộ yêu cầu của câu hỏi.

![img](https://static.cloudexam.pro/courses/5/1757177582941-oaeju8ck-image.png)

Các đáp án sai:

❌ Establish an AWS Site-to-Site VPN connection to each VPC.

AWS Site-to-Site VPN là đường truyền dành riêng đi qua internet, do đó không đảm bảo được độ trễ độ trễ hàng mili giây (single-digit latency)

❌ Establish an AWS Site-to-Site VPN connection to an AWS Direct Connect gateway.

Tương tự như trên, VPN không đảm bảo được độ trễ độ trễ hàng mili giây (single-digit latency)

Hơn nữa VPN không thể kết nối trực tiếp với Direct Connect Gateway, vô lý.

❌ Associate AWS Site-to-Site VPN connections with the transit gateway that is attached to the VPCs.

Tương tự như trên, VPN không đảm bảo được độ trễ độ trễ hàng mili giây (single-digit latency)

🔑 Tips and tricks:

Độ trễ hàng mili giây (single-digit latency) kết nối đến môi trường on-premise thì nghĩ đến Direct Connect
</details>

---

### Q64.
A company is migrating its data processing application to the AWS Cloud. The application processes several short-lived batch jobs that cannot be disrupted. Data is generated after each batch job is completed. The data is accessed for 30 days and retained for 2 years.

The company wants to keep the cost of running the application in the AWS Cloud as low as possible.

Which solution will meet these requirements?
- Deploy Amazon EC2 On-Demand Instances to run the batch jobs. Store the data in Amazon S3 Standard. Move the data to Amazon S3 Glacier Deep Archive after 30 days. Set an expiration to delete the data after 2 years.
- Deploy Amazon EC2 Spot Instances to run the batch jobs. Store the data in Amazon S3 Standard. Move the data to Amazon S3 Glacier Flexible Retrieval after 30 days. Set an expiration to delete the data after 2 years.
- Migrate the data processing application to Amazon EC2 Spot Instances. Store the data in Amazon S3 Standard. Move the data to Amazon S3 Glacier Instant. Retrieval after 30 days. Set an expiration to delete the data after 2 years.
- Migrate the data processing application to Amazon EC2 On-Demand Instances. Store the data in Amazon S3 Glacier Instant Retrieval. Move the data to S3 Glacier Deep Archive after 30 days. Set an expiration to delete the data after 2 years.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Công ty migrate ứng dụng xử lý dữ liệu lên AWS Cloud

Ứng dụng chạy các batch job ngắn hạn không thể bị gián đoạn (cannot be disrupted)

Dữ liệu được truy cập trong 30 ngày đầu, lưu trữ tổng cộng 2 năm

Mục tiêu: giảm chi phí tối đa (keep cost as low as possible)

✅ Đáp án đúng:

Deploy Amazon EC2 On-Demand Instances to run the batch jobs. Store the data in Amazon S3 Standard. Move the data to Amazon S3 Glacier Deep Archive after 30 days. Set an expiration to delete the data after 2 years.

Lựa chọn EC2 On-Demand sẽ giúp đảm bảo batch jobs chạy không bị gián đoạn, vì có tính ổn định.

Về việc lưu trữ data, sử dụng S3 Standard cho 30 ngày đầu do cần truy cập thường xuyên. Sau đó 30 ngày chỉ cần lưu trữ lâu dài trong 2 năm nên sẽ chuyển qua Glacier Deep Archive - storage class rẻ nhất cho việc lưu trữ lâu dài. Hết 2 năm thì xoá data.

Các đáp án sai:

❌ Migrate the data processing application to Amazon EC2 Spot Instances. Store the data in Amazon S3 Standard. Move the data to Amazon S3 Glacier Instant. Retrieval after 30 days. Set an expiration to delete the data after 2 years.

→ Spot Instances có thể bị terminate bất kỳ lúc nào, không đáp ứng yêu cầu "cannot be disrupted"

❌ Migrate the data processing application to Amazon EC2 On-Demand Instances. Store the data in Amazon S3 Glacier Instant Retrieval. Move the data to S3 Glacier Deep Archive after 30 days. Set an expiration to delete the data after 2 years.

→ Không phù hợp vì dữ liệu cần truy cập thường xuyên trong 30 ngày đầu. Glacier Instant Retrieval sẽ bị đắt hơn S3 Standard cho use case truy cập thường xuyên này.

❌ Deploy Amazon EC2 Spot Instances to run the batch jobs. Store the data in Amazon S3 Standard. Move the data to Amazon S3 Glacier Flexible Retrieval after 30 days. Set an expiration to delete the data after 2 years.

→ Như trên, vẫn sử dụng Spot Instances.

🔑 Tips and tricks:

Chạy job mà cần tính ổn định, Cannot be interrupted thì sẽ tránh đáp án Spot Instances
</details>

---

### Q65. 
A company's applications run on Amazon EC2 instances in Auto Scaling groups. The company notices that its applications experience sudden traffic increases on random days of the week. The company wants to maintain application performance during sudden traffic increases.

Which solution will meet these requirements MOST cost-effectively?
- Use manual scaling to change the size of the Auto Scaling group.
- Use predictive scaling to change the size of the Auto Scaling group.
- Use target tracking scaling to change the size of the Auto Scaling group.
- Use schedule scaling to change the size of the Auto Scaling group.

<details>
<summary>Answer</summary>
📝 Tóm tắt đề:

Ứng dụng chạy trên EC2 instances & Auto Scaling groups

Traffic tăng thất thường vào các ngày ngẫu nhiên (sudden traffic increases on random days)

Mục tiêu: đảm bảo performance trong lúc traffic tăng đột biến

Yêu cầu: giải pháp cost-effective nhất

✅ Đáp án đúng:

Use target tracking scaling to change the size of the Auto Scaling group.

Target tracking scaling là cơ chế tự động scale EC2 dựa trên ngưỡng metric do mình định nghĩa (Vd: CPU 40%). Khi traffic tăng đột biến, sẽ tự động scale out ngay lập tức và scale in khi traffic giảm.

Với việc đáp ứng scale nhanh như vậy thì sẽ đảm bảo được cả performance lẫn tối ưu chi phí (cost-effective).

Các đáp án sai:

❌ Use manual scaling to change the size of the Auto Scaling group.

→ Sai vì cần can thiệp thủ công, không thể phản ứng kịp thời với sudden traffic increases xảy ra ngẫu nhiên.

❌ Use predictive scaling to change the size of the Auto Scaling group.
→ Predictive scaling là cơ chế scale EC2 dựa theo traffic trong quá khứ để từ đó đoán trước capacity để scale. Tuy nhiên đề bài nói là traffic sẽ thất thường, ngẫu nhiên nên không thể đoán trước được.

❌ Use schedule scaling to change the size of the Auto Scaling group.

→ Sai vì scheduled scaling dành cho traffic có lịch trình cố định (ví dụ: 9AM-5PM). Do đó traffic thất thường, ngẫu nhiên thì không thể áp dụng được

🔑 Tips and tricks:

Auto Scaling Group cần scale tự động để đáp ứng traffic thì nghĩ đến target tracking scaling
</details>