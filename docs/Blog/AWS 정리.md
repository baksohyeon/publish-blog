---  
category: Blog  
share: true  
---  
  
# 참고 문서  
> [!info]- 참고 문서  
> - [Understanding Modern Cloud Architecture on AWS: A Concepts Series](https://start.jcolemorrison.com/understanding-modern-cloud-architecture-on-aws-a-concepts-series/)  
> - [GitHub - open-guides/og-aws: 📙 Amazon Web Services — a practical guide](https://github.com/open-guides/og-aws#general-information)  
> - [GitHub - keenanromain/AWS-SAA-C02-Study-Guide: How to become a certified AWS Solutions Architect](https://github.com/keenanromain/AWS-SAA-C02-Study-Guide)  
> - [Amazon ECS Workshop :: Amazon ECS Workshop](https://ecsworkshop.com/)  
> - [AWS ECS 살펴보기 | 비브로스 기술 블로그](https://boostbrothers.github.io/technology/2020/01/29/AWS-ECS-%EC%82%B4%ED%8E%B4%EB%B3%B4%EA%B8%B0/#:~:text=Cluster%E2%80%8B,%ED%81%B4%EB%9F%AC%EC%8A%A4%ED%84%B0%EB%9D%BC%EA%B3%A0%20%ED%95%A0%20%EC%88%98%20%EC%9E%88%EC%8A%B5%EB%8B%88%EB%8B%A4.)  
> - [Understanding Modern Cloud Architecture on AWS: Docker and Containers](https://start.jcolemorrison.com/understanding-modern-cloud-architecture-on-aws-docker-and-containers/)  
> - [[AWS] ECS란? – 교보정보통신 기술 블로그](https://blog.kico.co.kr/2022/03/25/aws-ecs%EB%9E%80/)  
  
  
# When to Use AWS  
  
[AWS](https://en.wikipedia.org/wiki/Amazon_Web_Services) is the dominant public cloud computing provider.  
  
## AWS의 핵심 기능  
1. [infrastructure-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Infrastructure_as_a_service_.28IaaS.29) (IaaS), 가상 머신과 인프라 지원  
2. [platform-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Platform_as_a_service_.28PaaS.29) (PaaS): 고객의 애플리케이션을 배포하는 완전 관리형 서비스  
3. [software-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Software_as_a_service_.28SaaS.29) (SaaS): 클라우드 기반 애플리케이션  
  
  
## 웹 어플리케이션 아키텍처  
  
![Pasted image 20230226211205.png](../attachments/Pasted%20image%2020230226211205.png)  
  
  
- 웹 어플리케이션: 서버와 데이터베이스, 정적 스토리지 관리  
애플리케이션 코드에 새로운 업데이트를 푸시하는 개발자의 흐름, 클라이언트가 해당 애플리케이션에 액세스하는 방법, 백그라운드에서 일어나는 일을 다이어그램으로 보여주는 전체 아키텍처입니다.   
  
  
  
- **AWS vs. other cloud providers:**   
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
  
  
  
  
  
  
  
  
# Common Concepts  
📒  AWS [**General Reference**](https://docs.aws.amazon.com/general/latest/gr/Welcome.html)   
  
![Pasted image 20230226173716.png](../attachments/Pasted%20image%2020230226173716.png)  
  
- AWS에서는 지연 시간을 줄이거나 추가적인 이중화를 제공하는 데 도움이 되는 격리된 지리적 위치인  [**regions**](https://docs.aws.amazon.com/general/latest/gr/rande.html) 배포를 허용합니다.  
  AZ는 같은 리전 내에서도 [물리적으로 서로 분리](https://www.youtube.com/watch?v=JIQETrFC_SQ&feature=youtu.be&t=1428)되어 있으며, 여러 물리적 데이터 센터에 걸쳐 있을 수 있습니다. 지연 시간이 짧은 링크를 통해 연결되어 있지만, 한 곳에 자연 재해가 발생해도 다른 곳에 영향을 미치지 않습니다.  
    
- 각 서비스에는 각 지역에 대한 API 엔드포인트가 있습니다. 엔드포인트는 서비스마다 다르며  [해당 tables](https://docs.aws.amazon.com/general/latest/gr/rande.html)에 나열된 것처럼 각 지역에서 모든 서비스를 사용할 수 있는 것은 아닙니다.  
- [**Amazon Resource Names (ARNs)**](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) 은 리소스를 식별하기 위한 특수 형식의 식별자입니다. `arn:`로 시작하며 특히 IAM 정책에 사용됩니다.  
  
  
  
## 제한 및 기타 참고 사항  
  
- 아마존의 많은 리소스에는 [**한도**](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html)가 설정되어 있습니다.   
- 이는 실제로 도움이 되므로 실수로 큰 비용이 발생하지 않습니다. 지원 티켓을 열어 할당량을 늘리도록 요청해야 합니다. 어떤 한도는 쉽게 올릴 수 있지만 어떤 한도는 그렇지 않습니다. 또한 모든 서비스 한도가 공개되는 것은 아닙니다.  
	- 현재 한도 및 사용량 확인하기: **서비스에 대한 한도 정보는 서비스 API, 트러스티드 어드바이저, 둘 다 또는 둘 다에서 제공되지 않을 수 있습니다(이 경우 지원팀에 문의해야 함).   
	- awslimitchecker 도구 설명서의 [이 페이지](http://awslimitchecker.readthedocs.io/en/latest/limits.html)에 각 한도에 대해 사용 가능한 검색 옵션에 대한 요약이 나와 있습니다.   
	- [도구](https://github.com/jantman/awslimitchecker) 자체도 한도 확인을 자동화하는 데 유용합니다.  
  
  
# Managing AWS  
  
  
## Managing Infrastructure State and Change  
  
AWS를 사용하여 복잡한 시스템을 구축할 때(그리고 일반적으로 데브옵스를 사용할 때) 가장 큰 과제는 시간이 지남에 따라 인프라 상태를 효과적으로 관리하는 것입니다. 일반적으로 인프라 상태에 대한 목표는 크게 세 가지로 요약됩니다:  
  
- **Visibility**  
	1. 인프라의 상태(사용 중인 서비스와 정확한 방법)를 알기  
	2. 또한 여러분과 팀원 중 누가 언제 변경을 하는지 알고 있나요?   
	3. 서비스의 잘못된 구성, 문제, incidents를 감지할 수 있나요?  
- **Automation**  
	1. 많은 추가 수작업이나 누군가의 머릿속에만 있는 지식 없이도 과거 구성을 재현하거나 기존 구성을 확장하기 위해 인프라를 재구성할 수 있나요?   
	2. 사고에 쉽게 또는 자동으로 대응할 수 있나요?  
- **Flexibility**  
	1. 큰 노력 없이 구성을 개선하고 새로운 방식으로 확장할 수 있나요?   
	2. 동일한 도구를 사용하여 더 많은 복잡성을 추가할 수 있나요?   
	3. 팀 내에서 구성을 공유, 검토 및 개선하고 있나요?  
  
  
콘솔부터 복잡한 자동화 도구, 타사 서비스까지, 가시성, 자동화, 유연성을 달성하는 데 도움이 되는 여러 가지 접근 방식으로 AWS를 통해 인프라를 배포할 수 있습니다.  
  
  
  
## AWS 디버깅  
  
대부분의 사람들이 AWS를 실험하는 첫 번째 방법은 웹 인터페이스인 AWS 콘솔을 이용하는 것입니다. 그러나 콘솔을 사용하는 것은 매우 수동적인 프로세스이며 자동화 또는 유연성에 반하는 경우가 많습니다.  
  
그렇다면 AWS 구성을 수동으로 관리하지 않으려면 어떻게 해야 할까요? 안타깝게도 간단하고 보편적인 정답은 없습니다. 각 접근 방식에는 장단점이 있으며, 회사마다 API를 직접 사용하거나 직접 도구를 구축하는 방법, 명령줄 도구를 사용하는 방법, 타사 도구 및 서비스를 사용하는 방법 등 매우 다양한 접근 방식을 취하고 있습니다.  
### AWS Console  
  
[AWS Console](https://aws.amazon.com/console/)을 사용하면 웹 인터페이스를 통해 AWS의 많은 기능(전부는 아님)을 제어할 수 있습니다.  
이상적으로는 몇 가지 특정 상황에서만 AWS 콘솔을 사용하는 것이 좋습니다  
1. 읽기 전용으로 사용하기에 좋습니다. 시스템 상태를 이해하려는 경우 로그인하여 탐색하는 것이 매우 유용합니다.  
2. 매우 작은 시스템과 팀(예: 자주 변경하지 않는 서버를 한 명의 엔지니어가 설정하는 경우)에서도 합리적으로 사용할 수 있습니다.  
3. 한 달에 한 번 미만으로 거의 하지 않는 작업(예: 1년 동안 다시 방문하지 않을 일회성 VPC 설정)에도 유용할 수 있습니다. 이 경우 콘솔을 사용하는 것이 가장 간단한 방법이 될 수 있습니다.  
  
> [!info] 콘솔을 사용하기 전에 생각하기  
> AWS 콘솔은 편리하지만 자동화, 재현성 및 팀 커뮤니케이션의 적이기도 합니다. 동일한 변경을 여러 번 수행할 가능성이 높다면 콘솔을 사용하지 마세요.   
> 다음에 설명하는 것처럼 자동화를 선호하거나 최소한 자동화를 향한 경로를 마련하세요.   
> 콘솔을 사용하면 자동화를 배제하여 나중에 시간을 낭비할 뿐만 아니라 자신과 팀을 위한 문서화, 명확성 및 프로세스에 대한 표준화도 방해합니다.  
  
### Command-Line tools  
  
  
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
  
  
### General Visibility  
  
- [Tagging resources](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html)은 특히 조직이 성장함에 따라 리소스 사용량을 더 잘 파악하기 위해 필수적인 관행입니다. 예를 들어 자동화 또는 규칙을 통해 태그를 추가할 수 있습니다:  
	- 해당 리소스를 '소유'하는 조직 또는 개발자의 경우  
	- 리소스가 지원하는 제품용  
	- 임시 리소스 또는 향후 프로비저닝을 해제해야 하는 리소스와 같은 수명 주기에 레이블 지정  
	- 프로덕션에 중요한 인프라(예: 서비스 시스템과 백엔드 파이프라인)를 구분하기 위해  
	- 특별한 보안 또는 규정 준수 요구사항이 있는 리소스를 구분하기 위해  
	- (활성화된 경우) [비용 할당](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html)으로. 비용 할당 태그는 미래 기준으로만 적용되며, 이미 청구된 항목에는 소급하여 적용할 수 없다는 점에 유의하세요.  
	- 수년 동안 리소스당 10개라는 악명 높은 태그 제한이 있었는데, 이 제한을 늘릴 수 없어 많은 회사가 큰 어려움을 겪었습니다. 2016년부터는 리소스당 태그 수가 50개로 [상향](https://aws.amazon.com/blogs/security/now-organize-your-aws-resources-by-using-up-to-50-tags-per-resource/)되었습니다.  
	- 2017년에 AWS는 인스턴스 및 볼륨 생성 시 [태그 지정](https://aws.amazon.com/blogs/aws/new-tag-ec2-instances-ebs-volumes-on-creation/) 기능을 도입하여 [Cloud Custodian](https://github.com/capitalone/cloud-custodian)과 같은 타사 도구의 일부를 더 이상 사용하지 않게 되었습니다.  
	- 태그는 대소문자를 구분하며, '환경'과 '환경'은 서로 다른 두 개의 태그입니다. 상당한 규모에서는 태그 설정 자동화가 유일한 합리적인 옵션일 가능성이 높습니다.  
	- ASG 콘솔에 태그 이름 뒤에 공백이 유지되는 버그가 있습니다. 따라서 끝에 공백이 있는 "이름"을 입력하면 예상한 동작이 나타나지 않습니다. 이는 다른 위치 및 SDK에서도 마찬가지일 수 있습니다. 정말 의도하지 않는 한 태그 키에 후행 공백을 추가하지 않도록 하세요. (2018년 7월 기준)  
	- 조직 전체에서 리소스를 공유할 때는 태그가 공유되지 않습니다. 예를 들어 대중교통 게이트웨이 또는 AMI를 공유하면 해당 리소스를 만든 계정에는 올바른 태그가 표시되지만 해당 리소스가 공유된 계정에는 태그가 표시되지 않습니다.  
  
  
  
## Managing Servers and Applications  
---  
  
이 가이드는 DevOps나 서버 구성 관리 일반이 아닌 AWS에 관한 것입니다. 하지만 AWS에 대해 자세히 알아보기 전에 AWS 리소스에 대한 구성 관리 외에도 서버 자체에 대한 구성 관리라는 오랜 문제가 있다는 점에 주목할 필요가 있습니다.  
  
### AWS vs Server Configuration  
  
This guide is about AWS, not DevOps or server configuration management in general. But before getting into AWS in detail, it’s worth noting that in addition to the configuration management for your AWS resources, there is the long-standing problem of configuration management for servers themselves.  
  
  
### Philosophy  
  
- 헤로쿠의 [**Twelve-Factor App**](http://12factor.net/) 원칙에는 애플리케이션 배포를 위한 몇 가지 확립된 일반적인 모범 사례가 나열되어 있습니다.  
- 시스템 설계: 서버를 [애완동물이 아닌 가축처럼](https://www.engineyard.com/blog/pets-vs-cattle) 다루세요. 즉, 인프라가 일회용이 되도록 시스템을 설계하세요. 서버가 예기치 않게 파괴되더라도 영향을 최소화 해야합니다.  
- [immutable infrastructure](http://radar.oreilly.com/2015/06/an-introduction-to-immutable-infrastructure.html)라는 개념은 이 개념의 연장선상에 있습니다.  
- EC2 인스턴스에서 애플리케이션 상태를 최소화합니다. 일반적으로 인스턴스는 최소한의 충격으로 예기치 않게 종료되거나 소멸될 수 있어야 합니다. 애플리케이션에 있는 상태는 해당 인스턴스가 아닌 RDS, S3, DynamoDB, EFS 또는 기타 데이터 저장소로 신속하게 이동해야 합니다. EBS도 옵션이지만, 일반적으로 부팅 가능한 볼륨이 되어서는 안 되며 EBS는 수동 또는 자동 재마운팅이 필요합니다.  
  
  
  
### Tips for Managing Servers  
  
- **서버의 표준 시간대 설정**: 절대적으로 필요한 경우가 아니라면 항상 서버의 표준 시간대를 [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)로 설정하세요([우분투](https://www.digitalocean.com/community/tutorials/how-to-set-up-timezone-and-ntp-synchronization-on-ubuntu-14-04-quickstart), [CentOS](https://www.vultr.com/docs/setup-timezone-and-ntp-on-centos-6) 또는 [Amazon](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html) Linux 등 사용 중인 배포판의 지침을 참조).   
	- 수많은 분산 시스템은 동기화 및 조정을 위해 시간에 의존하며, UTC는 [제공](https://blog.serverdensity.com/set-your-server-timezone-to-utc/) 보편적인 기준면으로 서머타임 변경 및 현지 시간 조정의 영향을 받지 않습니다.   
	- 또한 [파악하기 어려운 시간대 문제](http://yellerapp.com/posts/2015-01-12-the-worst-server-setup-you-can-make.html)를 디버깅하는 데 따르는 많은 골칫거리를 덜어주고 로깅 및 감사 시스템에서 일관된 이벤트 타임라인을 제공합니다.  
- **NTP and accurate time:**: 사전 구성된 Amazon Linux를 사용하지 않는 경우, 교활한 시간 드리프트(API 호출 중단부터 잘못된 로그에 이르기까지 모든 종류의 문제를 일으킬 수 있음)를 방지하기 위해 서버를 [올바르게 NTP 구성](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html#configure_ntp)하여 확인해야 합니다. 이 설정은 모든 서버에 대한 자동 구성의 일부가 되어야 합니다. 시간이 이미 상당히 많이 흘러간 경우(일반적으로 1000초 이상) NTP로 시간을 되돌릴 수 없으므로 수동으로 해결해야 할 수도 있습니다(예: Ubuntu에서 [이와 같이](http://askubuntu.com/questions/254826/how-to-force-a-clock-update-using-ntp)).  
- **Testing immutable infrastructure:**: 인스턴스 종료 또는 장애에 대한 서비스의 대처 능력을 사전에 테스트하려면 업무 시간 중에 무작위 인스턴스 종료를 도입하여 엔지니어가 문제를 식별하고 수정할 수 있는 시간에 해당 문제를 노출하는 것이 도움이 될 수 있습니다. 넷플릭스의 [Simian Army](https://github.com/Netflix/SimianArmy)(특히, [카오스 몽키](https://github.com/Netflix/SimianArmy/wiki/Chaos-Monkey)는 이를 위한 인기 있는 도구입니다. 또는 BBC의 [chaos-lambda](https://github.com/bbc/chaos-lambda)는 AWS [Lambda](AWS%20정리.md#lambda)에서 실행되는 경량 옵션입니다.  
  
  
  
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
	`SES`: Send and receive e-mail for marketing or transactions 문자 인증할 때 쓰는건데 이름 기능만   
	 `API` Gateway: Proxy, manage, and secure API calls 람다랑 연결되는 기능  
	`Lambda`: Running small, fully managed tasks “serverless” (뭔 기능인지만)  
# Security and IAM  
추가 참고 [AWS IAM Policies in a Nutshell](https://start.jcolemorrison.com/aws-iam-policies-in-a-nutshell/)  
We cover security basics first, since configuring user accounts is something you usually have to do early on when setting up your system.  
  
## Security and IAM Basics  
  
![Pasted image 20230227101646.png](../attachments/Pasted%20image%2020230227101646.png)  
  
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
  
EC2 인스턴스 또는 모든 클라우드 기반 서버를 가장 간단하게 생각하는 방법은 다음과 같습니다:  
여전히 컴퓨터일 뿐입니다. 다른 사람의 컴퓨터일 뿐이죠. (이 경우에는 AWS의 컴퓨터입니다.)  
  
다른 컴퓨터와 마찬가지로 로그인하고, 설정하고, 원하는 모든 작업을 수행할 수 있습니다.   
EC2 인스턴스(서버라고도 함)를 만들고 내 컴퓨터에서와 마찬가지로 애플리케이션을 설정합니다.   
  
  
  
이 서버가 가동되면 애플리케이션을 서버에 올려놓을 수 있습니다.  
- [EC2 가격](https://aws.amazon.com/ec2/pricing/)** 및 [비용 관리](AWS%20정리.md#ec2-cost-management)  
	- 사용량에 따라 무료([AWS 무료 티어](https://aws.amazon.com/free/)에서)부터 매우 비싼 가격까지 다양합니다.   
	- 가격은 인스턴스 유형, 초 또는 시간 단위로 책정되며, AWS 지역과 인스턴스를  [On-Demand](https://aws.amazon.com/ec2/pricing/on-demand/), [Spot market](https://aws.amazon.com/ec2/spot/) 또는 사전 구매([Reserved Instances](https://aws.amazon.com/ec2/pricing/reserved-instances/)로 구매하는지 여부에 따라 달라집니다.)  
  
  
  
**Servers on AWS: EC2 and its Instances**  
  
  
  
  
  
  
  
   
  
# ECS: EC2 Container Service  
---  
  
  
![Pasted image 20230227112853.png](../attachments/Pasted%20image%2020230227112853.png)  
ECS의 워크 플로우를 살펴보면 위의 사진과 같습니다.  
  
Container registry에 저장된 도커 이미지를 가져옵니다. 그 후 ECS에서 이미지를 실행하여 컨테이너를 생성하는데 그때, 리소스와 Compute Option을 설정합니다. 이는 도커 이미지를 run 할 때 명령어로 옵션을 설정하는 것과 같습니다.  
  
ECS는 이렇게 실행한 컨테이너들과 그 집합인 클러스터를 관리하는 역할을 합니다.  
  
이때 사용하는 컨테이너 저장소는 Docker hub를 써도 연결이 가능하지만 Amazon ECR은 AWS IAM 인증을 통해 이미지 push/pull에 대한 권한 관리가 가능하여 보안상 좋고, S3에 저장되기 때문에 고가용성 유지되며 다른 AWS 서비스들과 편리하게 연결 가능하다는 장점이 있습니다.  
## ECS Basics: Docker를 통해 배포된 서비스 클러스터를 관리  
  
- 📒 [Homepage](https://aws.amazon.com/ecs/) ∙ [Developer guide](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/) ∙ [FAQ](https://aws.amazon.com/ecs/faqs/) ∙ [Pricing](https://aws.amazon.com/ecs/pricing/)  
- ECS 란 컨테이너의 생성과 종료, 자동 배치 및 복제, 로드 밸런싱, 클러스터링, 장애 복구, 스케줄링 등의 역할을 수행해 주는 오케스트레이선 도구입니다.  
- 웹사이트를 실행할 수 있도록 Docker파일을 EC2 인스턴스에 넣습니다.  
- [ECR](https://aws.amazon.com/ecr/)(EC2 컨테이너 레지스트리)은 Amazon의 관리형 Docker 레지스트리 서비스입니다. (도커 파일 저장)  
-   
  
ECS에서는 컨테이너를 올리는 서버, 컨테이너를 어디에 호스팅 할 건지 정할 수 있는데 서버리스 방식인 Fargate 위에 올릴 수도 있고, EC2에 올릴 수도 있습니다.  
  
서버리스 방식인 Fargate는 아마존에서 많은 부분을 관리해 주어 유연성은 좀 떨어지지만, AWS에 많은 부분을 맡길 수 있는 AWS 매니지드 서비스이므로 운영하기에 매우 편리합니다.   
따라서 AWS나 도커 운영에 대해 잘 모르는 사용자도 사용하기 편리합니다.   
  
EC2는 사용자가 커스터마이징 할 수 있는 부분이 많고 비용도 저렴하나 관리에 손이 많이 가기 때문에 어느 정도 컨테이너와 AWS를 다룰 수있는 사용자가 사용하여야 합니다.   
  
  
  
  
## The core components of ECS  
  
![Pasted image 20230227112310.png](../attachments/Pasted%20image%2020230227112310.png)  
  
- `Container`: Docker Image라는 읽기 전용 탬플릿을 통하여 생성된 것입니다.  
- `Task`: Task Definition에 의해 배포된 컨테이너 Set입니다.  
- `Task Definition`: 컨테이너를 실행하기 위해 정의한 설정입니다.  
- `Cluster`: Task가 배포되는 환경들이 논리적으로 그룹화되는 단위를 의미합니다.  
- `Service`: 클러스터에 Task를 몇 개나 배포할 것인지 결정하고 ELB와 연결하거나 Auto Scaling을 설정하는 등 Task의 LifeCycle을 관리합니다.  
- `Container Instances`: ECS를 통해서 Task가 배포되는 EC2 인스턴스를 Container Instance라고 합니다.  
- `Container Agent`  
  
## Docker & Container  
  
  
![Pasted image 20230227112610.png](../attachments/Pasted%20image%2020230227112610.png)  
  
  
1. <font style="color:indianred">Dockerfile</font>은 컨테이너에 설치해야하는 패키지, 소스코드, 명령어, 환경변수설정 등을 기록한 스크립트 파일입니다.  
2. 이렇게 만든 Dockerfile을 build 하면 <font style="color:indianred">도커 이미지</font>가 됩니다.   
3. 도커 이미지를 Run 하면 이미지가 실행되어 하나의 애플리케이션으로 서비스를 하는 <font style="color:indianred">컨테이너</font>(미니 컴퓨터)가 생성됩니다.   
  
  
  
## Container & Image  
  
  
![Pasted image 20230227105649.png](../attachments/Pasted%20image%2020230227105649.png)  
컨테이너는 Docker Image라는 읽기 전용 탬플릿을 통하여 생성되는것을 컨테이너라고 하며, 이미지는 컨테이너에 포함되는 모든 구성 요소(runtime base Image, 어플리케이션 소스, 라이브러리 등)을 지정해주는 일반 텍스트 파일인 Docker file을 통하여 빌드 생성합니다.   
Docker를 사용하면 이제 더 이상 필요한 런타임 환경 구축을 직접 세팅, 어플리케이션 구동에 필요한 라이브러리 설치 및 기타 세팅을 매번 할 필요가 없으며 특정 PC 마다 런타임 환경이나 설치된 라이브러리 버전이 달라 문제가 생기는 것에서 해방될 수 있습니다.  
  
  
  
### Cluster  
  
ECS의 클러스터는 도커 컨테이너를 실행할 수 있는 논리적인 공간이라고 볼 수 있는데요, 우리가 사용하는 도커 컨테이너는 도커가 설치된 컨테이너 인스턴스에서 컨테이너가 실행되며 이 인스턴스들을 목적에 따라 하나로 묶어주는 것이 바로 클러스터라고 할 수 있습니다.  
  
ECS에서는 이렇게 논리적으로 그룹화된 컨테이너 인스턴스들에게 특정 컨테이너를 띄우고, 증가 시키고 등의 명령을 내려 오케스트레이션을 행할수 있게 되는것이죠. 그리고 논리적인 공간이다 보니 빈 클러스터 즉 컨테이너 인스턴스가 없는 클러스터도 생성이 가능합니다.  
  
물론 컨테이너 인스턴스를 클러스트에 연결 시키고 명령을 전달 즉 오케스트레이션을 하기 위해서는 AWS에서 제공하는 ECS Agent가 컨테이너 인스턴스(EC2)에 설치가 되어있어야 하며, ECS Agent를 통해서 ECS 서비스에서의 오케스트레이션 명령 수행이 가능하게 됩니다.  
  
클러스터 생성 시 빈 클러스터가 아닌 컨테이너 인스턴스 타입과 수를 지정하여 생성한 경우, 기본적으로 ECS Agent가 설치된 AMI로 EC2가 자동 생성되어 클러스터와 연결됩니다.  
  
다시 한번 간단하게 요약 정리해본다면  
  
1. 클러스터는 컨테이너 인스턴스(EC2)의 논리적인 그룹화 이다.  
2. 논리적인 공간이므로, 컨테이너 인스턴스가 없는 빈 클러스터도 생성이 가능하다.  
3. ECS Agent를 통해 논리적인 클러스터에 연결된다.  
  
  
  
이미지는 컨테이너 실행에 필요한 파일과 설정값등을 포함하고 있는 것으로 상태값을 가지지 않고 변하지 않습니다(Immutable). 컨테이너는 이미지를 실행한 상태라고 볼 수 있고 추가되거나 변하는 값은 컨테이너에 저장됩니다. 같은 이미지에서 여러개의 컨테이너를 생성할 수 있고 컨테이너의 상태가 바뀌거나 컨테이너가 삭제되더라도 이미지는 변하지 않고 그대로 남아있습니다.  
  
말그대로 이미지는 컨테이너를 실행하기 위한 모오오오오든 정보를 가지고 있기 때문에 더 이상 의존성 파일을 컴파일하고 이것저것 설치할 필요가 없습니다. 이제 새로운 서버가 추가되면 미리 만들어 놓은 이미지를 다운받고 컨테이너를 생성만 하면 됩니다. 한 서버에 여러개의 컨테이너를 실행할 수 있고, 수십, 수백, 수천대의 서버도 문제없습니다.  
  
컨테이너를 만들면 컨테이너에는 Docker 이미지에서 정의한 모든 항목이 포함될 것입니다. 예를 들어, 이 Docker 이미지로 컨테이너 3개를 만들면 모두 Ubuntu, Node.js, 애플리케이션 코드의 개별 인스턴스를 갖게 됩니다.  
  
이 방법의 가장 좋은 점은 완전히 분리되어 격리되어 있다는 것입니다. 서로 영향을 미치지 않으므로 컨테이너를 생성하면 하나의 머신에서 여러 가지를 호스팅할 수 있는 매우 깔끔한 방법입니다. 예시는 조금 단순하지만, 하나의 머신에 Node, Python, Rails의 세 가지 애플리케이션이 필요하다고 가정해 보겠습니다. 그리고 모두 웹 서비스라고 가정해 봅시다.  
  
Node 컨테이너는 메인 애플리케이션이고, Python 애플리케이션은 관리 애플리케이션이며, Rails 애플리케이션은 레거시 애플리케이션일 수도 있습니다. 컨테이너를 사용하면 이 세 가지가 서로의 발가락을 밟지 않고도 같은 머신에서 쉽게 공존할 수 있습니다. 이 세 가지 모두 완전히 다른 운영 체제와 종속성을 가질 수 있지만 (그리고 여기에 Docker의 놀라운 이점이 있습니다.) 이 호스트 머신에 필요한 것은 오직 Docker뿐입니다!  
  
맞습니다. 다른 버전의 종속성 설치에 대해 걱정할 필요가 없으며 호스트의 운영 체제 버전에 대해서도 걱정할 필요가 없습니다. 호스트에 Docker가 설치되어 있는지 확인하기만 하면 됩니다. 그게 다입니다.  
  
이제 이 컨테이너를 Windows 머신에서 호스팅하거나 Linux 머신으로 옮기고 싶으신가요? 문제 없습니다! Docker가 설치되어 있는지 확인하기만 하면 됩니다. 호스트를 업데이트하고 싶으신가요? 컨테이너는 어쨌든 컨테이너에 포함되어 있으므로 영향을 받지 않습니다.  
  
  
컴퓨터 클러스터란 ‘여러 대의 컴퓨터들이 연결되어 하나의 시스템처럼 동작하는 컴퓨터들의 집합’을 말한다. 즉 ECS의 클러스터는 Task를 배포하기 위한 인스턴스의 집합을 말한다.  
  
  
  
### Task Definitions & Task  
![Pasted image 20230227105831.png](../attachments/Pasted%20image%2020230227105831.png)  
  
  
작업(Task)는 컨테이너를 실행하는 최소 단위 입니다. 그렇다고 컨테이너 1개 = Task 1개를 의미하지는 않으며, 최소 1개 이상의 컨테이너로 구성될 수 있으며, 해당 Task 내의 컨테이너는 모두 같은 ECS 클러스터 인스턴스 내에 실행되도록 보장 받습니다. 여러 컨테이너를 하나로 묶어서 실행한다고 생각하면 되며, docker-compose와 비슷한 개념이라고 보시면 됩니다.  
  
작업 정의(Task Definitions)은 위에서 말한 Task를 정의한 작업 정의 입니다. Task 실행 시 Task Role, Docker Netwrok Mode, Docker 실행 명령, CPU/Memory 제한, Logging Driver, Vloum Mount 등의 옵션 설정을 할 수 있으며, 생성 또는 업데이트된 Task Definitions는 모두 Revision Version 이 기록되어 저장 되므로, 특정 Revision Version으로 Task를 Roll back 등을 하고자 할 때 유용합니다.  
  
Task는 Task definition과 Task 두 가지가 있다.  
  
Task definition은 컨테이너를 실행하기 위해 설정한 내용이다. 컨테이너 이미지, 리소스, 포트 설정들을 한다. docker run 명령과 비슷한 역할을 한다.  
  
   
  
Task는 Task definition에서 정의된 내용을 기반으로 배포된 컨테이너들을 Task라고 한다. Task에는 컨테이너가 하나 있을 수도 있고, 여러 개가 있을 수도 있다.  
  
### Service   
ECS Cluster에서의 최소 실행 단위는 Task라고 말씀드렸는데요, Task를 실행 하는 방식은 직접 Task Definitions를 이용한 실행과 Service를 이용한 실행 두 가지 방식을 제공합니다. 직접 Task Definitions를 실행하는 방식은 Task에 대한 관리가 되지 않으며, AWS에서 제공하는 다른 서비스 (ELB, AutoScale)등을 이용할 수 없으므로, 대부분 사용하지 않습니다.  
  
Service는 Task Definitions를 이용하여 Task 실행 유형(EC2 or Fargate), 타입(복제 or 데몬), 테스크가 실행 되어야할 작업 개수, 배포 방식(롤링 or Blue Green), 배치 전략, AutoScale 설정 및 컨테이너와 ELB(ALB or NLB)등을 설정하여 Task를 실행 및 관리 하도록 도와줍니다. ECS Cluster에 Task를 관리하는 상위 그룹 개념으로 보시면 됩니다.  
![Pasted image 20230227105917.png](../attachments/Pasted%20image%2020230227105917.png)  
  
서비스에는 EC2, Fargate 두가지 Task 실행 유형을 제공합니다. 실행 유형은 ECS Cluster 내에 EC2 인스턴스 타입의 클러스터에 Task를 실행 할 것인지 또는 Fargate를 이용하여 서비스를 실행할지를 선택하는 것이며, 만일 Fargate를 선택하신다면 이전에 Task Defintions 작성 시 호환성 여부 항목에 Fargate가 선택되어 있어야 합니다. 만일 Fargate로 서비스를 구성하신다면, ECS 클러스터내에 인스턴스가 없어도, Task에 정의한 CPU, 메모리 설정에 따라 관리하는 EC2 인스턴스 없이 Serverless 하게 서비스를 실행할 수 있습니다.  
  
서비스에는 타입은 복제(Replica)와 데몬(Daemon) 두 가지를 제공하며, 복제는 서비스에 정의된 작업 개수 및 AutoScale 설정에 따라 클러스터 인스턴스에 Task가 복제하여 실행하는 방식이며, 데몬은 클러스터 내의 모든 ECS 인스턴스에 무조건 하나씩 실행되는 방식입니다.  
Task의 상위의 있는 부분이다. Task를 Cluster에 몇 개나 배포할 것인지 결정한다. 외부의 서비스 하기 위해 로드밸런서를 연동한다. Task가 문제가 생기면 자동으로 새로운 Task를 생성한다.  
  
  
## ECS Alternatives and Lock-in  
  
- [Kubernetes](https://kubernetes.io): Extensive container platform. Available as a hosted solution on [Google Cloud](https://cloud.google.com/kubernetes-engine), [AWS](https://aws.amazon.com/eks/), [Azure](https://azure.microsoft.com/en-us/services/kubernetes-service/), [DigitalOcean](https://www.digitalocean.com/products/kubernetes/), and [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift).  
- [Nomad](https://www.nomadproject.io/): Orchestrator/Scheduler, tightly integrated in the HashiCorp stack (Consul, Vault, etc).  
  
  
  
  
컨테이너를 "미니 컴퓨터"로 생각하기  
컨테이너에는 여기서는 자세히 다루지 않겠지만, 컨테이너를 "미니 컴퓨터"라고 생각하면 좋은 정신적 모델이 될 수 있습니다.   
  
컨테이너가 미니 컴퓨터와 얼마나 비슷한지 이해하려면 다음과 같이 해보세요:  
  
(Docker가 설치되어 있다고 가정)  
  
  
요점은 컨테이너가 VM 보다 훨씬 가볍기 때문에 컨테이너를 사용하여 서버 리소스를 최대한 활용할 수 있다는 것입니다.   
쉽게 설명하자면, 하나의 서버에 3개의 애플리케이션을 설치하고자 한다고 가정해 보겠습니다. 각 애플리케이션에 대해 3개의 개별 가상 머신을 만들면 확실히 작동하지만 일부 가상 머신의 크기가 기가바이트에 달할 수 있으므로 서버의 공간과 전력을 많이 사용하게 됩니다. 반면에 각 애플리케이션에 대해 3개의 컨테이너를 별도로 만들면 컨테이너의 크기가 몇 메가바이트 정도로 작을 수 있으므로 훨씬 더 많은 공간을 확보할 수 있습니다.  
  
  
이제 Docker와 컨테이너가 왜 그렇게 유용한지 알 수 있기를 바랍니다. 이제 호스트 머신 자체에 대한 걱정 없이 서버의 리소스를 최대한 활용할 수 있습니다. 우리가 물어봐야 할 것은 Docker가 있는가? 있다면 설치하면 됩니다.  
  
이제 Docker는 백그라운드에서 우리를 위해 많은 일을 합니다.   
메모리 사용량, CPU, 네트워크 접근성... 모든 종류의 것들을 관리합니다. 덕분에 컨테이너 작업이 고통이 아닌 즐거움이 될 수 있습니다. 컨테이너를 고려한 지 오래되었는데도 여전히 LXC를 떠올리는 분들을 위해 말씀드리자면, 그런 컨테이너는 제한적이고 Docker 기반 컨테이너만큼 작업하기에 편리하지 않습니다.  
  
  
  
"ECS는 Docker 컨테이너를 관리하고 배포합니다."라고 할 수 있습니다. 그게 전부입니다.  
  
물론 다른 수백 가지의 다른 기능도 나열할 수 있지만, 이 모든 기능을 통해 Docker 컨테이너를 관리하고 배포할 수 있습니다.  
  
  
클러스터는 동일한 클러스터를 가리키도록 구성된 EC2 인스턴스 그룹으로, 각 인스턴스에는 동일한 클러스터를 가리키도록 구성된 ECS 컨테이너 에이전트가 있습니다.  
  
  
  
  
  
  
  
  
# S3: Simple Storage Service  
---  
  
### S3 Basics: 정적 파일 저장  
  
- 📒 [Homepage](https://aws.amazon.com/s3/) ∙ [Developer guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html) ∙ [FAQ](https://aws.amazon.com/s3/faqs/) ∙ [Pricing](https://aws.amazon.com/s3/pricing/)  
- **S3** (Simple Storage Service) is AWS’ standard cloud storage service, offering file (opaque “blob”) storage of arbitrary numbers of files of almost any size, from 0 to **5TB**. (Prior to [2011](https://aws.amazon.com/releasenotes/Amazon-S3/1917932037969964) the maximum size was 5 GB; larger sizes are now well supported via [multipart support](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html).)  
- Items, or **objects**, are placed into named **buckets** stored with names which are usually called **keys**. The main content is the **value**.  
- 웹사이트용 이미지 및 기타 자산을 저장합니다. 백업을 보관하고 서비스 간에 파일을 공유합니다. 정적 웹사이트 호스팅. 또한 다른 많은 AWS 서비스도 S3에서 쓰고 읽습니다.  
- Objects are created, deleted, or updated. Large objects can be streamed, but you cannot modify parts of a value; you need to update the whole object. Partial data access can work via [S3 Select](https://aws.amazon.com/blogs/aws/s3-glacier-select/).  
- Every object also has [**metadata**](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html), which includes arbitrary key-value pairs, and is used in a way similar to HTTP headers. Some metadata is system-defined, some are significant when serving HTTP content from buckets or CloudFront, and you can also define arbitrary metadata for your own use.  
- **S3 URIs:** Although often bucket and key names are provided in APIs individually, it’s also common practice to write an S3 location in the form 's3://bucket-name/path/to/key' (where the key here is 'path/to/key'). (You’ll also see 's3n://' and 's3a://' prefixes [in Hadoop systems](https://cwiki.apache.org/confluence/display/HADOOP2/AmazonS3).)  
- **S3 vs Glacier, EBS, and EFS:** AWS offers many storage services, and several besides S3 offer file-type abstractions. [Glacier](AWS%20정리.md#glacier) is for cheaper and infrequently accessed archival storage. [EBS](AWS%20정리.md#ebs), unlike S3, allows random access to file contents via a traditional filesystem, but can only be attached to one EC2 instance at a time. [EFS](AWS%20정리.md#efs) is a network filesystem many instances can connect to, but at higher cost. See the [comparison table](AWS%20정리.md#storage-durability-availability-and-price).  
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
## Load Balancer Tips  
  
- If you don’t have opinions on your load balancing up front, and don’t have complex load balancing needs like application-specific routing of requests, it’s reasonable just to use a CLB or ALB for load balancing instead.  
- Even if you don’t want to think about load balancing at all, because your architecture is so simple (say, just one server), put a load balancer in front of it anyway. This gives you more flexibility when upgrading, since you won’t have to change any DNS settings that will be slow to propagate, and also it lets you do a few things like terminate SSL more easily.  
- **CLBs and ALBs have many IPs:** Internally, an AWS load balancer is simply a collection of individual software load balancers hosted within EC2, with DNS load balancing traffic among them. The pool can contain many IPs, at least one per availability zone, and depending on traffic levels. They also support SSL termination, which is very convenient.  
- **Scaling:** CLBs and ALBs can scale to very high throughput, but scaling up is not instantaneous. If you’re expecting to be hit with a lot of traffic suddenly, it can make sense to load test them so they scale up in advance. You can also [contact Amazon](http://aws.amazon.com/articles/1636185810492479) and have them “pre-warm” the load balancer.  
- **Client IPs:** In general, if servers want to know true client IP addresses, load balancers must forward this information somehow. CLBs add the standard [X-Forwarded-For](https://en.wikipedia.org/wiki/X-Forwarded-For) header. When using a CLB as an HTTP load balancer, it’s possible to get the client’s IP address from this.  
- **Using load balancers when deploying:** One common pattern is to swap instances in the load balancer after spinning up a new stack with your latest version, keep old stack running for one or two hours, and either flip back to old stack in case of problems or tear it down.  
-   **Rotating Certificates while retaining ARN:** Rotating IAM Server Certificates can be difficult as the standard practice is to upload a new one then update all resources with the new ARN. You can however retain the same ARN using the `update-certificate` call with the following process:  
  1. Upload a new IAM Server Certificate with a unique name (e.g fuzzy.com.new)  
  2. Rename the existing IAM Server Certificate (e.g fuzzy.com to fuzzy.com.expired)  
  3. Rename the new IAM Server Certificate to the name of the previously existing certificate (e.g fuzzy.com.new to fuzzy.com)  
  4. Jiggle the CLB/ALB Listener to pick up the change:  
      * ALB: Invoke modify-listener with the existing details for the ALB Listener  
	  * CLB: Invoke create-load-balancer-listeners with the existing details for the CLB listener  
  
  
## Load Balancer Gotchas and Limitations  
  
- ❗CLBs and ALBs have **no fixed external IP** that all clients see. For most consumer apps this doesn’t matter, but enterprise customers of yours may want this. IPs will be different for each user, and will vary unpredictably for a single client over time (within the standard [EC2 IP ranges](http://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html)). And similarly, never resolve a CLB name to an IP and put it as the value of an A record — it will work for a while, then break!  
- ❗Some web clients or reverse proxies cache DNS lookups for a long time, which is problematic for CLBs and ALBs, since they change their IPs. This means after a few minutes, hours, or days, your client will stop working, unless you disable DNS caching. Watch out for [Java’s settings](http://docs.oracle.com/javase/8/docs/api/java/net/InetAddress.html) and be sure to [adjust them properly](http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-jvm-ttl.html). Another example is nginx as a reverse proxy, which [normally resolves backends only at start-up](https://www.jethrocarr.com/2013/11/02/nginx-reverse-proxies-and-dns-resolution/) (although there is [a way to get around this](https://tenzer.dk/nginx-with-dynamic-upstreams/)).  
- ❗It’s not unheard of for IPs to be recycled between customers without a long cool-off period. So as a client, if you cache an IP and are not using SSL (to verify the server), you might get not just errors, but responses from completely different services or companies!  
- As an operator of a service behind a CLB or ALB, the latter phenomenon means you can also see puzzling or erroneous requests by clients of other companies. This is most common with clients using back-end APIs (since web browsers typically cache for a limited period).  
- ❗CLBs and ALBs take time to scale up, it does not handle sudden spikes in traffic well. Therefore, if you anticipate a spike, you need to “pre-warm” the load balancer by gradually sending an increasing amount of traffic.  
- ❗Tune your healthchecks carefully — if you are too aggressive about deciding when to remove an instance and conservative about adding it back into the pool, the service that your load balancer is fronting may become inaccessible for seconds or minutes at a time. Be extra careful about this when an autoscaler is configured to terminate instances that are marked as being unhealthy by a managed load balancer.  
- ❗CLB HTTPS listeners don't support Server Name Indication (SNI). If you need SNI, you can work around this limitation by either providing a certificate with Subject Alternative Names (SANs) or by using TCP listeners and terminating SSL at your backend.  
-  There is a limit on the number of ALBs, CLBs and NLBs per region (separately). As of late 2017, the default limit for each is 20 per region. These limits can be easily raised for ALB and CLB, but AWS is quite reluctant to raise the limit on NLBs.  
-  If using a Network Load Balancer (NLB) then EC2 clients cannot connect to an NLB that resides in another VPC (VPC Peering) or AWS managed VPN unless the EC2 client is a C5, i3.metal or M5 instance type. For VPC peering, both VPCs must be in the same region. See [Troubleshooting](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-troubleshooting.html#target-not-in-service).  
  
  
  
  
  
# ALB: Application Load Balancers  
---  
![Pasted image 20230226152623.png](../attachments/Pasted%20image%2020230226152623.png)  
## ALB Basics  
  
- 📒 [Homepage](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/) ∙ [User guide](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/developer-resources/) ∙ [FAQ](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/faqs/) ∙ [Pricing](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/pricing/)  
- 🐥**Websockets and HTTP/2** are [now supported](https://aws.amazon.com/blogs/aws/new-aws-application-load-balancer/).  
- 🐥**Internet Protocol Version 6 (IPv6)** is [now supported](https://aws.amazon.com/about-aws/whats-new/2017/01/announcing-internet-protocol-version-6-ipv6-support-for-elastic-load-balancing-in-amazon-virtual-private-cloud-vpc/).  
- 🐥**Load Balancing via IP** is [now supported](https://aws.amazon.com/about-aws/whats-new/2017/08/elastic-load-balancing-application-load-balancer-now-supports-load-balancing-to-ip-addresses-as-targets-for-aws-and-on-premises-resources/).  
- Prior to the Application Load Balancer, you were advised to use TCP instead of HTTP as the protocol to make it work (as described [here](http://www.quora.com/When-will-Amazon-ELB-offer-SPDY-support)) and use [the obscure but useful Proxy Protocol](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/enable-proxy-protocol.html) ([more on this](https://chrislea.com/2014/03/20/using-proxy-protocol-nginx/)) to pass client IPs over a TCP load balancer.  
  
  
## ALB Tips  
  
- Use ALBs to route to services that are hosted on shared clusters with dynamic port assignment (like ECS or Mesos).  
- ALBs support [HTTP host-based routing](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html#host-conditions) (send HTTP requests for “api.mydomain.com” -> {target-group-1}, “blog.mydomain.com” -> {target group 2}) as well as [HTTP path-based routing](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html#path-conditions) (send HTTP requests for “/api/&ast;” ->  {target-group-1}, “/blog/&ast;” -> {target group 2}).  
  
  
## ALB Gotchas and Limitations  
  
- ALBs only support HTTP/2 over HTTPS (no plain-text HTTP/2).  
- ALBs only support HTTP/2 to external clients and not to internal resources (instances/containers).  
- ALBs support HTTP routing but not port-based TCP routing.  
- Instances in the ALB’s target groups have to either have a single, fixed healthcheck port (“EC2 instance”-level healthcheck) or the healthcheck port for a target has to be the same as its application port (“Application instance”-level healthcheck) - you can't configure a per-target healthcheck port that is different than the application port.  
- ALBs are VPC-only (they are not available in EC2 Classic)  
- In a target group, if there is no healthy target, all requests are routed to all targets. For example, if you point a listener at a target group containing a single service that has a long initialization phase (during which the health checks would fail), requests will reach the service while it is still starting up.  
- 📜 Although ALBs [now support SNI](https://aws.amazon.com/about-aws/whats-new/2017/10/elastic-load-balancing-application-load-balancers-now-support-multiple-ssl-certificates-and-smart-certificate-selection-using-server-name-indication-sni/), they only support 25 HTTPS certificates per Load Balancer. This limitation is not described [here](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-limits.html), so it might be subject to change.  
  
  
  
# Elastic IPs  
  
  
## Elastic IP Basics  
  
- 📒 [Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) ∙ [FAQ](https://aws.amazon.com/ec2/faqs/#Elastic_IP) ∙ [Pricing](https://aws.amazon.com/ec2/pricing/on-demand/#Elastic_IP_Addresses)  
- **Elastic IPs** are static IP addresses you can rent from AWS to assign to EC2 instances.  
  
  
  
  
# RDS  
  
  
![Pasted image 20230226152639.png](../attachments/Pasted%20image%2020230226152639.png)  
  
  
  
- 데이터베이스 분리하기: RDS와 Aurora  
외로운 단일 서버의 피할 수 없는 운명에 대처하기 위해 데이터베이스를 애플리케이션 서버에서 분리하려고 합니다. 애플리케이션과 데이터베이스를 별도로 확장할 수 있도록 하기 위해서입니다. AWS에서는 두 가지 방법으로 이를 수행할 수 있습니다.  
  
첫 번째 방법은 완전히 수동으로 다른 EC2 인스턴스(즉, 다른 서버)를 생성하고 데이터베이스를 그 위에 올려놓는 것입니다.  
  
  
다시 말하지만, EC2 인스턴스를 "또 하나의 컴퓨터"라고 생각하면 자신의 컴퓨터에서 하는 것과 비슷하게 할 수 있습니다. 예를 들어 MySQL을 다운로드하고, 설정하고, 데이터베이스를 시작하고, 애플리케이션에서 트래픽을 허용하면 됩니다.  
  
이 작업이 완료되면 애플리케이션 서버를 데이터베이스 서버로 가리키기만 하면 바로 사용할 수 있습니다. 데이터베이스 관리가 고유한 분야인 데에는 이유가 있습니다.   
여기에는 많은 것이 있습니다. 데이터베이스를 패치하고, 업데이트하고, 유지 관리하는 일은 매우 까다로운 작업입니다. 따라서 이를 전담하는 사내 전문가나 팀이 없다면 자체 관리 방식으로 전환하기 전에 다시 한 번 생각해 보시기 바랍니다.  
  
반면에 AWS는 관계형 데이터베이스 서비스(RDS라고도 함)를 제공합니다.  
  
![Pasted image 20230226213809.png](../attachments/Pasted%20image%2020230226213809.png)  
  
  
특히 Aurora라는 매우 강력한 데이터베이스가 있습니다. 확장, 관리 및 패치를 모두 처리하기 때문에 매우 놀랍습니다.   
또한 MySQL 및 PostgreSQL과도 직접 호환됩니다. 따라서 이 두 가지 중 하나를 사용하여 로컬로 개발하는 경우에도 애플리케이션을 배포할 때 Aurora를 사용할 수 있습니다. MySQL 대신 Aurora의 데이터베이스 엔드포인트를 가리키는 것만큼이나 쉽습니다. 그리고 RDS 마케팅 팀에서 강조하듯이, MySQL보다 5배 빠르며 비용은 10분의 1 수준입니다.  
  
사용자는 서버에 접속하여 애플리케이션을 사용할 수 있고 애플리케이션은 RDS Aurora 데이터베이스와 상호 작용합니다.  
  
  
## RDS Basics: 앱의 데이터베이스  
  
- 📒 [Homepage](https://aws.amazon.com/rds/) ∙ [User guide](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/) ∙ [FAQ](https://aws.amazon.com/rds/faqs/) ∙ [Pricing](https://aws.amazon.com/rds/pricing/) (see also [ec2instances.info/rds/](http://www.ec2instances.info/rds/)\)  
- **RDS** is a managed relational database service, allowing you to deploy and scale databases more easily. It supports [Oracle](https://aws.amazon.com/rds/oracle/), [Microsoft SQL Server](https://aws.amazon.com/rds/sqlserver/), [PostgreSQL](https://aws.amazon.com/rds/postgresql/), [MySQL](https://aws.amazon.com/rds/mysql/), [MariaDB](https://aws.amazon.com/rds/mariadb/), and Amazon’s own [Aurora](https://aws.amazon.com/rds/aurora/).  
- RDS offers out of the box support for [high availability and failover](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) for your databases.  
  
다음 용도로 사용  
  
## RDS Tips  
  
- RDS instances start with a default timezone of UTC. If necessary, this can be [changed to a different timezone](https://aws.amazon.com/premiumsupport/knowledge-center/rds-change-time-zone/).  
- Save costs by stopping the RDS instance(s) when not in use, you can achieve this by configuring a stop and start schedule using the the [Instance Scheduler](https://aws.amazon.com/solutions/implementations/instance-scheduler/) solution.  
  
  
## RDS Gotchas and Limitations  
  
- ⏱RDS instances run on EBS volumes (either general-purpose or provisioned IOPS), and hence are constrained by EBS performance.  
- Verify what database features you need, as not everything you might want is available on RDS. For example, if you are using Postgres, check the list of [supported features and extensions](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html#SQLServer.Concepts.General.FeatureSupport). If the features you need aren't supported by RDS, you'll have to deploy your database yourself.  
- If you use the failover support offered by RDS, keep in mind that it is based on DNS changes, and make sure that your client reacts to these changes appropriately. This is particularly important for Java, given how its DNS resolver’s TTL is [configured by default](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/java-dg-jvm-ttl.html).  
- **DB migration to RDS:** While importing your database into RDS ensure you take into consideration the maintenance window settings. If a backup is running at the same time, your import can take a considerably longer time than you would have expected.  
- [Database sizes are limited](https://aws.amazon.com/about-aws/whats-new/2015/06/amazon-rds-increases-storage-limits-to-6TB-for-piops-and-gp2/) to **6TB** for all database engines except for SQL Server which has a **4TB** limit and Aurora which supports up to **64TB** databases.  
  
  
  
# RDS Aurora  
---  
## RDS Aurora Basics  
Aurora는 자가 복구 스토리지와 인스턴스당 최대 64TB의 자동 확장 기능을 갖춘 분산형 내결함성 관계형 데이터베이스를 제공하도록 설계된 클라우드 전용 데이터베이스 서비스입니다.  현재 MySQL 호환 시스템과 PostgreSQL 호환 시스템의 두 가지 버전으로 제공됩니다.  
  
# RDS Aurora PostgreSQL  
---------------------  
  
## RDS Aurora PostgreSQL Basics  
  
- Amazon’s proprietary fork of PostgreSQL, intended to scale up for high concurrency workloads while maintaining ease of use. Currently based on PostgreSQL 9.6.  
- Higher throughput (up to 3x with similar hardware).  
- Automatic storage scale in 10GB increments up to 64TB.  
- Low latency read replicas that share the storage layer with the master which significantly reduces replica lag.  
- Point in time recovery.  
- Fast database snapshots.  
  
  
## RDS Aurora PostgreSQL Tips  
- Aurora Postgres by default is supposed to utilize high connection rates and for this reason connection pooling must be configured accordingly.  
- Because Aurora is based on PostgreSQL 9.6, it lacks features like declarative partitioning or logical replication.  
  
  
## RDS Aurora PostgreSQL Gotchas and Limitations  
- Aurora PostgreSQL falls behind normal RDS when it comes to available versions, so if you need features from the latest PostgreSQL version you might be better off with plain RDS.  
- Patching and bug fixing is separate from open source PostgreSQL.  
  
  
  
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
독립적인 작업을 수행하기 위해 JS, Java 또는 Python의 작은 독립형 스니펫을 실행합니다. 일종의 큐와 실행을 하나로 합친 것입니다. AWS 설정에 대한 변경 사항을 저장한 다음 실행하거나 S3 또는 DynamoDB의 이벤트에 응답하는 데 사용됩니다.  
  
## Lambda Code Samples  
  
- [Fan-out](https://github.com/awslabs/aws-lambda-fanout) is an example of using Lambda to “fan-out” or copy data from one service, in this case Kinesis, to multiple other AWS data services. Destinations for fan-out data in the sample include IoT, SQS and more.  
- This [AWS limit monitor using Lambdas](https://github.com/awslabs/aws-limit-monitor) shows use of multiple Lambdas for monitoring.  
- This [Lambda ECS Worker Pattern](https://github.com/awslabs/lambda-ecs-worker-pattern) shows use of Lambda in a workflow where data from S3 is picked up by the Lambda, pushed to a queue, then sent to ECS for more processing.  
- The [Secure Pet Store](https://github.com/awslabs/api-gateway-secure-pet-store) is a sample Java application which uses Lambda and API Gateway with Cognito (for user identity).  
- [aws-lambda-list](https://github.com/unixorn/aws-lambda-list) is a list of "hopefully useful AWS lambdas and lambda-related resources". Quite a few code samples here; as usual, not guaranteed tested. Caveat Emptor.  
  
🚧 [*Please help expand this incomplete section.*](CONTRIBUTING.md)  
  
# API Gateway  
-----------  
  
## API Gateway Basics  
  
- 📒 [Homepage](https://aws.amazon.com/api-gateway/) ∙ [Developer guide](http://docs.aws.amazon.com/apigateway/latest/developerguide/) ∙ [FAQ](https://aws.amazon.com/api-gateway/faqs/) ∙ [Pricing](https://aws.amazon.com/api-gateway/pricing/)  
- **API Gateway** provides a scalable, secured front-end for service APIs, and can work with Lambda, Elastic Beanstalk, or regular EC2 services.  
- API 게이트웨이는 개발자를 위한 완전 관리형 서비스로, 전체 API를 쉽게 빌드, 게시, 관리 및 보호할 수 있습니다. AWS 관리 콘솔에서 몇 번의 클릭만으로 애플리케이션이 백엔드 서비스(예: EC2에서 실행되는 워크로드)의 데이터, 비즈니스 로직 또는 기능에 액세스할 수 있는 '프런트 도어' 역할을 하는 API를 생성할 수 있습니다.  
- It allows “serverless” deployment of applications built with Lambda.  
- Switching over deployments after upgrades can be tricky. There are no built-in mechanisms to have a single domain name migrate from one API gateway to another one. So it may be necessary to build an additional layer in front (even another API Gateway) to allow smooth migration from one deployment to another.  
  
  
## API Gateway Alternatives and Lock-In  
  
- [Kong](https://getkong.org) is an open-source, on-premises API and microservices gateway built on nginx with Lua. Kong is extensible through “plugins”.  
- [Tyk](https://tyk.io) is an open-source API gateway implemented in Go and available in the cloud, on-premises or hybrid.  
  
  
  
  
# Route 53  
--------  
  
## Route 53 Basics: 도베인 발급  
  
- 📒 [Homepage](https://aws.amazon.com/route53/) ∙ [Developer guide](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/) ∙ [FAQ](https://aws.amazon.com/route53/faqs/) ∙ [Pricing](https://aws.amazon.com/route53/pricing/)  
- **Route 53** is AWS’ DNS service.  
  
  
  
  
# VPC  
-------------------------------------------  
## VPC Basics  
![Pasted image 20230227102638.png](../attachments/Pasted%20image%2020230227102638.png)  
  
- 📒 [Homepage](https://aws.amazon.com/vpc/) ∙ [User guide](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide) ∙ [FAQ](https://aws.amazon.com/vpc/faqs/) ∙ [Security groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) ∙ [Pricing](https://aws.amazon.com/vpc/pricing/)  
- **VPC** (Virtual Private Cloud) is the virtualized networking layer of your AWS systems.  
- VPC를 사용하면 정의한 가상 네트워크 내에서 서비스 및 시스템을 시작할 수 있는 논리적으로 격리된 AWS 클라우드 섹션을 프로비저닝할 수 있습니다. 어떤 AWS 리소스를 공개할지, 어떤 리소스를 공개하지 않을지 선택할 수 있는 옵션이 있기 때문에 VPC는 보안을 훨씬 더 세밀하게 제어할 수 있습니다.  
- Most AWS users should have a basic understanding of VPC concepts, but few need to get into all the details. VPC configurations can be trivial or extremely complex, depending on the extent of your network and security needs.  
- All modern AWS accounts (those created [after 2013-12-04](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html)) are “EC2-VPC” accounts that support VPCs, and all instances will be in a default VPC. Older accounts may still be using “EC2-Classic” mode. Some features don’t work without VPCs, so you probably will want to [migrate](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-migrate.html).  
![Pasted image 20230227102626.png](../attachments/Pasted%20image%2020230227102626.png)  
다음과 같은 용도로 사용하세요.  
보안 계층을 추가하여 "우리 모든 것이 인터넷에 있다!"는 반대를 극복할 수 있습니다. 모든 AWS 서비스가 훨씬 더 큰 네트워크의 작은 조각이 아니라 동일한 작은 네트워크에 있는 것처럼 보이게 합니다.  
  
공유기 같은 것  
추가 참고 [AWS VPC Core Concepts in an Analogy and Guide](https://start.jcolemorrison.com/aws-vpc-core-concepts-analogy-guide/)  
  
  
## PrivateLink  
- 📒[Homepage](https://aws.amazon.com/privatelink/) ∙ [User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) ∙  [Pricing](https://aws.amazon.com/privatelink/pricing/)  
- One of the uses for Private link is [Interface VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) deploys an ENI into your VPC and subnets which allows you direct access to the AWS API's as if the were accessible locally in your VPC without having to go out to the internet.  
- Another use case would be to expose a service of your own to other accounts in AWS through a [VPC Endpoint Service](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service.html)  
  
  
  
  
  
  
  
# Certificate Manager  
-------------------  
  
## Certificate Manager Basics  
  
- 📒 [Homepage](https://aws.amazon.com/certificate-manager/) ∙ [User guide](http://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) ∙ [FAQ](https://aws.amazon.com/certificate-manager/faqs/) ∙ [Pricing](https://aws.amazon.com/certificate-manager/pricing/)  
- Use the **Certificate Manager** to manage SSL/TLS certificates in other AWS services.  
- Supports importing existing certificates as well as issuing new ones.  
- Provides Domain Validated (DV) certificates. [Validation](http://docs.aws.amazon.com/acm/latest/userguide/gs-acm-validate.html) can be done in two ways. The first (and recommended) way is [via DNS](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-validate-dns.html). If the zone lives within Route 53 and the user has access, the necessary record can be added in the console via a single click during the certificate request process. If the zone is not within Route 53 the user is required to update DNS manually. This is still preferred to the second way, which requires more user interaction, and is done by sending an email to 3 contact addresses in WHOIS and 5 common addresses for the domain, for each domain name present in the request.  
- ACM will attempt to automatically [renew](http://docs.aws.amazon.com/acm/latest/userguide/how-domain-validation-works.html) a certificate issued by Amazon. It will first attempt to connect to the domain on HTTPS and check that the certificate used by the domain is the same with the certificate that it intends to renew. Failing that, it will check the DNS record used previously for validation. Failing that, ACM will attempt manual validation by sending emails to all domains in the certificate.  
  
- During the domain validation process, if DNS validation is unsuccessful Certificate Manager will send an email to every contact address specified in the domain’s WHOIS record and up to five common administrative addresses. Some anti-spam filters can mark emails as spam because of this. You should check the spam folder of your email if you don’t receive a confirmation email.  
  
  
  
  
  
  
  
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
  
  
![Pasted image 20230226163650.png](../attachments/Pasted%20image%2020230226163650.png)  
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
  
# Getting Help and Support  
  
- **Forums:** For many problems, it’s worth searching or asking for help in the [discussion forums](https://forums.aws.amazon.com/index.jspa) to see if it’s a known issue.   
- **Contact:** The main web contact point for AWS is [here](https://aws.amazon.com/contact-us/). Many technical requests can be made via these channels.  
- **AWS Professional Services:** AWS provides [consulting services](https://aws.amazon.com/professional-services/) alone or in combination with partners.