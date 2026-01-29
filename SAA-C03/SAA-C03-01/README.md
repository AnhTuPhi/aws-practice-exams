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

### Q26. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q27. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q28. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q29. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q30. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q31. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q32. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q30. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q31. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q32. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q33. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q34. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q35. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q36. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q37. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q38. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q39. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q40. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q41. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q42. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q43. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>

---

### Q44. Which is not a NoSQL DB?
- A. MongoDB
- B. Redis
- C. MySQL
- D. Cassandra

<details>
<summary>Answer</summary>
C
</details>