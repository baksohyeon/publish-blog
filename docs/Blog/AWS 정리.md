---  
category: Blog  
share: true  
---  
[GitHub - open-guides/og-aws: 📙 Amazon Web Services — a practical guide](https://github.com/open-guides/og-aws#general-information)  
  
  
Legend  
------  
  
-	📒 Marks standard/official AWS pages and docs  
-	🔹 Important or often overlooked tip  
-	❗ “Serious” gotcha (used where risks or time or resource costs are significant: critical security risks, mistakes with significant financial cost, or poor architectural choices that are fundamentally difficult to correct)  
-	🔸 “Regular” gotcha, limitation, or quirk (used where consequences are things not working, breaking, or not scaling gracefully)  
-	📜 Undocumented feature (folklore)  
-	🐥 Relatively new (and perhaps immature) services or features  
-	⏱ Performance discussions  
-	⛓ Lock-in: Products or decisions that are likely to tie you to AWS in a new or significant way — that is, later moving to a non-AWS alternative would be costly in terms of engineering effort  
-	🚪 Alternative non-AWS options  
-	💸 Cost issues, discussion, and gotchas  
-	🕍 A mild warning attached to “full solution” or opinionated frameworks that may take significant time to understand and/or might not fit your needs exactly; the opposite of a point solution (the cathedral is a nod to [Raymond’s metaphor](https://en.wikipedia.org/wiki/The_Cathedral_and_the_Bazaar)\)  
-	📗📘📙 Colors indicate basics, tips, and gotchas, respectively.  
-	🚧 Areas where correction or improvement are needed (possibly with link to an issue — do help!)  
  
General Information  
-------------------  
  
# When to Use AWS  
  
-	[AWS](https://en.wikipedia.org/wiki/Amazon_Web_Services) is the dominant public cloud computing provider.  
	-	In general, “[cloud computing](https://en.wikipedia.org/wiki/Cloud_computing)” can refer to one of three types of cloud: “public,” “private,” and “hybrid.” AWS is a public cloud provider, since anyone can use it. Private clouds are within a single (usually large) organization. Many companies use a hybrid of private and public clouds.  
	-	The core features of AWS are [infrastructure-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Infrastructure_as_a_service_.28IaaS.29) (IaaS) — that is, virtual machines and supporting infrastructure. Other cloud service models include [platform-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Platform_as_a_service_.28PaaS.29) (PaaS), which typically are more fully managed services that deploy customers’ applications, or [software-as-a-service](https://en.wikipedia.org/wiki/Cloud_computing#Software_as_a_service_.28SaaS.29) (SaaS), which are cloud-based applications. AWS does offer a few products that fit into these other models, too.  
	-	In business terms, with infrastructure-as-a-service you have a variable cost model — it is [OpEx, not CapEx](http://www.investopedia.com/ask/answers/020915/what-difference-between-capex-and-opex.asp) (though some [pre-purchased contracts](https://aws.amazon.com/ec2/purchasing-options/reserved-instances/) are still CapEx).  
  - AWS’s TTM revenue was [**$37.549 billion**](https://ir.aboutamazon.com/news-release/news-release-details/2020/Amazoncom-Announces-First-Quarter/default.aspx) as of Q1 2020 according to their earnings results (slide 14 in the linked deck), or roughly **14%** of Amazon.com’s total revenue (slide 11 in the same deck) for the same TTM period.  
-	**Main reasons to use AWS:**  
	- 회사에서 확장해야 하는 시스템이나 제품을 구축 중이고  
	- 기술 노하우가 있고  
	- 가장 유연한 도구를 원하고  
	- 이미 다른 인프라에 크게 묶여 있지 않고  
	- 퍼블릭 클라우드 기반 솔루션을 사용할 수 없는 내부, 규제 또는 규정 준수 이유가 없고  
	- Microsoft 우선 기술 스택을 사용하지 않는 경우  
	- Google Cloud를 사용해야 할 특별한 이유가 없는 경우  
	- 다소 높은 비용을 감당하거나 관리하거나 협상할 수 있는 경우  
  
  
-	**Costs:** Billing and cost management are such big topics that we have [an entire section on this](AWS%20정리.md#billing-and-cost-management).  
  
- 🚪**AWS vs. other cloud providers:**   
1. [**Google Cloud Platform**](https://cloud.google.com/)  
	GCP arrived later to market than AWS, but has vast resources and is now used widely by many companies, including a few large ones. It is gaining market share. Not all AWS services have similar or analogous services in GCP. And vice versa: In particular, GCP offers some more advanced machine learning-based services like the [Vision](https://cloud.google.com/vision/), [Speech](https://cloud.google.com/speech/), and [Natural Language](https://cloud.google.com/natural-language/) APIs. It’s not common to switch once you’re up and running, but it does happen: [Spotify migrated](http://www.wsj.com/articles/google-cloud-lures-amazon-web-services-customer-spotify-1456270951) from AWS to Google Cloud. There is more discussion [on Quora](https://www.quora.com/What-are-the-reasons-to-choose-AWS-over-Google-Cloud-or-vice-versa-for-a-high-traffic-web-application) about relative benefits. Of particular note is that VPCs in GCP are [global by default](https://cloud.google.com/vpc/) with subnetworks per region, while AWS’ VPCs have to live within a particular region. This gives GCP an edge if you’re designing applications with geo-replication from the beginning. It’s also possible to [share one GCP VPC](https://cloud.google.com/compute/docs/shared-vpc/) between multiple projects (roughly analogous to AWS accounts), while in AWS you’d have to peer them. It’s also possible to [peer GCP VPCs](https://cloud.google.com/compute/docs/vpc/vpc-peering) in a similar manner to how it’s done in AWS  
  
2. [**Microsoft Azure**](https://azure.microsoft.com/en) is the de facto choice for companies and teams that are focused on a Microsoft stack, and it has now placed significant emphasis on Linux as well  
3. Companies at (very) large scale may want to reduce costs by managing their own infrastructure. For example, [Dropbox migrated](https://news.ycombinator.com/item?id=11282948) to their own infrastructure.  
4. Other cloud providers such as [Digital Ocean](https://www.digitalocean.com/) offer similar services, sometimes with greater ease of use, more personalized support, or lower cost. However, none of these match the breadth of products, mind-share, and market domination AWS now enjoys.  
5. Traditional managed hosting providers such as [Rackspace](https://www.rackspace.com/) offer cloud solutions as well.  
  
  
-	**Hybrid cloud:** In larger enterprises, it is common to have [hybrid deployments](https://aws.amazon.com/enterprise/hybrid/) encompassing private cloud or on-premises servers and AWS — or other enterprise cloud providers like [IBM](https://www.ibm.com/it-infrastructure/solutions/hybrid-cloud)/[Bluemix](http://www.ibm.com/cloud-computing/bluemix/hybrid/), [Microsoft](https://www.microsoft.com/en-us/cloud-platform/hybrid-cloud)/[Azure](https://azure.microsoft.com/en-us/overview/azure-stack/), [NetApp](http://www.netapp.com/us/solutions/cloud/hybrid-cloud/), or [EMC](http://www.emc.com/en-us/cloud/hybrid-cloud-computing/index.htm).  
- **Major customers:** Who uses AWS and Google Cloud?  
	- AWS’s [list of customers](https://aws.amazon.com/solutions/case-studies/) includes large numbers of mainstream online properties and major brands, such as Netflix, Pinterest, Spotify (moving to Google Cloud), Airbnb, Expedia, Yelp, Zynga, Comcast, Nokia, and Bristol-Myers Squibb.  
	- Azure’s [list of customers](https://azure.microsoft.com/en-us/case-studies/) includes companies such as NBC Universal, 3M and Honeywell Inc.  
	- Google Cloud’s [list of customers](https://cloud.google.com/customers/) is large as well, and includes a few mainstream sites, such as [Snapchat](http://www.businessinsider.com/snapchat-is-built-on-googles-cloud-2014-1), Best Buy, Domino’s, and Sony Music.  
  
### Which Services to Use  
  
-	AWS  [아마존 제공 서비스](https://aws.amazon.com/products/)   
  
  
# Infrastructure  
  
⭐ **중요**  
-  IAM: User accounts and identities (you need to think about accounts early on!)  
- EC2: Virtual servers and associated components,  
	- Load Balancers: CLBs and ALBs  
	- Elastic IPs: Assigned IP addresses  
- S3: Storage of files  
- Route 53: DNS and domain registration  
- RDS: Managed relational databases (managed MySQL, Postgres, and Amazon’s own Aurora database)  
ECS: Docker container/cluster management (note Docker can also be used directly, without ECS)  
 [ECR](https://aws.amazon.com/ecr/): Hosted private Docker registry 중요  
 [Certificate Manager](https://aws.amazon.com/certificate-manager/): Manage SSL/TLS certificates for AWS services  
 [Fargate](https://aws.amazon.com/fargate/): Docker containers management, backend for ECS and EKS  
  
  
**훑어보기**  
:       ⛓[DynamoDB](AWS%20정리.md#dynamodb): Low-latency NoSQL key-value store 이름 기능만 알기   
	⛓[SQS](https://aws.amazon.com/sqs/): Message queueing service 이름만 알기  
	[SES](https://aws.amazon.com/ses/): Send and receive e-mail for marketing or transactions 문자 인증할 때 쓰는건데 이름 기능만   
	⛓ [API Gateway](https://aws.amazon.com/api-gateway/): Proxy, manage, and secure API calls 람다랑 연결되는 기능 람다할때 보게될건데 걍 기능이 있구나만 라우트53이랑 비슷  
	CloudWatch: Alerts, paging, monitoring  
	⛓[Lambda](AWS%20정리.md#lambda): Running small, fully managed tasks “serverless” (뭔 기능인지만)  
## Tools and Services Market Landscape  
  
AWS 실무자들에게 인기가 있는 서비스, 즉 AWS에 특별히 도움이 되거나 보완적인 서비스 또는 AWS를 사용하는 거의 모든 사람이 배워야 하는 도구를 강조하려고 노력했습니다.  
  
![Pasted image 20230226161924.png](../attachments/Pasted%20image%2020230226161924.png)  
  
## Common Concepts  
  
-	📒  AWS [**General Reference**](https://docs.aws.amazon.com/general/latest/gr/Welcome.html) 에서는 여러 서비스와 관련된 여러 가지 일반적인 개념을 다룹니다.  
AWS에서는 지연 시간을 줄이거나 추가적인 리던던시를 제공하는 데 도움이 되는 격리된 지리적 위치인 리전 배포를 허용합니다. 리전에는 일반적으로 고가용성을 위해 가장 먼저 선택되는 도구인 가용 영역(AZ)이 포함됩니다.) AZ는 같은 리전 내에서도 물리적으로 서로 분리되어 있으며, 여러 물리적 데이터 센터에 걸쳐 있을 수 있습니다. 지연 시간이 짧은 링크를 통해 연결되어 있기 때문에 한 곳에 자연재해가 발생해도 다른 곳에는 영향을 미치지 않습니다.  
-	AWS allows deployments in [**regions**](https://docs.aws.amazon.com/general/latest/gr/rande.html), which are isolated geographic locations that help you reduce latency or offer additional redundancy. Regions contain availability zones(AZs), which are typically the first tool of choice for [high availability](AWS%20정리.md#high-availability)). AZs are [physically separate from one another](https://www.youtube.com/watch?v=JIQETrFC_SQ&feature=youtu.be&t=1428) even within the same region, and [may span multiple physical data centers](https://blog.rackspace.com/aws-101-regions-availability-zones). While they are connected via low latency links, natural disasters afflicting one should not affect others.  
-	Each service has API **endpoints** for each region. Endpoints differ from service to service and not all services are available in each region, as listed in [these tables](https://docs.aws.amazon.com/general/latest/gr/rande.html).  
-	[**Amazon Resource Names (ARNs)**](https://docs.aws.amazon.com/general/latest/gr/aws-arns-and-namespaces.html) are specially formatted identifiers for identifying resources. They start with 'arn:' and are used in many services, and in particular for IAM policies.  
  
AWS 일반 참조에서는 여러 서비스와 관련된 여러 가지 일반적인 개념을 다룹니다.  
AWS에서는 지연 시간을 줄이거나 추가적인 이중화를 제공하는 데 도움이 되는 격리된 지리적 위치인 리전 배포를 허용합니다. 리전에는 일반적으로 고가용성을 위해 가장 먼저 선택되는 도구인 가용 영역(AZ)이 포함됩니다.) AZ는 같은 리전 내에서도 물리적으로 서로 분리되어 있으며, 여러 물리적 데이터 센터에 걸쳐 있을 수 있습니다. 지연 시간이 짧은 링크를 통해 연결되어 있지만, 한 곳에 자연 재해가 발생해도 다른 곳에 영향을 미치지 않습니다.  
각 서비스에는 각 지역에 대한 API 엔드포인트가 있습니다. 엔드포인트는 서비스마다 다르며 다음 표에 나열된 것처럼 각 지역에서 모든 서비스를 사용할 수 있는 것은 아닙니다.  
ARN(Amazon 리소스 이름)은 리소스를 식별하기 위한 특수 형식의 식별자입니다. 'arn:'로 시작하며 많은 서비스에서 사용되며 특히 IAM 정책에 사용됩니다.  
  
  
  
## Getting Help and Support  
  
-	**Forums:** For many problems, it’s worth searching or asking for help in the [discussion forums](https://forums.aws.amazon.com/index.jspa) to see if it’s a known issue.  
-	**Account manager:** If you are at significant levels of spend (thousands of US dollars plus per month), you may be assigned (or may wish to ask for) a dedicated account manager.  
	-	These are a great resource, even if you’re not paying for premium support. Build a good relationship with them and make use of them, for questions, problems, and guidance.  
	-	Assign a single point of contact on your company’s side, to avoid confusing or overwhelming them.  
-	**Contact:** The main web contact point for AWS is [here](https://aws.amazon.com/contact-us/). Many technical requests can be made via these channels.  
-	**AWS Professional Services:** AWS provides [consulting services](https://aws.amazon.com/professional-services/) alone or in combination with partners.  
  
## Restrictions and Other Notes  
  
-	🔸Lots of resources in Amazon have [**limits**](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) on them. This is actually helpful, so you don’t incur large costs accidentally. You have to request that quotas be increased by opening support tickets. Some limits are easy to raise, and some are not. (Some of these are noted in sections below.) Additionally, not all service limits are published.  
	- **Obtaining Current Limits and Usage:** Limit information for a service may be available from the service API, Trusted Advisor, both or neither (in which case you'll need to contact Support). [This page](http://awslimitchecker.readthedocs.io/en/latest/limits.html) from the awslimitchecker tool's documentation provides a nice summary of available retrieval options for each limit. The [tool](https://github.com/jantman/awslimitchecker) itself is also valuable for automating limit checks.  
-	🔸[**AWS terms of service**](https://aws.amazon.com/service-terms/) are extensive. Much is expected boilerplate, but it does contain important notes and restrictions on each service. In particular, there are restrictions against using many AWS services in **safety-critical systems**. (Those appreciative of legal humor may wish to review [clause 42.10](https://www.theguardian.com/technology/2016/feb/11/amazon-terms-of-service-zombie-apocalypse).)  
  
## Related Topics  
  
-	[OpenStack](https://www.openstack.org/) is a private cloud alternative to AWS used by large companies that wish to avoid public cloud offerings.  
  
  
# Managing AWS  
  
  
## Managing Infrastructure State and Change  
  
AWS를 사용하여 복잡한 시스템을 구축할 때(그리고 일반적으로 데브옵스를 사용할 때) 가장 큰 과제는 시간이 지남에 따라 인프라 상태를 효과적으로 관리하는 것입니다. 일반적으로 인프라 상태에 대한 목표는 크게 세 가지로 요약됩니다:  
  
-	**Visibility**: 인프라의 상태(사용 중인 서비스와 정확한 방법)를 알고 있나요? 또한 여러분과 팀원 중 누가 언제 변경을 하는지 알고 있나요? 서비스의 잘못된 구성, 문제, incidents를 감지할 수 있나요?  
-	**Automation**: 많은 추가 수작업이나 누군가의 머릿속에만 있는 지식 없이도 과거 구성을 재현하거나 기존 구성을 확장하기 위해 인프라를 재구성할 수 있나요? 인시던트에 쉽게 또는 자동으로 대응할 수 있나요?  
-	**Flexibility**:  큰 노력 없이 구성을 개선하고 새로운 방식으로 확장할 수 있나요? 동일한 도구를 사용하여 더 많은 복잡성을 추가할 수 있나요? 팀 내에서 구성을 공유, 검토 및 개선하고 있나요?  
  
Much of what we discuss below is really about how to improve the answers to these questions.  
  
There are several approaches to deploying infrastructure with AWS, from the console to complex automation tools, to third-party services, all of which attempt to help achieve visibility, automation, and flexibility.  
  
## AWS Configuration Management  
  
대부분의 사람들이 AWS를 실험하는 첫 번째 방법은 웹 인터페이스인 AWS 콘솔을 이용하는 것입니다. 그러나 콘솔을 사용하는 것은 매우 수동적인 프로세스이며 자동화 또는 유연성에 반하는 경우가 많습니다.  
  
그렇다면 AWS 구성을 수동으로 관리하지 않으려면 어떻게 해야 할까요? 안타깝게도 간단하고 보편적인 정답은 없습니다. 각 접근 방식에는 장단점이 있으며, 회사마다 API를 직접 사용하거나 직접 도구를 구축하는 방법, 명령줄 도구를 사용하는 방법, 타사 도구 및 서비스를 사용하는 방법 등 매우 다양한 접근 방식을 취하고 있습니다.  
## AWS Console  
  
-	The [AWS Console](https://aws.amazon.com/console/) lets you control much (but not all) functionality of AWS via a web interface.  
-	Ideally, you should only use the AWS Console in a few specific situations:  
	-	It’s great for read-only usage. If you’re trying to understand the state of your system, logging in and browsing it is very helpful.  
	-	It is also reasonably workable for very small systems and teams (for example, one engineer setting up one server that doesn’t change often).  
	-	It can be useful for operations you’re only going to do rarely, like less than once a month (for example, a one-time VPC setup you probably won’t revisit for a year). In this case using the console can be the simplest approach.  
-	❗**Think before you use the console:** The AWS Console is convenient, but also the enemy of automation, reproducibility, and team communication. If you’re likely to be making the same change multiple times, avoid the console. Favor some sort of automation, or at least have a path toward automation, as discussed next. Not only does using the console preclude automation, which wastes time later, but it prevents documentation, clarity, and standardization around processes for yourself and your team.  
  
AWS 콘솔을 사용하면 웹 인터페이스를 통해 AWS의 많은 기능(전부는 아님)을 제어할 수 있습니다.  
이상적으로는 몇 가지 특정 상황에서만 AWS 콘솔을 사용하는 것이 좋습니다:  
읽기 전용으로 사용하기에 좋습니다. 시스템 상태를 이해하려는 경우 로그인하여 탐색하는 것이 매우 유용합니다.  
또한 매우 작은 시스템과 팀(예: 자주 변경하지 않는 서버를 한 명의 엔지니어가 설정하는 경우)에서도 합리적으로 사용할 수 있습니다.  
한 달에 한 번 미만으로 거의 하지 않는 작업(예: 1년 동안 다시 방문하지 않을 일회성 VPC 설정)에도 유용할 수 있습니다. 이 경우 콘솔을 사용하는 것이 가장 간단한 방법이 될 수 있습니다.  
콘솔을 사용하기 전에 생각하기: AWS 콘솔은 편리하지만 자동화, 재현성 및 팀 커뮤니케이션의 적이기도 합니다. 동일한 변경을 여러 번 수행할 가능성이 높다면 콘솔을 사용하지 마세요. 다음에 설명하는 것처럼 자동화를 선호하거나 최소한 자동화를 향한 경로를 마련하세요. 콘솔을 사용하면 자동화를 배제하여 나중에 시간을 낭비할 뿐만 아니라 자신과 팀을 위한 문서화, 명확성 및 프로세스에 대한 표준화도 방해합니다.  
  
## Command-Line tools  
  
-	The [**aws command-line interface**](https://aws.amazon.com/cli/) (CLI), used via the **aws** command, is the most basic way to save and automate AWS operations.  
-	Don’t underestimate its power. It also has the advantage of being well-maintained — it covers a large proportion of all AWS services, and is up to date.  
-	In general, whenever you can, prefer the command line to the AWS Console for performing operations.  
-	🔹Even in the absence of fancier tools, you can **write simple Bash scripts** that invoke *aws* with specific arguments, and check these into Git. This is a primitive but effective way to document operations you’ve performed. It improves automation, allows code review and sharing on a team, and gives others a starting point for future work.  
-	🔹For use that is primarily interactive (not scripted), consider instead using the [**aws-shell**](https://github.com/awslabs/aws-shell) tool from AWS. It is easier to use, with auto-completion and a colorful UI, but still works on the command line. If you’re using [SAWS](https://github.com/donnemartin/saws), a previous version of the program, [you should migrate to aws-shell](https://github.com/donnemartin/saws/issues/68#issuecomment-240067034).  
  
  
aws 명령어를 통해 사용되는 aws 명령줄 인터페이스(CLI)는 AWS 작업을 저장하고 자동화하는 가장 기본적인 방법입니다.  
그 힘을 과소평가하지 마세요. 또한 모든 AWS 서비스의 대부분을 다루며 최신 상태로 유지 관리가 잘 된다는 장점도 있습니다.  
일반적으로 작업을 수행할 때는 가능하면 AWS 콘솔보다 명령줄을 선호하세요.  
🔹더 멋진 도구가 없더라도 특정 인수를 사용하여 aws를 호출하는 간단한 Bash 스크립트를 작성하고 이를 Git에서 확인할 수 있습니다. 이것은 수행한 작업을 문서화하는 원시적이지만 효과적인 방법입니다. 자동화를 개선하고, 팀에서 코드 검토 및 공유를 가능하게 하며, 다른 사람들이 향후 작업을 위한 출발점을 제공할 수 있습니다.  
🔹주로 대화형(스크립트가 아닌)으로 사용하려면 AWS의 aws-shell 도구를 사용하는 것을 고려하세요. 자동 완성 기능과 다채로운 UI로 사용하기가 더 쉽지만 여전히 명령줄에서 작동합니다. 이전 버전의 프로그램인 SAWS를 사용 중이라면 aws-shell로 마이그레이션해야 합니다.  
  
  
## APIs and SDKs  
  
-	**SDKs** for using AWS APIs are available in most major languages, with [Go](https://github.com/aws/aws-sdk-go), [iOS](https://github.com/aws/aws-sdk-ios), [Java](https://github.com/aws/aws-sdk-java), [JavaScript](https://github.com/aws/aws-sdk-js), [Python](https://github.com/boto/boto3), [Ruby](https://github.com/aws/aws-sdk-ruby), and [PHP](https://github.com/aws/aws-sdk-php) being most heavily used. AWS maintains [a short list](https://aws.amazon.com/tools/#sdk), but the [awesome-aws list](https://github.com/donnemartin/awesome-aws#sdks-and-samples) is the most comprehensive and current. Note [support for C++](https://github.com/donnemartin/awesome-aws#c-sdk) is [still new](https://aws.amazon.com/blogs/aws/introducing-the-aws-sdk-for-c/).  
-	**Retry logic:** An important aspect to consider whenever using SDKs is error handling; under heavy use, a wide variety of failures, from programming errors to throttling to AWS-related outages or failures, can be expected to occur. SDKs typically implement [**exponential backoff**](https://docs.aws.amazon.com/general/latest/gr/api-retries.html) to address this, but this may need to be understood and adjusted over time for some applications. For example, it is often helpful to alert on some error codes and not on others.  
-	❗Don’t use APIs directly. Although AWS documentation includes lots of API details, it’s better to use the SDKs for your preferred language to access APIs. SDKs are more mature, robust, and well-maintained than something you’d write yourself.  
  
  
## General Visibility  
  
-	🔹[**Tagging resources**](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/Using_Tags.html) is an essential practice, especially as organizations grow, to better understand your resource usage. For example, through automation or convention, you can add tags:  
	-	For the org or developer that “owns” that resource  
	-	For the product that resource supports  
	-	To label lifecycles, such as temporary resources or one that should be deprovisioned in the future  
	-	To distinguish production-critical infrastructure (e.g. serving systems vs backend pipelines)  
	-	To distinguish resources with special security or compliance requirements  
	-	To (once enabled) [allocate cost](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-alloc-tags.html). Note that cost allocation tags only apply on a forward-looking basis; you can't retroactively apply them to items already billed.  
	-	For many years, there was a notorious 10 tag limit per resource, which could not be raised and caused many companies significant pain. As of 2016, this was [raised](https://aws.amazon.com/blogs/security/now-organize-your-aws-resources-by-using-up-to-50-tags-per-resource/) to 50 tags per resource.  
	-	🔹In 2017, AWS introduced the ability to [enforce tagging](https://aws.amazon.com/blogs/aws/new-tag-ec2-instances-ebs-volumes-on-creation/) on instance and volume creation, deprecating portions of third party tools such as [Cloud Custodian](https://github.com/capitalone/cloud-custodian).  
	-	🔸 Tags are case sensitive; 'environment' and 'Environment' are two different tags. Automation in setting tags is likely the only sensible option at significant scale.  
	-	🔸 There is a bug in the ASG console where spaces after tag names are preserved. So if you type "Name " with a space at the end you will not get the expected behavior. This is probably true in other locations and SDKs also. Be sure you do not add trailing spaces to tag keys unless you really mean it. (As of Jul 2018)  
	-	🔸 When resources are shared across the org, tags are not shared with it. For example, sharing Transit Gateway or AMIs will show the correct tags in the account that created these resources but not in the accounts where these resources were shared.  
  
  
리소스에 태그를 지정하는 것은 특히 조직이 성장함에 따라 리소스 사용량을 더 잘 이해하기 위해 필수적인 관행입니다. 예를 들어 자동화 또는 규칙을 통해 태그를 추가할 수 있습니다:  
해당 리소스를 '소유'하는 조직 또는 개발자의 경우  
리소스가 지원하는 제품용  
임시 리소스 또는 향후 프로비저닝 해제해야 하는 리소스와 같은 수명 주기에 레이블 지정  
프로덕션에 중요한 인프라(예: 서비스 시스템과 백엔드 파이프라인)를 구분하기 위해  
특별한 보안 또는 규정 준수 요구사항이 있는 리소스를 구분하기 위해  
(활성화된 경우) 비용 할당. 비용 할당 태그는 미래 예측 기준으로만 적용되며, 이미 청구된 항목에는 소급하여 적용할 수 없다는 점에 유의하세요.  
수년 동안 리소스당 10개라는 악명 높은 태그 제한이 있었는데, 이 제한을 늘릴 수 없어 많은 회사가 큰 어려움을 겪었습니다. 2016년부터는 리소스당 태그 수가 50개로 증가했습니다.  
2017년, AWS는 인스턴스 및 볼륨 생성에 태그를 적용하는 기능을 도입하여 Cloud Custodian과 같은 타사 도구의 일부를 더 이상 사용하지 않게 되었습니다.  
태그는 대소문자를 구분하며, '환경'과 '환경'은 서로 다른 두 개의 태그입니다. 상당한 규모에서는 태그 설정 자동화가 유일한 합리적인 옵션일 가능성이 높습니다.  
태그 이름 뒤에 공백이 유지되는 버그가 ASG 콘솔에 있습니다. 따라서 끝에 공백이 있는 "이름"을 입력하면 예상되는 동작이 나타나지 않습니다. 이는 다른 위치 및 SDK에서도 마찬가지일 수 있습니다. 정말 의도하지 않는 한 태그 키에 후행 공백을 추가하지 않도록 하세요. (2018년 7월 기준)  
🔸 조직 전체에서 리소스를 공유할 때는 태그가 공유되지 않습니다. 예를 들어 대중교통 게이트웨이 또는 AMI를 공유하면 해당 리소스를 만든 계정에는 올바른 태그가 표시되지만 해당 리소스를 공유한 계정에는 태그가 표시되지 않습니다.  
  
# Managing Servers and Applications  
  
이 가이드는 DevOps나 서버 구성 관리 일반이 아닌 AWS에 관한 것입니다. 하지만 AWS에 대해 자세히 알아보기 전에 AWS 리소스에 대한 구성 관리 외에도 서버 자체에 대한 구성 관리라는 오랜 문제가 있다는 점에 주목할 필요가 있습니다.  
  
## AWS vs Server Configuration  
  
This guide is about AWS, not DevOps or server configuration management in general. But before getting into AWS in detail, it’s worth noting that in addition to the configuration management for your AWS resources, there is the long-standing problem of configuration management for servers themselves.  
  
  
## Philosophy  
  
-	Heroku’s [**Twelve-Factor App**](http://12factor.net/) principles list some established general best practices for deploying applications.  
-	**Pets vs cattle:** Treat servers [like cattle, not pets](https://www.engineyard.com/blog/pets-vs-cattle). That is, design systems so infrastructure is disposable. It should be minimally worrisome if a server is unexpectedly destroyed.  
-	The concept of [**immutable infrastructure**](http://radar.oreilly.com/2015/06/an-introduction-to-immutable-infrastructure.html) is an extension of this idea.  
-	Minimize application state on EC2 instances. In general, instances should be able to be killed or die unexpectedly with minimal impact. State that is in your application should quickly move to RDS, S3, DynamoDB, EFS, or other data stores not on that instance. EBS is also an option, though it generally should not be the bootable volume, and EBS will require manual or automated re-mounting.  
  
  
## Server Configuration Management  
  
-	There is a [large set](https://en.wikipedia.org/wiki/Comparison_of_open-source_configuration_management_software) of open source tools for managing configuration of server instances.  
-	These are generally not dependent on any particular cloud infrastructure, and work with any variety of Linux (or in many cases, a variety of operating systems).  
-	Leading configuration management tools are [Puppet](https://github.com/puppetlabs/puppet), [Chef](https://github.com/chef/chef), [Ansible](https://github.com/ansible/ansible), and [Saltstack](https://github.com/saltstack/salt). These aren’t the focus of this guide, but we may mention them as they relate to AWS.  
  
  
## Containers and AWS  
  
-	[Docker](http://blog.scottlowe.org/2014/03/11/a-quick-introduction-to-docker/) and the containerization trend are changing the way many servers and services are deployed in general.  
-	Containers are designed as a way to package up your application(s) and all of their dependencies in a known way. When you build a container, you are including every library or binary your application needs, outside of the kernel. A big advantage of this approach is that it’s easy to test and validate a container locally without worrying about some difference between your computer and the servers you deploy on.  
-	A consequence of this is that you need fewer AMIs and boot scripts; for most deployments, the only boot script you need is a template that fetches an exported docker image and runs it.  
-	Companies that are embracing [microservice architectures](http://martinfowler.com/articles/microservices.html) will often turn to container-based deployments.  
-	AWS launched [ECS](https://aws.amazon.com/ecs/) as a service to manage clusters via Docker in late 2014, though many people still deploy Docker directly themselves. See the [ECS section](AWS%20정리.md#ecs) for more details.  
-	AWS launched [EKS](https://aws.amazon.com/eks/) as a service to manage Kubernetes Clusters mid 2018, though many people still deploy ECS or use Docker directly themselves. See the [EKS section](AWS%20정리.md#eks) for more details.  
  
  
## Visibility  
  
-	Store and track instance metadata (such as instance id, availability zone, etc.) and deployment info (application build id, Git revision, etc.) in your logs or reports. The [**instance metadata service**](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) can help collect some of the AWS data you’ll need.  
-	**Use log management services:** Be sure to set up a way to view and manage logs externally from servers.  
	-	Cloud-based services such as [Sumo Logic](https://www.sumologic.com/), [Splunk Cloud](http://www.splunk.com/en_us/cloud.html), [Scalyr](https://www.scalyr.com/), [LogDNA](https://www.logdna.com/), and [Loggly](https://www.loggly.com/) are the easiest to set up and use (and also the most expensive, which may be a factor depending on how much log data you have).  
	-	Major open source alternatives include [Elasticsearch](https://github.com/elastic/elasticsearch), [Logstash](https://github.com/elastic/logstash), and [Kibana](https://github.com/elastic/kibana) (the “[Elastic Stack](https://www.elastic.co/webinars/introduction-elk-stack)”) and [Graylog](https://www.graylog.org/).  
	-	If you can afford it (you have little data or lots of money) and don’t have special needs, it makes sense to use hosted services whenever possible, since setting up your own scalable log processing systems is notoriously time consuming.  
-	**Track and graph metrics:** The AWS Console can show you simple graphs from CloudWatch, you typically will want to track and graph many kinds of metrics, from CloudWatch and your applications. Collect and export helpful metrics everywhere you can (and as long as volume is manageable enough you can afford it).  
	-	Services like [Librato](https://www.librato.com/), [KeenIO](https://keen.io/), and [Datadog](https://www.datadoghq.com/) have fancier features or better user interfaces that can save a lot of time. (A more detailed comparison is [here](http://blog.takipi.com/production-tools-guide/visualization-and-metrics/).)  
	-	Use [Prometheus](https://prometheus.io) or [Graphite](https://github.com/graphite-project/graphite-web) as timeseries databases for your metrics (both are open source).  
	-	[Grafana](https://github.com/grafana/grafana) can visualize with dashboards the stored metrics of both timeseries databases (also open source).  
  
인스턴스 메타데이터(예: 인스턴스 ID, 가용 영역 등)와 배포 정보(애플리케이션 빌드 ID, Git 리비전 등)를 로그 또는 보고서에 저장하고 추적하세요. 인스턴스 메타데이터 서비스는 필요한 AWS 데이터 중 일부를 수집하는 데 도움이 될 수 있습니다.  
로그 관리 서비스를 사용하세요: 서버 외부에서 로그를 보고 관리할 수 있는 방법을 설정해야 합니다.  
Sumo Logic, Splunk Cloud, Scalyr, LogDNA, Loggly와 같은 클라우드 기반 서비스는 설정과 사용이 가장 쉬우며, 로그 데이터의 양에 따라 비용이 가장 많이 들기도 합니다.  
주요 오픈 소스 대안으로는 Elasticsearch, Logstash, Kibana("Elastic Stack"), Graylog 등이 있습니다.  
확장 가능한 자체 로그 처리 시스템을 설정하는 것은 시간이 많이 걸리는 것으로 악명이 높기 때문에, 여유가 있고(데이터가 적거나 돈이 많거나) 특별한 요구 사항이 없는 경우에는 가능한 한 호스팅 서비스를 사용하는 것이 좋습니다.  
메트릭 추적 및 그래프: AWS 콘솔은 CloudWatch에서 간단한 그래프를 표시할 수 있으며, 일반적으로 CloudWatch와 애플리케이션에서 다양한 종류의 메트릭을 추적하고 그래프로 표시하고 싶을 것입니다. 가능한 모든 곳에서 유용한 메트릭을 수집하고 내보내세요(관리할 수 있을 만큼의 양만 있다면).  
Librato, KeenIO, Datadog 같은 서비스는 더 멋진 기능이나 더 나은 사용자 인터페이스를 갖추고 있어 시간을 많이 절약할 수 있습니다. (더 자세한 비교는 여기에서 확인하세요.)  
메트릭의 시계열 데이터베이스로 Prometheus 또는 Graphite를 사용하세요(둘 다 오픈 소스입니다).  
Grafana는 두 시계열 데이터베이스에 저장된 메트릭을 대시보드로 시각화할 수 있습니다(이 역시 오픈 소스).  
  
  
## Tips for Managing Servers  
  
-	❗**Timezone settings on servers**: unless *absolutely necessary*, always **set the timezone on servers to [UTC](https://en.wikipedia.org/wiki/Coordinated_Universal_Time)** (see instructions for your distribution, such as [Ubuntu](https://www.digitalocean.com/community/tutorials/how-to-set-up-timezone-and-ntp-synchronization-on-ubuntu-14-04-quickstart), [CentOS](https://www.vultr.com/docs/setup-timezone-and-ntp-on-centos-6) or [Amazon](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html) Linux). Numerous distributed systems rely on time for synchronization and coordination and UTC [provides](https://blog.serverdensity.com/set-your-server-timezone-to-utc/) the universal reference plane: it is not subject to  daylight savings changes and adjustments in local time. It will also save you a lot of headache debugging [elusive timezone issues](http://yellerapp.com/posts/2015-01-12-the-worst-server-setup-you-can-make.html) and provide coherent timeline of events in your logging and audit systems.  
-	**NTP and accurate time:** If you are not using Amazon Linux (which comes preconfigured), you should confirm your servers [configure NTP correctly](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/set-time.html#configure_ntp), to avoid insidious time drift (which can then cause all sorts of issues, from breaking API calls to misleading logs). This should be part of your automatic configuration for every server. If time has already drifted substantially (generally >1000 seconds), remember NTP won’t shift it back, so you may need to remediate manually (for example, [like this](http://askubuntu.com/questions/254826/how-to-force-a-clock-update-using-ntp) on Ubuntu).  
-	**Testing immutable infrastructure:** If you want to be proactive about testing your service’s ability to cope with instance termination or failure, it can be helpful to introduce random instance termination during business hours, which will expose any such issues at a time when engineers are available to identify and fix them. Netflix’s [Simian Army](https://github.com/Netflix/SimianArmy) (specifically, [Chaos Monkey](https://github.com/Netflix/SimianArmy/wiki/Chaos-Monkey)) is a popular tool for this. Alternatively, [chaos-lambda](https://github.com/bbc/chaos-lambda) by the BBC is a lightweight option which runs on AWS [Lambda](AWS%20정리.md#lambda).  
  
# Security and IAM  
  
  
We cover security basics first, since configuring user accounts is something you usually have to do early on when setting up your system.  
  
## Security and IAM Basics  
  
-	📒 IAM [Homepage](https://aws.amazon.com/iam/) ∙ [User guide](https://docs.aws.amazon.com/IAM/latest/UserGuide/introduction.html) ∙ [FAQ](https://aws.amazon.com/iam/faqs/)  
-	The [AWS Security Blog](https://blogs.aws.amazon.com/security) is one of the best sources of news and information on AWS security.  
-	**IAM** is the service you use to manage accounts and permissioning for AWS.  
-	Managing security and access control with AWS is critical, so every AWS administrator needs to use and understand IAM, at least at a basic level.  
-	[IAM identities](https://docs.aws.amazon.com/IAM/latest/UserGuide/id.html) include users (people or services that are using AWS), groups (containers for sets of users and their permissions), and roles (containers for permissions assigned to AWS service instances). [Permissions](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_permissions.html) for these identities are governed by [policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) You can use AWS pre-defined policies or custom policies that you create.  
-	IAM manages various kinds of authentication, for both users and for software services that may need to authenticate with AWS, including:  
	-	[**Passwords**](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_passwords.html) to log into the console. These are a username and password for real users.  
	-	[**Access keys**](https://docs.aws.amazon.com/IAM/latest/UserGuide/id_credentials_access-keys.html), which you may use with command-line tools. These are two strings, one the “id”, which is an upper-case alphabetic string of the form 'AXXXXXXXXXXXXXXXXXXX', and the other is the secret, which is a 40-character mixed-case base64-style string. These are often set up for services, not just users.  
		-	📜 Access keys that start with AKIA are normal keys. Access keys that start with ASIA are session/temporary keys from STS, and will require an additional "SessionToken" parameter to be sent along with the id and secret. See the documentation for [a complete list of access key prefixes](https://docs.aws.amazon.com/IAM/latest/UserGuide/reference_identifiers.html#identifiers-prefixes).  
	-	[**Multi-factor authentication (MFA)**](https://aws.amazon.com/iam/details/mfa/), which is the highly recommended practice of using a keychain fob or smartphone app as a second layer of protection for user authentication.  
-	IAM allows complex and fine-grained control of permissions, dividing users into groups, assigning permissions to roles, and so on. There is a [policy language](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies.html) that can be used to customize security policies in a fine-grained way.  
	-	An excellent high level overview of IAM policy concepts lives at [IAM Policies In A Nutshell](http://start.jcolemorrison.com/aws-iam-policies-in-a-nutshell/).  
	-	🔸The policy language has a complex and error-prone JSON syntax that’s quite confusing, so unless you are an expert, it is wise to base yours off trusted examples or AWS’ own pre-defined [managed policies](https://docs.aws.amazon.com/IAM/latest/UserGuide/access_policies_managed-vs-inline.html).  
-	At the beginning, IAM policy may be very simple, but for large systems, it will grow in complexity, and need to be managed with care.  
	-	🔹Make sure one person (perhaps with a backup) in your organization is formally assigned ownership of managing IAM policies, make sure every administrator works with that person to have changes reviewed. This goes a long way to avoiding accidental and serious misconfigurations.  
-	It is best to give each user or service the minimum privileges needed to perform their duties. This is the [principle of least privilege](https://en.wikipedia.org/wiki/Principle_of_least_privilege), one of the foundations of good security. Organize all IAM users and groups according to levels of access they need.  
-	IAM has the [permission hierarchy](http://docs.aws.amazon.com/IAM/latest/UserGuide/reference_policies_evaluation-logic.html) of:  
	1. Explicit deny: The most restrictive policy wins.  
	2. Explicit allow: Access permissions to any resource has to be explicitly given.  
	3. Implicit deny: All permissions are implicitly denied by default.  
-	You can test policy permissions via the AWS IAM [policy simulator tool](https://policysim.aws.amazon.com/home/index.jsp). This is particularly useful if you write custom policies.  
  
  
  
## Security and IAM Tips  
  
-	🔹Use IAM to create individual user accounts and **use IAM accounts for all users from the beginning**. This is slightly more work, but not that much.  
	-	That way, you define different users, and groups with different levels of privilege (if you want, choose from Amazon’s default suggestions, of administrator, power user, etc.).  
	-	This allows credential revocation, which is critical in some situations. If an employee leaves, or a key is compromised, you can revoke credentials with little effort.  
	-	You can set up [Active Directory federation](https://blogs.aws.amazon.com/security/post/Tx71TWXXJ3UI14/Enabling-Federation-to-AWS-using-Windows-Active-Directory-ADFS-and-SAML-2-0) to use organizational accounts in AWS.  
-	❗**Enable [MFA](https://aws.amazon.com/iam/details/mfa/)** on your account.  
	-	You should always use MFA, and the sooner the better — enabling it when you already have many users is extra work.  
	-	Unfortunately it can’t be enforced in software, so an administrative policy has to be established.  
	-	Most users can use the Google Authenticator app (on [iOS](https://itunes.apple.com/us/app/google-authenticator/id388497605) or [Android](https://play.google.com/store/apps/details?id=com.google.android.apps.authenticator2)) to support two-factor authentication. For the root account, consider a hardware fob.  
- ❗Restrict use of significant IAM credentials as much as possible. Remember that in the cloud, loss of a highly capable IAM credential could essentially mean “game over,” for your deployment, your users, or your whole company.  
  -	**Do NOT use the [Root User account](http://docs.aws.amazon.com/IAM/latest/UserGuide/id_root-user.html)** other than when you initially create your account.  Create custom IAM users and/or roles and use those for your applications instead.  
	- Lock up access and use of the root credentials as much as possible. Ideally they should be effectively “offline.” For critical deployments, this means attached to an actual MFA device, physically secured and rarely used.  
-	❗**Turn on CloudTrail:** One of the first things you should do is [enable CloudTrail](https://docs.aws.amazon.com/awscloudtrail/latest/userguide/cloudtrail-create-a-trail-using-the-console-first-time.html). Even if you are not a security hawk, there is little reason not to do this from the beginning, so you have data on what has been happening in your AWS account should you need that information. You’ll likely also want to set up a [log management service](AWS%20정리.md#visibility) to search and access these logs.  
-	🔹**Use IAM roles for EC2:** Rather than assign IAM users to applications like services and then sharing the sensitive credentials, [define and assign roles to EC2 instances](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html) and have applications retrieve credentials from the [instance metadata](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html).  
-	Assign IAM roles by realm — for example, to development, staging, and production. If you’re setting up a role, it should be tied to a specific realm so you have clean separation. This prevents, for example, a development instance from connecting to a production database.  
-	**Best practices:** AWS’ [list of best practices](https://docs.aws.amazon.com/IAM/latest/UserGuide/best-practices.html) is worth reading in full up front.  
-	**IAM Reference:** [This interactive reference for all IAM actions, effects, and resources](https://iam.cloudonaut.io/) is great to have open while writing new or trying to understand existing IAM policies.  
-	**Multiple accounts:** Decide on whether you want to use multiple AWS accounts and [research](https://dab35129f0361dca3159-2fe04d8054667ffada6c4002813eccf0.ssl.cf1.rackcdn.com/downloads/pdfs/Rackspace%20Best%20Practices%20for%20AWS%20-%20Identity%20Managment%20-%20Billing%20-%20Auditing.pdf) how to organize access across them. Factors to consider:  
	-	Number of users  
	-	Importance of isolation  
		-	Resource Limits  
		-	Permission granularity  
		-	Security  
		-	API Limits  
	-	Regulatory issues  
	-	Workload  
	-	Size of infrastructure  
	-	Cost of multi-account “overhead”: Internal AWS service management tools may need to be custom built or adapted.  
	-	🔹It can help to use separate AWS accounts for independent parts of your infrastructure if you expect a high rate of AWS API calls, since AWS [throttles calls](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/query-api-troubleshooting.html#api-request-rate) at the AWS account level.  
-	[**Inspector**](https://aws.amazon.com/inspector/) is an automated security assessment service from AWS that helps identify common security risks. This allows validation that you adhere to certain security practices and may help with compliance.  
-	[**Trusted Advisor**](https://aws.amazon.com/blogs/aws/trusted-advisor-console-basic/) addresses a variety of best practices, but also offers some basic security checks around IAM usage, security group configurations, and MFA. At paid support tiers, Trusted Advisor exposes additional checks around other areas, such as reserved instance optimization.  
-	**Use KMS for managing keys**: AWS offers [KMS](AWS%20정리.md#kms) for securely managing encryption keys, which is usually a far better option than handling key security yourself. See [below](AWS%20정리.md#kms).  
-	[**AWS WAF**](https://aws.amazon.com/waf) is a web application firewall to help you protect your applications from common attack patterns.  
-	**Security auditing:**  
	-	[Security Monkey](https://github.com/Netflix/security_monkey) is an open source tool that is designed to assist with security audits.  
	-	[Scout2](https://github.com/nccgroup/Scout2) is an open source tool that uses AWS APIs to assess an environment’s security posture. Scout2 is stable and actively maintained.  
	-	🔹**Export and audit security settings:** You can audit security policies simply by exporting settings using AWS APIs, e.g. using a Boto script like [SecConfig.py](https://gist.github.com/jlevy/cce1b44fc24f94599d0a4b3e613cc15d) (from [this 2013 talk](http://www.slideshare.net/AmazonWebServices/intrusion-detection-in-the-cloud-sec402-aws-reinvent-2013)) and then reviewing and monitoring changes manually or automatically.  
  
  
## Security and IAM Gotchas and Limitations  
  
-	❗**Don’t share user credentials:** It’s remarkably common for first-time AWS users to create one account and one set of credentials (access key or password), and then use them for a while, sharing among engineers and others within a company. This is easy. But *don’t do this*. This is an insecure practice for many reasons, but in particular, if you do, you will have reduced ability to revoke credentials on a per-user or per-service basis (for example, if an employee leaves or a key is compromised), which can lead to serious complications.  
-	❗**Instance metadata throttling:** The [instance metadata service](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html) has rate limiting on API calls. If you deploy IAM roles widely (as you should!) and have lots of services, you may hit global account limits easily.  
	-	One solution is to have code or scripts cache and reuse the credentials locally for a short period (say 2 minutes). For example, they can be put into the ~/.aws/credentials file but must also be refreshed automatically.  
	-	But be careful not to cache credentials for too long, as [they expire](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/iam-roles-for-amazon-ec2.html#instance-metadata-security-credentials). (Note the other [dynamic metadata](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html#dynamic-data-categories) also changes over time and should not be cached a long time, either.)  
-	🔸Some IAM operations are slower than other API calls (many seconds), since AWS needs to propagate these globally across regions.  
-	❗The uptime of IAM’s API has historically been lower than that of the instance metadata API. Be wary of incorporating a dependency on IAM’s API into critical paths or subsystems — for example, if you validate a user’s IAM group membership when they log into an instance and aren’t careful about precaching group membership or maintaining a back door, you might end up locking users out altogether when the API isn’t available.  
-	❗**Don't check in AWS credentials or secrets to a git repository.** There are bots that scan GitHub looking for credentials. Use scripts or tools, such as [git-secrets](https://github.com/awslabs/git-secrets) to prevent anyone on your team from checking in sensitive information to your git repositories.  
  
# S3  
--  
  
### S3 Basics  
  
-	📒 [Homepage](https://aws.amazon.com/s3/) ∙ [Developer guide](https://docs.aws.amazon.com/AmazonS3/latest/dev/Welcome.html) ∙ [FAQ](https://aws.amazon.com/s3/faqs/) ∙ [Pricing](https://aws.amazon.com/s3/pricing/)  
-	**S3** (Simple Storage Service) is AWS’ standard cloud storage service, offering file (opaque “blob”) storage of arbitrary numbers of files of almost any size, from 0 to **5TB**. (Prior to [2011](https://aws.amazon.com/releasenotes/Amazon-S3/1917932037969964) the maximum size was 5 GB; larger sizes are now well supported via [multipart support](https://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html).)  
-	Items, or **objects**, are placed into named **buckets** stored with names which are usually called **keys**. The main content is the **value**.  
-	Objects are created, deleted, or updated. Large objects can be streamed, but you cannot modify parts of a value; you need to update the whole object. Partial data access can work via [S3 Select](https://aws.amazon.com/blogs/aws/s3-glacier-select/).  
-	Every object also has [**metadata**](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html), which includes arbitrary key-value pairs, and is used in a way similar to HTTP headers. Some metadata is system-defined, some are significant when serving HTTP content from buckets or CloudFront, and you can also define arbitrary metadata for your own use.  
-	**S3 URIs:** Although often bucket and key names are provided in APIs individually, it’s also common practice to write an S3 location in the form 's3://bucket-name/path/to/key' (where the key here is 'path/to/key'). (You’ll also see 's3n://' and 's3a://' prefixes [in Hadoop systems](https://cwiki.apache.org/confluence/display/HADOOP2/AmazonS3).)  
-	**S3 vs Glacier, EBS, and EFS:** AWS offers many storage services, and several besides S3 offer file-type abstractions. [Glacier](AWS%20정리.md#glacier) is for cheaper and infrequently accessed archival storage. [EBS](AWS%20정리.md#ebs), unlike S3, allows random access to file contents via a traditional filesystem, but can only be attached to one EC2 instance at a time. [EFS](AWS%20정리.md#efs) is a network filesystem many instances can connect to, but at higher cost. See the [comparison table](AWS%20정리.md#storage-durability-availability-and-price).  
  
  
## S3 Tips  
  
-	For most practical purposes, you can consider S3 capacity unlimited, both in total size of files and number of objects. The number of objects in a bucket is essentially also unlimited. Customers routinely have millions of objects.  
-   ❗**Permissions:**  
    -   🔸If you're storing business data on Amazon S3, it’s important to manage permissions sensibly. In 2017 companies like [Dow Jones and Verizon](http://www.techrepublic.com/article/massive-amazon-s3-breaches-highlight-blind-spots-in-enterprise-race-to-the-cloud/) saw data breaches due to poorly-chosen S3 configuration for sensitive data. Fixing this later can be a difficult task if you have a lot of assets and internal users.  
    -   🔸There are 3 different ways to grant permissions to access Amazon S3 content in your buckets.  
        + **IAM policies** use the familiar [Identity and Access Management](AWS%20정리.md#security-and-iam) permission scheme to control access to specific operations.  
        + **Bucket policies** grant or deny permissions to an entire bucket. You might use this when hosting a website in S3, to make the bucket publicly readable, or to restrict access to a bucket by IP address. Amazon's [sample bucket policies](http://docs.aws.amazon.com/AmazonS3/latest/dev/example-bucket-policies.html) show a number of use cases where these policies come in handy.  
        + **[Access Control Lists](http://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html)** (ACLs) can also be applied to every bucket and object stored in S3. ACLs grant additional permissions beyond those specified in IAM or bucket policies. ACLs can be used to grant access to another AWS user, or to predefined groups like the general public. This is powerful but can be dangerous, because you need to inspect every object to see who has access.  
    -   🔸AWS' [predefined access control groups](http://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#specifying-grantee-predefined-groups) allow access that may not be what you'd expect from their names:  
        +   **"All Users", or "Everyone", grants permission to the general public**, not only to users defined in your own AWS account. If an object is available to All Users, then it can be retrieved with a simple HTTP request of the form `http://s3.amazonaws.com/bucket-name/filename`. No authorization or signature is required to access data in this category.  
        +   **"Authenticated Users" grants permissions to anyone with an AWS account**, again not limited to your own users. Because anyone can sign up for AWS, for all intents and purposes **this is also open to the general public**.  
        +   **"Log Delivery" group is used by AWS to write logs to buckets** and should be safe to enable on the buckets that need it.  
        +   A typical use case of this ACL is used in conjunction with the [requester pays](http://docs.aws.amazon.com/AmazonS3/latest/dev/RequesterPaysBuckets.html) functionality of S3.  
    -   ❗ Bucket permissions and object permissions are two different things and independent of each other. A private object in a public bucket can be seen when listing the bucket, but not downloaded. At the same time, a public object in a private bucket won't be seen because the bucket contents can't be listed, but can still be downloaded by anyone who knows its exact key. Users that don't have access to set bucket permissions can still make objects public if they have `s3:PutObjectAcl` or `s3:PutObjectVersionAcl` [permissions](http://docs.aws.amazon.com/AmazonS3/latest/dev/using-with-s3-actions.html).  
    -   🐥In August 2017, AWS added [AWS Config rules to ensure your S3 buckets are secure](https://aws.amazon.com/blogs/aws/aws-config-update-new-managed-rules-to-secure-s3-buckets/).  
        +    ❗These AWS Config rules only check the security of your bucket policy and bucket-level ACLs. You can still create object ACLs that grant additional permissions, including opening files to the whole world.  
    -   🔹Do create new buckets if you have different types of data with different sensitivity levels. This is much less error prone than complex permissions rules. For example, if data is for administrators only, like log data, put it in a new bucket that only administrators can access.  
    -   For more guidance, see:  
        +   [How to Secure an Amazon S3 Bucket](https://read.acloud.guru/how-to-secure-an-s3-bucket-7e2dbd34e81b)  
        +   [Deep dive into S3 access controls](https://labs.detectify.com/2017/07/13/a-deep-dive-into-aws-s3-access-controls-taking-full-control-over-your-assets/).  
        +   [How do S3 permissions work?](https://brandonwamboldt.ca/understanding-s3-permissions-1662/).  
-	**Bucket naming:** Buckets are chosen from a global namespace (across all regions, even though S3 itself stores data in [whichever S3 region](https://docs.aws.amazon.com/general/latest/gr/rande.html#s3_region) you select), so you’ll find many bucket names are already taken. Creating a bucket means taking ownership of the name until you delete it. Bucket names have [a few restrictions](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html) on them.  
	-	Bucket names can be used as part of the hostname when accessing the bucket or its contents, like `<bucket_name>.s3-us-east-1.amazonaws.com`, as long as the name is [DNS compliant](http://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html).  
	-	A common practice is to use the company name acronym or abbreviation to prefix (or suffix, if you prefer DNS-style hierarchy) all bucket names (but please, don’t use a check on this as a security measure — this is highly insecure and easily circumvented!).  
	-	🔸Bucket names with '.' (periods) in them [can cause certificate mismatches](https://forums.aws.amazon.com/thread.jspa?threadID=169951) when used with SSL. Use '-' instead, since this then conforms with both SSL expectations and is DNS compliant.  
-	**Versioning:** S3 has [optional versioning support](https://docs.aws.amazon.com/AmazonS3/latest/dev/ObjectVersioning.html), so that all versions of objects are preserved on a bucket. This is mostly useful if you want an archive of changes or the ability to back out mistakes (caution: it lacks the featureset of full version control systems like Git).  
-	**Durability:** Durability of S3 is extremely high, since internally it keeps several replicas. If you don’t delete it by accident, you can count on S3 not losing your data. (AWS offers the seemingly improbable durability rate of [99.999999999%](https://aws.amazon.com/s3/faqs/#How_durable_is_Amazon_S3), but this is a mathematical calculation based on independent failure rates and levels of replication — not a true probability estimate. Either way, S3 has had [a very good record](https://www.quora.com/Has-Amazon-S3-ever-lost-data-permanently) of durability.) Note this is *much* higher durability than EBS!  
-	💸**S3 pricing** depends on [storage, requests, and transfer](https://aws.amazon.com/s3/pricing/).  
	-	For transfer, putting data into AWS is free, but you’ll pay on the way out. Transfer from S3 to EC2 in the *same region* is free. Transfer to other regions or the Internet in general is not free.  
	-	Deletes are free.  
-	**S3 Reduced Redundancy and Infrequent Access:** Most people use the Standard storage class in S3, but there are other storage classes with lower cost:  
	-	🔸[Reduced Redundancy Storage (RRS)](https://aws.amazon.com/s3/reduced-redundancy/) has been [effectively deprecated](https://www.lastweekinaws.com/blog/s3-reduced-redundancy-storage-is-dead/), and has lower durability (99.99%, so just four nines) than standard S3. Note that it no longer participates in S3 price reductions, so it offers worse redundancy for more money than standard S3. As a result, there's no reason to use it.  
	-	[Infrequent Access (IA)](https://aws.amazon.com/s3/storage-classes/#Infrequent_Access) lets you get cheaper storage in exchange for more expensive access. This is great for archives like logs you already processed, but might want to look at later. To get an idea of the cost savings when using Infrequent Access (IA), you can use this [S3 Infrequent Access Calculator](http://www.gulamshakir.com/apps/s3calc/index.html).  
	-	[S3 - Intelligent Tiering](https://aws.amazon.com/about-aws/whats-new/2018/11/s3-intelligent-tiering/) storage class is designed to optimize costs by automatically moving data to the most cost-effective access tier, without performance impact or operational overhead.  
	-	[S3 - One Zone - IA](https://aws.amazon.com/s3/storage-classes/#__) is for data that is accessed less frequently, but requires rapid access when needed. Unlike other S3 Storage Classes which store data in a minimum of three Availability Zones (AZs), S3 One Zone-IA stores data in a single AZ and costs 20% less than S3 Standard-IA.  
	-	[Glacier](AWS%20정리.md#glacier) is a third alternative discussed as a separate product.  
	-	See [the comparison table](AWS%20정리.md#storage-durability-availability-and-price).  
-	⏱**Performance:** Maximizing S3 performance means improving overall throughput in terms of bandwidth and number of operations per second.  
	-	S3 is highly scalable, so in principle you can get arbitrarily high throughput. (A good example of this is [S3DistCp](https://docs.aws.amazon.com/ElasticMapReduce/latest/ReleaseGuide/UsingEMR_s3distcp.html).)  
	-	But usually you are constrained by the pipe between the source and S3 and/or the level of concurrency of operations.  
	-	Throughput is of course highest from within AWS to S3, and between EC2 instances and S3 buckets that are in the same region.  
	-	Bandwidth from EC2 depends on instance type. See the “Network Performance” column at [ec2instances.info](http://www.ec2instances.info/).  
	-	Throughput of many objects is extremely high when data is accessed in a distributed way, from many EC2 instances. It’s possible to read or write objects from S3 from hundreds or thousands of instances at once.  
	-	However, throughput is very limited when objects accessed sequentially from a single instance. Individual operations take many milliseconds, and bandwidth to and from instances is limited.  
	-	Therefore, to perform large numbers of operations, it’s necessary to use multiple worker threads and connections on individual instances, and for larger jobs, multiple EC2 instances as well.  
	-	**Multi-part uploads:** For large objects you want to take advantage of the multi-part uploading capabilities (starting with minimum chunk sizes of 5 MB).  
	-	**Large downloads:** Also you can download chunks of a single large object in parallel by exploiting the HTTP GET range-header capability.  
	-	🔸**List pagination:** Listing contents happens at 1000 responses per request, so for buckets with many millions of objects listings will take time.  
	-	❗**Key prefixes:** Previously randomness in the beginning of key names was necessary in order to avoid hot spots, but that is [no longer necessary](https://aws.amazon.com/about-aws/whats-new/2018/07/amazon-s3-announces-increased-request-rate-performance/) as of July, 2018.  
	-	For data outside AWS, [**DirectConnect**](https://aws.amazon.com/directconnect/) and [**S3 Transfer Acceleration**](https://aws.amazon.com/blogs/aws/aws-storage-update-amazon-s3-transfer-acceleration-larger-snowballs-in-more-regions/) can help. For S3 Transfer Acceleration, you [pay](https://aws.amazon.com/s3/pricing/) about the equivalent of 1-2 months of storage for the transfer in either direction for using nearer endpoints.  
-	**Command-line applications:** There are a few ways to use S3 from the command line:  
	-	Originally, [**s3cmd**](https://github.com/s3tools/s3cmd) was the best tool for the job. It’s still used heavily by many.  
	-	The regular [**aws**](https://aws.amazon.com/cli/) command-line interface now supports S3 well, and is useful for most situations.  
	-	[**s4cmd**](https://github.com/bloomreach/s4cmd) is a replacement, with greater emphasis on performance via multi-threading, which is helpful for large files and large sets of files, and also offers Unix-like globbing support.  
-	**GUI applications:** You may prefer a GUI, or wish to support GUI access for less technical users. Some options:  
	-	The [AWS Console](https://aws.amazon.com/console/) does offer a graphical way to use S3. Use caution telling non-technical people to use it, however, since without tight permissions, it offers access to many other AWS features.  
	-	[Transmit](https://panic.com/transmit/) is a good option on macOS for most use cases.  
	-	[Cyberduck](https://cyberduck.io/) is a good option on macOS and Windows with support for multipart uploads, ACLs, versioning, lifecycle configuration, storage classes and server side encryption (SSE-S3 and SSE-KMS).  
-	**S3 and CloudFront:** S3 is tightly integrated with the CloudFront CDN. See the CloudFront section for more information, as well as [S3 transfer acceleration](https://docs.aws.amazon.com/AmazonS3/latest/dev/transfer-acceleration.html).  
-	**Static website hosting:**  
	-	S3 has a [static website hosting option](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteHosting.html) that is simply a setting that enables configurable HTTP index and error pages and [HTTP redirect support](http://docs.aws.amazon.com/AmazonS3/latest/dev/how-to-page-redirect.html) to [public content](http://docs.aws.amazon.com/AmazonS3/latest/dev/WebsiteAccessPermissionsReqd.html) in S3. It’s a simple way to host static assets or a fully static website.  
	-	Consider using CloudFront in front of most or all assets:  
		-	Like any CDN, CloudFront improves performance significantly.  
		-	🔸SSL is only supported on the built-in amazonaws.com domain for S3. S3 supports serving these sites through a [custom domain](http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html), but [not over SSL on a custom domain](http://stackoverflow.com/questions/11201316/how-to-configure-ssl-for-amazon-s3-bucket). However, [CloudFront allows you to serve a custom domain over https](http://docs.aws.amazon.com/acm/latest/userguide/gs-cf.html). Amazon provides free SNI SSL/TLS certificates via Amazon Certificate Manager. [SNI does not work on very outdated browsers/operating systems](https://en.wikipedia.org/wiki/Server_Name_Indication#Support). Alternatively, you can provide your own certificate to use on CloudFront to support all browsers/operating systems for a fee.  
		-	🔸If you are including resources across domains, such as fonts inside CSS files, you may need to [configure CORS](https://docs.aws.amazon.com/AmazonS3/latest/dev/cors.html) for the bucket serving those resources.  
		-	Since pretty much everything is moving to SSL nowadays, and you likely want control over the domain, you probably want to set up CloudFront with your own certificate in front of S3 (and to ignore the [AWS example on this](http://docs.aws.amazon.com/AmazonS3/latest/dev/website-hosting-custom-domain-walkthrough.html) as it is non-SSL only).  
		-	That said, if you do, you’ll need to think through invalidation or updates on CloudFront. You may wish to [include versions or hashes in filenames](https://abhishek-tiwari.com/CloudFront-design-patterns-and-best-practices) so invalidation is not necessary.  
-	**Data lifecycles:**  
	-	When managing data, the understanding the lifecycle of the data is as important as understanding the data itself. When putting data into a bucket, think about its lifecycle — its end of life, not just its beginning.  
	-	🔹In general, data with different expiration policies should be stored under separate prefixes at the top level. For example, some voluminous logs might need to be deleted automatically monthly, while other data is critical and should never be deleted. Having the former in a separate bucket or at least a separate folder is wise.  
	-	🔸Thinking about this up front will save you pain. It’s very hard to clean up large collections of files created by many engineers with varying lifecycles and no coherent organization.  
	-	Alternatively you can set a lifecycle policy to archive old data to Glacier. [Be careful](https://alestic.com/2012/12/s3-glacier-costs/) with archiving large numbers of small objects to Glacier, since it may actually cost more.  
	-	There is also a storage class called [**Infrequent Access**](https://aws.amazon.com/s3/storage-classes/#Infrequent_Access) that has the same durability as Standard S3, but is discounted per GB. It is suitable for objects that are infrequently accessed.  
-	**Data consistency:** Understanding [data consistency](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html#ConsistencyModel) is critical for any use of S3 where there are multiple producers and consumers of data.  
	-	Creation and updates to individual objects in S3 are **atomic**, in that you’ll never upload a new object or change an object and have another client see only part half the change.  
	- The uncertainty lies with *when* your clients and other clients see updates.  
	-	**New objects:** If you create a new object, you’ll be able to read it instantly, which is called **read-after-write consistency**.  
		-	Well, with the additional caveat that if you do a read on an object before it exists, then create it, [you get eventual consistency](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html#ConsistencyModel) (not read-after-write).  
		-	This does not apply to any list operations; newly created objects are [not guaranteed to appear in a list operation right away](https://docs.aws.amazon.com/AmazonS3/latest/dev/Introduction.html#ConsistencyModel)  
	-	**Updates to objects:** If you overwrite or delete an object, you’re only guaranteed **eventual consistency**, i.e. the change will happen but you have no guarantee of when.  
	- 🔹For many use cases, treating S3 objects as **immutable** (i.e. deciding by convention they will be created or deleted but not updated) can greatly simplify the code that uses them, avoiding complex state management.  
	-	🔹Note that [until 2015](https://aws.amazon.com/about-aws/whats-new/2015/08/amazon-s3-introduces-new-usability-enhancements/), 'us-standard' region had had a weaker eventual consistency model, and the other (newer) regions were read-after-write. This was finally corrected — but watch for many old blogs mentioning this!  
	-	**Slow updates:** In practice, “eventual consistency” usually means within seconds, but expect rare cases of minutes or [hours](https://web.archive.org/web/20160324095125/http://www.stackdriver.com/eventual-consistency-really-eventual/).  
-	**S3 as a filesystem:**  
	-	In general S3’s APIs have inherent limitations that make S3 hard to use directly as a POSIX-style filesystem while still preserving S3’s own object format. For example, appending to a file requires rewriting, which cripples performance, and atomic rename of directories, mutual exclusion on opening files, and hardlinks are impossible.  
	-	[s3fs](https://github.com/s3fs-fuse/s3fs-fuse) is a FUSE filesystem that goes ahead and tries anyway, but it has performance limitations and surprises for these reasons.  
	-	[Riofs](https://github.com/skoobe/riofs) (C) and [Goofys](https://github.com/kahing/goofys) (Go) are more recent efforts that attempt adopt a different data storage format to address those issues, and so are likely improvements on s3fs.  
	-	[S3QL](https://github.com/s3ql/s3ql) ([discussion](https://news.ycombinator.com/item?id=10150684)) is a Python implementation that offers data de-duplication, snap-shotting, and encryption, but only one client at a time.  
	-	[ObjectiveFS](https://objectivefs.com/) ([discussion](https://news.ycombinator.com/item?id=10117506)) is a commercial solution that supports filesystem features and concurrent clients.  
-	If you are primarily using a VPC, consider setting up a [VPC Endpoint](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html) for S3 in order to allow your VPC-hosted resources to easily access it without the need for extra network configuration or hops.  
-	**Cross-region replication:** S3 has [a feature](https://docs.aws.amazon.com/AmazonS3/latest/dev/crr.html) for replicating a bucket between one region and another. Note that S3 is already highly replicated within one region, so usually this isn’t necessary for durability, but it could be useful for compliance (geographically distributed data storage), lower latency, or as a strategy to reduce region-to-region bandwidth costs by mirroring heavily used data in a second region.  
-	**IPv4 vs IPv6:** For a long time S3 only supported IPv4 at the default endpoint `https://BUCKET.s3.amazonaws.com`. However, [as of Aug 11, 2016](https://aws.amazon.com/blogs/aws/now-available-ipv6-support-for-amazon-s3/) it now supports both IPv4 & IPv6! To use both, you have to [enable dualstack](http://docs.aws.amazon.com/AmazonS3/latest/dev/dual-stack-endpoints.html) either in your preferred API client or by directly using this url scheme `https://BUCKET.s3.dualstack.REGION.amazonaws.com`. This extends to S3 Transfer Acceleration as well.  
-	**S3 event notifications:** S3 can be configured to send an [SNS notification](https://aws.amazon.com/blogs/aws/introducing-the-amazon-simple-notification-service/), [SQS message](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/Welcome.html), or [AWS Lambda function](http://docs.aws.amazon.com/lambda/latest/dg/welcome.html) on [bucket events](http://docs.aws.amazon.com/AmazonS3/latest/dev/NotificationHowTo.html).  
-   💸Limit your individual users (or IAM roles) to the minimal required S3 locations, and catalog the “approved” locations. Otherwise, S3 tends to become the dumping ground where people put data to random locations that are not cleaned up for years, costing you big bucks.  
- If a bucket is deleted in S3, it can take up to 10 hours before a bucket with the same name can be created again. ([discussion](https://forums.aws.amazon.com/thread.jspa?threadID=37532))  
  
  
## S3 Gotchas and Limitations  
  
-   ❗S3 buckets sit outside the VPC and can be accessed from anywhere in the world if bucket policies are not set to deny it. Read the permissions section above carefully, there are countless cases of buckets exposed to the public.  
-	🔸For many years, there was a notorious [**100-bucket limit**](https://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_s3) per account, which could not be raised and caused many companies significant pain. As of 2015, you can [request increases](https://aws.amazon.com/about-aws/whats-new/2015/08/amazon-s3-introduces-new-usability-enhancements/). You can ask to increase the limit, but it will still be capped (generally below ~1000 per account).  
-	🔸Be careful not to make implicit assumptions about transactionality or sequencing of updates to objects. Never assume that if you modify a sequence of objects, the clients will see the same modifications in the same sequence, or if you upload a whole bunch of files, that they will all appear at once to all clients.  
-	🔸S3 has an [**SLA**](https://aws.amazon.com/s3/sla/) with 99.9% uptime. If you use S3 heavily, you’ll inevitably see occasional error accessing or storing data as disks or other infrastructure fail. Availability is usually restored in seconds or minutes. Although availability is not extremely high, as mentioned above, durability is excellent.  
-	🔸After uploading, any change that you make to the object causes a full rewrite of the object, so avoid appending-like behavior with regular files.  
-	🔸Eventual data consistency, as discussed above, can be surprising sometimes. If S3 suffers from internal replication issues, an object may be visible from a subset of the machines, depending on which S3 endpoint they hit. Those usually resolve within seconds; however, we’ve seen isolated cases when the issue lingered for 20-30 hours.  
-	🔸**MD5s and multi-part uploads:** In S3, the [ETag header in S3](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTCommonResponseHeaders.html) is a hash on the object. And in many cases, it is the MD5 hash. However, this [is not the case in general](http://stackoverflow.com/questions/12186993/what-is-the-algorithm-to-compute-the-amazon-s3-etag-for-a-file-larger-than-5gb) when you use multi-part uploads. One workaround is to compute MD5s yourself and put them in a custom header (such as is done by [s4cmd](https://github.com/bloomreach/s4cmd)).  
-	🔸**Incomplete multi-part upload costs:** Incomplete multi-part uploads accrue [storage charges](http://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html#mpuploadpricing) even if the upload fails and no S3 object is created. [Amazon](http://docs.aws.amazon.com/AmazonS3/latest/dev/mpuoverview.html#mpu-abort-incomplete-mpu-lifecycle-config) ([and](http://www.deplication.net/2016/06/aws-tip-save-s3-costs-with-abort.html) [others](https://www.sumologic.com/aws/s3/s3-cost-optimization/)) recommend using a lifecycle policy to clean up incomplete uploads and save on storage costs. Note that if you have many of these, it may be worth investigating whatever's failing regularly.  
-	🔸**US Standard region:** Previously, the us-east-1 region (also known as the US Standard region) was replicated across coasts, which led to greater variability of latency. Effective Jun 19, 2015 this is [no longer the case](https://forums.aws.amazon.com/ann.jspa?annID=3112). All Amazon S3 regions now support read-after-write consistency. Amazon S3 also renamed the US Standard region to the US East (N. Virginia) region to be consistent with AWS regional naming conventions.  
- 🔸**S3 authentication versions and regions:** In newer regions, S3 [only supports the latest authentication](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingAWSSDK.html#specify-signature-version). If an S3 file operation using CLI or SDK doesn't work in one region, but works correctly in another region, make sure you are using the latest [authentication signature](https://docs.aws.amazon.com/AmazonS3/latest/API/sig-v4-authenticating-requests.html).  
  
  
## Storage Durability, Availability, and Price  
  
As an illustration of comparative features and price, the table below gives S3 Standard, RRS, IA, in comparison with [Glacier](AWS%20정리.md#glacier), [EBS](AWS%20정리.md#ebs), [EFS](AWS%20정리.md#efs), and EC2 d2.xlarge instance store using **Virginia region** as of **Sept 2017**.  
  
|                 | Durability (per year)  | Availability “designed” | Availability SLA | Storage (per TB per month)                                                                                               | GET or retrieve (per million) | Write or archive (per million) |  
|-----------------|------------------------|-------------------------|------------------|--------------------------------------------------------------------------------------------------------------------------|-------------------------------|--------------------------------|  
| **Glacier**     | Eleven 9s              | Sloooow                 | –                | $4                                                                                                                       | $50                           | $50                            |  
| **S3 IA**       | Eleven 9s              | 99.9%                   | **99%**          | $12.50                                                                                                                   | $1                            | $10                            |  
| ~~**S3 RRS**~~      | ~~**99.99%**~~             | ~~99.99%~~                  | ~~99.9%~~            | ~~$24 (first TB)~~                                                                                                                      | ~~$0.40~~                         | ~~$5~~                             |  
| **S3 Standard** | Eleven 9s              | 99.99%                  | 99.9%            | $23                                                                                                                     | $0.40                         | $5                             |  
| **EBS**         | **99.8%**              | Unstated                | 99.99%           | $25/$45/**$100**/$125+ ([sc1/st1/**gp2**/io1](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/EBSVolumeTypes.html)\) |                               |                                |  
| **EFS**         | “High”                 | “High”                  | –                | $300                                                                                                                     |                               |                                |  
| **EC2 d2.xlarge instance store**  | Unstated | Unstated                | –                | $25.44                                                                                                                   | $0                            | $0                             |  
  
Especially notable items are in **boldface**. Sources: [S3 pricing](https://aws.amazon.com/s3/pricing/), [S3 SLA](https://aws.amazon.com/s3/sla/), [S3 FAQ](https://aws.amazon.com/s3/faqs/), [RRS info](https://aws.amazon.com/s3/reduced-redundancy/) (note that this is considered deprecated), [Glacier pricing](https://aws.amazon.com/glacier/pricing/), [EBS availability and durability](https://aws.amazon.com/ebs/details/#Amazon_EBS_Availability_and_Durability), [EBS pricing](https://aws.amazon.com/ebs/pricing/), [EFS pricing](https://aws.amazon.com/efs/pricing/), [EC2 SLA](https://aws.amazon.com/ec2/sla/)  
  
# EC2  
  
  
## EC2 Basics  
  
-	📒 [Homepage](https://aws.amazon.com/ec2/) ∙ [Documentation](https://aws.amazon.com/documentation/ec2/) ∙ [FAQ](https://aws.amazon.com/ec2/faqs/) ∙ [Pricing](https://aws.amazon.com/ec2/pricing/) (see also [ec2instances.info](http://www.ec2instances.info/)\)  
-	**EC2** (Elastic Compute Cloud) is AWS’ offering of the most fundamental piece of cloud computing: A [virtual private server](https://en.wikipedia.org/wiki/Virtual_private_server). These “instances” can run [most Linux, BSD, and Windows operating systems](https://aws.amazon.com/ec2/faqs/#What_operating_system_environments_are_supported). Internally, they've used a heavily modified [Xen](https://en.wikipedia.org/wiki/Xen) virtualization. That said, new instance classes are being introduced with a KVM derived hypervisor instead, called [Nitro](http://www.brendangregg.com/blog/2017-11-29/aws-ec2-virtualization-2017.html). So far, this is limited to the C5 and M5 instance types. Lastly, there's a "bare metal hypervisor" available for [i3.metal instances](https://aws.amazon.com/about-aws/whats-new/2018/05/announcing-general-availability-of-amazon-ec2-bare-metal-instances/)  
-	The term “EC2” is sometimes used to refer to the servers themselves, but technically refers more broadly to a whole collection of supporting services, too, like load balancing (CLBs/ALBs/NLBs), IP addresses (EIPs), bootable images (AMIs), security groups, and network drives (EBS) (which we discuss individually in this guide).  
-	**💸[EC2 pricing](https://aws.amazon.com/ec2/pricing/)** and **[cost management](AWS%20정리.md#ec2-cost-management)** is a complicated topic. It can range from free (on the [AWS free tier](https://aws.amazon.com/free/)) to a lot, depending on your usage. Pricing is by instance type, by second or hour, and changes depending on AWS region and whether you are purchasing your instances [On-Demand](https://aws.amazon.com/ec2/pricing/on-demand/), on the [Spot market](https://aws.amazon.com/ec2/spot/) or pre-purchasing ([Reserved Instances](https://aws.amazon.com/ec2/pricing/reserved-instances/)).  
- **Network Performance:** For some instance types, AWS uses general terms like Low, Medium, and High to refer to network performance. Users have done [benchmarking](http://stackoverflow.com/questions/18507405/ec2-instance-typess-exact-network-performance) to provide expectations for what these terms can mean.  
  
  
  
## EC2 Alternatives and Lock-In  
  
-	Running EC2 is akin to running a set of physical servers, as long as you don’t do automatic scaling or tooled cluster setup. If you just run a set of static instances, migrating to another VPS or dedicated server provider should not be too hard.  
-	🚪**Alternatives to EC2:** The direct alternatives are Google Cloud, Microsoft Azure, Rackspace, DigitalOcean, AWS's own Lightsail offering, and other VPS providers, some of which offer similar APIs for setting up and removing instances. (See the comparisons [above](AWS%20정리.md#when-to-use-aws).)  
-	**Should you use Amazon Linux?** AWS encourages use of their own [Amazon Linux](https://aws.amazon.com/amazon-linux-ami/), which is evolved from [Red Hat Enterprise Linux (RHEL)](https://en.wikipedia.org/wiki/Red_Hat_Enterprise_Linux) and [CentOS](https://en.wikipedia.org/wiki/CentOS). It’s used by many, but [others are skeptical](https://www.exratione.com/2014/08/do-not-use-amazon-linux/). Whatever you do, think this decision through carefully. It’s true Amazon Linux is heavily tested and better supported in the unlikely event you have deeper issues with OS and virtualization on EC2. But in general, many companies do just fine using a standard, non-Amazon Linux distribution, such as Ubuntu or CentOS. Using a standard Linux distribution means you have an exactly replicable environment should you use another hosting provider instead of (or in addition to) AWS. It’s also helpful if you wish to test deployments on local developer machines running the same standard Linux distribution (a practice that’s getting more common with Docker, too. Amazon now supports an official [Amazon Linux Docker image](http://docs.aws.amazon.com/AmazonECR/latest/userguide/amazon_linux_container_image.html), aimed at assisting with local development on a comparable environment, though this is new enough that it should be considered experimental). Note that the currently-in-testing [Amazon Linux 2](https://aws.amazon.com/about-aws/whats-new/2017/12/introducing-amazon-linux-2/) supports on-premise deployments explicitly.  
-	**EC2 costs:** See the [section on this](AWS%20정리.md#ec2-cost-management).  
  
  
## EC2 Tips  
  
-	🔹**Picking regions:** When you first set up, consider which [regions](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-available-regions) you want to use first. Many people in North America just automatically set up in the us-east-1 (N. Virginia) region, which is the default, but it’s worth considering if this is best up front. You'll want to evaluate service availability (some services [are not available in all regions](https://aws.amazon.com/about-aws/global-infrastructure/regional-product-services/)), costing (baseline costs also [vary by region](https://aws.amazon.com/ec2/pricing/) by up to 10-30% (generally lowest in us-east-1 for comparison purposes)), and compliance (various countries have differing regulations with regard to data privacy, for example).  
-	**Instance types:** EC2 instances come in many types, corresponding to the capabilities of the virtual machine in CPU architecture and speed, RAM, disk sizes and types (SSD or magnetic), and network bandwidth.  
	-	Selecting instance types is complex since there are so many types. Additionally there are different generations, released [over the years](https://aws.amazon.com/blogs/aws/ec2-instance-history/).  
	-	🔹Use the list at [**ec2instances.info**](http://www.ec2instances.info/) to review costs and features. [Amazon’s own list](https://aws.amazon.com/ec2/instance-types/) of instance types is hard to use, and doesn’t list features and price together, which makes it doubly difficult.  
	-	Prices vary a lot, so use [**ec2instances.info**](http://www.ec2instances.info/) to determine the set of machines that meet your needs and [**ec2price.com**](http://ec2price.com/) to find the cheapest type in the region you’re working in. Depending on the timing and region, it might be much cheaper to rent an instance with *more* memory or CPU than the bare minimum.  
  - **Turn off** your instances when they aren’t in use. For many situations such as testing or staging resources, you may not need your instances on 24/7, and you won’t need to pay EC2 running costs when they are suspended. Given that costs are calculated based on usage, this is a simple mechanism for cost savings. This can be achieved using [Lambda and CloudWatch](https://aws.amazon.com/premiumsupport/knowledge-center/start-stop-lambda-cloudwatch/), deploying the [Instance Scheduler](https://aws.amazon.com/solutions/implementations/instance-scheduler/) solution, an open source option like [cloudcycler](https://github.com/fairfaxmedia/cloudcycler), or a SaaS provider like [GorillaStack](https://www.gorillastack.com). (Note: if you turn off instances with an ephemeral root volume, any state will be lost when the instance is turned off. Therefore, for stateful applications it is safer to turn off EBS backed instances).  
-	[**Dedicated instances**](https://aws.amazon.com/ec2/purchasing-options/dedicated-instances/) and [**dedicated hosts**](https://aws.amazon.com/ec2/dedicated-hosts/) are assigned hardware, instead of usual virtual instances. They are more expensive than virtual instances but [can be preferable](https://aws.amazon.com/ec2/dedicated-hosts/) for performance, compliance, financial modeling, or licensing reasons.  
-	**32 bit vs 64 bit:** A few micro, small, and medium instances are still available to use as 32-bit architecture. You’ll be using 64-bit EC2 (“amd64”) instances nowadays, though smaller instances still support 32 bit (“i386”). Use 64 bit unless you have legacy constraints or other good reasons to use 32.  
-	**HVM vs PV:** There are two kinds of virtualization technology used by EC2, [hardware virtual machine (HVM) and paravirtual (PV)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/virtualization_types.html). Historically, PV was the usual type, but [now HVM is becoming the standard](https://www.opswat.com/blog/aws-2015-why-you-need-switch-pv-hvm). If you want to use the newest instance types, you must use HVM. See the [instance type matrix](https://aws.amazon.com/amazon-linux-ami/instance-type-matrix/) for details.  
-	**Operating system:** To use EC2, you’ll need to pick a base operating system. It can be Windows or Linux, such as Ubuntu or [Amazon Linux](https://aws.amazon.com/amazon-linux-ami/). You do this with AMIs, which are covered in more detail in their own section below.  
-	**Limits:** You can’t create arbitrary numbers of instances. Default limits on numbers of EC2 instances per account vary by instance type, as described in [this list](http://aws.amazon.com/ec2/faqs/#How_many_instances_can_I_run_in_Amazon_EC2).  
-	❗**Use termination protection:** For any instances that are important and long-lived (in particular, aren't part of auto-scaling), [enable termination protection](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/terminating-instances.html#Using_ChangingDisableAPITermination). This is an important line of defense against user mistakes, such as accidentally terminating many instances instead of just one due to human error.  
-	**SSH key management:**  
	-	When you start an instance, you need to have at least one [ssh key pair](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-key-pairs.html) set up, to bootstrap, i.e., allow you to ssh in the first time.  
	-	Aside from bootstrapping, you should manage keys yourself on the instances, assigning individual keys to individual users or services as appropriate.  
	-	Avoid reusing the original boot keys except by administrators when creating new instances.  
	-	Avoid sharing keys and [add individual ssh keys](http://security.stackexchange.com/questions/87480/managing-multiple-ssh-private-keys-for-a-team) for individual users.  
-	**GPU support:** You can rent GPU-enabled instances on EC2 for use in machine learning or graphics rendering workloads.  
  
	-	There are [three types](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using_cluster_computing.html) of GPU-enabled instances currently available:  
		- The P3 series offers NVIDIA Tesla V100 GPUs in 1, 4 and 8 GPU configurations targeting machine learning, scientific workloads, and other high performance computing applications.  
		- The P2 series offers NVIDIA Tesla K80 GPUs in 1, 8 and 16 GPU configurations targeting machine learning, scientific workloads, and other high performance computing applications.  
		- The G3 series offers NVIDIA Tesla M60 GPUs in 1, 2, or 4 GPU configurations targeting graphics and video encoding.  
	-	AWS offers two different AMIs that are targeted to GPU applications. In particular, they target deep learning workloads, but also provide access to more stripped-down driver-only base images.  
		-	AWS offers both an Amazon Linux [Deep Learning AMI](https://aws.amazon.com/marketplace/pp/B077GF11NF?qid=1536363169916&sr=0-3&ref_=srh_res_product_title) (based on Amazon Linux) as well as an Ubuntu [Deep Learning AMI](https://aws.amazon.com/marketplace/pp/B077GCH38C). Both come with most NVIDIA drivers and ancillary software (CUDA, CUBLAS, CuDNN, TensorFlow, PyTorch, etc.) installed to lower the barrier to usage.  
		-	⛓ Note that using these AMIs can lead to lock in due to the fact that you have no direct access to software configuration or versioning.  
		-	🔸 The compendium of frameworks included can lead to long instance startup times and difficult-to-reason-about environments.  
	-	🔹As with any expensive EC2 instance types, [Spot instances can offer significant savings](AWS%20정리.md#ec2-cost-management) with GPU workloads when interruptions are tolerable.  
- All current EC2 instance types can take advantage of IPv6 addressing, so long as they are launched in a subnet with an allocated CIDR range in an IPv6-enabled VPC.  
  
  
## EC2 Gotchas and Limitations  
  
-	❗Never use ssh passwords. Just don’t do it; they are too insecure, and consequences of compromise too severe. Use keys instead. [Read up on this](https://www.digitalocean.com/community/tutorials/how-to-set-up-ssh-keys--2) and fully disable ssh password access to your ssh server by making sure 'PasswordAuthentication no' is in your /etc/ssh/sshd_config file. If you’re careful about managing ssh private keys everywhere they are stored, it is a major improvement on security over password-based authentication.  
-	🔸For all [newer instance types](https://aws.amazon.com/amazon-linux-ami/instance-type-matrix/), when selecting the AMI to use, be sure you select the HVM AMI, or it just won’t work.  
-	❗When creating an instance and using a new ssh key pair, [make sure the ssh key permissions are correct](http://stackoverflow.com/questions/1454629/aws-ssh-access-permission-denied-publickey-issue).  
-	🔸Sometimes certain EC2 instances can get scheduled for retirement by AWS due to “detected degradation of the underlying hardware,” in which case you are given a couple of weeks to migrate to a new instance  
 	-	If your instance root device is an EBS volume, you can typically stop and then start the instance which moves it to healthy host hardware, giving you control over timing of this event. Note however that you will lose any instance store volume data ([ephemeral drives](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/InstanceStorage.html)) if your instance type has instance store volumes.  
 	-	The instance public IP (if it has one) will likely change unless you're using Elastic IPs. This could be a problem if other systems depend on the IP address.  
-	🔸Periodically you may find that your server or load balancer is receiving traffic for (presumably) a previous EC2 server that was running at the same IP address that you are handed out now (this may not matter, or it can be fixed by migrating to another new instance).  
-	❗If the EC2 API itself is a critical dependency of your infrastructure (e.g. for automated server replacement, custom scaling algorithms, etc.) and you are running at a large scale or making many EC2 API calls, make sure that you understand when they might fail (calls to it are [rate limited](http://docs.aws.amazon.com/AWSEC2/latest/APIReference/query-api-troubleshooting.html#api-request-rate) and the limits are not published and subject to change) and code and test against that possibility.  
-	❗Many newer EC2 instance types are either EBS-only, or backed by local NVMe disks assigned to the instance. Make sure to factor in EBS performance and costs when planning to use them.  
-	❗If you're operating at significant scale, you may wish to break apart API calls that enumerate all of your resources, and instead operate either on individual resources, or a subset of the entire list. EC2 APIs will time out! Consider using [filters](https://docs.aws.amazon.com/cli/latest/reference/ec2/describe-instances.html) to restrict what gets returned.  
-	❗⏱ Instances come in two types: **Fixed Performance Instances** (e.g. M3, C3, and R3) and [**Burstable Performance Instances**](https://aws.amazon.com/ec2/instance-types/#burst) (e.g. T2). A T2 instance receives CPU credits continuously, the rate of which depends on the instance size. T2 instances accrue CPU credits when they are idle, and use CPU credits when they are active. However, once an instance runs out of credits, you'll notice a severe degradation in performance. If you need consistently high CPU performance for applications such as video encoding, high volume websites or HPC applications, it is recommended to use Fixed Performance Instances.  
-	Instance user-data is [limited to 16 KB](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-instance-metadata.html#instancedata-add-user-data). (This limit applies to the data in raw form, not base64-encoded form.) If more data is needed, it can be downloaded from S3 by a user-data script.  
-	Very new accounts may not be able to launch some instance types, such as GPU instances, because of an initially imposed “soft limit” of zero. This limit can be raised by making a support request. See [AWS Service Limits](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html) for the method to make the support request. Note that this limit of zero is [not currently documented](http://docs.aws.amazon.com/general/latest/gr/aws_service_limits.html#limits_ec2).  
- Since multiple AWS instances all run on the same physical hardware, early cloud adopters encountered what became known as the [Noisy Neighbor problem](https://searchcloudcomputing.techtarget.com/definition/noisy-neighbor-cloud-computing-performance). This feeling of not getting what you are paying for led to [user frustration](https://twitter.com/technicallyjosh/status/668963405831651328), however "steal" may not be the best word to describe what's actually happening based on a [detailed explanation of how the kernel determine steal time](https://support.cloud.engineyard.com/hc/en-us/community/posts/203751578-Explanation-of-Steal-Time). Avoiding having CPU steal affect your application in the cloud may be best handled by [properly designing your cloud architecture](https://www.infoworld.com/article/3073503/cloud-computing/debunking-the-clouds-noisy-neighbor-myth.html).  
- AWS [introduced Dedicated Tenancy](https://aws.amazon.com/blogs/aws/amazon-ec2-dedicated-instances/) in 2011. This allows customers to have all resources from a single server. Some saw this as a way to solve the [noisy neighbor problem](https://www.infoworld.com/article/3008225/cloud-computing/amazon-dedicated-hosts-bye-bye-to-noisy-cloud-neighbors.html) since only that customer uses the CPU. This approach comes with a significant risk if that physical system needed any type of maintenance. If a customer had 20 instances running using shared tenancy and one underlying server needed maintenance, only the instance on that server would go offline. If that customer had 20 instances running using dedicated tenancy, when the underlying server needs maintenance, all 20 instances would go offline.  
-	🔸Only **i3.metal** type instances providing an ability to run Android x86 emulators on AWS at the moment.  
  
  
  
CloudWatch  
-------------------  
  
## CloudWatch Basics  
  
* 📒  [Homepage](https://aws.amazon.com/cloudwatch/) ∙ [Documentation](https://aws.amazon.com/documentation/cloudwatch/) ∙ [FAQ](https://aws.amazon.com/cloudwatch/faqs/) ∙ [Pricing](https://aws.amazon.com/cloudwatch/pricing/)  
* **CloudWatch** monitors resources and applications, captures logs, and sends events.  
* CloudWatch monitoring is the standard mechanism for keeping tabs on AWS resources. A wide range of  [**metrics and dimensions**](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CW_Support_For_AWS.html) are available via CloudWatch, allowing you to create time based graphs, **[alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/AlarmThatSendsEmail.html)**, and **[dashboards](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/CloudWatch_Dashboards.html)**.  
    * Alarms are the most practical use of CloudWatch, allowing you to trigger notifications from any given metric.  
    * Alarms can trigger [SNS notifications](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/ConsoleAlarms.html), [Auto Scaling actions](http://docs.aws.amazon.com/autoscaling/latest/userguide/policy_creating.html), or [EC2 actions](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/UsingAlarmActions.html).  
    * Alarms also support [alerting when any M out of N datapoints cross the alarm threshold](https://aws.amazon.com/about-aws/whats-new/2017/12/amazon-cloudwatch-alarms-now-alerts-you-when-any-m-out-of-n-metric-datapoints-in-an-interval-are-above-your-threshold/).  
    * Publish and share graphs of metrics by creating [customizable dashboard views](https://aws.amazon.com/blogs/aws/cloudwatch-dashboards-create-use-customized-metrics-views/).  
		* Monitor and report on EC2 [instance system check failure alarms](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/monitoring-system-instance-status-check.html#creating_status_check_alarms).  
* **Using CloudWatch Events:**  
    * Events create a mechanism to automate actions in various services on AWS. You can create [event rules](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/EventTypes.html) from instance states, AWS APIs, Auto Scaling, Run commands, deployments or time-based schedules (think Cron).  
    * [Triggered events](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/CWE_GettingStarted.html) can invoke Lambda functions, send SNS/SQS/Kinesis messages, or perform instance actions (terminate, restart, stop, or snapshot volumes).  
    * Custom payloads can be sent to targets in JSON format, this is especially useful when triggering Lambdas.  
* **Using CloudWatch Logs:**  
    * [CloudWatch Logs](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/WhatIsCloudWatchLogs.html) is a streaming log storage system. By storing logs within AWS you have access to unlimited paid storage, but you also have the option of streaming logs directly to ElasticSearch or custom Lambdas.  
    * A [log agent installed](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/CWL_GettingStarted.html) on your servers will process logs over time and send them to CloudWatch Logs.  
    * You can [export logged data to S3](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/S3Export.html) or stream results to other AWS services.  
    * CloudWatch Logs can be [encrypted using keys managed through KMS](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/encrypt-log-data-kms.html).  
* **Detailed monitoring:** [Detailed monitoring](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-cloudwatch-new.html) for EC2 instances must be enabled to get granular metrics, and is [billed under CloudWatch](https://aws.amazon.com/cloudwatch/pricing/).  
  
  
## CloudWatch Alternatives and Lock-In  
  
* CloudWatch offers fairly basic functionality that doesn't create significant (additional) AWS lock-in. Most of the metrics provided by the service can be obtained through APIs that can be imported into other aggregation or visualization tools or services (many specifically provide CloudWatch data import services).  
* 🚪 Alternatives to CloudWatch monitoring services include [NewRelic](http://newrelic.com/), [Datadog](http://datadog.com/), [Sumo Logic](http://sumologic.com/), [Zabbix](http://zabbix.com/), [Nagios](http://nagios.org/), [Ruxit](http://ruxit.com/), [Elastic Stack](https://www.elastic.co/elk-stack), open source options such as [StatsD](https://github.com/etsy/statsd) or [collectd](https://collectd.org/) with [Graphite](https://graphiteapp.org/), and many others.  
* 🚪 CloudWatch Log alternatives include [Splunk](http://splunk.com/), [Sumo Logic](http://sumologic.com/), [Loggly](http://loggly.com/), [LogDNA](https://logdna.com/), [Logstash](https://www.elastic.co/products/logstash), [Papertrail](https://papertrailapp.com/), [Elastic Stack](https://www.elastic.co/elk-stack), and other centralized logging solutions.  
  
  
## CloudWatch Tips  
  
* Some very common use cases for CloudWatch are **[billing alarms](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/monitor_estimated_charges_with_cloudwatch.html)**, **instance** **or [load balancer up/down alarms](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-cloudwatch-metrics.html)**, and **disk usage alerts**.  
* You can use [EC2Config](http://docs.aws.amazon.com/AWSEC2/latest/WindowsGuide/UsingConfig_WinAMI.html#send_logs_to_cwl) to monitor watch memory and disk metrics on Windows platform instances. For Linux, there are [example scripts](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/mon-scripts.html) that do the same thing.  
* You can [publish your own metrics](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/publishingMetrics.html) using the AWS API. [Incurs additional cost](https://aws.amazon.com/cloudwatch/pricing/).  
* You can stream directly from CloudWatch Logs to a Lambda or ElasticSearch cluster by creating [subscriptions](http://docs.aws.amazon.com/AmazonCloudWatch/latest/logs/Subscriptions.html) on Log Groups.  
* Don't forget to take advantage of the [CloudWatch non-expiring free tier](https://aws.amazon.com/free/#Amazon_CloudWatch).  
  
  
## CloudWatch Gotchas and Limitations  
  
* 🔸Metrics in CloudWatch originate [on the hypervisor](https://forums.aws.amazon.com/message.jspa?messageID=403578). The hypervisor doesn't have access to OS information, so certain metrics (most notably memory utilization) are not available unless pushed to CloudWatch from inside the instance.  
* 🔸You can not use [more than one metric for an alarm](https://forums.aws.amazon.com/thread.jspa?threadID=94984).  
* 🔸Notifications you receive from alarms will not have any contextual detail; they have only the specifics of the threshold, alarm state, and timing.  
* 🔸By default, CloudWatch metric resolution is 1 minute. If you send multiple values of a metric within the same minute, they will be aggregated into minimum, maximum, average and total (sum) per minute.  
* 🐥In July 2017, a new [high-resolution option](http://docs.aws.amazon.com/AmazonCloudWatch/latest/monitoring/publishingMetrics.html#high-resolution-metrics) was added for CloudWatch metrics and alarms. This feature allows you to record metrics with 1-second resolution, and to evaluate CloudWatch alarms every 10 seconds.  
    - The [blog post introducing this feature](https://aws.amazon.com/blogs/aws/new-high-resolution-custom-metrics-and-alarms-for-amazon-cloudwatch/) describes how to publish a high-resolution metric to CloudWatch. Note that when calling the `PutMetricData` API, `StorageResolution` is an attribute of each item you send in the `MetricData` array, not a direct parameter of the `PutMetricData` API call.  
* 🔸Data about metrics is kept in CloudWatch [for 15 months](https://aws.amazon.com/blogs/aws/amazon-cloudwatch-update-extended-metrics-retention-user-interface-update/), starting November 2016 (used to be 14 days). Minimum granularity increases after 15 days.  
  
  
  
  
# Load Balancers  
--------------  
  
## Load Balancer Basics  
  
-	AWS has 3 load balancing products - “Classic Load Balancers” (CLBs), “Application Load Balancers” (ALBs), and "Network Load Balancers" (NLB).  
-	Before the introduction of ALBs, “Classic Load Balancers” were known as “Elastic Load Balancers” (ELBs), so older documentation, tooling, and blog posts may still reference “ELBs”.  
-	CLBs have been around since 2009, ALBs in 2016, NLBs were added in 2017 to AWS.  
-	CLBs support TCP and HTTP load balancing. ALBs support HTTP load balancing only. NLBs support TCP layer 4 load balancing.  
-	CLBs and ALBs can optionally handle termination for a single SSL certificate.  
-	All can optionally perform active health checks of instances and remove them from the destination pool if they become unhealthy.  
-	CLBs don't support complex / rule-based routing. ALBs support a (currently small) set of rule-based routing features. NLBs have most extensive routing options.  
-	CLBs can only forward traffic to a single globally configured port on destination instances, while ALBs can forward to ports that are configured on a per-instance basis, better supporting routing to services on shared clusters with dynamic port assignment (like ECS or Mesos). NLBs support multiple ports on same IP; registering targets by IP address, including targets outside the VPC for the load balancer; ECS can select unused port for scheduling a task then register a target group using this port.  
-	CLBs are supported in EC2 Classic as well as in VPCs while ALBs are supported in VPCs only.  
-   ALBs can target groups of instances and IP based targets in the RFC1918 ranges allowing you to use on premise destinations via VPN or Direct Connect.  
  
  
## Load Balancer Tips  
  
-	If you don’t have opinions on your load balancing up front, and don’t have complex load balancing needs like application-specific routing of requests, it’s reasonable just to use a CLB or ALB for load balancing instead.  
-	Even if you don’t want to think about load balancing at all, because your architecture is so simple (say, just one server), put a load balancer in front of it anyway. This gives you more flexibility when upgrading, since you won’t have to change any DNS settings that will be slow to propagate, and also it lets you do a few things like terminate SSL more easily.  
-	**CLBs and ALBs have many IPs:** Internally, an AWS load balancer is simply a collection of individual software load balancers hosted within EC2, with DNS load balancing traffic among them. The pool can contain many IPs, at least one per availability zone, and depending on traffic levels. They also support SSL termination, which is very convenient.  
-	**Scaling:** CLBs and ALBs can scale to very high throughput, but scaling up is not instantaneous. If you’re expecting to be hit with a lot of traffic suddenly, it can make sense to load test them so they scale up in advance. You can also [contact Amazon](http://aws.amazon.com/articles/1636185810492479) and have them “pre-warm” the load balancer.  
-	**Client IPs:** In general, if servers want to know true client IP addresses, load balancers must forward this information somehow. CLBs add the standard [X-Forwarded-For](https://en.wikipedia.org/wiki/X-Forwarded-For) header. When using a CLB as an HTTP load balancer, it’s possible to get the client’s IP address from this.  
-	**Using load balancers when deploying:** One common pattern is to swap instances in the load balancer after spinning up a new stack with your latest version, keep old stack running for one or two hours, and either flip back to old stack in case of problems or tear it down.  
-   **Rotating Certificates while retaining ARN:** Rotating IAM Server Certificates can be difficult as the standard practice is to upload a new one then update all resources with the new ARN. You can however retain the same ARN using the `update-certificate` call with the following process:  
  1. Upload a new IAM Server Certificate with a unique name (e.g fuzzy.com.new)  
  2. Rename the existing IAM Server Certificate (e.g fuzzy.com to fuzzy.com.expired)  
  3. Rename the new IAM Server Certificate to the name of the previously existing certificate (e.g fuzzy.com.new to fuzzy.com)  
  4. Jiggle the CLB/ALB Listener to pick up the change:  
      * ALB: Invoke modify-listener with the existing details for the ALB Listener  
	  * CLB: Invoke create-load-balancer-listeners with the existing details for the CLB listener  
  
  
## Load Balancer Gotchas and Limitations  
  
-	❗CLBs and ALBs have **no fixed external IP** that all clients see. For most consumer apps this doesn’t matter, but enterprise customers of yours may want this. IPs will be different for each user, and will vary unpredictably for a single client over time (within the standard [EC2 IP ranges](http://docs.aws.amazon.com/general/latest/gr/aws-ip-ranges.html)). And similarly, never resolve a CLB name to an IP and put it as the value of an A record — it will work for a while, then break!  
-	❗Some web clients or reverse proxies cache DNS lookups for a long time, which is problematic for CLBs and ALBs, since they change their IPs. This means after a few minutes, hours, or days, your client will stop working, unless you disable DNS caching. Watch out for [Java’s settings](http://docs.oracle.com/javase/8/docs/api/java/net/InetAddress.html) and be sure to [adjust them properly](http://docs.aws.amazon.com/AWSSdkDocsJava/latest/DeveloperGuide/java-dg-jvm-ttl.html). Another example is nginx as a reverse proxy, which [normally resolves backends only at start-up](https://www.jethrocarr.com/2013/11/02/nginx-reverse-proxies-and-dns-resolution/) (although there is [a way to get around this](https://tenzer.dk/nginx-with-dynamic-upstreams/)).  
-	❗It’s not unheard of for IPs to be recycled between customers without a long cool-off period. So as a client, if you cache an IP and are not using SSL (to verify the server), you might get not just errors, but responses from completely different services or companies!  
-	🔸As an operator of a service behind a CLB or ALB, the latter phenomenon means you can also see puzzling or erroneous requests by clients of other companies. This is most common with clients using back-end APIs (since web browsers typically cache for a limited period).  
-	❗CLBs and ALBs take time to scale up, it does not handle sudden spikes in traffic well. Therefore, if you anticipate a spike, you need to “pre-warm” the load balancer by gradually sending an increasing amount of traffic.  
-	❗Tune your healthchecks carefully — if you are too aggressive about deciding when to remove an instance and conservative about adding it back into the pool, the service that your load balancer is fronting may become inaccessible for seconds or minutes at a time. Be extra careful about this when an autoscaler is configured to terminate instances that are marked as being unhealthy by a managed load balancer.  
-	❗CLB HTTPS listeners don't support Server Name Indication (SNI). If you need SNI, you can work around this limitation by either providing a certificate with Subject Alternative Names (SANs) or by using TCP listeners and terminating SSL at your backend.  
-	🔸 There is a limit on the number of ALBs, CLBs and NLBs per region (separately). As of late 2017, the default limit for each is 20 per region. These limits can be easily raised for ALB and CLB, but AWS is quite reluctant to raise the limit on NLBs.  
-	🔸 If using a Network Load Balancer (NLB) then EC2 clients cannot connect to an NLB that resides in another VPC (VPC Peering) or AWS managed VPN unless the EC2 client is a C5, i3.metal or M5 instance type. For VPC peering, both VPCs must be in the same region. See [Troubleshooting](https://docs.aws.amazon.com/elasticloadbalancing/latest/network/load-balancer-troubleshooting.html#target-not-in-service).  
  
# CLB  
---  
  
## CLB Basics  
  
-	📒 [Homepage](https://aws.amazon.com/elasticloadbalancing/classicloadbalancer/) ∙ [User guide](https://aws.amazon.com/elasticloadbalancing/classicloadbalancer/developer-resources/) ∙ [FAQ](https://aws.amazon.com/elasticloadbalancing/classicloadbalancer/faqs/) ∙ [Pricing](https://aws.amazon.com/elasticloadbalancing/classicloadbalancer/pricing/)  
- Classic Load Balancers, formerly known as Elastic Load Balancers, are HTTP and TCP load balancers that are managed and scaled for you by Amazon.  
  
  
## CLB Tips  
  
-	**Best practices:** [This article](http://aws.amazon.com/articles/1636185810492479) is a must-read if you use CLBs heavily, and has a lot more detail.  
  
  
## CLB Gotchas and Limitations  
  
-	In general, CLBs are not as “smart” as some load balancers, and don’t have fancy features or fine-grained control a traditional hardware load balancer would offer. For most common cases involving sessionless apps or cookie-based sessions over HTTP, or SSL termination, they work well.  
-	🔸By default, CLBs will refuse to route traffic from a load balancer in one Availability Zone (AZ) to a backend instance in another. This [will cause 503s](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/ts-elb-error-message.html#ts-elb-errorcodes-http503) if the last instance in an AZ becomes unavailable, even if there are healthy instances in other zones. If you’re running fewer than two backend instances per AZ, you almost certainly want to [enable cross-zone load balancing](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/enable-disable-crosszone-lb.html#enable-cross-zone).  
-	🔸Complex rules for directing traffic are not supported. For example, you can’t direct traffic based on a regular expression in the URL, like HAProxy offers.  
-	**Apex DNS names:** Once upon a time, you couldn’t assign a CLB to an apex DNS record (i.e. example.com instead of foo.example.com) because it needed to be an A record instead of a CNAME. This is now possible with a Route 53 alias record directly pointing to the load balancer.  
-	🔸CLBs use [HTTP keep-alives](https://en.wikipedia.org/wiki/HTTP_persistent_connection) on the internal side. This can cause an unexpected side effect: Requests from different clients, each in their own TCP connection on the external side, can end up on the same TCP connection on the internal side. Never assume that multiple requests on the same TCP connection are from the same client!  
-	🔸 Traffic between CLBs and back-end instances in the same subnet **will** have [Network ACL](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_ACLs.html) rules evaluated (EC2 to EC2 traffic in the same subnet would not have Network ACL rules evaluated). If the default '0.0.0.0/0 ALLOW' rule is removed from the Network ACL applied to the subnet, a rule that allows traffic on both the health check port and any listener port must be added.  
- As of December 2016, CLBs launched in VPCs do not support IPv6 addressing. CLBs launched in EC2-Classic support both IPv4 and IPv6 [with the "dualstack" DNS name](http://docs.aws.amazon.com/elasticloadbalancing/latest/classic/elb-internet-facing-load-balancers.html#internet-facing-ip-addresses).  
  
# ALB  
---  
![Pasted image 20230226152623.png](../attachments/Pasted%20image%2020230226152623.png)  
## ALB Basics  
  
-	📒 [Homepage](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/) ∙ [User guide](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/developer-resources/) ∙ [FAQ](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/faqs/) ∙ [Pricing](https://aws.amazon.com/elasticloadbalancing/applicationloadbalancer/pricing/)  
-	🐥**Websockets and HTTP/2** are [now supported](https://aws.amazon.com/blogs/aws/new-aws-application-load-balancer/).  
-	🐥**Internet Protocol Version 6 (IPv6)** is [now supported](https://aws.amazon.com/about-aws/whats-new/2017/01/announcing-internet-protocol-version-6-ipv6-support-for-elastic-load-balancing-in-amazon-virtual-private-cloud-vpc/).  
-	🐥**Load Balancing via IP** is [now supported](https://aws.amazon.com/about-aws/whats-new/2017/08/elastic-load-balancing-application-load-balancer-now-supports-load-balancing-to-ip-addresses-as-targets-for-aws-and-on-premises-resources/).  
-	Prior to the Application Load Balancer, you were advised to use TCP instead of HTTP as the protocol to make it work (as described [here](http://www.quora.com/When-will-Amazon-ELB-offer-SPDY-support)) and use [the obscure but useful Proxy Protocol](http://docs.aws.amazon.com/ElasticLoadBalancing/latest/DeveloperGuide/enable-proxy-protocol.html) ([more on this](https://chrislea.com/2014/03/20/using-proxy-protocol-nginx/)) to pass client IPs over a TCP load balancer.  
  
  
## ALB Tips  
  
-	Use ALBs to route to services that are hosted on shared clusters with dynamic port assignment (like ECS or Mesos).  
-	ALBs support [HTTP host-based routing](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html#host-conditions) (send HTTP requests for “api.mydomain.com” -> {target-group-1}, “blog.mydomain.com” -> {target group 2}) as well as [HTTP path-based routing](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-listeners.html#path-conditions) (send HTTP requests for “/api/&ast;” ->  {target-group-1}, “/blog/&ast;” -> {target group 2}).  
  
  
## ALB Gotchas and Limitations  
  
-	🔸ALBs only support HTTP/2 over HTTPS (no plain-text HTTP/2).  
-	🔸ALBs only support HTTP/2 to external clients and not to internal resources (instances/containers).  
-	ALBs support HTTP routing but not port-based TCP routing.  
-	Instances in the ALB’s target groups have to either have a single, fixed healthcheck port (“EC2 instance”-level healthcheck) or the healthcheck port for a target has to be the same as its application port (“Application instance”-level healthcheck) - you can't configure a per-target healthcheck port that is different than the application port.  
-	ALBs are VPC-only (they are not available in EC2 Classic)  
-	In a target group, if there is no healthy target, all requests are routed to all targets. For example, if you point a listener at a target group containing a single service that has a long initialization phase (during which the health checks would fail), requests will reach the service while it is still starting up.  
- 📜 Although ALBs [now support SNI](https://aws.amazon.com/about-aws/whats-new/2017/10/elastic-load-balancing-application-load-balancers-now-support-multiple-ssl-certificates-and-smart-certificate-selection-using-server-name-indication-sni/), they only support 25 HTTPS certificates per Load Balancer. This limitation is not described [here](http://docs.aws.amazon.com/elasticloadbalancing/latest/application/load-balancer-limits.html), so it might be subject to change.  
  
  
  
# Elastic IPs  
  
  
## Elastic IP Basics  
  
-	📒 [Documentation](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html) ∙ [FAQ](https://aws.amazon.com/ec2/faqs/#Elastic_IP) ∙ [Pricing](https://aws.amazon.com/ec2/pricing/on-demand/#Elastic_IP_Addresses)  
-	**Elastic IPs** are static IP addresses you can rent from AWS to assign to EC2 instances.  
  
  
## Elastic IP Tips  
  
-	🔹**Prefer load balancers to elastic IPs:** For single-instance deployments, you could just assign elastic IP to an instance, give that IP a DNS name, and consider that your deployment. Most of the time, you should provision a [load balancer](AWS%20정리.md#load-balancers) instead:  
	-	It’s easy to add and remove instances from load balancers. It’s also quicker to add or remove instances from a load balancer than to reassign an elastic IP.  
	-	It’s more convenient to point DNS records to load balancers, instead of pointing them to specific IPs you manage manually. They can also be Route 53 aliases, which are easier to change and manage.  
	-	But in some situations, you do need to manage and fix IP addresses of EC2 instances, for example if a customer needs a fixed IP. These situations require elastic IPs.  
-	Elastic IPs are limited to 5 per account. It’s possible to [request more](https://console.aws.amazon.com/support/home#/case/create?issueType=service-limit-increase&limitType=service-code-elastic-ips-ec2-classic).  
-	If an Elastic IP is not attached to an active resource there is a small [hourly fee](https://aws.amazon.com/ec2/pricing/on-demand/#Elastic_IP_Addresses).  
-	Elastic IPs are [no extra charge](https://aws.amazon.com/ec2/pricing/on-demand/#Elastic_IP_Addresses) as long as you’re using them. They have a (small) cost when not in use, which is a mechanism to prevent people from squatting on excessive numbers of IP addresses.  
  
  
## Elastic IP Gotchas and Limitations  
  
-	🔸There is [officially no way](https://forums.aws.amazon.com/thread.jspa?threadID=171550) to allocate a contiguous block of IP addresses, something you may desire when giving IPs to external users. Though when allocating at once, you may get lucky and have some be part of the same CIDR block. If this is important to you, you may want to [bring your own IP](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ec2-byoip.html), which is more involved than this guide will go into.  
-	Unofficially, if you have Enterprise support, you can ask your account rep to try to allocate a block of Elastic IPs with a business justification. For example, some of [Duo's fixed ranges](https://help.duo.com/s/article/1337?language=en_US) are [blocks of AWS IP space reassigned to `AWS-DUOSECURITYINC`](https://search.arin.net/rdap/?query=54.241.191.128). This is a best effort request, expect a denial.   
  
  
# RDS  
  
  
![Pasted image 20230226152639.png](../attachments/Pasted%20image%2020230226152639.png)  
  
## RDS Basics  
  
-	📒 [Homepage](https://aws.amazon.com/rds/) ∙ [User guide](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/) ∙ [FAQ](https://aws.amazon.com/rds/faqs/) ∙ [Pricing](https://aws.amazon.com/rds/pricing/) (see also [ec2instances.info/rds/](http://www.ec2instances.info/rds/)\)  
-	**RDS** is a managed relational database service, allowing you to deploy and scale databases more easily. It supports [Oracle](https://aws.amazon.com/rds/oracle/), [Microsoft SQL Server](https://aws.amazon.com/rds/sqlserver/), [PostgreSQL](https://aws.amazon.com/rds/postgresql/), [MySQL](https://aws.amazon.com/rds/mysql/), [MariaDB](https://aws.amazon.com/rds/mariadb/), and Amazon’s own [Aurora](https://aws.amazon.com/rds/aurora/).  
-	RDS offers out of the box support for [high availability and failover](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Concepts.MultiAZ.html) for your databases.  
  
  
## RDS Tips  
  
-	If you're looking for the managed convenience of RDS for other data stores such as MongoDB or Cassandra, you may wish to consider third-party services from providers such as [[Compose](https://www.compose.com/), or [InstaClustr](https://www.instaclustr.com/).  
-	🔹Make sure to create a new [parameter group](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html) and option group for your database since the default parameter group does not allow dynamic configuration changes.  
-	RDS instances start with a default timezone of UTC. If necessary, this can be [changed to a different timezone](https://aws.amazon.com/premiumsupport/knowledge-center/rds-change-time-zone/).  
-	Save costs by stopping the RDS instance(s) when not in use, you can achieve this by configuring a stop and start schedule using the the [Instance Scheduler](https://aws.amazon.com/solutions/implementations/instance-scheduler/) solution.  
  
  
## RDS Gotchas and Limitations  
  
-	⏱RDS instances run on EBS volumes (either general-purpose or provisioned IOPS), and hence are constrained by EBS performance.  
-	🔸Verify what database features you need, as not everything you might want is available on RDS. For example, if you are using Postgres, check the list of [supported features and extensions](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html#SQLServer.Concepts.General.FeatureSupport). If the features you need aren't supported by RDS, you'll have to deploy your database yourself.  
-	🔸If you use the failover support offered by RDS, keep in mind that it is based on DNS changes, and make sure that your client reacts to these changes appropriately. This is particularly important for Java, given how its DNS resolver’s TTL is [configured by default](http://docs.aws.amazon.com/sdk-for-java/v1/developer-guide/java-dg-jvm-ttl.html).  
-	🔸**DB migration to RDS:** While importing your database into RDS ensure you take into consideration the maintenance window settings. If a backup is running at the same time, your import can take a considerably longer time than you would have expected.  
-	[Database sizes are limited](https://aws.amazon.com/about-aws/whats-new/2015/06/amazon-rds-increases-storage-limits-to-6TB-for-piops-and-gp2/) to **6TB** for all database engines except for SQL Server which has a **4TB** limit and Aurora which supports up to **64TB** databases.  
  
# RDS MySQL and MariaDB  
  
  
## RDS MySQL and MariaDB Basics  
  
-      RDS offers MySQL versions 5.5, 5.6, 5.7 and 5.8.  
-      RDS offers MariaDB versions 10.0, 10.1, 10.2 and 10.3.  
  
  
## RDS MySQL and MariaDB Tips  
  
-	MySQL RDS allows access to [binary logs](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_LogAccess.Concepts.MySQL.html#USER_LogAccess.MySQL.BinaryFormat).  
-	Multi-AZ instances of MySQL transparently replicate data across AZs using DRBD. Automated backups of multi-AZ instances [run off the backup instance](https://www.percona.com/live/mysql-conference-2014/sessions/rds-mysql-tips-patterns-and-common-pitfalls) to reduce latency spikes on the primary.  
-	🔸**Performance Schema:** While [Performance Schema](http://dev.mysql.com/doc/refman/en/performance-schema.html) is enabled by default in MySQL 5.6.6 and later, it is disabled by default in all versions of RDS. If you wish to enable Performance Schema, a reboot of the RDS instance will be required.  
-	🔸**MySQL vs MariaDB vs Aurora:** If you prefer a MySQL-style database but are starting something new, you probably should consider Aurora and MariaDB as well. **Aurora** has increased availability and is the next-generation solution. That said, Aurora [may not be](http://blog.takipi.com/benchmarking-aurora-vs-mysql-is-amazons-new-db-really-5x-faster/) that much faster than MySQL for certain workloads. **MariaDB**, the modern [community fork](https://en.wikipedia.org/wiki/MariaDB) of MySQL, [likely now has the edge over MySQL](http://cloudacademy.com/blog/mariadb-vs-mysql-aws-rds/) for many purposes and is supported by RDS.  
  
  
## RDS MySQL and MariaDB Gotchas and Limitations  
  
-	🔸**No SUPER privileges.** RDS provides some [stored procedures](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Appendix.MySQL.SQLRef.html) to perform some tasks that require SUPER privileges such as starting or stopping replication.  
-	🔸You can replicate to non-RDS instances of MySQL, but [replication to these instances will break during AZ failovers](https://www.percona.com/live/mysql-conference-2014/sessions/rds-mysql-tips-patterns-and-common-pitfalls).  
-	🔸There is no ability to manually CHANGE MASTER on replicas, so they must all be rebuilt after a failover of the master.  
-	🔸Most global options are exposed only via [DB parameter groups](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_WorkingWithParamGroups.html). Some variables that were introduced in later MySQL dot releases such as [avoid_temporal_upgrade](https://dev.mysql.com/doc/refman/5.6/en/server-system-variables.html#sysvar_avoid_temporal_upgrade) in MySQL 5.6.24 are not made available in RDS's 5.6.x parameter group and making use of them requires an upgrade to MySQL 5.7.x.  
-	🔸RDS features such as Point-In-Time restore and snapshot restore are not supported on MyISAM tables. Ensure you lock and flush each MyISAM table before executing a snapshot or backup operation to ensure consistency.  
  
# RDS PostgreSQL  
  
--------------  
  
## RDS PostgreSQL Basics  
  
- RDS offers PostgreSQL 9.3, 9.4, 9.5, 9.6, and 10.  
  
  
## RDS PostgreSQL Tips  
- Recently Logical Replication is being supported, [both as subscriber and publisher](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html#PostgreSQL.Concepts.General.version104).  
- Supports a relatively large range of native [extensions](https://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_PostgreSQL.html#PostgreSQL.Concepts.General.FeatureSupport.Extensions).  
- RDS PostgreSQL 10 Supports native partitioning and most of the major features and tunables.  
- Supports connections over SSL.  
- Supports multi A-Z and Point-in-time recovery.  
  
  
  
## RDS PostgreSQL Gotchas and Limitations  
- No superuser privileges. RDS provides a role `rds_superuser` that can do most of the needed operations but there are some limitations.  
- Some major features are delayed compared to open source PostgreSQL.  
- By default RDS is spec’d with general purpose SSD , if you need better performance you have to spec provisioned IOPS SSD.  
- You can't use RDS as a replica outside RDS without using logical replication.  
- There are settings that cannot be changed and most of the settings that can change can only be changed using database parameter groups.  
- It’s harder to troubleshoot performance problems since you have no access to the host.  
- Be sure to verify that all the [extensions](https://www.postgresql.org/docs/current/static/view-pg-available-extensions.html) you need are available. If you are using an extension not listed there, you will need to come up with a work around, or deploy your own database in EC2.  
- Many Postgres utilities and maintenance items expect command line access, that can usually be satisfied by using an external ec2 server.  
  
# RDS SQL Server  
--------------  
  
## RDS SQL Server Basics  
  
-	[RDS offers SQL Server 2008 R2, 2012, 2014, 2016 and 2017](https://aws.amazon.com/rds/sqlserver/) including Express, Web, Standard and Enterprise.  
  
  
## RDS SQL Server Tips  
  
-	Recently added support for [backup and restore to/from S3](https://www.brentozar.com/archive/2016/07/holy-cow-amazon-rds-sql-server-just-changed-everything/) which may make it an attractive [DR option](https://aws.amazon.com/blogs/aws/amazon-rds-for-sql-server-support-for-native-backuprestore-to-amazon-s3/) for on-premises installations.  
  
  
## RDS SQL Server Gotchas and Limitations  
  
-	🔸The user is granted only db_owner privileges for each database on the instance.  
-	🔸Storage cannot be expanded for existing databases. If you need more space, you must restore your database on a new instance with larger storage.  
-	🔸There is a **16TB** database size limit for non-Express editions. There is also a minimum storage size, 20GB for Web and Express, 200GB for Standard and Enterprise.  
-	🔸Limited to [30 databases per instance](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/CHAP_SQLServer.html)  
  
# RDS Aurora  
---  
## RDS Aurora Basics  
Aurora is a cloud only database service designed to provide a distributed, fault-tolerant relational database with self-healing storage and auto-scaling up to 64TB per instance.  It currently comes in two versions, a MySQL compatible system, and a PostgreSQL compatible system.  
  
# RDS Aurora MySQL  
----------------  
  
## RDS Aurora MySQL Basics  
  
-	Amazon’s proprietary fork of MySQL intended to scale up for high concurrency workloads. Generally speaking, individual query performance under Aurora is not expected to improve significantly relative to MySQL or MariaDB, but Aurora is intended to maintain performance while executing many more queries concurrently than an equivalent MySQL or MariaDB server could handle.  
-	[Notable new features](http://www.slideshare.net/AmazonWebServices/amazon-aurora-amazons-new-relational-database-engine) include:  
	-	Log-structured storage instead of B-trees to improve write performance.  
	-	Out-of-process buffer pool so that databases instances can be restarted without clearing the buffer pool.  
	-	The underlying physical storage is a specialized SSD array that automatically maintains 6 copies of your data across 3 AZs.  
	-	Aurora read replicas share the storage layer with the write master which significantly reduces replica lag, eliminates the need for the master to write and distribute the binary log for replication, and allows for zero-data-loss failovers from the master to a replica. The master and all the read replicas that share storage are known collectively as an **Aurora cluster**. Read replicas can span up to [5 regions](https://aws.amazon.com/about-aws/whats-new/2018/09/amazon-aurora-databases-support-up-to-five-cross-region-read-replicas/).  
  
  
## RDS Aurora MySQL Tips  
  
-	In order to take advantage of Aurora’s higher concurrency, applications should be configured with large database connection pools and should execute as many queries concurrently as possible. For example, Aurora servers have been tested to produce increasing performance on some OLTP workloads with [up to 5,000 connections](http://www.slideshare.net/AmazonWebServices/amazon-aurora-amazons-new-relational-database-engine/31).  
-	[Aurora scales well with multiple CPUs](https://www.percona.com/blog/2016/05/26/aws-aurora-benchmarking-part-2/) and may require a large instance class for optimal performance.  
-	The easiest migration path to Aurora is restoring a database snapshot from MySQL 5.6 or 5.7. The next easiest method is restoring a dump from a MySQL-compatible database such as MariaDB. For [low-downtime migrations](https://aws.amazon.com/blogs/aws/amazon-aurora-update-spatial-indexing-and-zero-downtime-patching/) from other MySQL-compatible databases, you can set up an Aurora instance as a replica of your existing database. If none of those methods are options, Amazon offers a fee-based data migration service.  
-	You can replicate [from an Aurora cluster to MySQL or to another Aurora cluster](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/Aurora.Overview.Replication.MySQLReplication.html). This requires binary logging to be enabled and is not as performant as native Aurora replication.  
-	Because Aurora read replicas are the [equivalent of a multi-AZ backup](http://stackoverflow.com/a/32428651/129052) and they can be configured as zero-data-loss failover targets, there are fewer scenarios in which the creation of a multi-AZ Aurora instance is required.  
  
  
## RDS Aurora MySQL Gotchas and Limitations  
  
- 🔸[Aurora 1.x is based on MySQL 5.6.x](https://news.ycombinator.com/item?id=12415693) with some cherry-picking of later MySQL features. It is missing most 5.7 features as well as some online DDL features introduced in 5.6.17.  
- 🔸[Aurora 2.x is based on MySQL 5.7.x](https://aws.amazon.com/about-aws/whats-new/2018/02/amazon-aurora-is-compatible-with-mysql-5-7/)  
- Aurora does not support GTID transactions in either the 5.6/Aurora 1.x or the 5.7/Aurora 2.x release lines.  
- Aurora maximum cluster size is 64 TB  
  
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
  
  
  
  
# ECS  
---  
  
  
![Pasted image 20230226152559.png](../attachments/Pasted%20image%2020230226152559.png)  
  
## ECS Basics  
  
-	📒 [Homepage](https://aws.amazon.com/ecs/) ∙ [Developer guide](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/) ∙ [FAQ](https://aws.amazon.com/ecs/faqs/) ∙ [Pricing](https://aws.amazon.com/ecs/pricing/)  
-	**ECS** (EC2 Container Service) is a relatively new service (launched end of 2014) that manages clusters of services deployed via Docker.  
-	See the [Containers and AWS](AWS%20정리.md#containers-and-aws) section for more context on containers.  
-	ECS is growing in adoption, especially for companies that embrace microservices.  
-	Deploying Docker directly in EC2 yourself is another common approach to using Docker on AWS. Using ECS is not required, and ECS does not (yet) seem to be the predominant way many companies are using Docker on AWS.  
-	It’s also possible to use [Elastic Beanstalk with Docker](http://docs.aws.amazon.com/elasticbeanstalk/latest/dg/create_deploy_docker.html), which is reasonable if you’re already using Elastic Beanstalk.  
-	Using Docker may change the way your services are deployed within EC2 or Elastic Beanstalk, but it does not radically change how most other services are used.  
-	[ECR](https://aws.amazon.com/ecr/) (EC2 Container Registry) is Amazon’s managed Docker registry service. While simpler than running your own registry, it is missing some features that might be desired by some users:  
	-	Doesn’t support cross-region replication of images.  
		-	If you want fast fleet-wide pulls of large images, you’ll need to push your image into a region-local registry.  
	-	Doesn’t support custom domains / certificates.  
-	A container’s health is monitored via [CLB](AWS%20정리.md#clb) or [ALB](AWS%20정리.md#alb). Those can also be used to address a containerized service. When using an ALB you do not need to handle port contention (i.e. services exposing the same port on the same host) since an ALB’s target groups can be associated with ECS-based services directly.  
-	[The Hitchhikers Guide to AWS ECS and Docker](http://start.jcolemorrison.com/the-hitchhikers-guide-to-aws-ecs-and-docker/) by J. Cole Morrison is an excellent article for Introduction to AWS ECS concepts.  
  
  
  
  
## ECS Tips  
  
-	**Log drivers:** ECS supports multiple log drivers (awslogs, splunk, fluentd, syslog, json, ... ). Use [`awslogs`](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/using_awslogs.html) for CloudWatch (make sure a group is made for the logs first). [Drivers such as fluentd are not enabled by default](https://github.com/aws/amazon-ecs-agent/issues/535). You can, install the agent and enable the driver by adding `ECS_AVAILABLE_LOGGING_DRIVERS='["awslogs","fluentd"]'` to `/etc/ecs/ecs.config`.  
-	[This blog from Convox](https://convox.com/blog/ecs-challenges) (and [commentary](https://news.ycombinator.com/item?id=11598058)) lists a number of common challenges with ECS as of early 2016.  
- It is possible to optimize disk clean up on ECS. By default, the unused containers are deleted after 3 hours and the unused images after 30 minutes. These settings can be changed by adding `ECS_ENGINE_TASK_CLEANUP_WAIT_DURATION=10m` and `ECS_IMAGE_CLEANUP_INTERVAL=10m` to `/etc/ecs/ecs.config`. [More information on optimizing ECS disk cleanup](https://aws.amazon.com/blogs/compute/optimizing-disk-usage-on-amazon-ecs/).  
  
  
## ECS Alternatives and Lock-in  
  
-	[Kubernetes](https://kubernetes.io): Extensive container platform. Available as a hosted solution on [Google Cloud](https://cloud.google.com/kubernetes-engine), [AWS](https://aws.amazon.com/eks/), [Azure](https://azure.microsoft.com/en-us/services/kubernetes-service/), [DigitalOcean](https://www.digitalocean.com/products/kubernetes/), and [OpenShift](https://www.redhat.com/en/technologies/cloud-computing/openshift).  
-	[Nomad](https://www.nomadproject.io/): Orchestrator/Scheduler, tightly integrated in the HashiCorp stack (Consul, Vault, etc).  
  
🚧 [*Please help expand this incomplete section.*](CONTRIBUTING.md)  
  
# EKS  
---  
  
## EKS Basics  
-	📒 [Homepage](https://aws.amazon.com/eks/) ∙ [User guide](http://docs.aws.amazon.com/eks/latest/userguide/what-is-eks.html) ∙ [FAQ](https://aws.amazon.com/eks/faq/) ∙ [Pricing](https://aws.amazon.com/eks/pricing/)  
- EKS (Elastic Kubernetes Service) is a new service (launched June 2018) that provides managed Kubernetes Masters in a Highly Available pair to deploy K8s Services and Pods on top of EC2 based Kubernetes nodes.  
- See the [Containers and AWS](AWS%20정리.md#containers-and-aws) section for more context on containers.  
- EKS is AWS's solution to hosting Kubernetes natively on AWS. It is not a replacement for ECS directly but is in response to the large market dominance of Kubernetes.  
- EKS does not launch EC2 nodes and would have to be configured and setup either manually or via Cloudformation (or other automation solution)  
- EKS management is done through a utility called kubectl, and with Kube configuration files. These files will need to be configured to speak with the K8s Master with a certificate and URL. The AWS CLI can autogenerate the configuration file that kubectl requires for communicating with the cluster.<sup>[1](AWS%20정리.md#user-content-eks-aws-cli-create-kubeconfig)</sup>  
- EKS authentication is integrated with IAM roles/permissions. The AWS CLI has an integrated sub-command for generating authentication tokens.<sup>[2](AWS%20정리.md#user-content-eks-aws-cli-get-token)</sup> This was formerly done via a custom plugin for kubectl called [aws-iam-authenticator](https://github.com/kubernetes-sigs/aws-iam-authenticator) (formerly heptio-authenticator-aws).  
- EKS provides [Calico](https://docs.aws.amazon.com/eks/latest/userguide/calico.html) from Tigera for securing workloads within a cluster using Kubernetes network policy.  
  
  
## EKS Tips  
- Multiple clusters can be supported by using different kubeconfig files.  
- AWS has a [Kubernetes Quickstart](https://aws.amazon.com/quickstart/architecture/heptio-kubernetes/) developed in collaboration with Heptio.  
  
  
## EKS Alternatives and Lock-in  
- [ECS](AWS%20정리.md#ecs): Amazon's native Container Scheduled platform released in 2014.  If you don't utilise containers today and are looking to get started, ECS is an excellent product.  
-	[Kubernetes](https://kubernetes.io): Extensive container platform. Available as a hosted solution on [Google Cloud](https://cloud.google.com/container-engine/), [AWS](https://aws.amazon.com/eks/), [Digital Ocean](https://www.digitalocean.com/products/kubernetes/) and [Azure](https://azure.microsoft.com/en-us/services/kubernetes-service/).  
-	[Nomad](https://www.nomadproject.io/): Orchestrator/Scheduler, tightly integrated in the HashiCorp stack (Consul, Vault, etc).  
  
  
## EKS Gotchas and Limitations  
- Pods and Service configurations can rapidly consume IP addresses inside a VPC.  Proper care and maintenance should be applied to ensure IP exhaustion does not occur.  
- There is currently no integrated monitoring in CloudWatch for EKS pods or services, you will need to deploy a monitoring system that supports Kubernetes such as Prometheus.  
- Autoscaling based off CPU/Memory of a node is limited as you will not be aware of pending Services/Pods that cannot start. Using [cluster-autoscaler](https://github.com/kubernetes/autoscaler/tree/master/cluster-autoscaler) can be useful for scaling based on Node resource usage and unschedulable Pods.  
- [Prometheus](https://prometheus.io/) is a very popular monitoring solution for K8s, metrics and alerts can be used to send events to Lambda, SQS or other solutions to take autoscaling actions.  
  
  
## Footnotes  
<a name="user-content-eks-aws-cli-create-kubeconfig">**1**</a>: https://docs.aws.amazon.com/eks/latest/userguide/create-kubeconfig.html<br />  
<a name="user-content-eks-aws-cli-get-token">**2**</a>: https://aws.amazon.com/about-aws/whats-new/2019/05/amazon-eks-simplifies-kubernetes-cluster-authentication/<br />  
  
# Fargate  
-------  
  
## Fargate Basics  
-	📒 [Homepage](https://aws.amazon.com/fargate/) ∙ [FAQ](https://aws.amazon.com/fargate/faqs/) ∙ [Pricing](https://aws.amazon.com/fargate/pricing/)  
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
  
-	📒 [Homepage](https://aws.amazon.com/lambda/) ∙ [Developer guide](http://docs.aws.amazon.com/lambda/latest/dg/) ∙ [FAQ](https://aws.amazon.com/lambda/faqs/) ∙ [Pricing](https://aws.amazon.com/lambda/pricing/)  
-	**Lambda** is AWS' serverless compute offering, allowing users to define Lambda functions in a selection of runtimes that can be invoked via a variety of triggers, including SNS notifications and API Gateway invocations. Lambda is the key service that enables ['serverless' architecture on AWS](https://aws.amazon.com/lambda/serverless-architectures-learn-more/), alongside AWS API Gateway, AWS Batch, and AWS DynamoDB.  
  
  
## Lambda Tips  
  
-	The idea behind 'serverless' is that users don't manage provisioning, scaling, or maintenance of the physical machines that host their application code. With Lambda, the machine that actually executes the user-defined function is abstracted as a ['container'](http://docs.aws.amazon.com/lambda/latest/dg/lambda-introduction.html). When defining a Lambda function, users are able to declare the amount of memory available to the function, which directly affects the physical hardware specification of the Lambda container.  
-	Changing the amount of memory available to your Lambda functions also affects the amount of [CPU power](https://aws.amazon.com/lambda/faqs/) available to it.  
-	While AWS does not offer hard guarantees around container reuse, in general it can be expected that an unaltered Lambda function will reuse a warm (previously used) container if called shortly after another invocation. Users can use this as a way to optimize their functions by smartly caching application data on initialization.  
-	A Lambda that hasn't been invoked in some time may not have any warm containers left. In this case, the Lambda system will have to load and initialize the Lambda code in a 'cold start' scenario, which can add significant latency to Lambda invocations.  Lambda cold start performance [has improved significantly over the 2018-2019 timeframe](https://levelup.gitconnected.com/aws-lambda-cold-start-language-comparisons-2019-edition-%EF%B8%8F-1946d32a0244) and is now typically in the range of 200-500 ms for a simple function depending on the language runtime.  
-	Lambda functions running insides of VPCs have also seen [recent improvements](https://aws.amazon.com/blogs/compute/announcing-improved-vpc-networking-for-aws-lambda-functions/) to cold start times.  Previously these VPC-hosted functions would have cold starts of ~15 seconds; now those same functions cold start in < 1 second.  
-	There are a few strategies to avoiding or mitigating cold starts.  [Provisioned concurrency](https://aws.amazon.com/blogs/aws/new-provisioned-concurrency-for-lambda-functions/) was announced at re:invent 2019 and is an effective means to eliminating cold starts. Other techniques include keeping containers warm by periodic triggering and favoring lightweight runtimes such as Node as opposed to Java.  
-	Lambda is integrated with AWS CloudWatch and provides a logger at runtime that publishes CloudWatch events.  
-	Lambda offers out-of-the-box opt-in support for AWS X-Ray. X-Ray can help users diagnose Lambda issues by offering in-depth analysis of their Lambda's execution flow. This is especially useful when investigating issues calling other AWS services as X-Ray gives you a detailed and easy-to-parse [visualization of the call graph](http://docs.aws.amazon.com/lambda/latest/dg/lambda-x-ray.html#lambda-service-map).  
-	Using [timed CloudWatch events](http://docs.aws.amazon.com/AmazonCloudWatch/latest/events/ScheduledEvents.html#CronExpressions), users can use Lambda to run periodic jobs in a cron-like manner.  
-	Events sent to Lambda that fail processing can be managed using a [Dead Letter Queue (DLQ) in SQS.](http://docs.aws.amazon.com/lambda/latest/dg/dlq.html)  
-	More on serverless:  
	-	[Mike Roberts's thoughts on martinfowler.com.](http://martinfowler.com/articles/serverless.html)  
	-	[AWS Serverless Application Model (SAM)](https://github.com/awslabs/serverless-application-model), a simplification built on top of CloudFormation that can help to define, manage, and deploy serverless applications using Lambda.  
	-	[Serverless](https://github.com/serverless/serverless), one of the most popular frameworks for building serverless applications using AWS Lambda and other serverless compute options.  
	-	[Other helpful frameworks.](https://github.com/anaibol/awesome-serverless#frameworks)  
  
  
## Lambda Alternatives and Lock-in  
  
-	🚪Other clouds offer similar services with different names, including [Google Cloud Functions](https://cloud.google.com/functions/), [Azure Functions](https://azure.microsoft.com/en-us/services/functions/), and [IBM OpenWhisk](http://www.ibm.com/cloud-computing/bluemix/openwhisk/). Also if you are running Kubernetes another Lambda alternative is [OpenFaaS](https://github.com/openfaas/faas)  
  
  
## Lambda Gotchas and Limitations  
  
- 🔸Testing Lambdas, locally and remotely, can be a challenge. Several tools are available to make this easier, including the officially supported [SAM Local](https://github.com/awslabs/aws-sam-local).  
- 🔸Managing lots of Lambda functions is a workflow challenge, and tooling to manage Lambda deployments is still immature.  
- 🔸AWS’ official workflow around managing function [versioning and aliases](https://docs.aws.amazon.com/lambda/latest/dg/versioning-aliases.html) is painful. One option is to avoid Lambda versioning by abstracting your deployment workflow outside of Lambda. One way this can be accomplished is by deploying your application in successive stages, with a distinct AWS account per stage, where each account only needs to be aware of the latest version, and rollbacks and updates are handled by external tooling.  
- 🔸While adding/removing S3 buckets as triggers for Lambda function, this error may occur: "There was an error creating the trigger: Configuration is ambiguously defined. Cannot have overlapping suffixes in two rules if the prefixes are overlapping for the same event type." In this case, you can manually remove the Lambda event in the "Events" tab in the "Properties" section of the S3 bucket.  
- 🔸Managing the size of your deployment artifact can be a challenge, especially if using Java. Options to mitigate this include [proguard](https://www.guardsquare.com/en/proguard) and loading dependencies at runtime into /tmp.  
- When using DynamoDB as a trigger for your Lambda functions, this error may occur: "PROBLEM: internal Lambda error. Please contact Lambda customer support." This usually just means that Lambda can't detect anything in the DynamoDB stream within the last 48 hours. If the issue persists, deleting and recreating your trigger may help.  
- 🔸If your lambda needs access to resources in a VPC (for example ElastiCache or RDS), it will need to be deployed within it. This will increase cold-start times as an Elastic Network Interface (ENI) will have to be registered within the VPC for each concurrent function. AWS also has a relatively low initial limit (350) on the number ENI's that can be created within an VPC, however this can be increased to the 1000s if a good case is made to AWS support.  
- 🔸If your lambda is in a VPC and needs access to resources outside VPC such outbound internet access, it will need a NAT gateway associated even if it is in public subnet. Lambdas won't be assigned a public IP irrespective of subnet. If it is accessing AWS services (for example S3), you may use VPC endpoints.  
-	🔸 Lambda has several [**resource limits**](http://docs.aws.amazon.com/lambda/latest/dg/limits.html) as of 2017-06:  
	-	A **6MB** request or response payload size.  
	-	A **50 MB** limit on the compressed .zip/.jar file deployment package size.  
	-	A **250 MB** limit on the code/dependencies in the package before compression.  
	- A **500 MB** limit on local storage in /tmp.  
  
  
## Lambda Code Samples  
  
-	[Fan-out](https://github.com/awslabs/aws-lambda-fanout) is an example of using Lambda to “fan-out” or copy data from one service, in this case Kinesis, to multiple other AWS data services. Destinations for fan-out data in the sample include IoT, SQS and more.  
-	This [AWS limit monitor using Lambdas](https://github.com/awslabs/aws-limit-monitor) shows use of multiple Lambdas for monitoring.  
-	This [Lambda ECS Worker Pattern](https://github.com/awslabs/lambda-ecs-worker-pattern) shows use of Lambda in a workflow where data from S3 is picked up by the Lambda, pushed to a queue, then sent to ECS for more processing.  
-	The [Secure Pet Store](https://github.com/awslabs/api-gateway-secure-pet-store) is a sample Java application which uses Lambda and API Gateway with Cognito (for user identity).  
-	[aws-lambda-list](https://github.com/unixorn/aws-lambda-list) is a list of "hopefully useful AWS lambdas and lambda-related resources". Quite a few code samples here; as usual, not guaranteed tested. Caveat Emptor.  
  
🚧 [*Please help expand this incomplete section.*](CONTRIBUTING.md)  
  
# API Gateway  
-----------  
  
## API Gateway Basics  
  
-	📒 [Homepage](https://aws.amazon.com/api-gateway/) ∙ [Developer guide](http://docs.aws.amazon.com/apigateway/latest/developerguide/) ∙ [FAQ](https://aws.amazon.com/api-gateway/faqs/) ∙ [Pricing](https://aws.amazon.com/api-gateway/pricing/)  
-	**API Gateway** provides a scalable, secured front-end for service APIs, and can work with Lambda, Elastic Beanstalk, or regular EC2 services.  
-	It allows “serverless” deployment of applications built with Lambda.  
-	🔸Switching over deployments after upgrades can be tricky. There are no built-in mechanisms to have a single domain name migrate from one API gateway to another one. So it may be necessary to build an additional layer in front (even another API Gateway) to allow smooth migration from one deployment to another.  
  
  
## API Gateway Alternatives and Lock-In  
  
- [Kong](https://getkong.org) is an open-source, on-premises API and microservices gateway built on nginx with Lua. Kong is extensible through “plugins”.  
- [Tyk](https://tyk.io) is an open-source API gateway implemented in Go and available in the cloud, on-premises or hybrid.  
  
  
## API Gateway Tips  
  
-	🔹Prior to 2016-11, you could only send and receive plain text data (so people would base64-encode binary data), but binary data is [now](https://aws.amazon.com/about-aws/whats-new/2016/11/binary-data-now-supported-by-api-gateway/) supported.  
-	API Gateway supports the OpenApi specification (aka [Swagger](https://swagger.io/)). This allows you to describe your API in a language-agnostic way and use various tools to generate code supporting your API.  
-	Generating clients is extremely easy, either through the AWS console or using the get-sdk API.  
-	API Gateway integrates with CloudWatch out-of-the-box, allowing for easy logging of requests and responses.  
	-	Note that if your request or response are too large, CloudWatch will truncate the log. For full request/reply logging, make sure to do so in your integration (e.g. Lambda).  
	-	A good practice when calling API Gateway APIs is to log the request ID on the client. You can later refer to these request IDs in CloudWatch for easier tracing and debugging.  
-	There are multiple ways to secure your API, including built-in support for [AWS Cognito](http://docs.aws.amazon.com/apigateway/latest/developerguide/apigateway-integrate-with-cognito.html). For most use-cases, Cognito is the easiest and simplest way to authenticate users.  
	-	Although you can roll your own solution using a [custom authorizer](http://docs.aws.amazon.com/apigateway/latest/developerguide/use-custom-authorizer.html), which is basically a Lambda you define that determines if a request is acceptable or not.  
-	While API Gateway lends itself well to REST-style development, it's perfectly reasonable to implement an RPC-style API in API Gateway as well. Depending on your use-case, this can often lead to a much simpler API structure and smoother client experience.  
	-	RPC-style APIs are particularly useful when designing services that sit deeper in the stack and don't serve content directly to users.  
  
  
  
## API Gateway Gotchas and Limitations  
  
-	🔸API Gateway only supports encrypted (https) endpoints, and does not support unencrypted HTTP. (This is probably a good thing.)  
-	🔸API Gateway doesn’t support multi-region deployments for high availability. It is a service that is deployed in a single region but comes with a global endpoint that is served from AWS edge locations (similar to a CloudFront distribution). You cannot have multiple API Gateways with the same hostname in different AWS regions and use Route 53 to distribute the traffic. More in [this forum post](https://forums.aws.amazon.com/thread.jspa?messageID=735342&#735342).  
- 🔸Integration timeout: All of the various integration types (eg: Lambda, HTTP) for API Gateway have timeouts, as described [here](http://docs.aws.amazon.com/apigateway/latest/developerguide/limits.html#api-gateway-limits). Unlike some limits, these timeouts can't be increased.  
- 🔸API Gateway returns a 504 status code for any network or low level transport related issue. When this happens, you may see a message in the CloudWatch logs for the request that includes the message: `Execution failed due to an internal error`. One possible reason for this error is that even though your backend server is up and running, it may be doing something outside of the HTTP specification (like not sending well-formed chunked messages). You can test by hitting your backend directly with the `curl --raw -S -i <backend-endpoint-url>` and seeing if it complains.  
-	🔸AWS X-Ray support exists but cumbersome to use. If you have other AWS services calling API Gateway, your trace will seemingly end there. API Gateway will also not appear as a node in your service map. [More here](http://docs.aws.amazon.com/xray/latest/devguide/xray-services-apigateway.html).  
-	🔸Be careful using the export feature. The resulting Swagger template is often incomplete and doesn't integrate well with the Swagger extensions for things such as CORS.  
-	🔸Many changes to API Gateway resources need to be 'deployed' via console or API call. Unfortunately, API Gateway is terrible about notifying the user when changes are staged for deployment and what changes require deployment. If you've changed something about your API and it's not taking effect, there's a decent chance you just need to deploy it.  
	-	In particular, when deploying an API Gateway as part of a CloudFormation stack, changes will not automatically deploy unless the deployment resource itself was changed. You can change work around this by always changing the deployment resource on a CloudFormation update, or running a custom resource that ensures the deployment is made.  
	-	Alternatively, by using the [Serverless Application Model](https://github.com/awslabs/serverless-application-model) definition for an API Gateway resource, you can always expect the API to be deployed on a stack update since SAM will generate a new deployment every time.  
- 🔸API Gateway does not support nested query parameters on method requests.  
- 🔸API Gateway limits number of resources to 300, as described [here](http://docs.aws.amazon.com/apigateway/latest/developerguide/limits.html#api-gateway-limits). This is something to be considered when you start using API Gateway as a platform where your team/organization deploys to the same API Gateway.  
  
🚧 [*Please help expand this incomplete section.*](CONTRIBUTING.md)  
  
  
  
# Route 53  
--------  
  
## Route 53 Basics  
  
-	📒 [Homepage](https://aws.amazon.com/route53/) ∙ [Developer guide](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/) ∙ [FAQ](https://aws.amazon.com/route53/faqs/) ∙ [Pricing](https://aws.amazon.com/route53/pricing/)  
-	**Route 53** is AWS’ DNS service.  
  
  
## Route 53 Alternatives and Lock-In  
  
-	Historically, AWS was slow to penetrate the DNS market (as it is often driven by perceived reliability and long-term vendor relationships) but Route 53 has matured and [is becoming the standard option](https://www.datanyze.com/market-share/dns/) for many companies. Route 53 is cheap by historic DNS standards, as it has a fairly large global network with geographic DNS and other formerly “premium” features. It’s convenient if you are already using AWS.  
-	⛓Generally you don’t get locked into a DNS provider for simple use cases, but increasingly become tied in once you use specific features like geographic routing or Route 53’s alias records.  
-	🚪Many alternative DNS providers exist, ranging from long-standing premium brands like [UltraDNS](https://www.neustar.biz/services/dns-services) and [Dyn](http://dyn.com/managed-dns/) to less well known, more modestly priced brands like [DNSMadeEasy](http://www.dnsmadeeasy.com/). Most DNS experts will tell you that the market is opaque enough that reliability and performance don’t really correlate well with price.  
-	⏱Route 53 is usually somewhere in the middle of the pack on performance tests, e.g. the [SolveDNS reports](http://www.solvedns.com/dns-comparison/).  
  
  
## Route 53 Tips  
  
-	🔹Know about Route 53’s “alias” records:  
	-	Route 53 supports all the standard DNS record types, but note that [**alias resource record sets**](http://docs.aws.amazon.com/Route53/latest/DeveloperGuide/resource-record-sets-choosing-alias-non-alias.html) are not standard part of DNS, but a specific Route 53 feature. (It’s available from other DNS providers too, but each provider has a different name for it.)  
	-	Aliases are like an internal name (a bit like a CNAME) that is resolved internally on the server side. For example, traditionally you could have a CNAME to the DNS name of a CLB or ALB, but it’s often better to make an alias to the same load balancer. The effect is the same, but in the latter case, externally, all a client sees is the target the record points to.  
	-	It’s often wise to use alias record as an alternative to CNAMEs, since they can be updated instantly with an API call, without worrying about DNS propagation.  
	-	You can use them for CLBs/ALBs or any other resource where AWS supports it.  
	-	Somewhat confusingly, you can have CNAME and A aliases, depending on the type of the target.  
	-	Because aliases are extensions to regular DNS records, if exported, the output [zone file](https://en.wikipedia.org/wiki/Zone_file) will have additional non-standard “ALIAS” lines in it.  
-	[**Latency-based routing**](https://docs.aws.amazon.com/Route53/latest/DeveloperGuide/routing-policy.html#routing-policy-latency) allows users around the globe to be automatically directed to the nearest AWS region where you are running, so that latency is reduced.  
-	Understand that domain registration and DNS management (hosted zones) are two separate Route 53 services. When you buy/transfer a domain, Route 53 automatically assigns four name servers to it (e.g. ns-2.awsdns-00.com). Route 53 also offers to automatically create a hosted zone for DNS management, but you are not required do your DNS management in the same account or even in Route 53; you just need to create an NS record pointing to the servers assigned to your domain in Route 53.  
  - One use case would be to put your domain registration (very mission critical) in a [bastion account](https://cloudonaut.io/your-single-aws-account-is-a-serious-risk/) while managing the hosted zones within another account which is accessible by your applications.  
  
  
## Route 53 Gotchas and Limitations  
-   🔸Private Hosted Zone will only respond to DNS queries that originate from within a VPC. As a result Route53 will not respond to request made via a VPN or Direct connect. To get around this you will need to implement [Hybrid Cloud DNS Solutions](https://d1.awsstatic.com/whitepapers/hybrid-cloud-dns-options-for-vpc.pdf) or use the Simple AD provided IP addresses to query the hosted zone.  
  
  
# VPCs, Network Security, and Security Groups  
-------------------------------------------  
  
## VPC Basics  
  
-	📒 [Homepage](https://aws.amazon.com/vpc/) ∙ [User guide](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide) ∙ [FAQ](https://aws.amazon.com/vpc/faqs/) ∙ [Security groups](https://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-network-security.html) ∙ [Pricing](https://aws.amazon.com/vpc/pricing/)  
-	**VPC** (Virtual Private Cloud) is the virtualized networking layer of your AWS systems.  
-	Most AWS users should have a basic understanding of VPC concepts, but few need to get into all the details. VPC configurations can be trivial or extremely complex, depending on the extent of your network and security needs.  
-	All modern AWS accounts (those created [after 2013-12-04](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-vpc.html)) are “EC2-VPC” accounts that support VPCs, and all instances will be in a default VPC. Older accounts may still be using “EC2-Classic” mode. Some features don’t work without VPCs, so you probably will want to [migrate](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-migrate.html).  
  
  
## VPC and Network Security Tips  
  
-	❗**Security groups** are your first line of defense for your servers. Be extremely restrictive of what ports are open to all incoming connections. In general, if you use CLBs, ALBs or other load balancing, the only ports that need to be open to incoming traffic would be port 22 and whatever port your application uses. Security groups access policy is 'deny by default'.  
-	**Port hygiene:** A good habit is to pick unique ports within an unusual range for each different kind of production service. For example, your web frontend might use 3010, your backend services 3020 and 3021, and your Postgres instances the usual 5432. Then make sure you have fine-grained security groups for each set of servers. This makes you disciplined about listing out your services, but also is more error-proof. For example, should you accidentally have an extra Apache server running on the default port 80 on a backend server, it will not be exposed.  
-	**Migrating from Classic**: For migrating from older EC2-Classic deployments to modern EC2-VPC setup, [this article](https://blog.playfab.com/blog/how-playfab-migrated-ec2-classic-vpc-zero-downtime) may be of help.  
	-	You can [migrate Elastic IPs between EC2-Classic and EC2-VPC](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/elastic-ip-addresses-eip.html#using-eip-migration).  
-	For basic AWS use, one default VPC may be sufficient. But as you scale up, you should consider mapping out network topology more thoroughly. A good overview of best practices is [here](http://blog.flux7.com/blogs/aws/vpc-best-configuration-practices).  
-	Consider controlling access to you private AWS resources through a [VPN](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpn-connections.html).  
	-	You get better visibility into and control of connection and connection attempts.  
	-	You expose a smaller surface area for attack compared to exposing separate (potentially authenticated) services over the public internet.  
		-	e.g. A bug in the YAML parser used by the Ruby on Rails admin site is much less serious when the admin site is only visible to the private network and accessed through VPN.  
	-	Another common pattern (especially as deployments get larger, security or regulatory requirements get more stringent, or team sizes increase) is to provide a [bastion host](https://www.pandastrike.com/posts/20141113-bastion-hosts) behind a VPN through which all SSH connections need to transit.  
	-	For a cheap VPN to access private AWS resources, consider using a point-to-site software VPN such as [OpenVPN](https://openvpn.net/). It can either be installed using the [official AMI](https://docs.openvpn.net/how-to-tutorialsguides/virtual-platforms/amazon-ec2-appliance-ami-quick-start-guide/), though you are limited to 2 concurrent users on the free license, or it can be installed using the openvpn package on linux. The linux package allows for unlimited concurrent users but the installation is less straightforward. This [OpenVPN installer script](https://github.com/Nyr/openvpn-install) can help you install it and add client keys easily.  
-	🔹Consider using other security groups as sources for security group rules instead of using CIDRs — that way, all hosts in the source security group and only hosts in that security group are allowed access. This is a much more dynamic and secure way of managing security group rules.  
-	**VPC Flow Logs** allow you to monitor the network traffic to, from, and within your VPC. Logs are stored in CloudWatch Logs groups, and can be used for security monitoring (with third party tools), performance evaluation, and forensic investigation.  
	-	See the [VPC Flow Logs User Guide](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/flow-logs.html) for basic information.  
	-	See the [flowlogs-reader](https://github.com/obsrvbl/flowlogs-reader) CLI tool and Python library to retrieve and work with VPC Flow Logs.  
- **IPv6** [is available in VPC](https://aws.amazon.com/blogs/aws/new-ipv6-support-for-ec2-instances-in-virtual-private-clouds/). Along with this announcement came the introduction of the [Egress-Only Internet Gateway](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/egress-only-internet-gateway.html). In cases where one would use NAT Gateways to enable egress-only traffic for their VPC in IPv4, one can use an Egress-Only Internet Gateway for the same purpose in IPv6.  
  
-	Amazon provides an IPv6 CIDR block for your VPC at your request - at present you cannot implement your own IPv6 block if you happen to own one already.  
-	New and existing VPCs can both use IPv6. Existing VPCs will need to be configured to have an IPv6 CIDR block associated with them, just as new VPCs do.  
  
  
## PrivateLink  
- 📒[Homepage](https://aws.amazon.com/privatelink/) ∙ [User Guide](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) ∙  [Pricing](https://aws.amazon.com/privatelink/pricing/)  
- One of the uses for Private link is [Interface VPC Endpoints](https://docs.aws.amazon.com/vpc/latest/userguide/vpce-interface.html) deploys an ENI into your VPC and subnets which allows you direct access to the AWS API's as if the were accessible locally in your VPC without having to go out to the internet.  
- Another use case would be to expose a service of your own to other accounts in AWS through a [VPC Endpoint Service](https://docs.aws.amazon.com/vpc/latest/userguide/endpoint-service.html)  
  
  
## VPC and Network Security Gotchas and Limitations  
-	🔸VPCs are tied to one Region in one Account. Subnets are tied to one VPC and limited to one Availability Zone.  
-	🔸Security groups are tied to one VPC. If you are utilizing infrastructure in multiple VPCs you should make sure your configuration/deployment tools take that into account.  
-	🔸[VPC Endpoints](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-endpoints.html) are currently only available for S3 and DynamoDB. If you have a security requirement to lockdown outbound traffic from your VPC you may want to use [DNS filtering](https://aws.amazon.com/blogs/security/how-to-add-dns-filtering-to-your-nat-instance-with-squid/) to control outbound traffic to other services.  
-	❗Be careful when choosing your VPC IP CIDR block: If you are going to need to make use of [ClassicLink](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html), make sure that your private IP range [doesn’t overlap](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/vpc-classiclink.html#classiclink-limitations) with that of EC2 Classic.  
-	❗If you are going to peer VPCs, carefully consider the cost of [data transfer between VPCs](https://aws.amazon.com/vpc/faqs/#Peering_Connections), since for some workloads and integrations, this can be prohibitively expensive.  
- ❗New RDS instances require a [subnet group](http://docs.aws.amazon.com/AmazonRDS/latest/UserGuide/USER_VPC.WorkingWithRDSInstanceinaVPC.html#USER_VPC.Subnets) within your VPC. If you’re using the [default VPC](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html) this isn’t a concern, it will contain a subnet for each availability zone in your region. However, if you’re creating your own VPC and plan on using RDS, make sure you have at least two subnets within the VPC to act as the subnet group.  
-	❗If you delete the default VPC, you can [recreate it via the CLI or the console](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html#create-default-vpc).  
-	❗Be careful with VPC VPN credentials! If lost or compromised, the VPN endpoint must be deleted and recreated. See the instructions for [Replacing Compromised Credentials](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPC_VPN.html#CompromisedCredentials).  
-	❗Security Groups and Route Tables apply entries separately for IPv4 and IPv6, so one must ensure they add entries for both protocols accordingly.  
- 	💸Managed NAT gateways are a convenient alternative to  
manually managing [NAT instances](https://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/VPCNATInstance.html), but they do come at a cost per gigabyte. Consider [alternatives](http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/vpc-nat-comparison.html) if you're transferring many terabytes from private subnets to the internet. If you transfer terabytes/petabytes of data from EC2 instances in private subnets to S3, avoid the [NAT gateway data processing charge](https://aws.amazon.com/vpc/pricing/) by setting up a Gateway Type VPC Endpoint and route the traffic to/from S3 through the VPC endpoints instead of going through the NAT gateways.  
  
  
  
  
# DirectConnect  
-------------  
  
## DirectConnect Basics  
  
-	📒 [Homepage](https://aws.amazon.com/directconnect/) ∙ [User guide](http://docs.aws.amazon.com/directconnect/latest/UserGuide/) ∙ [FAQ](https://aws.amazon.com/directconnect/faqs/) ∙ [Pricing](https://aws.amazon.com/directconnect/pricing/)  
-	**Direct Connect** is a private, dedicated connection from your network(s) to AWS.  
  
  
## DirectConnect Tips  
  
-	If your data center has [a partnering relationship](https://aws.amazon.com/directconnect/partners/) with AWS, setup is streamlined.  
-	Use for more consistent predictable network performance guarantees (**1 Gbps** or **10 Gbps** per link).  
-	Use to peer your colocation, corporate, or physical datacenter network with your VPC(s).  
	-	Example: Extend corporate LDAP and/or Kerberos to EC2 instances running in a VPC.  
	-	Example: Make services that are hosted outside of AWS for financial, regulatory, or legacy reasons callable from within a VPC.  
  
Reds  
  
# Certificate Manager  
-------------------  
  
## Certificate Manager Basics  
  
- 📒 [Homepage](https://aws.amazon.com/certificate-manager/) ∙ [User guide](http://docs.aws.amazon.com/acm/latest/userguide/acm-overview.html) ∙ [FAQ](https://aws.amazon.com/certificate-manager/faqs/) ∙ [Pricing](https://aws.amazon.com/certificate-manager/pricing/)  
- Use the **Certificate Manager** to manage SSL/TLS certificates in other AWS services.  
- Supports importing existing certificates as well as issuing new ones.  
- Provides Domain Validated (DV) certificates. [Validation](http://docs.aws.amazon.com/acm/latest/userguide/gs-acm-validate.html) can be done in two ways. The first (and recommended) way is [via DNS](https://docs.aws.amazon.com/acm/latest/userguide/gs-acm-validate-dns.html). If the zone lives within Route 53 and the user has access, the necessary record can be added in the console via a single click during the certificate request process. If the zone is not within Route 53 the user is required to update DNS manually. This is still preferred to the second way, which requires more user interaction, and is done by sending an email to 3 contact addresses in WHOIS and 5 common addresses for the domain, for each domain name present in the request.  
- ACM will attempt to automatically [renew](http://docs.aws.amazon.com/acm/latest/userguide/how-domain-validation-works.html) a certificate issued by Amazon. It will first attempt to connect to the domain on HTTPS and check that the certificate used by the domain is the same with the certificate that it intends to renew. Failing that, it will check the DNS record used previously for validation. Failing that, ACM will attempt manual validation by sending emails to all domains in the certificate.  
  
  
## Certificate Manager Alternatives and Lock-in  
  
- ⛓Certificates issued by the Certificate Manager can’t be used outside of the services that support it. Imported certificates, however, can still be used elsewhere.  
  
  
## Certificate Manager Tips  
  
- 🔹**Supported services:** Managed [Load Balancers](AWS%20정리.md#load-balancers), [CloudFront](AWS%20정리.md#cloudfront), [API Gateway](AWS%20정리.md#api-gateway) and [Elastic Beanstalk](https://aws.amazon.com/elasticbeanstalk/).  
- 🔸During the domain validation process, if DNS validation is unsuccessful Certificate Manager will send an email to every contact address specified in the domain’s WHOIS record and up to five common administrative addresses. Some anti-spam filters can mark emails as spam because of this. You should check the spam folder of your email if you don’t receive a confirmation email.  
- 🔹 Setting up a certificate for a test domain you don't have email set up on? You can now use DNS validation instead.  
- 🔹Remember when requesting a wildcard domain that the request will not be valid for the level just below the wildcard, or any subdomains preceding the wildcard. Take for example an approved, issued certificate for `*.bar.example.com`. This would be valid for `foo.bar.example.com` but not `bar.example.com`. Likewise it would also not be valid for `www.bar.foo.example.com`. You would need to add each of these domains to the certificate request.  
  
  
## Certificate Manager Gotchas and Limitations  
  
- 🔸In order to use **Certificate Manager** for CloudFront distributions, the certificate must be issued or imported from us-east-1 (N. Virginia) region.  
- 🔸Certificates used with Elastic Load Balancers must be issued in the same region as the load balancer. Certificates can not be moved or copied between regions, as of July 2017. If a domain uses load balancers present in multiple regions, a different certificate must be requested for each region.  
- 🔸**IoT** has its [own way](http://docs.aws.amazon.com/iot/latest/developerguide/create-device-certificate.html) of setting up certificates.  
- 🔸By default the maximum number of domains per certificate is 10. You can get this limit increased to a maximum of 100 by contacting AWS support. **Note** for every different domain you have on the requested cert, you'll need to press accept on an email sent to that domain. For example if you request a cert with 42 different domains or sub domains, you'll need to press accept on 42 different links.  
	- 🔹If you request a limit increase to AWS support for this, they will respond to you asking to confirm this. Bypass this by saying in the body of your initial request:  
```"I acknowledge at the moment, there is no method to add or remove a name from a certificate. Instead, you must request a new certificate with the revised namelist and you must then re-approve all of the names in the certificate, even if they'd been previously approved."```  
- 🔸There is no way at the moment to add or remove a domain to/from an existing certificate. You must request a new certificate and re-approve it from each of the domains requested.  
  
  
  
  
  
# Batch  
-------------------  
  
## Batch Basics  
  
-	📒 [Homepage](https://aws.amazon.com/batch/) ∙ [Documentation](https://aws.amazon.com/documentation/batch/) ∙ [FAQ](https://aws.amazon.com/batch/faqs/) ∙ [Pricing](https://aws.amazon.com/batch/pricing/)  
- **AWS Batch** is a service that offers an environment to run batch computing jobs. The service dynamically provisions the optimal compute resources needed by the jobs based on their resource requirements, and can scale up to hundreds of thousands of [jobs](http://docs.aws.amazon.com/batch/latest/userguide/jobs.html).  
- These batch workloads have access to all other AWS services and features.  
- AWS Batch, coupled with [spot instances](https://aws.amazon.com/blogs/compute/cost-effective-batch-processing-with-amazon-ec2-spot/) can help run the jobs when appropriate capacity is available, providing for optimal utilization of compute resources.  
- The batch workloads are built as a [Docker](https://www.docker.com/) Image. These images can then pushed to the [EC2 Container Registry](https://aws.amazon.com/ecr/) (ECR), or any private repository that can be accessed from AWS.  
- A [Job Definition](http://docs.aws.amazon.com/batch/latest/userguide/job_definitions.html) has the workload's Docker Image URI, and also lets the users specify the environment details like vCPUs, memory, volume mappings, environment variables, parameters, retry strategy, container properties, and the job's IAM role.  
- The [Compute Environments](http://docs.aws.amazon.com/batch/latest/userguide/compute_environments.html) are EC2 clusters that provide the runtime for the batch workloads to execute in.  
- AWS Batch provides managed, as well as unmanaged compute environments. The Managed Environments are provisioned and managed by AWS, while the Unmanaged Environments are managed by the customers.  
- The Job Definitions are submitted to [Job Queue(s)](http://docs.aws.amazon.com/batch/latest/userguide/job_queues.html) for execution. Each queue has a priority, and has at least one Compute Environment associated with it.  
- AWS Batch uses [ECS](https://aws.amazon.com/ecs/) to execute the containerized jobs.  
  
  
## Batch Tips  
  
- AWS Batch supports prioritization of jobs via the Job Queue Priority. Higher the number - higher the priority.  
- AWS Batch supports launching the Compute Environment into specific VPC and subnets.  
- A Compute Environment is same as an [ECS Cluster](http://docs.aws.amazon.com/AmazonECS/latest/developerguide/ECS_clusters.html).  
- There is no additional cost for AWS Batch. You only pay the cost associated with the AWS Services being used - like EC2 Instances and any resources consumed by the batch jobs.  
- Associate [IAM Roles and policies](http://docs.aws.amazon.com/batch/latest/userguide/IAM_policies.html) with the Compute Environment to enable the containers access to other AWS resources.  
- 🔹 Use Unmanaged Compute Environments if you need specialized resources like Dedicated Hosts, or [EFS](https://aws.amazon.com/efs/).  
  
  
  
# High Availability  
-----------------  
  
This section covers tips and information on achieving [high availability](https://en.wikipedia.org/wiki/High_availability).  
  
  
  
## High Availability Tips  
  
-	AWS offers two levels of redundancy, [regions and availability zones (AZs)](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-regions-availability-zones.html#concepts-regions-availability-zones).  
-	When used correctly, regions and zones do allow for high availability. You may want to use non-AWS providers for larger business risk mitigation (i.e. not tying your company to one vendor), but reliability of AWS across regions is very high.  
-	**Multiple regions:** Using multiple regions is complex, since it’s essentially like managing completely separate infrastructures. It is necessary for business-critical services with the highest levels of redundancy. However, for many applications (like your average consumer startup), deploying extensive redundancy across regions may be overkill.  
-	The [High Scalability Blog](http://highscalability.com/blog/2016/1/11/a-beginners-guide-to-scaling-to-11-million-users-on-amazons.html) has a good guide to help you understand when you need to scale an application to multiple regions.  
-	🔹**Multiple AZs:** Using AZs wisely is the primary tool for high availability!  
	-	A typical single-region high availability architecture would be to deploy in two or more availability zones, with load balancing in front, as in [this AWS diagram](http://media.amazonwebservices.com/architecturecenter/AWS_ac_ra_ftha_04.pdf).  
	-	The bulk of outages in AWS services affect one zone only. There have been rare outages affecting multiple zones simultaneously (for example, the [great EBS failure of 2011](http://aws.amazon.com/message/65648/)) but in general most customers’ outages are due to using only a single AZ for some infrastructure.  
	-	Consequently, design your architecture to minimize the impact of AZ outages, especially single-zone outages.  
	-	Deploy key infrastructure across at least two or three AZs. Replicating a single resource across more than three zones often won’t make sense if you have other backup mechanisms in place, like S3 snapshots.  
	-	A second or third AZ should significantly improve availability, but additional reliability of 4 or more AZs may not justify the costs or complexity (unless you have other reasons like capacity or Spot market prices).  
	-	💸Watch out for **cross-AZ traffic costs**. This can be an unpleasant surprise in architectures with large volume of traffic crossing AZ boundaries.  
	-	Deploy instances evenly across all available AZs, so that only a minimal fraction of your capacity is lost in case of an AZ outage.  
	-	If your architecture has single points of failure, put all of them into a single AZ. This may seem counter-intuitive, but it minimizes the likelihood of any one SPOF to go down on an outage of a single AZ.  
-	**EBS vs instance storage:** For a number of years, EBSs had a poorer track record for availability than instance storage. For systems where individual instances can be killed and restarted easily, instance storage with sufficient redundancy could give higher availability overall. EBS has improved, and modern instance types (since 2015) are now EBS-only, so this approach, while helpful at one time, may be increasingly archaic.  
-	Be sure to [use and understand CLBs/ALBs](AWS%20정리.md#load-balancers) appropriately. Many outages are due to not using load balancers, or misunderstanding or misconfiguring them.  
  
  
## High Availability Gotchas and Limitations  
  
-	🔸**AZ naming** differs from one customer account to the next. Your “us-west-1a” is not the same as another customer’s “us-west-1a” — the letters are assigned to physical AZs randomly per account. This can also be a gotcha if you have multiple AWS accounts. Note that Zone IDs are consistent between accounts, and can be used to reliably align between AWS accounts.  
-	🔸💸**Cross-AZ traffic** is not free. At large scale, the costs add up to a significant amount of money. If possible, optimize your traffic to stay within the same AZ as much as possible.  
  
Billing and Cost Management  
---------------------------  
  
## Billing and Cost Visibility  
  
-	AWS offers a [**free tier**](https://aws.amazon.com/free/) of service, that allows very limited usage of resources at no cost. For example, a micro instance and small amount of storage is available for no charge. Many services are only eligible for the free tier for the first twelve months that an account exists, but other services offer a free usage tier indefinitely. (If you have an old account but starting fresh, sign up for a new one to qualify for the free tier.) [AWS Activate](https://aws.amazon.com/activate/) extends this to tens of thousands of dollars of free credits to startups in [certain funds or accelerators](https://aws.amazon.com/activate/portfolio-detail/).  
-	You can set [**billing alerts**](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/free-tier-alarms.html) to be notified of unexpected costs, such as costs exceeding the free tier. You can set these in a [granular way](https://wblinks.com/notes/aws-tips-i-wish-id-known-before-i-started/#billing).  
-	AWS offers [Cost Explorer](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/cost-explorer-what-is.html), a tool to get better visibility into costs.  
-	Unfortunately, the AWS console and billing tools are rarely enough to give good visibility into costs. For large accounts, the AWS billing console can time out or be too slow to use.  
-	**Tools:**  
	-	🔹Enable [billing reports](https://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/detailed-billing-reports.html) and install an open source tool to help manage or monitor AWS resource utilization. [**Teevity Ice**](https://github.com/Teevity/ice) (originally written by Netflix) is probably the first one you should try. Check out [docker-ice](https://github.com/jonbrouse/docker-ice) for a Dockerized version that eases installation.  
	-	🔸One challenge with Ice is that it doesn’t cover amortized cost of reserved instances.  
	-	Other tools include [Security Monkey](https://github.com/Netflix/security_monkey) and [Cloud Custodian](https://github.com/capitalone/cloud-custodian).  
	-	Use [AWS Simple Monthly Calculator](https://calculator.s3.amazonaws.com/index.html) to get an estimate of usage charges for AWS services based on certain information you provide. Monthly charges will be based on your actual usage of AWS services, and may vary from the estimates the Calculator has provided.  
-	**Third-party services:** Several companies offer services designed to help you gain insights into expenses or lower your AWS bill, such as [Cloudability](https://www.cloudability.com/), [CloudHealth Technologies](https://www.cloudhealthtech.com/), and [ParkMyCloud](http://www.parkmycloud.com/). Some of these charge a percentage of your bill, which may be expensive. See the [market landscape](AWS%20정리.md#tools-and-services-market-landscape).  
-	AWS’s [Trusted Advisor](https://aws.amazon.com/premiumsupport/trustedadvisor/) is another service that can help with cost concerns.  
-	Don’t be shy about asking your account manager for guidance in reducing your bill. It’s their job to keep you happily using AWS.  
-	**Tagging for cost visibility:** As the infrastructure grows, a key part of managing costs is understanding where they lie. It’s strongly advisable to [tag resources](https://aws.amazon.com/blogs/aws/resource-groups-and-tagging/), and as complexity grows, group them effectively. If you [set up billing allocation appropriately](http://aws.amazon.com/blogs/aws/aws-cost-allocation/), you can then get visibility into expenses according to organization, product, individual engineer, or any other way that is helpful.  
-	If you need to do custom analysis of raw billing data or want to feed it to a third party cost analysis service, [enable](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/detailed-billing-reports.html#turnonreports) the [detailed billing report](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/detailed-billing-reports.html#detailed-billing-report) feature.  
-	Multiple Amazon accounts can be linked for billing purposes using the [Consolidated Billing](http://docs.aws.amazon.com/awsaccountbilling/latest/aboutv2/consolidated-billing.html) feature. Large enterprises may need complex billing structures depending on ownership and approval processes.  
-	Multiple Amazon accounts can be managed centrally using [AWS Organizations](https://aws.amazon.com/organizations/).  
  
  
## AWS Data Transfer Costs  
  
-	For deployments that involve significant network traffic, a large fraction of AWS expenses are around data transfer. Furthermore, costs of data transfer, within AZs, within regions, between regions, and into and out of AWS and the internet vary significantly depending on deployment choices.  
-	Some of the most common gotchas:  
	-	🔸*AZ-to-AZ traffic:* Note EC2 traffic between AZs is effectively the same as between regions. For example, deploying a Cassandra cluster across AZs is helpful for [high availability](AWS%20정리.md#high-availability), but can hurt on network costs.  
	-	🔸*Using public IPs when not necessary:* If you use an Elastic IP or public IP address of an EC2 instance, you will incur network costs, even if it is accessed locally within the AZ.  
	-	🔸*Managed NAT Gateway data processing:* Managed NAT Gateways are used to let traffic egress from private subnets--at a cost of 4.5¢ as a data processing fee layered on top of data transfer pricing. Past a certain point, running your own NAT instances becomes far more cost effective.  
	-	🔸*Some services do cross-AZ traffic for free:* Many AWS services you'd not consider on their own merits offer a hidden value of free cross-AZ data transfer. EFS, RDS, MSK, and others are examples of this.  
-	This figure gives an overview:  
  
  
![Pasted image 20230226163650.png](../attachments/Pasted%20image%2020230226163650.png)  
## EC2 Cost Management  
  
-	With EC2, there is a trade-off between engineering effort (more analysis, more tools, more complex architectures) and spend rate on AWS. If your EC2 costs are small, many of the efforts here are not worth the engineering time required to make them work. But once you know your costs will be growing in excess of an engineer’s salary, serious investment is often worthwhile.  
-	Larger instances aren’t necessarily priced higher in the spot market – therefore, you should look at the available options and determine which instances will be most cost effective for your jobs. See [Bid Advisor](https://aws.amazon.com/ec2/spot/bid-advisor/).  
-	🔹**Spot instances:**  
	-	EC2 [Spot instances](https://aws.amazon.com/ec2/spot/) are a way to get EC2 resources at significant discount — often many times cheaper than standard on-demand prices — if you’re willing to accept the possibility that they be terminated with little to no warning.  
	-	Use Spot instances for potentially very significant discounts whenever you can use resources that may be restarted and don’t maintain long-term state.  
	-	The huge savings that you can get with Spot come at the cost of a significant increase in complexity when provisioning and reasoning about the availability of compute capacity.  
	-	Amazon maintains Spot prices at a market-driven fluctuating level, based on their inventory of unused capacity. Prices are typically low but can [spike](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-limits.html#spot-bid-limit) very high. See the [price history](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/using-spot-instances-history.html) to get a sense for this.  
	-	You set a bid price high to indicate how high you’re willing to pay, but you only pay the going rate, not the bid rate. If the market rate exceeds the bid, your instance may be terminated.  
	-	Prices are per instance type and per availability zone. The same instance type may have wildly different price in different zones at the same time. Different instance types can have very different prices, even for similarly powered instance types in the same zone.  
	-	Compare prices across instance types for better deals.  
	-	Use Spot instances whenever possible. Setting a high bid price will assure your machines stay up the vast majority of the time, at a fraction of the price of normal instances.  
	-	Get notified up to two minutes before price-triggered shutdown by polling [your Spot instances’ metadata](https://aws.amazon.com/blogs/aws/new-ec2-spot-instance-termination-notices/), or by watching for [the termination CloudWatch event](https://aws.amazon.com/about-aws/whats-new/2018/01/amazon-ec2-spot-two-minute-warning-is-now-available-via-amazon-cloudwatch-events/).  
	-	Make sure your usage profile works well for Spot before investing heavily in tools to manage a particular configuration.  
-	**Spot fleet:**  
	-	You can realize even bigger cost reductions at the same time as improvements to fleet stability relative to regular Spot usage by using [Spot fleet](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-fleet.html) to bid on instances across instance types, availability zones, and (through multiple Spot Fleet Requests) regions.  
	-	Spot fleet targets maintaining a specified (and weighted-by-instance-type) total capacity across a cluster of servers. If the Spot price of one instance type and availability zone combination rises above the weighted bid, it will rotate running instances out and bring up new ones of another type and location up in order to maintain the target capacity without going over target cluster cost.  
-	**Spot usage best practices:**  
	-	**Application profiling:**  
		-	Profile your application to figure out its runtime characteristics. That would help give an understanding of the minimum cpu, memory, disk required. Having this information is critical before you try to optimize spot costs.  
		-	Once you know the minimum application requirements, instead of resorting to fixed instance types, you can bid across a variety of instance types (that gives you higher chances of getting a spot instance to run your application).E.g., If you know that 4 cpu cores are enough for your job, you can choose any instance type that is equal or above 4 cores and that has the least Spot price based on history. This helps you bid for instances with greater discount (less demand at that point).  
	-	**Spot price monitoring and intelligence:**  
		-	Spot Instance prices fluctuate depending on instance types, time of day, region and availability zone. The AWS CLI tools and API allow you to describe Spot price metadata given time, instance type, and region/AZ.  
		-	Based on history of Spot instance prices, you could potentially build a myriad of algorithms that would help you to pick an instance type in a way that **optimizes cost**, **maximizes availability**, or **offers predictable performance**.  
		-	You can also track the number of times an instance of certain type got taken away (out bid) and plot that in graphite to improve your algorithm based on time of day.  
	-	**Spot machine resource utilization:**  
		-	For running spiky workloads (spark, map reduce jobs) that are schedule based and where failure is non critical, Spot instances become the perfect candidates.  
		-	The time it takes to satisfy a Spot instance could vary between 2-10 mins depending on the type of instance and availability of machines in that AZ.  
		-	If you are running an infrastructure with hundreds of jobs of spiky nature, it is advisable to start pooling instances to optimize for cost, performance and most importantly time to acquire an instance.  
		-	Pooling implies creating and maintaining Spot instances so that they do not get terminated after use. This promotes re-use of Spot instances across jobs. This of course comes with the overhead of lifecycle management.  
		-	Pooling has its own set of metrics that can be tracked to optimize resource utilization, efficiency and cost.  
		-	Typical pooling implementations give anywhere between 45-60% cost optimizations and 40% reduction in spot instance creation time.  
		-	An excellent example of Pooling implementation described by Netflix ([part1](http://techblog.netflix.com/2015/09/creating-your-own-ec2-spot-market.html), [part2](http://techblog.netflix.com/2015/11/creating-your-own-ec2-spot-market-part-2.html)\)  
-	**Spot management gotchas**  
	-	🔸**Lifetime:** There is [no guarantee](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html) for the lifetime of a Spot instance. It is purely based on bidding. If anyone outbids your price, the instance is taken away. Spot is not suitable for time sensitive jobs that have strong SLA. Instances will fail based on demand for Spot at that time. AWS provides a [two-minute warning](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/spot-interruptions.html#spot-instance-termination-notices) before Amazon EC2 must terminate your Spot instance.  
	-	🔹**API return data:** - The Spot price API returns Spot prices of varying granularity depending on the time range specified in the api call.E.g If the last 10 min worth of history is requested, the data is more fine grained. If the last 2 day worth of history is requested, the data is more coarser. Do not assume you will get all the data points. There **will** be skipped intervals.  
	-	❗**Lifecycle management:** Do not attempt any fancy Spot management unless absolutely necessary. If your entire usage is only a few machines and your cost is acceptable and your failure rate is lower, do not attempt to optimize. The pain for building/maintaining it is not worth just a few hundred dollar savings.  
-	**Reserved Instances:** allow you to get significant discounts on EC2 compute hours in return for a commitment to pay for instance hours of a specific instance type in a specific AWS region and availability zone for a pre-established time frame (1 or 3 years). Further discounts can be realized through “partial” or “all upfront” payment options.  
	-	Consider using Reserved Instances when you can predict your longer-term compute needs and need a stronger guarantee of compute availability and continuity than the (typically cheaper) Spot market can provide. However be aware that if your architecture changes your computing needs may change as well so long term contracts can seem attractive but may turn out to be cumbersome.  
	-	There are two types of Reserved Instances - [Standard and Convertible](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/reserved-instances-types.html). If you purchase excess Standard Reserved Instances, you may offer to “sell back” unused Reserved Instances via the [Reserved Instance Marketplace](http://docs.aws.amazon.com/AWSEC2/latest/UserGuide/ri-market-selling-guide.html), this allows you to potentially recoup the cost of unused EC2 compute instance hours by selling them to other AWS customers.  
	-	Instance reservations are not tied to specific EC2 instances - they are applied at the billing level to eligible compute hours as they are consumed across all of the instances in an account.  
	-	📜There have been scattered reports of Convertible RI purchases needing to be exercised in a block-- namely, if you buy five convertible RIs in one purchase, you can't convert just two of them. Reach out to your account manager for clarification if this may impact you.  
-	If you have multiple AWS accounts and have configured them to roll charges up to one account using the “Consolidated Billing” feature, you can expect *unused* Reserved Instance hours from one account to be applied to matching (region, availability zone, instance type) compute hours from another account.  
-	If you have multiple AWS accounts that are linked with Consolidated Billing, plan on using reservations, and want unused reservation capacity to be able to apply to compute hours from other accounts, you’ll need to create your instances in the availability zone with the same *name* across accounts. Keep in mind that when you have done this, your instances may not end up in the same *physical* data center across accounts - Amazon shuffles availability zones names across accounts in order to equalize resource utilization.  
-	Make use of dynamic [Auto Scaling](AWS%20정리.md#auto-scaling), where possible, in order to better match your cluster size (and cost) to the current resource requirements of your service.  
-	If you use RHEL instances and happen to have existing RHEL on-premise Red Hat subscriptions, then you can leverage Red Hat's [Cloud Access program](https://www.redhat.com/en/technologies/cloud-computing/cloud-access) to migrate a portion of your on-premise subscriptions to AWS, and thereby saving on AWS charges for RHEL subscriptions. You can either use your own self-created RHEL AMI's or Red Hat provided [Gold Images](https://access.redhat.com/articles/2962171) that will be added to your private AMI's once you sign up for Red Hat Cloud Access.  
  
Further Reading  
---------------  
  
This section covers a few unusually useful or “must know about” resources or lists.  
  
-	AWS  
	-	[AWS In Plain English](https://www.expeditedssl.com/aws-in-plain-english): A readable overview of all the AWS services.  
	-	[Awesome AWS](https://github.com/donnemartin/awesome-aws): A curated list of AWS tools and software.  
	-	[AWS Tips I Wish I'd Known Before I Started](https://wblinks.com/notes/aws-tips-i-wish-id-known-before-i-started/): A list of tips from [Rich Adams](https://richadams.me/)  
	-	[AWS Whitepapers](https://aws.amazon.com/whitepapers/): A list of technical AWS whitepapers, covering topics such as architecture, security and economics.  
	-	[Last Week in AWS](https://lastweekinaws.com): A weekly email newsletter covering the latest happenings in the AWS ecosystem.  
	-	[AWS Geek](https://www.awsgeek.com): A blog by AWS Community Hero Jerry Hargrove, with notes and hand-drawn diagrams about various AWS services.  
-	General references  
	-	[AWS Well Architected Framework Guide](https://d0.awsstatic.com/whitepapers/architecture/AWS_Well-Architected_Framework.pdf): Amazon’s own 56 page guide to operational excellence - guidelines and checklists to validate baseline security, reliability, performance (including high availability) and cost optimization practices.  
	-	[Awesome Microservices](https://github.com/mfornos/awesome-microservices): A curated list of tools and technologies for microservice architectures. Worth browsing to learn about popular open source projects.  
	-	[Is it fast yet?](https://istlsfastyet.com/): Ilya Grigorik’s TLS performance overview  
	-	[High Performance Browser Networking](https://hpbn.co/): A full, modern book on web network performance; a presentation on the HTTP/2 portion is [here](https://docs.google.com/presentation/d/1r7QXGYOLCh4fcUq0jDdDwKJWNqWK1o4xMtYpKZCJYjM/edit?usp=sharing).  
  
