---  
category: Blog  
title: AWS Web Service  
share: true  
---  
  
  
# Common Concepts  
📒  AWS [**General Reference**](https://docs.aws.amazon.com/general/latest/gr/Welcome.html)   
  
![Pasted image 20230226173716.png](./attachments/Pasted%20image%2020230226173716.png)  
  
- AWS에서는 지연 시간을 줄이거나 추가적인 이중화를 제공하는 데 도움이 되는 격리된 지리적 위치인  [**regions**](https://docs.aws.amazon.com/general/latest/gr/rande.html) 배포를 허용합니다.  
  AZ는 같은 리전 내에서도 [물리적으로 서로 분리](https://www.youtube.com/watch?v=JIQETrFC_SQ&feature=youtu.be&t=1428)되어 있으며, 여러 물리적 데이터 센터에 걸쳐 있을 수 있습니다. 지연 시간이 짧은 링크를 통해 연결되어 있지만, 한 곳에 자연 재해가 발생해도 다른 곳에 영향을 미치지 않습니다.  
    
- 각 서비스에는 각 지역에 대한 API 엔드포인트가 있습니다. 엔드포인트는 서비스마다 다르며  [해당 tables](https://docs.aws.amazon.com/general/latest/gr/rande.html)에 나열된 것처럼 각 지역에서 모든 서비스를 사용할 수 있는 것은 아닙니다.  
- [**Amazon Resource Names (ARNs)**](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) 은 리소스를 식별하기 위한 특수 형식의 식별자입니다. `arn:`로 시작하며 특히 IAM 정책에 사용됩니다.  
  
[https://ap-northeast-1.console.aws.amazon.com/cloud9/ide/993cbcca9e1643cebdc0ca262b7092e6?region=us-west-2#](https://ap-northeast-1.console.aws.amazon.com/cloud9/ide/993cbcca9e1643cebdc0ca262b7092e6?region=us-west-2#)  
## 제한 및 기타 참고 사항  
  
- 아마존의 많은 리소스에는 [**한도**](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)가 설정되어 있습니다.   
- 이는 실제로 도움이 되므로 실수로 큰 비용이 발생하지 않습니다. 지원 티켓을 열어 할당량을 늘리도록 요청해야 합니다. 어떤 한도는 쉽게 올릴 수 있지만 어떤 한도는 그렇지 않습니다. 또한 모든 서비스 한도가 공개되는 것은 아닙니다.  
	- 현재 한도 및 사용량 확인하기: **서비스에 대한 한도 정보는 서비스 API, 트러스티드 어드바이저, 둘 다 또는 둘 다에서 제공되지 않을 수 있습니다(이 경우 지원팀에 문의해야 함).   
	- awslimitchecker 도구 설명서의 [이 페이지](http://awslimitchecker.readthedocs.io/en/latest/limits.html)에 각 한도에 대해 사용 가능한 검색 옵션에 대한 요약이 나와 있습니다.   
	- [도구](https://github.com/jantman/awslimitchecker) 자체도 한도 확인을 자동화하는 데 유용합니다.  
  
  
  
  
[AWS](https://en.wikipedia.org/wiki/Amazon_Web_Services) is the dominant public cloud computing provider.  
  
# AWS vs. other cloud providers  
1. [**Google Cloud Platform**](https://cloud.google.com/)  
	GCP arrived later to market than AWS, but has vast resources and is now used widely by many companies, including a few large ones. It is gaining market share. Not all AWS services have similar or analogous services in GCP. And vice versa: In particular, GCP offers some more advanced machine learning-based services like the [Vision](https://cloud.google.com/vision/), [Speech](https://cloud.google.com/speech/), and [Natural Language](https://cloud.google.com/natural-language/) APIs. It’s not common to switch once you’re up and running, but it does happen: [Spotify migrated](http://www.wsj.com/articles/google-cloud-lures-amazon-web-services-customer-spotify-1456270951) from AWS to Google Cloud. There is more discussion [on Quora](https://www.quora.com/What-are-the-reasons-to-choose-AWS-over-Google-Cloud-or-vice-versa-for-a-high-traffic-web-application) about relative benefits. Of particular note is that VPCs in GCP are [global by default](https://cloud.google.com/vpc/) with subnetworks per region, while AWS’ VPCs have to live within a particular region. This gives GCP an edge if you’re designing applications with geo-replication from the beginning. It’s also possible to [share one GCP VPC](https://cloud.google.com/compute/docs/shared-vpc/) between multiple projects (roughly analogous to AWS accounts), while in AWS you’d have to peer them. It’s also possible to [peer GCP VPCs](https://cloud.google.com/compute/docs/vpc/vpc-peering) in a similar manner to how it’s done in AWS  
  
2. [**Microsoft Azure**](https://azure.microsoft.com/en) is the de facto choice for companies and teams that are focused on a Microsoft stack, and it has now placed significant emphasis on Linux as well  
3. Companies at (very) large scale may want to reduce costs by managing their own infrastructure. For example, [Dropbox migrated](https://news.ycombinator.com/item?id=11282948) to their own infrastructure.  
4. Other cloud providers such as [Digital Ocean](https://www.digitalocean.com/) offer similar services, sometimes with greater ease of use, more personalized support, or lower cost. However, none of these match the breadth of products, mind-share, and market domination AWS now enjoys.  
5. Traditional managed hosting providers such as [Rackspace](https://www.rackspace.com/) offer cloud solutions as well.  
  
- **Major customers:** Who uses AWS and Google Cloud?  
	- AWS’s [list of customers](https://aws.amazon.com/solutions/case-studies/) includes large numbers of mainstream online properties and major brands, such as Netflix, Pinterest, Spotify (moving to Google Cloud), Airbnb, Expedia, Yelp, Zynga, Comcast, Nokia, and Bristol-Myers Squibb.  
	- Azure’s [list of customers](https://azure.microsoft.com/en-us/case-studies/) includes companies such as NBC Universal, 3M and Honeywell Inc.  
	- Google Cloud’s [list of customers](https://cloud.google.com/customers/) is large as well, and includes a few mainstream sites, such as [Snapchat](http://www.businessinsider.com/snapchat-is-built-on-googles-cloud-2014-1), Best Buy, Domino’s, and Sony Music.  
  
  
  
# AWS의 핵심 기능  
- [infrastructure-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Infrastructure_as_a_service_.28IaaS.29) (IaaS): <font style="color:#8A9A5B">가상 머신과 인프라 지원</font>  
: 서비스형 인프라(IaaS)에는 클라우드 IT의 기본 구성 요소가 포함되어 있으며, 일반적으로 네트워킹 기능, 컴퓨터(가상 또는 전용 하드웨어), 데이터 저장 공간에 대한 액세스를 제공합니다. IaaS는 IT 리소스에 대한 최고 수준의 유연성과 관리 제어를 제공하며, 오늘날 많은 개발자에게 익숙한 기존 IT 리소스와 가장 유사합니다.  
  
- [platform-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Platform_as_a_service_.28PaaS.29) (PaaS): <font style="color:#8A9A5B">고객의 애플리케이션을 배포하는 완전 관리형 서비스</font>  
: 서비스형 플랫폼(PaaS)을 사용하면 기본 인프라(일반적으로 하드웨어 및 운영 체제)를 관리할 필요가 없으므로 애플리케이션 배포 및 관리에만 집중할 수 있습니다. 리소스 조달, 용량 계획, 소프트웨어 유지 관리, 패치 또는 애플리케이션 실행과 관련된 기타 차별화되지 않은 과중한 작업에 대해 걱정할 필요가 없으므로 효율성을 높일 수 있습니다.  
  
- [software-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Software_as_a_service_.28SaaS.29) (SaaS): <font style="color:#8A9A5B">클라우드 기반 애플리케이션</font>  
: 서비스형 소프트웨어(SaaS)는 서비스 제공업체가 실행하고 관리하는 완성된 제품을 제공합니다. 대부분의 경우 SaaS를 언급하는 사람들은 최종 사용자 애플리케이션을 의미합니다. SaaS 제품을 사용하면 서비스가 유지되는 방식이나 기본 인프라가 관리되는 방식에 대해 생각할 필요 없이 해당 소프트웨어를 어떻게 사용할 것인지만 생각하면 됩니다. SaaS 애플리케이션의 일반적인 예로는 이메일 제품에 추가되는 기능을 관리하거나 이메일 프로그램이 실행되는 서버 및 운영 체제를 유지 관리할 필요 없이 이메일을 주고받는 데 사용할 수 있는 웹 기반 이메일을 들 수 있습니다.  
  
## 웹 어플리케이션 아키텍처  
  
  
### Traditional Web hosting  
![Pasted image 20230227181511.png](./attachments/Pasted%20image%2020230227181511.png)  
  
  
### Aws Cloud Architecture For Web Hosting  
  
![Pasted image 20230227183057.png](./attachments/Pasted%20image%2020230227183057.png)  
[사진 출처](https://mobidev.biz/blog/web-application-architecture-types)  
  
![Pasted image 20230227181259.png](./attachments/Pasted%20image%2020230227181259.png)  
  
  
- DNS services with [Amazon Route 53](https://aws.amazon.com/ko/route53/)   
: Provides DNS services to simplify domain management.  
  
- Edge caching with [Amazon CloudFront](https://aws.amazon.com/ko/cloudfront/)  
: Edge caches high-volume content to decrease the latency to customers.  
  
- Edge security for Amazon CloudFront with [AWS WAF](https://aws.amazon.com/ko/waf/)  
: Filters malicious traffic, including cross site scripting (XSS) and SQL injection via customer-defined rules.  
  
- Load balancing with [Elastic Load Balancing (ELB)](https://aws.amazon.com/ko/elasticloadbalancing/)   
: Enables you to spread load across multiple Availability Zones and AWS Auto Scaling groups for redundancy and decoupling of services.  
  
- DDoS protection with [AWS Shield](https://aws.amazon.com/ko/shield/)  
: Safeguards your infrastructure against the most common network and transport layer DDoS attacks automatically.  
  
- Firewalls with security groups  
: Moves security to the instance to provide a stateful, host-level firewall for both web and application servers.  
  
- Caching with [Amazon ElastiCache](https://aws.amazon.com/ko/elasticache/)  
: Provides caching services with Redis or Memcached to remove load from the app and database, and lower latency for frequent requests.  
  
- Managed database with [Amazon Relational Database Service (Amazon RDS)](https://aws.amazon.com/ko/rds/)   
: Creates a highly available, multi-AZ database architecture with six possible DB engines.  
  
- Static storage and backups with [Amazon Simple Storage Service (Amazon S3)](https://aws.amazon.com/ko/s3/)   
: Enables simple HTTP-based object storage for backups and static assets like images and video.  
  
  
  
  
  
![Pasted image 20230226211205.png](./attachments/Pasted%20image%2020230226211205.png)  
  
  
# AWS 디버깅  
## AWS Console  
  
[AWS Console](https://aws.amazon.com/console/)을 사용하면 웹 인터페이스를 통해 AWS의 많은 기능(전부는 아님)을 제어할 수 있습니다.  
이상적으로는 몇 가지 특정 상황에서만 AWS 콘솔을 사용하는 것이 좋습니다  
1. 읽기 전용으로 사용하기에 좋습니다. 시스템 상태를 이해하려는 경우 로그인하여 탐색하는 것이 매우 유용합니다.  
2. 매우 작은 시스템과 팀(예: 자주 변경하지 않는 서버를 한 명의 엔지니어가 설정하는 경우)에서도 합리적으로 사용할 수 있습니다.  
3. 한 달에 한 번 미만으로 거의 하지 않는 작업(예: 1년 동안 다시 방문하지 않을 일회성 VPC 설정)에도 유용할 수 있습니다. 이 경우 콘솔을 사용하는 것이 가장 간단한 방법이 될 수 있습니다.  
  
> [!info] 콘솔을 사용하기 전에 생각하기  
> AWS 콘솔은 편리하지만 자동화, 재현성 및 팀 커뮤니케이션의 적이기도 합니다. 동일한 변경을 여러 번 수행할 가능성이 높다면 콘솔을 사용하지 마세요.   
> 다음에 설명하는 것처럼 자동화를 선호하거나 최소한 자동화를 향한 경로를 마련하세요.   
> 콘솔을 사용하면 자동화를 배제하여 나중에 시간을 낭비할 뿐만 아니라 자신과 팀을 위한 문서화, 명확성 및 프로세스에 대한 표준화도 방해합니다.  
  
## Command-Line tools  
  
aws 명령어를 통해 사용되는  [**aws command-line interface**](https://aws.amazon.com/cli/) 는 AWS 작업을 저장하고 자동화하는 가장 기본적인 방법입니다.  
모든 AWS 서비스의 대부분을 다루며 최신 상태로 유지 관리가 잘 된다는 장점도 있습니다.  
일반적으로 작업을 수행할 때는 가능하면 AWS 콘솔보다 명령줄을 사용합니다.  
- 더 멋진 도구가 없더라도 특정 인수를 사용하여 aws를 호출하는 간단한 Bash 스크립트를 작성하고 이를 Git에서 확인할 수 있습니다. 이것은 수행한 작업을 문서화하는 원시적이지만 효과적인 방법입니다. 자동화를 개선하고, 팀에서 코드 검토 및 공유를 가능하게 하며, 다른 사람들이 향후 작업을 위한 출발점을 제공할 수 있습니다.  
- 주로 대화형(스크립트가 아닌)으로 사용하려면 AWS의 [**aws-shell**](https://github.com/awslabs/aws-shell) 도구를 사용하는 것을 고려하세요. 자동 완성 기능과 다채로운 UI로 사용하기가 더 쉽지만 여전히 명령줄에서 작동합니다. 이전 버전의 프로그램인 SAWS를 사용 중이라면 [aws-shell](https://github.com/donnemartin/saws/issues/68#issuecomment-240067034)로 마이그레이션해야 합니다.  
  
  
### APIs and SDKs  
  
- AWS API를 사용하기 위한 SDK는 대부분의 주요 언어로 제공되며,  [Go](https://github.com/aws/aws-sdk-go), [iOS](https://github.com/aws/aws-sdk-ios), [Java](https://github.com/aws/aws-sdk-java), [JavaScript](https://github.com/aws/aws-sdk-js), [Python](https://github.com/boto/boto3), [Ruby](https://github.com/aws/aws-sdk-ruby), [PHP](https://github.com/aws/aws-sdk-php)가 가장 많이 사용되고 있습니다.   
[awesome-aws list](https://github.com/donnemartin/awesome-aws#sdks-and-samples) 를 주로 사용합니다.  
  
- `Retry logic`: SDK를 사용할 때마다 고려해야 할 중요한 측면은 오류 처리입니다. 많이 사용하면 프로그래밍 오류부터 스로틀링, AWS 관련 중단 또는 장애에 이르기까지 다양한 장애가 발생할 수 있습니다. SDK는 일반적으로 이를 해결하기 위해  [**exponential backoff**](https://docs.aws.amazon.com/general/latest/gr/api-retries.html)를 구현하지만, 일부 애플리케이션의 경우 시간이 지남에 따라 이를 이해하고 조정해야 할 수 있습니다. 예를 들어, 일부 오류 코드에 대해서는 경고하고 다른 오류 코드에 대해서는 경고하지 않는 것이 도움이 되는 경우가 많습니다.  
  
> [!info] API를 직접 사용하지 말자.  
> AWS 설명서에는 많은 API 세부 정보가 포함되어 있지만, API에 액세스하려면 선호하는 언어의 SDK를 사용하는 것이 좋습니다. SDK는 직접 작성하는 것보다 더 성숙하고 견고하며 유지 관리가 잘 되어 있습니다.  
  
  
  
# Infrastructure  
  
## 개요  
⭐ **중요**  
-  `IAM`: User accounts and identities (you need to think about accounts early on!)  
- `EC2`: Virtual servers and associated components,  
	- Load Balancers: CLBs and <font style="color:indianred">ALBs</font>  
	- Elastic IPs: Assigned IP addresses  
- `S3`: Storage of files  
- `Route 53`: DNS and domain registration  
- `RDS`: Managed relational databases (managed MySQL, Postgres, and Amazon’s own Aurora database)  
- `ECS`: Docker container/cluster management (note Docker can also be used directly, without ECS)  
- `ECR`: Hosted private Docker registry   
- `Certificate Manager`: Manage SSL/TLS certificates for AWS services  
- `Fargate`: Docker containers management, backend for ECS and EKS  
- `CloudFront`: CDN for hosting content  
  
  
**훑어보기**  
:       `DynamoDB`: Low-latency NoSQL key-value store  
	`SQS`: Message queueing service 이름만 알기  
	`SES`: Send and receive e-mail for marketing or transactions 문자 인증할 때 사용  
	 `API Gateway`: Proxy, manage, and secure API calls (람다랑 연결)  
	`Lambda`: Running small, fully managed tasks “serverless”   
# Security and IAM  
추가 참고 [AWS IAM Policies in a Nutshell](https://start.jcolemorrison.com/aws-iam-policies-in-a-nutshell/)  
We cover security basics first, since configuring user accounts is something you usually have to do early on when setting up your system.  
  
## Security and IAM Basics  
  
![Pasted image 20230227101646.png](./attachments/Pasted%20image%2020230227101646.png)  
  
- 📒 IAM [Homepage](https://aws.amazon.com/iam/) ∙ [User guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) ∙ [FAQ](https://aws.amazon.com/iam/faqs/)  
- [AWS Security Blog](https://blogs.aws.amazon.com/security): AWS 보안에 관한 뉴스와 정보 공식 블로그  
- `IAM`은 AWS의 계정과 권한을 관리하는 데 사용하는 서비스입니다.  
- AWS에서 보안 및 액세스 제어를 관리하는 것은 매우 중요하므로 모든 AWS 관리자는 최소한 기본 수준에서 IAM을 사용하고 이해해야 합니다.  
- [IAM identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) 에는 사용자(AWS를 사용하는 사람 또는 서비스), 그룹(사용자 집합 및 해당 권한을 위한 컨테이너), 역할(AWS 서비스 인스턴스에 할당된 권한을 위한 컨테이너)이 포함됩니다.   
  이러한 ID에 대한  [Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions.html) 은  [policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)에 의해 관리됩니다. AWS 사전 정의된 정책 또는 사용자가 만든 사용자 지정 정책을 사용할 수 있습니다.  
  
인증 관리  
- [**Passwords**](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords.html): 콘솔에 로그인하기 위한 비밀번호. 이는 실제 사용자의 사용자 이름과 비밀번호  
- [**Access keys**](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html): 명령줄 도구에서 사용하는 액세스 키 → 서비스 셋업에도 사용함.  
	1. 대문자 알파벳 문자열인 'id' (e.g. `AXXXXXXXXXXXXXXXXX` 형식)  
	2. 40자 대소문자 혼합 Base64 스타일 문자열  
  
- AKIA로 시작하는 액세스 키는 일반 키입니다. ASIA로 시작하는 액세스 키는 STS의 세션/임시 키이며, 아이디 및 비밀번호와 함께 추가 "SessionToken" 파라미터를 전송해야 합니다. [a complete list of access key prefixes](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-prefixes) 문서 참조  
  
  
[**Multi-factor authentication (MFA)**](https://aws.amazon.com/iam/details/mfa/)은 사용자 인증을 위한 두 번째 보호 계층으로 키체인 포브 또는 스마트폰 앱을 사용하는 것을 적극 권장하는 방식입니다.  
  
IAM을 사용하면 사용자를 그룹으로 나누고 역할에 권한을 할당하는 등 복잡하고 세밀하게 권한을 제어할 수 있습니다. 보안 정책을 세밀하게 사용자 지정하는 데 사용할 수 있는  [policy language](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html)가 있습니다.  
  
  
IAM 정책 개념에 대한 훌륭한 개괄적인 개요는 [IAM Policies In A Nutshell](http://start.jcolemorrison.com/aws-iam-policies-in-a-nutshell/)에 나와 있습니다.  
- 정책 언어에는 복잡하고 오류가 발생하기 쉬운 JSON 구문이 있어 매우 혼란스럽기 때문에 전문가가 아닌 경우 신뢰할 수 있는 예제나 AWS의 사전 정의된  [managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html)을 기반으로 하는 것이 현명합니다.  
- 처음에는 IAM 정책이 매우 간단할 수 있지만 대규모 시스템의 경우 복잡성이 증가하므로 주의 깊게 관리해야 합니다.  
- 조직에서 한 사람(백업이 있는 경우)에게 공식적으로 IAM 정책 관리 소유권을 할당하고 모든 관리자가 해당 담당자와 협력하여 변경 사항을 검토하도록 하세요. 이렇게 하면 우발적이고 심각한 구성 오류를 방지하는 데 큰 도움이 됩니다.  
- 각 사용자나 서비스에 업무 수행에 필요한 최소한의 권한만 부여하는 것이 가장 좋습니다. 이것이 바로 보안의 기본 중 하나인 [principle of least privilege (최소 권한 원칙)](https://en.wikipedia.org/wiki/Principle_of_least_privilege)입니다. 필요한 액세스 수준에 따라 모든 IAM 사용자와 그룹을 구성하세요.  
  
권한 계층 구조  
1. 명시적 거부: 가장 제한적인 정책이 승리합니다.  
2. 명시적 허용: 모든 리소스에 대한 액세스 권한을 명시적으로 부여해야 합니다.  
3. 암시적 거부: 모든 권한이 기본적으로 암시적으로 거부됩니다.  
- AWS IAM  [policy simulator tool](https://policysim.aws.amazon.com/home/index.jsp)를 통해 정책 권한을 테스트할 수 있습니다. 이 도구는 사용자 지정 정책을 작성하는 경우에 특히 유용합니다.  
  
  
  
  
# EC2 (Elastic Compute Cloud)  
---  
  
## EC2 Basics: 컴퓨터라고 생각하는 것들을 호스팅  
  
- 📒 [Homepage](https://aws.amazon.com/ec2/) ∙ [Documentation](https://aws.amazon.com/documentation/ec2/) ∙ [FAQ](https://aws.amazon.com/ec2/faqs/) ∙ [Pricing](https://aws.amazon.com/ec2/pricing/) (see also [ec2instances.info](http://www.ec2instances.info/)\)  
  
  
- AWS가 제공하는 클라우드 컴퓨팅의 가장 기본적인 부분입니다.  
- [Virual private server](https://en.wikipedia.org/wiki/Virtual_private_server) 의  `인스턴스`는 [대부분의 Linux, BSD, Windows 운영 체제](https://aws.amazon.com/ec2/faqs/#What_operating_system_environments_are_supported) 를 실행할 수 있습니다.  
  
- AWS에서 서버를 생성하는 방식은 EC2라고도 하는 Elastic Cloud Compute 서비스를 통해 이루어집니다.   
이 서비스에서 운영 체제, CPU 성능, 메모리 수준 등과 같은 서버 설정을 선택할 수 있습니다. 모든 설정을 선택한 후에는 몇 개의 버튼을 클릭하는 것만큼이나 간단하게 서버를 시작할 수 있습니다.   
EC2를 통해 생성된 서버를 "EC2 인스턴스"라고 합니다.  
  
  
하지만 실제로 EC2 인스턴스는 실제 머신도 아닙니다. 가상 머신입니다. 따라서 우리가 생성하는 모든 서버는 실제로는 AWS의 실제 하드웨어에서 공간을 공유하는 격리된 가상 머신입니다.   
간단히 말해, 가상 머신(VM)은 실제 컴퓨터에서 시뮬레이션된 컴퓨터와 같습니다.   
자체 운영 체제, 종속성 등을 가질 수 있지만 실제 컴퓨터의 리소스를 사용하고 공유합니다.   
  
EC2 인스턴스 또는 모든 클라우드 기반 서버를 가장 간단하게 생각하는 방법은 다음과 같습니다: 그냥 AWS의 컴퓨터를 가져다 쓰는 것이라고 이해하면 쉽습니다.  
  
다른 컴퓨터와 마찬가지로 로그인하고, 설정하고, 원하는 모든 작업을 수행할 수 있습니다.   
EC2 인스턴스(서버라고도 함)를 만들고 내 컴퓨터에서와 마찬가지로 애플리케이션을 설정합니다.   
  
  
  
이 서버가 가동되면 애플리케이션을 서버에 올려놓을 수 있습니다.  
- [EC2 가격](https://aws.amazon.com/ec2/pricing/)** 및 [비용 관리](AWS%20정리.md#ec2-cost-management)  
	- 사용량에 따라 무료([AWS 무료 티어](https://aws.amazon.com/free/)에서)부터 매우 비싼 가격까지 다양합니다.   
	- 가격은 인스턴스 유형, 초 또는 시간 단위로 책정되며, AWS 지역과 인스턴스를  [On-Demand](https://aws.amazon.com/ec2/pricing/on-demand/), [Spot market](https://aws.amazon.com/ec2/spot/) 또는 사전 구매([Reserved Instances](https://aws.amazon.com/ec2/pricing/reserved-instances/)로 구매하는지 여부에 따라 달라집니다.)  
  
  
  
  
  
  
  
  
  
   
  
# ECS: EC2 Container Service  
---  
## ECS Basics: Docker를 통해 배포된 서비스 클러스터를 관리  
  
- 📒 [Homepage](https://aws.amazon.com/ecs/) ∙ [Developer guide](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/) ∙ [FAQ](https://aws.amazon.com/ecs/faqs/) ∙ [Pricing](https://aws.amazon.com/ecs/pricing/)  
  
  
  
![Pasted image 20230227163114.png](./attachments/Pasted%20image%2020230227163114.png)  
![Pasted image 20230227175741.png](./attachments/Pasted%20image%2020230227175741.png)  
ECS의 워크 플로우를 살펴보면 위의 사진과 같습니다.  
  
Container registry에 저장된 도커 이미지를 가져옵니다. 그 후 ECS에서 이미지를 실행하여 컨테이너를 생성하는데 그때, 리소스와 Compute Option을 설정합니다. 이는 도커 이미지를 run 할 때 명령어로 옵션을 설정하는 것과 같습니다.  
  
ECS는 이렇게 실행한 컨테이너들과 그 집합인 클러스터를 관리하는 역할을 합니다.  
  
이때 사용하는 컨테이너 저장소는 Docker hub를 써도 연결이 가능하지만 Amazon ECR은 AWS IAM 인증을 통해 이미지 push/pull에 대한 권한 관리가 가능하여 보안상 좋고, S3에 저장되기 때문에 고가용성 유지되며 다른 AWS 서비스들과 편리하게 연결 가능하다는 장점이 있습니다.  
  
  
- ECS 란 컨테이너의 생성과 종료, 자동 배치 및 복제, 로드 밸런싱, 클러스터링, 장애 복구, 스케줄링 등의 역할을 수행해 주는 오케스트레이선 도구입니다.  
- 웹사이트를 실행할 수 있도록 Docker파일을 EC2 인스턴스에 넣습니다.   
  ECS는 Docker 컨테이너를 관리하고 배포합니다.  
- [ECR](https://aws.amazon.com/ecr/)(EC2 컨테이너 레지스트리)은 Amazon의 관리형 Docker 레지스트리 서비스입니다. (도커 파일 저장)  
-   
  
ECS에서는 컨테이너를 올리는 서버, 컨테이너를 어디에 호스팅 할 건지 정할 수 있는데 서버리스 방식인 Fargate 위에 올릴 수도 있고, EC2에 올릴 수도 있습니다.  
  
서버리스 방식인 Fargate는 아마존에서 많은 부분을 관리해 주어 유연성은 좀 떨어지지만, AWS에 많은 부분을 맡길 수 있는 AWS 매니지드 서비스이므로 운영하기에 매우 편리합니다.   
따라서 AWS나 도커 운영에 대해 잘 모르는 사용자도 사용하기 편리합니다.   
  
EC2는 사용자가 커스터마이징 할 수 있는 부분이 많고 비용도 저렴하나 관리에 손이 많이 가기 때문에 어느 정도 컨테이너와 AWS를 다룰 수있는 사용자가 사용하여야 합니다.   
  
  
  
## Docker & Image & Container  
[Docker Series — What is Docker?. Over the last few weeks I’ve been… | by nolan grace | Pintail.ai | Medium](https://medium.com/pintail-labs/docker-series-what-is-docker-9eddca88f434)  
  
![Pasted image 20230227112610.png](./attachments/Pasted%20image%2020230227112610.png)  
  
  
1. <font style="color:indianred">Dockerfile</font>은 컨테이너에 설치해야하는 패키지, 소스코드, 명령어, 환경변수 설정 등을 기록한 스크립트 파일입니다.  
2. 이렇게 만든 Dockerfile을 build 하면 <font style="color:indianred">도커 이미지</font>가 됩니다.   
3. 도커 이미지를 Run 하면 이미지가 실행되어 하나의 애플리케이션으로 서비스를 하는 <font style="color:indianred">컨테이너</font>(미니 컴퓨터)가 생성됩니다.   
  
  
  
이미지는 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것으로 상태값을 가지지 않고 변하지 않습니다(Immutable). 컨테이너는 이미지를 실행한 상태라고 볼 수 있고 추가되거나 변하는 값은 컨테이너에 저장됩니다.   
같은 이미지에서 여러개의 컨테이너를 생성할 수 있고 컨테이너의 상태가 바뀌거나 컨테이너가 삭제되더라도 이미지는 변하지 않고 그대로 남아있습니다.  
  
말그대로 이미지는 컨테이너를 실행하기 위한 모든 정보를 가지고 있기 때문에 더 이상 의존성 파일을 컴파일하고 이것저것 설치할 필요가 없습니다. 이제 새로운 서버가 추가되면 미리 만들어 놓은 이미지를 다운받고 컨테이너를 생성만 하면 됩니다. 한 서버에 여러개의 컨테이너를 실행할 수 있고, 수십, 수백, 수천대의 서버도 문제없습니다.  
  
컨테이너를 만들면 컨테이너에는 Docker 이미지에서 정의한 모든 항목이 포함될 것입니다. 예를 들어, 이 Docker 이미지로 컨테이너 3개를 만들면 모두 Ubuntu, Node.js, 애플리케이션 코드의 개별 인스턴스를 갖게 됩니다.  
  
이 방법의 가장 좋은 점은 완전히 분리되어 격리되어 있다는 것입니다. 서로 영향을 미치지 않으므로 컨테이너를 생성하면 하나의 머신에서 여러 가지를 호스팅할 수 있는 매우 깔끔한 방법입니다.   
  
  
  
  
## The core components of ECS  
  
  
- `Task Definition`: 컨테이너를 실행하기 위해 정의한 설정입니다.  
- `Task`: Task Definition에 의해 배포된 컨테이너 Set입니다. ECS에서 컨테이너를 실행하는 최소 단위는 Task이다.  
- `Service`: 클러스터에 Task를 몇 개나 배포할 것인지 결정하고 ELB와 연결하거나 Auto Scaling을 설정하는 등 Task의 LifeCycle을 관리합니다.  
  
- `ECS Container Instances`: ECS를 통해서 Task가 배포되는 EC2 인스턴스를 Container Instance라고 합니다. (Fargate 는 사용 X)  
- `ECS Container Agent`: 컨테이너 에이전트는 Amazon ECS 클러스터 내의 각 인프라 리소스에서 실행됩니다. 컨테이너 에이전트는 리소스의 현재 실행 중인 작업 및 리소스 사용률에 대한 정보를 Amazon ECS로 전송하고, Amazon ECS로부터 요청을 받을 때마다 작업을 시작 및 중지합니다.  
- `Cluster`: Cluster Instance 들이 논리적으로 그룹화되는 단위를 의미합니다.  
  
  
  
- **WorkFlow**  
1. 컨테이너의 이미지를 저장소(ECR)에 커밋  
2. task definition에서 사용할 이미지 및 시작 유형, 리소스 정의  
3. cluster 생성  
4. service가 task definition을 참고하여 task 생성  
5. elb에 들어온 요청에 따라 오토 스케일링 및 로드 밸런싱  
  
## Description  
  
아래로 갈수록 큰 개념  
### container  
단순히 시야가 제한된 리눅스 프로세스입니다. 도커라 생각하면 됨!  
namespace 분리로 컨테이너 환경 안에서 호스트의 프로세스 목록을 볼 수 없고,  
루트 디렉터리 변경으로 호스트의 디렉터리를 볼 수 없도록 제한합니다. (chroot)  
끝으로 제어 그룹을 통해 컨테이너가 접근할 수 있는 자원을 제어합니다. (control group)  
  
  
  
### Task Definitions & Task  
  
![Pasted image 20230227142438.png](./attachments/Pasted%20image%2020230227142438.png)  
  
**작업 정의(Task Definitions)**  
:  <font style="color:#6082B6">애플리케이션을 구성하는 컨테이너를 설명하는 텍스트(JSON)</font>  
	태스크를 어떻게, 몇 개 이상 실행할지 정의한 Set입니다.  
	 미리 생성한 컨테이너를 1~N개까지 포함시키게 됩니다.  
	 이 공간 안에서 실행되는 컨테이너는 네트워크 자원을 공유합니다.  
	 - task definition 안에 컨테이너를 몇 개 포함시킬지는 설계하기 나름이지만, 보통 세트로 움직이는 컨테이너를 묶습니다.  
		 - 예를 들어, 컨테이너는 한 개의 역할(목적)에 집중하는 프로세스로 정의하고 해당 프로세스에서 발생하는 로그를 별도의 공간(CloudWatch 등)으로 처리하는 프로세스(fluentd 등)를 한 개의 task definition으로 정의할 수 있습니다.  
	- **옵션 설정:** Task 실행 시 Task Role, Docker Network Mode, Docker 실행 명령, CPU/Memory 제한, Logging Driver, Vloum Mount 등 설정 가능.  
	- **Rollback:** 모두 Revision Version 이 기록되어 저장 되므로, 특정 Revision Version으로 Task를 Roll back 가능  
  
  
**작업(Task)**  
: <font style="color:#6082B6">ECS Cluster에서의 최소 실행 단위 (수행되고 있는 단위)</font>  
	Task definition에서 정의된 대로 실제 생성된 Container set 입니다.   
	Task 안에는 한 개 이상의 컨테이너들이 포함되어 있으며 ECS에서 컨테이너를 실행하는 최소 단위입니다.  
	여러 컨테이너를 하나로 묶어서 실행한다고 생각하면 되며, docker-compose와 비슷한 개념	  
  
해당 Task 내의 도커 컨테이너는 Cluster에 속한 `ECS Container Instance` 내에 실행되도록 보장 받습니다.   
Task는 여러 Container Instance (EC2 Instance) 에 배포가능합니다.  
  
Task는 Cluster에 속한 Container instance(EC2 Instance)에 배포되게 됩니다.  
또한, Task는 여러 Container instance(EC2 Instace)에 배포 가능합니다.  
  
  
**Task를 실행하는 방식**  
 1. Task Definitions를 이용한 실행  
    :  Task에 대한 관리가 되지 않으며, AWS에서 제공하는 다른 서비스 (ELB, AutoScale)등을 이용할 수 없으므로, 대부분 사용하지 않습니다.  
 2. <font style="color:#6082B6">Service</font>를 이용한 실행  
	 1. 데몬(Daemon) 타입  
	    모든 컨테이너 인스턴스에 해당하는 태스크가 하나씩 실행됨. 이것은 인스턴스 관리를 위한 용도로 사용됨. 클러스터 내의 모든 ECS 인스턴스에 무조건 하나씩 실행되는 방식입니다.  
	 2. 리플리카(Replica) 타입  
	    서비스에 정의된 작업 개수 및 AutoScale 설정에 따라 클러스터 인스턴스에 Task가 복제하여 실행하는 방식  
	    실행하려는 태스크의 개수를 지정하여, 이 개수만큼 실행되도록 자동 관리해줍니다.  
	    웹서버를 비롯한 실제 서비스에서 주로 사용됩니다.  
  
 ![Pasted image 20230227180148.png](./attachments/Pasted%20image%2020230227180148.png)  
A simple comparison (not entirely correct but should give you an idea)  
  
### Service   
![Pasted image 20230227153207.png](./attachments/Pasted%20image%2020230227153207.png)  
  
- ECS Cluster에 Task를 관리하는 상위 그룹 개념 (Task의 묶음)  
- Task definition을 참고하여 Task들을 실행 및 관리합니다.  
- Task 를 Cluster에 몇 개나 배포할 것인지 결정하고, 실제 Task 들을 외부에 서비스 하기 위해 ELB 에 연동 되는 부분을 관리하게 됩니다.  
- 중요한 점은 서비스가 로드 밸런서를 사용하도록 구성할 수 있다는 것입니다. 즉, 태스크를 생성할 때(즉, 태스크 정의에 정의된 컨테이너를 시작할 때) 서비스가 컨테이너의 EC2 인스턴스를 로드 밸런서에 자동으로 등록합니다. 태스크는 로드 밸런서를 사용하도록 구성할 수 없으며, 서비스만 가능합니다.  
  
1. Task 실행 유형(EC2 or Fargate)  
2. 타입(복제 or 데몬)  
3. 테스크가 실행 되어야할 작업 개수 유지 및 관리하는 스케줄러 역할  
   ECS 서비스는 각 인스턴스들에 설치된 ecs-client에서 수집된 정보를 기반으로 어디에 어떤 태스크를 언제 실행할지 결정  
4. 배포 방식(롤링 or Blue Green)  
5. 배치 전략  
6. AutoScale 설정  
7. 컨테이너와 ELB(ALB or NLB) 등 설정  
  
> 참고: Fargate  
> ECS 클러스터내에 인스턴스가 없어도, Task에 정의한 CPU, 메모리 설정에 따라 관리하는 EC2 인스턴스 없이 Serverless 하게 서비스를 실행  
> Fargate는 물리적인 EC2의 존재를 사용자가 인지할 필요 없이 task 운영을 가능하게 해 줍니다.  
> EC2 만으로 ECS를 운영하는 경우 예측 가능한 트래픽이 아니면 인스턴스 scale-out에 필요한 최소한의 시간이 보장되어야 합니다. Fargate는 인스턴스 부팅에 필요한 시간이 없기 때문에 스파이크 트래픽에 보다 안정적으로 대응이 가능합니다.  
> 다만, 가격이 EC2로 운영하는 것 대비해서 비쌉니다. 인스턴스나 서비스 운영의 관리 여건이 안될 때 주로 사용합니다.  
  
  
  
이제 서비스가 생겼으니, 그 태스크에 액세스하려면 어딘가에서 실행해야 합니다.   
클러스터에 배치해야 하며, 컨테이너 관리 서비스는 하나 이상의 ECS 컨테이너 인스턴스에서 실행되는 작업을 처리합니다.  
  
### ECS Container Instances (= EC2 Instance)  
  
![Pasted image 20230227175612.png](./attachments/Pasted%20image%2020230227175612.png)  
  
  
This is an EC2 instance that has Docker and an ECS Container Agent running on it. A Container Instance can run many Tasks, from the same or different Services.  
  
- **ECS Container Instance (EC2 instance)**  
Amazon ECS 컨테이너 인스턴스는 Amazon ECS 컨테이너 에이전트를 실행하고 있으며 Amazon ECS 클러스터에 등록된 Amazon EC2 인스턴스입니다.  
  
ECS 는 Container 배포(Task 배포)를 EC2 instance 기반에 올리도록 설계 되어 있습니다.  
Task를 올리기 위해 등록된 `EC2 instance` 를 `Container Instance` 라고 부릅니다.  
컨테이너 인스턴스는 동일하거나 다른 서비스에서 많은 Task를 실행할 수 있습니다.  
   
다음과 같은 과정이 통과된 인스턴스를 container instance라고 부릅니다.  
1. ecs-agent를 설치   
2. 클러스터에 묶이도록 설정   
3. 정상적인 설정의 경우 클러스터에 묶임.   
  
  
ECS를 처음 시작하면 생성되는 Default Cluster 에는 Container instance를 자동으로 할당 시켜 주기도 하지만  
새롭게 Cluster 를 생성하게 되면 직접 Container instance 를 만들어야 합니다.  
  
Container instance 용 AMI 이미지는 AWS 측에서 제공해 주기 때문에 어렵지 않게 생성이 가능합니다.  
  
  
  
  
- **ECS Container Agent**  
: ECS와 인스턴스 간의 통신을 처리하여 인스턴스 안에서 동작하는 task와 container의 상태를 관리하는 역할을 합니다.   
	이 기능 덕분에 ECS를 콘솔에서 모니터링/관리할 수 있습니다.  
  
  
> [!info] 정리  
> - `ecs-client`: 컨테이너 인스턴스의 자원을 모니터링 및 관리하고, 클러스터로 요청된 컨테이너들을 적절하게 실행하는 역할을 합니다.  
> - `ECS Agent`: 컨테이너 인스턴스를 클러스트에 연결 시키고 명령을 전달(오케스트레이션)을 하기 위해서는 AWS에서 제공하는 ECS Agent가 컨테이너 인스턴스(EC2)에 설치가 되어있어야 합니다.  
  
  
  
### Cluster  
  
  
![Pasted image 20230227143109.png](./attachments/Pasted%20image%2020230227143109.png)  
  
- **ECS의 가장 기본적인 단위**  
Cluster: <font style="color:indianred">여러 대의 컴퓨터들이 연결되어 하나의 시스템처럼 동작하는 컴퓨터들의 집합 </font>으로 이해  
  
클러스터는 도커 컨테이너를 실행할 수 있는 가상의 공간입니다.   
클러스터를 사용하여 애플리케이션을 격리할 수 있습니다.   
보통 프로젝트 단위나 업무(service) 단위로 구분지어 생성합니다.  
  
1. 클러스터는 컨테이너 인스턴스(EC2)의 논리적인 그룹화입니다.  
2. 논리적인 공간이므로, 컨테이너 인스턴스가 없는 빈 클러스터도 생성이 가능합니다.  
3. 클러스터 생성 시 빈 클러스터가 아닌 컨테이너 인스턴스 타입과 수를 지정하여 생성한 경우, 기본적으로 ECS Agent가 설치된 AMI로 EC2가 자동 생성되어 클러스터와 연결됩니다.  
     
간단히 말해, 클러스터는 몇 개의 컨테이너 인스턴스를 논리적으로 그룹화한 것입니다. EC2 인스턴스의 논리적 그룹이라고 하면 더 간단합니다.  
  
클러스터의 각 컨테이너 인스턴스는 인스턴스가 클러스터에 연결할 수 있도록 하는 Amazon ECS 컨테이너 에이전트를 실행합니다.  
  
  
### Auto-scaling  
  
수평 확장 기능으로, service와 cluster에서 설정할 수 있습니다.  
- service의 auto-scaling은 task의 수를 조절  
	- service의 auto-scaling의 경우 물리적인 인스턴스 메모리 제약을 계산해서 최소/최대를 설정해야 합니다.   
- cluster의 auto-scaling은 container instances의 수를 조절합니다.  
  
  
service 안에 있는 task 1개가 1 GiB 메모리를 사용한다고 가정하고, 최소 task 개수를 16개로 설정한다고 생각해봅시다.   
그럼 container instances는 몇 대가 운영되어야 할까요? 4 GiB 인스턴스라면 최소 4대 이상은 돼야 모든 task를 운영할 수 있을 겁니다.  이렇듯 cluster의 물리적인 자원 한계도 인지하고 있어야 합니다.   
  
  
  
  
## 요약  
도커화된 애플리케이션이 서비스와 일대일 관계를 갖는 태스크 정의로 표현하고, 그리고 이를 사용하여 다양한 태스크 인스턴스를 생성할 수 있습니다.  
  
서비스는 애플리케이션을 실행하고 확장하는 데 필요한 리소스 풀을 제공하는 ECS 컨테이너 인스턴스 클러스터에 배포됩니다.   
또한, 추가 서비스를 동일한 클러스터에 배포할 수 있습니다.  
  
  
  
  
  
**Cluster, Container Instance와 Task, Service의 관계를 도식화 시켜 보면 아래와 같습니다.**  
  
![Pasted image 20230227154136.png](./attachments/Pasted%20image%2020230227154136.png)  
  
![Pasted image 20230227165942.png](./attachments/Pasted%20image%2020230227165942.png)  
  
  
![Pasted image 20230227164546.png](./attachments/Pasted%20image%2020230227164546.png)  
  
  
  
  
  
## ECS Alternatives and Lock-in  
  
- [Kubernetes](https://kubernetes.io): Extensive container platform. Available as a hosted solution on [Google Cloud](https://cloud.google.com/kubernetes-engine), [AWS](https://aws.amazon.com/eks/), [Azure](https://azure.microsoft.com/en-us/services/kubernetes-service/), [DigitalOcean](https://www.digitalocean.com/products/kubernetes/), and [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift).  
- [Nomad](https://www.nomadproject.io/): Orchestrator/Scheduler, tightly integrated in the HashiCorp stack (Consul, Vault, etc).  
  
  
  
  
  
# S3: Simple Storage Service  
---  
  
## S3 Basics: 정적 파일 저장  
  
- 📒 [Homepage](https://aws.amazon.com/s3/) ∙ [Developer guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html) ∙ [FAQ](https://aws.amazon.com/s3/faqs/) ∙ [Pricing](https://aws.amazon.com/s3/pricing/)  
- **S3** (Simple Storage Service) is AWS’ standard cloud storage service, offering file (opaque “blob”) storage of arbitrary numbers of files of almost any size, from 0 to **5TB**. (Prior to [2011](https://aws.amazon.com/releasenotes/Amazon-S3/1917932037969964) the maximum size was 5 GB; larger sizes are now well supported via [multipart support](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html).)  
- Items, or **objects**, are placed into named **buckets** stored with names which are usually called **keys**. The main content is the **value**.  
- 웹사이트용 이미지 및 기타 자산을 저장합니다. 백업을 보관하고 서비스 간에 파일을 공유합니다. 정적 웹사이트 호스팅. 또한 다른 많은 AWS 서비스도 S3에서 쓰고 읽습니다.  
- Objects are created, deleted, or updated. Large objects can be streamed, but you cannot modify parts of a value; you need to update the whole object. Partial data access can work via [S3 Select](https://aws.amazon.com/blogs/aws/s3-glacier-select/).  
- Every object also has [**metadata**](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html), which includes arbitrary key-value pairs, and is used in a way similar to HTTP headers. Some metadata is system-defined, some are significant when serving HTTP content from buckets or CloudFront, and you can also define arbitrary metadata for your own use.  
- **S3 URIs:** Although often bucket and key names are provided in APIs individually, it’s also common practice to write an S3 location in the form 's3://bucket-name/path/to/key' (where the key here is 'path/to/key'). (You’ll also see 's3n://' and 's3a://' prefixes [in Hadoop systems](https://cwiki.apache.org/confluence/display/HADOOP2/AmazonS3).)  
  
Amazon 무제한 FTP 서버  
  
  
  
  
  
  
  
  
# Load Balancers  
--------------  
  
## Load Balancer Basics  
Elastic Load Balancing은 들어오는 애플리케이션 트래픽을 Amazon EC2 인스턴스, Docker 컨테이너, IP 주소, Lambda 함수 등 여러 대상에 자동으로 분산합니다. 단일 가용 영역 또는 여러 가용 영역에 걸쳐 애플리케이션 트래픽의 다양한 부하를 처리할 수 있습니다. Elastic Load Balancing은 애플리케이션의 내결함성을 확보하는 데 필요한 고가용성, 자동 확장, 강력한 보안을 모두 갖춘 세 가지 유형의 로드 밸런서를 제공합니다.  
  
  
- AWS has 3 load balancing products   
	1. <font style="color:indianred">Application Load Balancers (ALBs)</font>  
	   애플리케이션 LB는 HTTP(S) 트래픽에 가장 적합하며 레이어 7 OSI에서 부하를 분산합니다. 애플리케이션을 인식할 수 있을 만큼 지능적이며, 애플리케이션 부하 분산 장치는 경로 기반 라우팅, 호스트 기반 라우팅, 컨테이너화된 애플리케이션 지원도 지원합니다. 예를 들어, 웹 브라우저의 언어를 프랑스어로 변경하면 애플리케이션 LB는 브라우저에서 수신하는 메타데이터를 통해 사용자가 사용하는 언어에 대한 세부 정보가 포함된 정보를 파악할 수 있습니다. 그러면 브라우징 환경을 최적화하기 위해 LB 뒤의 백엔드에 있는 프랑스어 서버로 사용자를 라우팅합니다. 또한 고급 요청 라우팅을 생성하여 특정 경우에 대해 직접 설정한 규칙에 따라 트래픽을 특정 서버로 이동할 수도 있습니다.  
	2. **Network Load Balancers (NLB)**  
	   네트워크 LB는 성능이 요구되는 TCP 트래픽에 가장 적합하며 레이어 4의 부하를 분산합니다. 초당 수백만 건의 요청을 관리하면서 지연 시간을 매우 낮게 유지할 수 있습니다.  
	3. **Classic Load Balancers (CLBs)**  
	   클래식 LB는 레거시 ELB 제품이며 HTTP(S) 또는 TCP 중 하나에서 균형을 맞추지만 둘 다는 아닙니다. 가장 오래된 LB이긴 하지만, 여전히 스티키 세션과 X-Forwarded-For 헤더와 같은 기능을 지원합니다.  
	     
  
  
- **Life cycle**  
	1. 브라우저가 DNS에 로드 밸런서의 IP 주소를 요청합니다.  
	2. DNS가 IP를 제공합니다.  
	3. IP를 받은 브라우저는 로드 밸런서에서 HTML 페이지에 대한 HTTP 요청을 합니다.  
	4. AWS 경계 장치는 요청을 확인하고 확인한 후 LB에 전달합니다.  
	5. LB는 HTTP 요청을 전달할 활성 웹서버를 찾습니다.  
	6. 웹 서버는 요청된 HTML 파일을 반환합니다.  
	7. 브라우저는 요청된 HTML 파일을 수신하여 화면에 그래픽으로 렌더링합니다.  
  
  
  
  
  
# ALB: Application Load Balancers  
---  
![Pasted image 20230227183321.png](./attachments/Pasted%20image%2020230227183321.png)  
다음 다이어그램은 기본 구성 요소를 보여줍니다. 각 수신기에는 기본 규칙이 포함되어 있고, 한 수신기에는 요청을 다른 대상 그룹으로 라우팅하는 또 다른 규칙이 포함되어 있음을 알 수 있습니다. 하나의 대상이 두 개의 대상 그룹에 등록되어 있습니다.  
  
## ALB Basics  
  
- 📒 [Homepage](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/) ∙ [User guide](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/developer-resources/) ∙ [FAQ](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/faqs/) ∙ [Pricing](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/pricing/)  
- 🐥**Websockets and HTTP/2** are [now supported](https://aws.amazon.com/blogs/aws/new-aws-application-load-balancer/).  
- 🐥**Internet Protocol Version 6 (IPv6)** is [now supported](https://aws.amazon.com/about-aws/whats-new/2017/01/announcing-internet-protocol-version-6-ipv6-support-for-elastic-load-balancing-in-amazon-virtual-private-cloud-vpc/).  
- 🐥**Load Balancing via IP** is [now supported](https://aws.amazon.com/about-aws/whats-new/2017/08/elastic-load-balancing-application-load-balancer-now-supports-load-balancing-to-ip-addresses-as-targets-for-aws-and-on-premises-resources/).  
- Prior to the Application Load Balancer, you were advised to use TCP instead of HTTP as the protocol to make it work (as described [here](http://www.quora.com/When-will-Amazon-ELB-offer-SPDY-support)) and use [the obscure but useful Proxy Protocol](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/enable-proxy-protocol.html) ([more on this](https://chrislea.com/2014/03/20/using-proxy-protocol-nginx/)) to pass client IPs over a TCP load balancer.  
  
  
### The Core Components  
A load balancer serves as the single point of contact for clients. The load balancer distributes incoming application traffic across multiple targets, such as EC2 instances, in multiple Availability Zones. This increases the availability of your application. You add one or more listeners to your load balancer.  
  
A listener checks for connection requests from clients, using the protocol and port that you configure. The rules that you define for a listener determine how the load balancer routes requests to its registered targets. Each rule consists of a priority, one or more actions, and one or more conditions. When the conditions for a rule are met, then its actions are performed. You must define a default rule for each listener, and you can optionally define additional rules.  
  
Each target group routes requests to one or more registered targets, such as EC2 instances, using the protocol and port number that you specify. You can register a target with multiple target groups. You can configure health checks on a per target group basis. Health checks are performed on all targets registered to a target group that is specified in a listener rule for your load balancer.  
  
The following diagram illustrates the basic components. Notice that each listener contains a default rule, and one listener contains another rule that routes requests to a different target group. One target is registered with two target groups.  
  
[Load Balancer](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancers.html)는 클라이언트를 위한 단일 연락 창구 역할을 합니다. 로드 밸런서는 들어오는 애플리케이션 트래픽을 여러 가용성 영역의 EC2 인스턴스 등 여러 대상에 분산합니다. 이렇게 하면 애플리케이션의 가용성이 향상됩니다. 로드 밸런서에 하나 이상의 리스너를 추가합니다.  
  
[Listener](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html)는 구성한 프로토콜 및 포트를 사용하여 클라이언트의 연결 요청을 확인합니다. 수신기에 대해 정의하는 규칙에 따라 로드 밸런서가 등록된 대상으로 요청을 라우팅하는 방법이 결정됩니다. 각 규칙은 우선순위, 하나 이상의 작업 및 하나 이상의 조건으로 구성됩니다. 규칙의 조건이 충족되면 해당 작업이 수행됩니다. 각 수신기에 대해 기본 규칙을 정의해야 하며, 선택적으로 추가 규칙을 정의할 수 있습니다.  
  
각 [Target group](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/application-load-balancers.html)은 지정한 프로토콜 및 포트 번호를 사용하여 요청을 하나 이상의 등록된 대상(예: EC2 인스턴스)으로 라우팅합니다. 하나의 대상을 여러 대상 그룹에 등록할 수 있습니다. 대상 그룹별로 상태 검사를 구성할 수 있습니다. 로드 밸런서의 수신기 규칙에 지정된 대상 그룹에 등록된 모든 대상에 대해 상태 검사가 수행됩니다.  
  
### Related services  
- Amazon EC2  
: 클라우드에서 애플리케이션을 실행하는 가상 서버입니다. EC2 인스턴스로 트래픽을 라우팅하도록 로드 밸런서를 구성할 수 있습니다.  
  
- Amazon EC2 Auto Scaling   
: 인스턴스에 장애가 발생하더라도 원하는 수의 인스턴스를 실행하도록 보장하며, 인스턴스 수요 변화에 따라 인스턴스 수를 자동으로 늘리거나 줄일 수 있습니다.   
	Elastic Load Balancing과 함께 자동 확장을 활성화하면 자동 확장에 의해 시작된 인스턴스는 자동으로 로드 밸런서에 등록되고, 자동 확장에 의해 종료된 인스턴스는 자동으로 로드 밸런서에서 등록이 해제됩니다.  
  
  
-  AWS Certificate Manager  
: HTTPS 리스너를 생성할 때 ACM에서 제공하는 인증서를 지정할 수 있습니다. 로드 밸런서는 인증서를 사용하여 연결을 종료하고 클라이언트의 요청을 해독합니다.   
	자세한 내용은 [SSL 인증서](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/create-https-listener.html#https-listener-certificates)를 참조  
  
- Amazon CloudWatch  
: 로드 밸런서를 모니터링하고 필요에 따라 조치를 취할 수 있습니다.   
	자세한 내용은 [애플리케이션 로드 밸런서에 대한 CloudWatch 메트릭](https://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudwatch-metrics.html)을 참조  
  
  
- Amazon ECS   
: EC2 인스턴스 클러스터에서 Docker 컨테이너를 실행, 중지 및 관리할 수 있습니다. 컨테이너로 트래픽을 라우팅하도록 로드 밸런서를 구성할 수 있습니다.   
	자세한 내용은 Amazon Elastic 컨테이너 서비스 개발자 가이드에서 [서비스 로드 밸런싱](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/service-load-balancing.html)을 참조  
  
  
  
# Elastic IPs  
---  
## Elastic IP Basics  
  
- 📒 [Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) ∙ [FAQ](https://aws.amazon.com/ec2/faqs/#Elastic_IP) ∙ [Pricing](https://aws.amazon.com/ec2/pricing/on-demand/#Elastic_IP_Addresses)  
- **Elastic IPs** are static IP addresses you can rent from AWS to assign to EC2 instances.  
  
  
  
  
# RDS  
  
  
![Pasted image 20230226152639.png](./attachments/Pasted%20image%2020230226152639.png)  
  
## RDS Basics: AWS의 관계형 데이터베이스 서비스  
  
- 📒 [Homepage](https://aws.amazon.com/rds/) ∙ [User guide](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/) ∙ [FAQ](https://aws.amazon.com/rds/faqs/) ∙ [Pricing](https://aws.amazon.com/rds/pricing/) (see also [ec2instances.info/rds/](http://www.ec2instances.info/rds/)\)  
- **RDS** is a managed relational database service, allowing you to deploy and scale databases more easily. It supports [Oracle](https://aws.amazon.com/rds/oracle/), [Microsoft SQL Server](https://aws.amazon.com/rds/sqlserver/), [PostgreSQL](https://aws.amazon.com/rds/postgresql/), [MySQL](https://aws.amazon.com/rds/mysql/), [MariaDB](https://aws.amazon.com/rds/mariadb/), and Amazon’s own [Aurora](https://aws.amazon.com/rds/aurora/).  
- RDS offers out of the box support for [high availability and failover](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) for your databases.  
  
  
  
  
  
# RDS Aurora  
---  
## RDS Aurora Basics  
Aurora는 자가 복구 스토리지와 인스턴스당 최대 64TB의 자동 확장 기능을 갖춘 분산형 내결함성 관계형 데이터베이스를 제공하도록 설계된 클라우드 전용 데이터베이스 서비스입니다.  현재 MySQL 호환 시스템과 PostgreSQL 호환 시스템의 두 가지 버전으로 제공됩니다.  
  
확장, 관리 및 패치를 모두 처리하고, MySQL 및 PostgreSQL과도 직접 호환됩니다.   
따라서 이 두 가지 중 하나를 사용하여 로컬로 개발하는 경우에도 애플리케이션을 배포할 때 Aurora를 사용할 수 있습니다.   
사용자는 서버에 접속하여 애플리케이션을 사용할 수 있고 애플리케이션은 RDS Aurora 데이터베이스와 상호 작용합니다.  
  
  
  
  
# Fargate  
-------  
  
## Fargate Basics  
- 📒 [Homepage](https://aws.amazon.com/fargate/) ∙ [FAQ](https://aws.amazon.com/fargate/faqs/) ∙ [Pricing](https://aws.amazon.com/fargate/pricing/)  
-   **Fargate** allows you to manage and deploy containers without having to worry about running the underlying compute infrastructure  
-   Fargate serves as a new backend (in addition to the legacy EC2 backend) on which ECS and EKS tasks can be run  
-   Fargate and EC2 backends are called "Launch Types"  
-   Fargate allows you to treat containers as fundamental building blocks of your infrastructure  
  
  
## Fargate Tips  
-   Fargate follows a similar mindset to Lambda, which lets you focus on applications, instead of dealing with underlying infrastructure  
-   Fargate is supported by CloudFormation, aws-cli and ecs-cli  
-   Fargate tasks can be launched alongside tasks that use EC2 Launch Type  
-   💸Before creating a large Fargate deployment, make sure to estimate costs and compare them against alternative solution that uses traditional EC2 deployment - Fargate prices can be several times those of equivalently-sized EC2 instances. To evaluate both solutions based on potential costs, refer to pricing for [EC2](https://aws.amazon.com/ec2/pricing/) and [Fargate](https://aws.amazon.com/fargate/pricing/).  
  
  
## Fargate Alternatives and Lock-in  
-   🚪[Azure Container Instances](https://azure.microsoft.com/en-us/services/container-instances/): Available on Microsoft Azure in preview version, allows to run applications in containers without having to manage virtual machines  
  
  
## Fargate Gotchas and Limitations  
-   The smallest resource values that can be configured for an ECS Task that uses Fargate is 0.25 vCPU and 0.5 GB of memory  
-   [Task storage is ephemeral. After a Fargate task stops, the storage is deleted.](https://docs.aws.amazon.com/AmazonECS/latest/developerguide/fargate-task-storage.html)  
  
  
# Lambda  
------  
  
## Lambda Basics  
  
  
- 📒 [Homepage](https://aws.amazon.com/lambda/) ∙ [Developer guide](http://docs.aws.amazon.com/lambda/latest/dg/) ∙ [FAQ](https://aws.amazon.com/lambda/faqs/) ∙ [Pricing](https://aws.amazon.com/lambda/pricing/)  
- **Lambda** is AWS' serverless compute offering, allowing users to define Lambda functions in a selection of runtimes that can be invoked via a variety of triggers, including SNS notifications and API Gateway invocations. Lambda is the key service that enables ['serverless' architecture on AWS](https://aws.amazon.com/lambda/serverless-architectures-learn-more/), alongside AWS API Gateway, AWS Batch, and AWS DynamoDB.  
  
다음 용도로 사용하세요.  
독립적인 작업을 수행하기 위해 JS, Java 또는 Python의 작은 독립형 스니펫을 실행합니다. 일종의 큐와 실행을 하나로 합친 것입니다.   
AWS 설정에 대한 변경 사항을 저장한 다음 실행하거나 S3 또는 DynamoDB의 이벤트에 응답하는 데 사용됩니다.  
  
AWS Lambda는 Amazon API Gateway를 통한 HTTP 요청, Amazon Simple Storage Service(Amazon S3) 버킷에 있는 객체에 대한 변경 사항, Amazon DynamoDB의 테이블 업데이트 또는 AWS Step Functions의 상태 전환과 같은 다양한 이벤트에 대한 응답으로 코드를 자동 실행할 수 있습니다.  
  
  
1. 파일 처리  
![Pasted image 20230227184729.png](./attachments/Pasted%20image%2020230227184729.png)  
Amazon Simple Storage Service(Amazon S3)를 사용하여 업로드 후에 실시간으로 AWS Lambda 데이터 처리를 트리거하거나 기존 Amazon EFS 파일 시스템에 연결하여 대규모 파일 처리를 위한 대규모 병렬 공유 액세스를 지원합니다.  
  
  
2. 스트림 처리  
![Pasted image 20230227184903.png](./attachments/Pasted%20image%2020230227184903.png)  
서버리스 스트림 처리 작동 방식을 보여주는 다이어그램입니다. 소셜 미디어 스트림이 Amazon Kinesis에 로드된 후 Lambda가 트리거됩니다. Lambda는 해시태그 추세 데이터를 생성하는 코드를 실행하고 데이터는 손쉽게 쿼리할 수 있도록 DynomoDB에 저장됩니다.  
  
  
3. 웹 어플리케이션  
![Pasted image 20230227184915.png](./attachments/Pasted%20image%2020230227184915.png)  
Amazon S3, API Gateway, AWS Lambda 및 DynamoDB가 함께 작동하여 웹 또는 모바일 애플리케이션에서 날씨 데이터를 검색하는 방법을 보여주는 다이어그램입니다.  
  
  
# 콜드 웜  
우리가 다큐에서 머신건을보면 처음부터 마구 발사되지는 않습니다. 맨처음 앞부분이 천천회 회전하더니 엄청난 양의 총알을 퍼붇습니다. 비슷하게 람다에는 Warm Start와 Cold Start가 있어 미리 예열된 상태라면 이전보다 훨씬 빠른속도로 함수들을 처리합니다. Warm/Cold가 사실 단어가 상당히 영어적인 표현입니다. 우리말로 바꾸자면 람다가 바로 준비 된 상태 그리고 람다가 바로 준비되지 않은 상태가 제일 적절할 것 같습니다.  
  
람다는 처음 호출한다면 Cold Start를 하게됩니다. 이 경우 초기 지연시간이 발생합니다. 그리고 Warm 상태를 유지하며 이때의 경우는 람다를 다시 호출해도 지연시간이 발생하지 않고 빠르게 응답하게됩니다. 그러다 약 5분여 이상 호출이 되지 않는다면 Cold상태로 바뀌고 다시 호출될 경우 지연시간을 가지고 Cold Start를 하게됩니다.  
  
  
[AWS Lambda를 시작하기 전 알았으면 좋았을것들. 제가 처음 Serverlesss를 접했을때 생각했습니다. 정말 힙하고… | by Harry The Great | 해리의 유목코딩 | Medium](https://medium.com/harrythegreat/aws-lambda%EB%A5%BC-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%EC%A0%84-%EC%95%8C%EC%95%98%EC%9C%BC%EB%A9%B4-%EC%A2%8B%EC%95%98%EC%9D%84%EA%B2%83%EB%93%A4-788bd3b3bdd2)  
  
  
## Lambda Code Samples  
  
- [Fan-out](https://github.com/awslabs/aws-lambda-fanout) is an example of using Lambda to “fan-out” or copy data from one service, in this case Kinesis, to multiple other AWS data services. Destinations for fan-out data in the sample include IoT, SQS and more.  
- This [AWS limit monitor using Lambdas](https://github.com/awslabs/aws-limit-monitor) shows use of multiple Lambdas for monitoring.  
- This [Lambda ECS Worker Pattern](https://github.com/awslabs/lambda-ecs-worker-pattern) shows use of Lambda in a workflow where data from S3 is picked up by the Lambda, pushed to a queue, then sent to ECS for more processing.  
- The [Secure Pet Store](https://github.com/awslabs/api-gateway-secure-pet-store) is a sample Java application which uses Lambda and API Gateway with Cognito (for user identity).  
- [aws-lambda-list](https://github.com/unixorn/aws-lambda-list) is a list of "hopefully useful AWS lambdas and lambda-related resources". Quite a few code samples here; as usual, not guaranteed tested. Caveat Emptor.  
  
  
  
# 추가 보완할 내용  
[프로덕션을 위한 AWS Lambda 최적화 팁 4가지 - Dashbird](https://dashbird.io/blog/optimizing-aws-lambda-for-production-kr/)  
[AWS Lambda – FAQ](https://aws.amazon.com/ko/lambda/faqs/)  
  
  
  
  
# API Gateway  
-----------  
  
## API Gateway Basics  
  
![Pasted image 20230227185154.png](./attachments/Pasted%20image%2020230227185154.png)  
- 📒 [Homepage](https://aws.amazon.com/api-gateway/) ∙ [Developer guide](http://docs.aws.amazon.com/apigateway/latest/developerguide/) ∙ [FAQ](https://aws.amazon.com/api-gateway/faqs/) ∙ [Pricing](https://aws.amazon.com/api-gateway/pricing/)  
- **API Gateway** provides a scalable, secured front-end for service APIs, and can work with Lambda, Elastic Beanstalk, or regular EC2 services.  
- API 게이트웨이는 개발자를 위한 완전 관리형 서비스로, 전체 API를 쉽게 빌드, 게시, 관리 및 보호할 수 있습니다. AWS 관리 콘솔에서 몇 번의 클릭만으로 애플리케이션이 백엔드 서비스(예: EC2에서 실행되는 워크로드)의 데이터, 비즈니스 로직 또는 기능에 액세스할 수 있는 '프런트 도어' 역할을 하는 API를 생성할 수 있습니다.  
- It allows “serverless” deployment of applications built with Lambda.  
- Switching over deployments after upgrades can be tricky. There are no built-in mechanisms to have a single domain name migrate from one API gateway to another one. So it may be necessary to build an additional layer in front (even another API Gateway) to allow smooth migration from one deployment to another.  
  
  
Amazon API Gateway는 어떤 규모에서든 개발자가 API를 손쉽게 생성, 게시, 유지 관리, 모니터링 및 보안 유지할 수 있도록 하는 완전관리형 서비스입니다. API는 애플리케이션이 백엔드 서비스의 데이터, 비즈니스 로직 또는 기능에 액세스할 수 있는 "정문" 역할을 합니다. API Gateway를 사용하면 실시간 양방향 통신 애플리케이션이 가능하도록 하는 RESTful API 및 WebSocket API를 작성할 수 있습니다. API Gateway는 컨테이너식 서버리스 워크로드 및 웹 애플리케이션을 지원합니다.  
  
  
  
  
  
# Route 53  
--------  
  
## Route 53 Basics: 도메인 발급  
  
![Pasted image 20230227185243.png](./attachments/Pasted%20image%2020230227185243.png)  
  
- 📒 [Homepage](https://aws.amazon.com/route53/) ∙ [Developer guide](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/) ∙ [FAQ](https://aws.amazon.com/route53/faqs/) ∙ [Pricing](https://aws.amazon.com/route53/pricing/)  
- **Route 53** is AWS’ DNS service.  
  
Amazon Route 53는 가용성과 확장성이 뛰어난 도메인 이름 시스템(DNS) 웹 서비스입니다. Route 53는 사용자 요청을 AWS 또는 온프레미스에서 실행되는 인터넷 애플리케이션에 연결합니다.  
  
  
# VPC  
-------------------------------------------  
## VPC Basics  
![Pasted image 20230227102638.png](./attachments/Pasted%20image%2020230227102638.png)  
  
- 📒 [Homepage](https://aws.amazon.com/vpc/) ∙ [User guide](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide) ∙ [FAQ](https://aws.amazon.com/vpc/faqs/) ∙ [Security groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) ∙ [Pricing](https://aws.amazon.com/vpc/pricing/)  
- Amazon Virtual Private Cloud(Amazon VPC)를 사용하면 리소스 배치, 연결 및 보안을 포함하여 가상 네트워킹 환경을 완전히 제어할 수 있습니다. AWS 서비스 콘솔에서 VPC를 설정하여 시작하십시오. 그런 다음 Amazon Elastic Compute Cloud(EC2) 및 Amazon Relational Database Service(RDS) 인스턴스와 같은 리소스를 VPC에 추가합니다. 마지막으로 VPC가 계정, 가용 영역 또는 AWS 리전에서 서로 통신하는 방법을 정의합니다.  
- **VPC** (Virtual Private Cloud) is the virtualized networking layer of your AWS systems.  
- VPC를 사용하면 정의한 가상 네트워크 내에서 서비스 및 시스템을 시작할 수 있는 논리적으로 격리된 AWS 클라우드 섹션을 프로비저닝할 수 있습니다. 어떤 AWS 리소스를 공개할지, 어떤 리소스를 공개하지 않을지 선택할 수 있는 옵션이 있기 때문에 VPC는 보안을 훨씬 더 세밀하게 제어할 수 있습니다.  
- Most AWS users should have a basic understanding of VPC concepts, but few need to get into all the details. VPC configurations can be trivial or extremely complex, depending on the extent of your network and security needs.  
- All modern AWS accounts (those created [after 2013-12-04](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html)) are “EC2-VPC” accounts that support VPCs, and all instances will be in a default VPC. Older accounts may still be using “EC2-Classic” mode. Some features don’t work without VPCs, so you probably will want to [migrate](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-migrate.html).  
![Pasted image 20230227102626.png](./attachments/Pasted%20image%2020230227102626.png)  
  
모든 AWS 서비스가 훨씬 더 큰 네트워크의 작은 조각이 아니라 동일한 작은 네트워크에 있는 것처럼 보이게 합니다.  
  
공유기 같은 것  
추가 참고 [AWS VPC Core Concepts in an Analogy and Guide](https://start.jcolemorrison.com/aws-vpc-core-concepts-analogy-guide/)  
  
  
  
  
  
  
  
# Certificate Manager  
-------------------  
  
## Certificate Manager Basics  
![Pasted image 20230227185440.png](./attachments/Pasted%20image%2020230227185440.png)  
  
- 📒 [Homepage](https://aws.amazon.com/certificate-manager/) ∙ [User guide](http://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) ∙ [FAQ](https://aws.amazon.com/certificate-manager/faqs/) ∙ [Pricing](https://aws.amazon.com/certificate-manager/pricing/)  
- Use the **Certificate Manager** to manage SSL/TLS certificates in other AWS services.  
- Supports importing existing certificates as well as issuing new ones.  
- Provides Domain Validated (DV) certificates. [Validation](http://docs.aws.amazon.com/acm/latest/userguide/gs-acm-validate.html) can be done in two ways. The first (and recommended) way is [via DNS](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-validate-dns.html). If the zone lives within Route 53 and the user has access, the necessary record can be added in the console via a single click during the certificate request process. If the zone is not within Route 53 the user is required to update DNS manually. This is still preferred to the second way, which requires more user interaction, and is done by sending an email to 3 contact addresses in WHOIS and 5 common addresses for the domain, for each domain name present in the request.  
- ACM will attempt to automatically [renew](http://docs.aws.amazon.com/acm/latest/userguide/how-domain-validation-works.html) a certificate issued by Amazon. It will first attempt to connect to the domain on HTTPS and check that the certificate used by the domain is the same with the certificate that it intends to renew. Failing that, it will check the DNS record used previously for validation. Failing that, ACM will attempt manual validation by sending emails to all domains in the certificate.  
  
- During the domain validation process, if DNS validation is unsuccessful Certificate Manager will send an email to every contact address specified in the domain’s WHOIS record and up to five common administrative addresses. Some anti-spam filters can mark emails as spam because of this. You should check the spam folder of your email if you don’t receive a confirmation email.  
  
  
AWS Certificate Manager(ACM)를 사용하면 AWS 서비스 및 연결된 내부 리소스에 사용할 공인 및 사설 SSL/TLS 인증서를 프로비저닝, 관리 및 배포할 수 있습니다. ACM은 SSL/TLS 인증서를 구매, 업로드 및 갱신하는 데 드는 시간 소모적인 수동 프로세스를 대신 처리해줍니다.  
  
  
  
  
# Billing and Cost Management  
---------------------------  
  
## Billing and Cost Visibility  
  
- AWS offers a [**free tier**](https://aws.amazon.com/free/) of service, that allows very limited usage of resources at no cost. For example, a micro instance and small amount of storage is available for no charge. Many services are only eligible for the free tier for the first twelve months that an account exists, but other services offer a free usage tier indefinitely. (If you have an old account but starting fresh, sign up for a new one to qualify for the free tier.) [AWS Activate](https://aws.amazon.com/activate/) extends this to tens of thousands of dollars of free credits to startups in [certain funds or accelerators](https://aws.amazon.com/activate/portfolio-detail/).  
- You can set [**billing alerts**](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/free-tier-alarms.html) to be notified of unexpected costs, such as costs exceeding the free tier. You can set these in a [granular way](https://wblinks.com/notes/aws-tips-i-wish-id-known-before-i-started/#billing).  
- AWS offers [Cost Explorer](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-explorer-what-is.html), a tool to get better visibility into costs.  
- Unfortunately, the AWS console and billing tools are rarely enough to give good visibility into costs. For large accounts, the AWS billing console can time out or be too slow to use.  
- **Tools:**  
	- Enable [billing reports](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/detailed-billing-reports.html) and install an open source tool to help manage or monitor AWS resource utilization. [**Teevity Ice**](https://github.com/Teevity/ice) (originally written by Netflix) is probably the first one you should try. Check out [docker-ice](https://github.com/jonbrouse/docker-ice) for a Dockerized version that eases installation.  
	- One challenge with Ice is that it doesn’t cover amortized cost of reserved instances.  
	- Other tools include [Security Monkey](https://github.com/Netflix/security_monkey) and [Cloud Custodian](https://github.com/capitalone/cloud-custodian).  
	- Use [AWS Simple Monthly Calculator](https://calculator.s3.amazonaws.com/index.html) to get an estimate of usage charges for AWS services based on certain information you provide. Monthly charges will be based on your actual usage of AWS services, and may vary from the estimates the Calculator has provided.  
- **Third-party services:** Several companies offer services designed to help you gain insights into expenses or lower your AWS bill, such as [Cloudability](https://www.cloudability.com/), [CloudHealth Technologies](https://www.cloudhealthtech.com/), and [ParkMyCloud](http://www.parkmycloud.com/). Some of these charge a percentage of your bill, which may be expensive. See the [market landscape](AWS%20정리.md#tools-and-services-market-landscape).  
- AWS’s [Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/) is another service that can help with cost concerns.  
- Don’t be shy about asking your account manager for guidance in reducing your bill. It’s their job to keep you happily using AWS.  
- **Tagging for cost visibility:** As the infrastructure grows, a key part of managing costs is understanding where they lie. It’s strongly advisable to [tag resources](https://aws.amazon.com/blogs/aws/resource-groups-and-tagging/), and as complexity grows, group them effectively. If you [set up billing allocation appropriately](http://aws.amazon.com/blogs/aws/aws-cost-allocation/), you can then get visibility into expenses according to organization, product, individual engineer, or any other way that is helpful.  
- If you need to do custom analysis of raw billing data or want to feed it to a third party cost analysis service, [enable](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/detailed-billing-reports.html#turnonreports) the [detailed billing report](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/detailed-billing-reports.html#detailed-billing-report) feature.  
- Multiple Amazon accounts can be linked for billing purposes using the [Consolidated Billing](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html) feature. Large enterprises may need complex billing structures depending on ownership and approval processes.  
- Multiple Amazon accounts can be managed centrally using [AWS Organizations](https://aws.amazon.com/organizations/).  
  
  
## AWS Data Transfer Costs  
  
- For deployments that involve significant network traffic, a large fraction of AWS expenses are around data transfer. Furthermore, costs of data transfer, within AZs, within regions, between regions, and into and out of AWS and the internet vary significantly depending on deployment choices.  
- Some of the most common gotchas:  
	- *AZ-to-AZ traffic:* Note EC2 traffic between AZs is effectively the same as between regions. For example, deploying a Cassandra cluster across AZs is helpful for [high availability](AWS%20정리.md#high-availability), but can hurt on network costs.  
	- *Using public IPs when not necessary:* If you use an Elastic IP or public IP address of an EC2 instance, you will incur network costs, even if it is accessed locally within the AZ.  
	- *Managed NAT Gateway data processing:* Managed NAT Gateways are used to let traffic egress from private subnets--at a cost of 4.5¢ as a data processing fee layered on top of data transfer pricing. Past a certain point, running your own NAT instances becomes far more cost effective.  
	- *Some services do cross-AZ traffic for free:* Many AWS services you'd not consider on their own merits offer a hidden value of free cross-AZ data transfer. EFS, RDS, MSK, and others are examples of this.  
- This figure gives an overview:  
  
  
![Pasted image 20230226163650.png](./attachments/Pasted%20image%2020230226163650.png)  
## EC2 Cost Management  
  
- With EC2, there is a trade-off between engineering effort (more analysis, more tools, more complex architectures) and spend rate on AWS. If your EC2 costs are small, many of the efforts here are not worth the engineering time required to make them work. But once you know your costs will be growing in excess of an engineer’s salary, serious investment is often worthwhile.  
- Larger instances aren’t necessarily priced higher in the spot market – therefore, you should look at the available options and determine which instances will be most cost effective for your jobs. See [Bid Advisor](https://aws.amazon.com/ec2/spot/bid-advisor/).  
- **Spot instances:**  
	- EC2 [Spot instances](https://aws.amazon.com/ec2/spot/) are a way to get EC2 resources at significant discount — often many times cheaper than standard on-demand prices — if you’re willing to accept the possibility that they be terminated with little to no warning.  
	- Use Spot instances for potentially very significant discounts whenever you can use resources that may be restarted and don’t maintain long-term state.  
	- The huge savings that you can get with Spot come at the cost of a significant increase in complexity when provisioning and reasoning about the availability of compute capacity.  
	- Amazon maintains Spot prices at a market-driven fluctuating level, based on their inventory of unused capacity. Prices are typically low but can [spike](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-limits.html#spot-bid-limit) very high. See the [price history](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances-history.html) to get a sense for this.  
	- You set a bid price high to indicate how high you’re willing to pay, but you only pay the going rate, not the bid rate. If the market rate exceeds the bid, your instance may be terminated.  
	- Prices are per instance type and per availability zone. The same instance type may have wildly different price in different zones at the same time. Different instance types can have very different prices, even for similarly powered instance types in the same zone.  
	- Compare prices across instance types for better deals.  
	- Use Spot instances whenever possible. Setting a high bid price will assure your machines stay up the vast majority of the time, at a fraction of the price of normal instances.  
	- Get notified up to two minutes before price-triggered shutdown by polling [your Spot instances’ metadata](https://aws.amazon.com/blogs/aws/new-ec2-spot-instance-termination-notices/), or by watching for [the termination CloudWatch event](https://aws.amazon.com/about-aws/whats-new/2018/01/amazon-ec2-spot-two-minute-warning-is-now-available-via-amazon-cloudwatch-events/).  
	- Make sure your usage profile works well for Spot before investing heavily in tools to manage a particular configuration.  
- **Spot fleet:**  
	- You can realize even bigger cost reductions at the same time as improvements to fleet stability relative to regular Spot usage by using [Spot fleet](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet.html) to bid on instances across instance types, availability zones, and (through multiple Spot Fleet Requests) regions.  
	- Spot fleet targets maintaining a specified (and weighted-by-instance-type) total capacity across a cluster of servers. If the Spot price of one instance type and availability zone combination rises above the weighted bid, it will rotate running instances out and bring up new ones of another type and location up in order to maintain the target capacity without going over target cluster cost.  
- **Spot usage best practices:**  
	- **Application profiling:**  
		- Profile your application to figure out its runtime characteristics. That would help give an understanding of the minimum cpu, memory, disk required. Having this information is critical before you try to optimize spot costs.  
		- Once you know the minimum application requirements, instead of resorting to fixed instance types, you can bid across a variety of instance types (that gives you higher chances of getting a spot instance to run your application).E.g., If you know that 4 cpu cores are enough for your job, you can choose any instance type that is equal or above 4 cores and that has the least Spot price based on history. This helps you bid for instances with greater discount (less demand at that point).  
	- **Spot price monitoring and intelligence:**  
		- Spot Instance prices fluctuate depending on instance types, time of day, region and availability zone. The AWS CLI tools and API allow you to describe Spot price metadata given time, instance type, and region/AZ.  
		- Based on history of Spot instance prices, you could potentially build a myriad of algorithms that would help you to pick an instance type in a way that **optimizes cost**, **maximizes availability**, or **offers predictable performance**.  
		- You can also track the number of times an instance of certain type got taken away (out bid) and plot that in graphite to improve your algorithm based on time of day.  
	- **Spot machine resource utilization:**  
		- For running spiky workloads (spark, map reduce jobs) that are schedule based and where failure is non critical, Spot instances become the perfect candidates.  
		- The time it takes to satisfy a Spot instance could vary between 2-10 mins depending on the type of instance and availability of machines in that AZ.  
		- If you are running an infrastructure with hundreds of jobs of spiky nature, it is advisable to start pooling instances to optimize for cost, performance and most importantly time to acquire an instance.  
		- Pooling implies creating and maintaining Spot instances so that they do not get terminated after use. This promotes re-use of Spot instances across jobs. This of course comes with the overhead of lifecycle management.  
		- Pooling has its own set of metrics that can be tracked to optimize resource utilization, efficiency and cost.  
		- Typical pooling implementations give anywhere between 45-60% cost optimizations and 40% reduction in spot instance creation time.  
		- An excellent example of Pooling implementation described by Netflix ([part1](http://techblog.netflix.com/2015/09/creating-your-own-ec2-spot-market.html), [part2](http://techblog.netflix.com/2015/11/creating-your-own-ec2-spot-market-part-2.html)\)  
- **Spot management gotchas**  
	- **Lifetime:** There is [no guarantee](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html) for the lifetime of a Spot instance. It is purely based on bidding. If anyone outbids your price, the instance is taken away. Spot is not suitable for time sensitive jobs that have strong SLA. Instances will fail based on demand for Spot at that time. AWS provides a [two-minute warning](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html#spot-instance-termination-notices) before Amazon EC2 must terminate your Spot instance.  
	- **API return data:** - The Spot price API returns Spot prices of varying granularity depending on the time range specified in the api call.E.g If the last 10 min worth of history is requested, the data is more fine grained. If the last 2 day worth of history is requested, the data is more coarser. Do not assume you will get all the data points. There **will** be skipped intervals.  
	- ❗**Lifecycle management:** Do not attempt any fancy Spot management unless absolutely necessary. If your entire usage is only a few machines and your cost is acceptable and your failure rate is lower, do not attempt to optimize. The pain for building/maintaining it is not worth just a few hundred dollar savings.  
- **Reserved Instances:** allow you to get significant discounts on EC2 compute hours in return for a commitment to pay for instance hours of a specific instance type in a specific AWS region and availability zone for a pre-established time frame (1 or 3 years). Further discounts can be realized through “partial” or “all upfront” payment options.  
	- Consider using Reserved Instances when you can predict your longer-term compute needs and need a stronger guarantee of compute availability and continuity than the (typically cheaper) Spot market can provide. However be aware that if your architecture changes your computing needs may change as well so long term contracts can seem attractive but may turn out to be cumbersome.  
	- There are two types of Reserved Instances - [Standard and Convertible](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/reserved-instances-types.html). If you purchase excess Standard Reserved Instances, you may offer to “sell back” unused Reserved Instances via the [Reserved Instance Marketplace](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ri-market-selling-guide.html), this allows you to potentially recoup the cost of unused EC2 compute instance hours by selling them to other AWS customers.  
	- Instance reservations are not tied to specific EC2 instances - they are applied at the billing level to eligible compute hours as they are consumed across all of the instances in an account.  
	- 📜There have been scattered reports of Convertible RI purchases needing to be exercised in a block-- namely, if you buy five convertible RIs in one purchase, you can't convert just two of them. Reach out to your account manager for clarification if this may impact you.  
- If you have multiple AWS accounts and have configured them to roll charges up to one account using the “Consolidated Billing” feature, you can expect *unused* Reserved Instance hours from one account to be applied to matching (region, availability zone, instance type) compute hours from another account.  
- If you have multiple AWS accounts that are linked with Consolidated Billing, plan on using reservations, and want unused reservation capacity to be able to apply to compute hours from other accounts, you’ll need to create your instances in the availability zone with the same *name* across accounts. Keep in mind that when you have done this, your instances may not end up in the same *physical* data center across accounts - Amazon shuffles availability zones names across accounts in order to equalize resource utilization.  
- Make use of dynamic [Auto Scaling](AWS%20정리.md#auto-scaling), where possible, in order to better match your cluster size (and cost) to the current resource requirements of your service.  
- If you use RHEL instances and happen to have existing RHEL on-premise Red Hat subscriptions, then you can leverage Red Hat's [Cloud Access program](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) to migrate a portion of your on-premise subscriptions to AWS, and thereby saving on AWS charges for RHEL subscriptions. You can either use your own self-created RHEL AMI's or Red Hat provided [Gold Images](https://access.redhat.com/articles/2962171) that will be added to your private AMI's once you sign up for Red Hat Cloud Access.  
  
# Further Reading  
---------------  
  
This section covers a few unusually useful or “must know about” resources or lists.  
  
- AWS  
	- [AWS In Plain English](https://www.expeditedssl.com/aws-in-plain-english): A readable overview of all the AWS services.  
	- [Awesome AWS](https://github.com/donnemartin/awesome-aws): A curated list of AWS tools and software.  
	- [AWS Tips I Wish I'd Known Before I Started](https://wblinks.com/notes/aws-tips-i-wish-id-known-before-i-started/): A list of tips from [Rich Adams](https://richadams.me/)  
	- [AWS Whitepapers](https://aws.amazon.com/whitepapers/): A list of technical AWS whitepapers, covering topics such as architecture, security and economics.  
	- [Last Week in AWS](https://lastweekinaws.com): A weekly email newsletter covering the latest happenings in the AWS ecosystem.  
	- [AWS Geek](https://www.awsgeek.com): A blog by AWS Community Hero Jerry Hargrove, with notes and hand-drawn diagrams about various AWS services.  
- General references  
	- [AWS Well Architected Framework Guide](https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf): Amazon’s own 56 page guide to operational excellence - guidelines and checklists to validate baseline security, reliability, performance (including high availability) and cost optimization practices.  
	- [Awesome Microservices](https://github.com/mfornos/awesome-microservices): A curated list of tools and technologies for microservice architectures. Worth browsing to learn about popular open source projects.  
	- [Is it fast yet?](https://istlsfastyet.com/): Ilya Grigorik’s TLS performance overview  
	- [High Performance Browser Networking](https://hpbn.co/): A full, modern book on web network performance; a presentation on the HTTP/2 portion is [here](https://docs.google.com/presentation/d/1r7QXGYOLCh4fcUq0jDdDwKJWNqWK1o4xMtYpKZCJYjM/edit?usp=sharing).  
	- [Amazon Web Services 시작하기 | AWS](https://aws.amazon.com/ko/getting-started/?sc_icontent=awssm-evergreen-getting_started&sc_iplace=2up&trk=ha_awssm-evergreen-getting_started&sc_ichannel=ha&sc_icampaign=evergreen-getting_started)  
  
# Getting Help and Support  
  
- **Forums:** For many problems, it’s worth searching or asking for help in the [discussion forums](https://forums.aws.amazon.com/index.jspa) to see if it’s a known issue.   
- **Contact:** The main web contact point for AWS is [here](https://aws.amazon.com/contact-us/). Many technical requests can be made via these channels.  
- **AWS Professional Services:** AWS provides [consulting services](https://aws.amazon.com/professional-services/) alone or in combination with partners.  
  
  
  
# 참고 문서  
> [!info]- 참고 문서  
> - [Understanding Modern Cloud Architecture on AWS: A Concepts Series](https://start.jcolemorrison.com/understanding-modern-cloud-architecture-on-aws-a-concepts-series/)  
> - [GitHub - open-guides/og-aws: 📙 Amazon Web Services — a practical guide](https://github.com/open-guides/og-aws#general-information)  
> - [GitHub - keenanromain/AWS-SAA-C02-Study-Guide: How to become a certified AWS Solutions Architect](https://github.com/keenanromain/AWS-SAA-C02-Study-Guide)  
> - [Amazon ECS Workshop :: Amazon ECS Workshop](https://ecsworkshop.com/)  
> - [AWS ECS 살펴보기 | 비브로스 기술 블로그](https://boostbrothers.github.io/technology/2020/01/29/AWS-ECS-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0/#:~:text=Cluster%E2%80%8B,%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%EB%9D%BC%EA%B3%A0%20%ED%95%A0%20%EC%88%98%20%EC%9E%88%EC%8A%B5%EB%8B%88%EB%8B%A4.)  
> - [Understanding Modern Cloud Architecture on AWS: Docker and Containers](https://start.jcolemorrison.com/understanding-modern-cloud-architecture-on-aws-docker-and-containers/)  
> - [[AWS] ECS란? – 교보정보통신 기술 블로그](https://blog.kico.co.kr/2022/03/25/aws-ecs%EB%9E%80/)  
> - [[소개] Amazon ECS란? | NDS Cloud Tech Blog](https://tech.cloud.nongshim.co.kr/2021/08/30/%EC%86%8C%EA%B0%9C-amazon-ecs%EB%9E%80/)  
> - [[AWS ECS] ECS(Elastic Container Service) 란?](https://wooono.tistory.com/133)  
> - [How to Host your App on the Cloud with AWS Using a Multi-Cloud Strategy](https://jfrog.com/blog/5-steps-to-hosting-your-application-on-amazon-cloud-container-service/)  
> - [An AWS Cloud architecture for web hosting - Web Application Hosting in the AWS Cloud](https://docs.aws.amazon.com/whitepapers/latest/web-application-hosting-best-practices/an-aws-cloud-architecture-for-web-hosting.html)  
  
  
이후 추가 보완할 문서  
  
[신입 웹 개발자일 때 알았더라면 좋았을 기본 웹 아키텍처 개념 - Less is more](https://kschoi.github.io/cs/the-basic-archtecture-concepts/)  
  
  
  
EC2 - Lambda 차이  
비용  
컴퓨팅 자원으로만 본다면 람다는 EC2에 비해 월등히 비싸고 온디맨드로 한다면 차이는 배 이상으로 발생한다. 그런데도 Lambda는 비용을 줄일 수 있다. 우리가 일반적으로 서비스를 운영할 때 CPU 사용률이나 메모리 사용량을 약 70%로 설정하고 넘어갈 경우 서버가 스케일링 되도록 구성한다. 그리고 다시 70~80% 이하로 유지하도록 설정한다. 하지만 Lambda를 사용한다면 항상 100%의사용량을 달성할 수 있고 사용을 하지 않는 동안에는 유휴상태로 금액을 지불하지 않기때문에 클라우드의 가장 큰 장점인 내가쓴만큼만 비용을 지불할 수 있다.  
  
람다는 최대 5분까지 사용할 수 있고, 메모리 사용량 또한 제한이 있다. 이 때문에 디비백업, 영상 인코딩 같이 많은 컴퓨팅용량이 단시간내에 많이 필요할경우 상당히 비효율적이다. 그렇다고 EC2 인스턴스를 사용하기도 애매할때가 있다.  
  
이럴때는 ECS Fargate를 사용하면 경제적으로 사용할 수 있다. ECS Fargate는 사용자가 만든 도커이미지를 별도의 EC2 인스턴스나 기타설정없이 실행하고 실행된 시간만큼만 금액을 지불한다. 동영상 구간별 썸네일 추출같이 단시간 많은량의 메모리가 필요한 경우 아주 효율적으로 사용할 수 있다.  
  
  
warm / cold start  
람다에는 Warm Start와 Cold Start가 있어 미리 예열된 상태라면 이전보다 훨씬 빠른속도로 함수들을 처리한다. Warm / Cold를 우리말로 바꾸자면 람다가 바로 준비 된 상태 / 람다가 바로 준비되지 않은 상태가 제일 적절할 것 같다.  
  
람다는 처음 호출한다면 Cold Start를 한다. 이 경우 초기 지연시간이 발생한다. 그리고 Warm 상태를 유지하며, 이 경우에는 람다를 다시 호출해도 지연시간이 발생하지 않고 빠르게 응답한다. 그러다 약 5분여 이상 호출이 되지 않는다면 Cold상태로 바뀌고 다시 호출될 경우 지연시간을 가지고 Cold Start를 한다.  
  
  
[프로덕션을 위한 AWS Lambda 최적화 팁 4가지 - Dashbird](https://dashbird.io/blog/optimizing-aws-lambda-for-production-kr/)