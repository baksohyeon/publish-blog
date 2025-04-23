---
share: true
---
  
특정 포트를 찾아 포트를 닫고 싶으면 다음과 같이 쳐서 PID를 알아낸다.  
  
```shell  
sudo lsof -i :3000  
```  
  
여기서 3000이 포트번호이다.  
  
위에서 나온 PID를 다음 명령어에 넣으면 포트가 닫힌다.  
  
```shell  
sudo kill -9 PID  
```  
  
  
```shell  
kill -9 $(lsof -t -i:5000)  
```  
  
RabbitMq: **5672**  
  
```bash  
# 프론트  
mssh -r ap-northeast-2 -d -u "dada-dorito" ubuntu@i-047e3e5185e9024c8  
# 백엔드  
mssh -r ap-northeast-2 -d -u "dada-dorito" ubuntu@i-000f8e277436878f7  
  
# 베타 서버 (구)  
mssh -r ap-northeast-2 -d -u "dada-dorito" ec2-user@i-0b5cb525a46fb21c0  
  
# 베타 서버 (신)  
mssh -r ap-northeast-2 -d -u "dada-dorito" ec2-user@i-0d5d504d566b75f11  
  
  
tail -f /var/log/nginx/error.log  
sudo lsof -i -P -n | grep LISTEN   
  
  
  
scp -i [pem file] -r [upload folder] [user id]@[ec2 public IP]:~/[transfer address]  
```  
  
[awspf - AWS Profile 쉽게 전환하기](https://cloudest.oopy.io/posting/101)  
  
cat ~/.aws/credentials  
  
export AWS_PROFILE=prod-account  
  
puttygen /Users/dorito/dev/credential/aws-dada-instance-public-key.ppk  -O private-openssh -o aws-dada-key.pem  
  
```  
 mssh -r ap-northeast-2 -d -u "dada-dorito" ec2-user@i-0b5cb525a46fb21c0  
2024-03-14 00:46:19,153 - botocore.utils - DEBUG - IMDS ENDPOINT: http://169.254.169.254/  
2024-03-14 00:46:19,157 - botocore.credentials - DEBUG - Skipping environment variable credential check because profile name was explicitly set.  
2024-03-14 00:46:19,157 - botocore.credentials - DEBUG - Looking for credentials via: assume-role  
2024-03-14 00:46:19,157 - botocore.credentials - DEBUG - Looking for credentials via: assume-role-with-web-identity  
2024-03-14 00:46:19,157 - botocore.credentials - DEBUG - Looking for credentials via: sso  
2024-03-14 00:46:19,157 - botocore.credentials - DEBUG - Looking for credentials via: shared-credentials-file  
2024-03-14 00:46:19,158 - botocore.credentials - INFO - Found credentials in shared credentials file: ~/.aws/credentials  
2024-03-14 00:46:19,158 - botocore.loaders - DEBUG - Loading JSON file: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/botocore/data/endpoints.json  
2024-03-14 00:46:19,164 - botocore.loaders - DEBUG - Loading JSON file: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/botocore/data/sdk-default-configuration.json  
2024-03-14 00:46:19,164 - botocore.hooks - DEBUG - Event choose-service-name: calling handler <function handle_service_name_alias at 0x105616d40>  
2024-03-14 00:46:19,204 - botocore.loaders - DEBUG - Loading JSON file: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/botocore/data/ec2/2016-11-15/service-2.json.gz  
2024-03-14 00:46:19,238 - botocore.loaders - DEBUG - Loading JSON file: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/botocore/data/ec2/2016-11-15/endpoint-rule-set-1.json.gz  
2024-03-14 00:46:19,238 - botocore.loaders - DEBUG - Loading JSON file: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/botocore/data/partitions.json  
2024-03-14 00:46:19,243 - botocore.hooks - DEBUG - Event creating-client-class.ec2: calling handler <function add_generate_presigned_url at 0x10556d630>  
2024-03-14 00:46:19,243 - botocore.configprovider - DEBUG - Looking for endpoint for ec2 via: environment_service  
2024-03-14 00:46:19,243 - botocore.configprovider - DEBUG - Looking for endpoint for ec2 via: environment_global  
2024-03-14 00:46:19,243 - botocore.configprovider - DEBUG - Looking for endpoint for ec2 via: config_service  
2024-03-14 00:46:19,243 - botocore.configprovider - DEBUG - Looking for endpoint for ec2 via: config_global  
2024-03-14 00:46:19,243 - botocore.configprovider - DEBUG - No configured endpoint found.  
2024-03-14 00:46:19,273 - botocore.endpoint - DEBUG - Setting ec2 timeout as (60, 60)  
2024-03-14 00:46:19,274 - botocore.loaders - DEBUG - Loading JSON file: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/botocore/data/_retry.json  
2024-03-14 00:46:19,274 - botocore.client - DEBUG - Registering retry handlers for service: ec2  
2024-03-14 00:46:19,274 - botocore.hooks - DEBUG - Event before-parameter-build.ec2.DescribeInstances: calling handler <bound method ParameterAlias.alias_parameter_in_call of <botocore.handlers.ParameterAlias object at 0x105606e90>>  
2024-03-14 00:46:19,275 - botocore.hooks - DEBUG - Event before-parameter-build.ec2.DescribeInstances: calling handler <function generate_idempotent_uuid at 0x105638280>  
2024-03-14 00:46:19,275 - botocore.regions - DEBUG - Calling endpoint provider with parameters: {'Region': 'ap-northeast-2', 'UseDualStack': False, 'UseFIPS': False}  
2024-03-14 00:46:19,275 - botocore.regions - DEBUG - Endpoint provider result: https://ec2.ap-northeast-2.amazonaws.com  
2024-03-14 00:46:19,275 - botocore.hooks - DEBUG - Event before-call.ec2.DescribeInstances: calling handler <function add_recursion_detection_header at 0x105617eb0>  
2024-03-14 00:46:19,275 - botocore.hooks - DEBUG - Event before-call.ec2.DescribeInstances: calling handler <function inject_api_version_header_if_needed at 0x105639ab0>  
2024-03-14 00:46:19,275 - botocore.endpoint - DEBUG - Making request for OperationModel(name=DescribeInstances) with params: {'url_path': '/', 'query_string': '', 'method': 'POST', 'headers': {'Content-Type': 'application/x-www-form-urlencoded; charset=utf-8', 'User-Agent': 'aws-ec2-instance-connect-cli/1.0.3 md/Botocore#1.34.61 ua/2.0 os/macos#23.2.0 md/arch#arm64 lang/python#3.10.13 md/pyimpl#CPython cfg/retry-mode#legacy Botocore/1.34.61'}, 'body': {'Action': 'DescribeInstances', 'Version': '2016-11-15', 'InstanceId.1': 'i-0b5cb525a46fb21c0'}, 'url': 'https://ec2.ap-northeast-2.amazonaws.com/', 'context': {'client_region': 'ap-northeast-2', 'client_config': <botocore.config.Config object at 0x10797de10>, 'has_streaming_input': False, 'auth_type': None}}  
2024-03-14 00:46:19,275 - botocore.hooks - DEBUG - Event request-created.ec2.DescribeInstances: calling handler <bound method RequestSigner.handler of <botocore.signers.RequestSigner object at 0x10797dcc0>>  
2024-03-14 00:46:19,275 - botocore.hooks - DEBUG - Event choose-signer.ec2.DescribeInstances: calling handler <function set_operation_specific_signer at 0x105638160>  
2024-03-14 00:46:19,276 - botocore.auth - DEBUG - Calculating signature using v4 auth.  
2024-03-14 00:46:19,276 - botocore.auth - DEBUG - CanonicalRequest:  
POST  
/  
  
content-type:application/x-www-form-urlencoded; charset=utf-8  
host:ec2.ap-northeast-2.amazonaws.com  
x-amz-date:20240313T154619Z  
  
content-type;host;x-amz-date  
ab838c63e19c1bf66ec5b505fcc2dd38f93f860a87882e1d6f15d6b804cf53fa  
2024-03-14 00:46:19,276 - botocore.auth - DEBUG - StringToSign:  
AWS4-HMAC-SHA256  
20240313T154619Z  
20240313/ap-northeast-2/ec2/aws4_request  
eca84ede6069c72d16a723e9baa55c7ea5a483758cc9f8a18c20cfcc9e992136  
2024-03-14 00:46:19,276 - botocore.auth - DEBUG - Signature:  
9a3b6ba2d98e34041e5f73137c2e88139b5bbd21910db5ec33bdcfc0e585dca9  
2024-03-14 00:46:19,276 - botocore.hooks - DEBUG - Event request-created.ec2.DescribeInstances: calling handler <function add_retry_headers at 0x10563a170>  
2024-03-14 00:46:19,277 - botocore.endpoint - DEBUG - Sending http request: <AWSPreparedRequest stream_output=False, method=POST, url=https://ec2.ap-northeast-2.amazonaws.com/, headers={'Content-Type': b'application/x-www-form-urlencoded; charset=utf-8', 'User-Agent': b'aws-ec2-instance-connect-cli/1.0.3 md/Botocore#1.34.61 ua/2.0 os/macos#23.2.0 md/arch#arm64 lang/python#3.10.13 md/pyimpl#CPython cfg/retry-mode#legacy Botocore/1.34.61', 'X-Amz-Date': b'20240313T154619Z', 'Authorization': b'AWS4-HMAC-SHA256 Credential=AKIASOLCW3F3CHFHA6HV/20240313/ap-northeast-2/ec2/aws4_request, SignedHeaders=content-type;host;x-amz-date, Signature=9a3b6ba2d98e34041e5f73137c2e88139b5bbd21910db5ec33bdcfc0e585dca9', 'amz-sdk-invocation-id': b'1fa84053-2734-4d3b-a233-a3f7a6c66a75', 'amz-sdk-request': b'attempt=1', 'Content-Length': '76'}>  
2024-03-14 00:46:19,278 - botocore.httpsession - DEBUG - Certificate path: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/certifi/cacert.pem  
2024-03-14 00:46:19,490 - botocore.parsers - DEBUG - Response headers: {'x-amzn-RequestId': 'e7f0d6cb-480d-49c0-bfc5-99cd132c240d', 'Cache-Control': 'no-cache, no-store', 'Strict-Transport-Security': 'max-age=31536000; includeSubDomains', 'vary': 'accept-encoding', 'Content-Type': 'text/xml;charset=UTF-8', 'Transfer-Encoding': 'chunked', 'Date': 'Wed, 13 Mar 2024 15:46:18 GMT', 'Server': 'AmazonEC2'}  
2024-03-14 00:46:19,490 - botocore.parsers - DEBUG - Response body:  
b'<?xml version="1.0" encoding="UTF-8"?>\n<DescribeInstancesResponse xmlns="http://ec2.amazonaws.com/doc/2016-11-15/">\n    <requestId>e7f0d6cb-480d-49c0-bfc5-99cd132c240d</requestId>\n    <reservationSet>\n        <item>\n            <reservationId>r-0503d9b68f03e9a95</reservationId>\n            <ownerId>168247613814</ownerId>\n            <groupSet/>\n            <instancesSet>\n                <item>\n                    <instanceId>i-0b5cb525a46fb21c0</instanceId>\n                    <imageId>ami-097bf0ec147165215</imageId>\n                    <instanceState>\n                        <code>16</code>\n                        <name>running</name>\n                    </instanceState>\n                    <privateDnsName>ip-172-31-10-10.ap-northeast-2.compute.internal</privateDnsName>\n                    <dnsName>ec2-43-201-128-25.ap-northeast-2.compute.amazonaws.com</dnsName>\n                    <reason/>\n                    <keyName>mukda-key</keyName>\n                    <amiLaunchIndex>0</amiLaunchIndex>\n                    <productCodes/>\n                    <instanceType>t2.micro</instanceType>\n                    <launchTime>2024-02-26T08:58:27.000Z</launchTime>\n                    <placement>\n                        <availabilityZone>ap-northeast-2a</availabilityZone>\n                        <groupName/>\n                        <tenancy>default</tenancy>\n                    </placement>\n                    <monitoring>\n                        <state>disabled</state>\n                    </monitoring>\n                    <subnetId>subnet-016c36825bab93893</subnetId>\n                    <vpcId>vpc-0311d7f4aebe27faf</vpcId>\n                    <privateIpAddress>172.31.10.10</privateIpAddress>\n                    <ipAddress>43.201.128.25</ipAddress>\n                    <sourceDestCheck>true</sourceDestCheck>\n                    <groupSet>\n                        <item>\n                            <groupId>sg-0f5499c8901503bfc</groupId>\n                            <groupName>ec2-rds-10</groupName>\n                        </item>\n                        <item>\n                            <groupId>sg-087f3e80f294cd11d</groupId>\n                            <groupName>MukdaWebServer</groupName>\n                        </item>\n                    </groupSet>\n                    <architecture>x86_64</architecture>\n                    <rootDeviceType>ebs</rootDeviceType>\n                    <rootDeviceName>/dev/xvda</rootDeviceName>\n                    <blockDeviceMapping>\n                        <item>\n                            <deviceName>/dev/xvda</deviceName>\n                            <ebs>\n                                <volumeId>vol-08899f8eae6c1f6ef</volumeId>\n                                <status>attached</status>\n                                <attachTime>2024-02-26T08:58:28.000Z</attachTime>\n                                <deleteOnTermination>true</deleteOnTermination>\n                            </ebs>\n                        </item>\n                    </blockDeviceMapping>\n                    <virtualizationType>hvm</virtualizationType>\n                    <clientToken>fd980468-f2c9-409f-8c87-d447111c2999</clientToken>\n                    <tagSet>\n                        <item>\n                            <key>Name</key>\n                            <value>mukda-beta</value>\n                        </item>\n                    </tagSet>\n                    <hypervisor>xen</hypervisor>\n                    <networkInterfaceSet>\n                        <item>\n                            <networkInterfaceId>eni-08af3049feb18832b</networkInterfaceId>\n                            <subnetId>subnet-016c36825bab93893</subnetId>\n                            <vpcId>vpc-0311d7f4aebe27faf</vpcId>\n                            <description/>\n                            <ownerId>168247613814</ownerId>\n                            <status>in-use</status>\n                            <macAddress>02:be:18:97:f9:eb</macAddress>\n                            <privateIpAddress>172.31.10.10</privateIpAddress>\n                            <privateDnsName>ip-172-31-10-10.ap-northeast-2.compute.internal</privateDnsName>\n                            <sourceDestCheck>true</sourceDestCheck>\n                            <groupSet>\n                                <item>\n                                    <groupId>sg-0f5499c8901503bfc</groupId>\n                                    <groupName>ec2-rds-10</groupName>\n                                </item>\n                                <item>\n                                    <groupId>sg-087f3e80f294cd11d</groupId>\n                                    <groupName>MukdaWebServer</groupName>\n                                </item>\n                            </groupSet>\n                            <attachment>\n                                <attachmentId>eni-attach-02c61989d105180dd</attachmentId>\n                                <deviceIndex>0</deviceIndex>\n                                <status>attached</status>\n                                <attachTime>2024-02-26T08:58:27.000Z</attachTime>\n                                <deleteOnTermination>true</deleteOnTermination>\n                                <networkCardIndex>0</networkCardIndex>\n                            </attachment>\n                            <association>\n                                <publicIp>43.201.128.25</publicIp>\n                                <publicDnsName>ec2-43-201-128-25.ap-northeast-2.compute.amazonaws.com</publicDnsName>\n                                <ipOwnerId>168247613814</ipOwnerId>\n                            </association>\n                            <privateIpAddressesSet>\n                                <item>\n                                    <privateIpAddress>172.31.10.10</privateIpAddress>\n                                    <privateDnsName>ip-172-31-10-10.ap-northeast-2.compute.internal</privateDnsName>\n                                    <primary>true</primary>\n                                    <association>\n                                        <publicIp>43.201.128.25</publicIp>\n                                        <publicDnsName>ec2-43-201-128-25.ap-northeast-2.compute.amazonaws.com</publicDnsName>\n                                        <ipOwnerId>168247613814</ipOwnerId>\n                                    </association>\n                                </item>\n                            </privateIpAddressesSet>\n                            <ipv6AddressesSet/>\n                            <interfaceType>interface</interfaceType>\n                        </item>\n                    </networkInterfaceSet>\n                    <ebsOptimized>false</ebsOptimized>\n                    <enaSupport>true</enaSupport>\n                    <cpuOptions>\n                        <coreCount>1</coreCount>\n                        <threadsPerCore>1</threadsPerCore>\n                    </cpuOptions>\n                    <capacityReservationSpecification>\n                        <capacityReservationPreference>open</capacityReservationPreference>\n                    </capacityReservationSpecification>\n                    <hibernationOptions>\n                        <configured>false</configured>\n                    </hibernationOptions>\n                    <enclaveOptions>\n                        <enabled>false</enabled>\n                    </enclaveOptions>\n                    <metadataOptions>\n                        <state>applied</state>\n                        <httpTokens>required</httpTokens>\n                        <httpPutResponseHopLimit>2</httpPutResponseHopLimit>\n                        <httpEndpoint>enabled</httpEndpoint>\n                        <httpProtocolIpv4>enabled</httpProtocolIpv4>\n                        <httpProtocolIpv6>disabled</httpProtocolIpv6>\n                        <instanceMetadataTags>disabled</instanceMetadataTags>\n                    </metadataOptions>\n                    <maintenanceOptions>\n                        <autoRecovery>default</autoRecovery>\n                    </maintenanceOptions>\n                    <bootMode>uefi-preferred</bootMode>\n                    <currentInstanceBootMode>legacy-bios</currentInstanceBootMode>\n                    <platformDetails>Linux/UNIX</platformDetails>\n                    <usageOperation>RunInstances</usageOperation>\n                    <usageOperationUpdateTime>2024-02-26T08:58:27.000Z</usageOperationUpdateTime>\n                    <privateDnsNameOptions>\n                        <hostnameType>ip-name</hostnameType>\n                        <enableResourceNameDnsARecord>true</enableResourceNameDnsARecord>\n                        <enableResourceNameDnsAAAARecord>false</enableResourceNameDnsAAAARecord>\n                    </privateDnsNameOptions>\n                </item>\n            </instancesSet>\n        </item>\n    </reservationSet>\n</DescribeInstancesResponse>'  
2024-03-14 00:46:19,497 - botocore.hooks - DEBUG - Event needs-retry.ec2.DescribeInstances: calling handler <botocore.retryhandler.RetryHandler object at 0x10797f1c0>  
2024-03-14 00:46:19,497 - botocore.retryhandler - DEBUG - No retry needed.  
2024-03-14 00:46:19,499 - EC2InstanceConnect - DEBUG - Successfully got instance information from EC2 API for i-0b5cb525a46fb21c0  
2024-03-14 00:46:19,499 - botocore.hooks - DEBUG - Event choose-service-name: calling handler <function handle_service_name_alias at 0x105616d40>  
2024-03-14 00:46:19,500 - botocore.loaders - DEBUG - Loading JSON file: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/botocore/data/ec2-instance-connect/2018-04-02/service-2.json.gz  
2024-03-14 00:46:19,501 - botocore.loaders - DEBUG - Loading JSON file: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/botocore/data/ec2-instance-connect/2018-04-02/endpoint-rule-set-1.json.gz  
2024-03-14 00:46:19,502 - botocore.hooks - DEBUG - Event creating-client-class.ec2-instance-connect: calling handler <function add_generate_presigned_url at 0x10556d630>  
2024-03-14 00:46:19,502 - botocore.configprovider - DEBUG - Looking for endpoint for ec2-instance-connect via: environment_service  
2024-03-14 00:46:19,502 - botocore.configprovider - DEBUG - Looking for endpoint for ec2-instance-connect via: environment_global  
2024-03-14 00:46:19,502 - botocore.configprovider - DEBUG - Looking for endpoint for ec2-instance-connect via: config_service  
2024-03-14 00:46:19,502 - botocore.configprovider - DEBUG - Looking for endpoint for ec2-instance-connect via: config_global  
2024-03-14 00:46:19,502 - botocore.configprovider - DEBUG - No configured endpoint found.  
2024-03-14 00:46:19,502 - botocore.regions - DEBUG - Creating a regex based endpoint for ec2-instance-connect, ap-northeast-2  
2024-03-14 00:46:19,551 - botocore.endpoint - DEBUG - Setting ec2-instance-connect timeout as (60, 60)  
2024-03-14 00:46:19,553 - botocore.client - DEBUG - Registering retry handlers for service: ec2-instance-connect  
2024-03-14 00:46:19,554 - botocore.hooks - DEBUG - Event before-parameter-build.ec2-instance-connect.SendSSHPublicKey: calling handler <function generate_idempotent_uuid at 0x105638280>  
2024-03-14 00:46:19,554 - botocore.regions - DEBUG - Calling endpoint provider with parameters: {'Region': 'ap-northeast-2', 'UseDualStack': False, 'UseFIPS': False}  
2024-03-14 00:46:19,554 - botocore.regions - DEBUG - Endpoint provider result: https://ec2-instance-connect.ap-northeast-2.amazonaws.com  
2024-03-14 00:46:19,555 - botocore.hooks - DEBUG - Event before-call.ec2-instance-connect.SendSSHPublicKey: calling handler <function add_recursion_detection_header at 0x105617eb0>  
2024-03-14 00:46:19,555 - botocore.hooks - DEBUG - Event before-call.ec2-instance-connect.SendSSHPublicKey: calling handler <function inject_api_version_header_if_needed at 0x105639ab0>  
2024-03-14 00:46:19,555 - botocore.endpoint - DEBUG - Making request for OperationModel(name=SendSSHPublicKey) with params: {'url_path': '/', 'query_string': '', 'method': 'POST', 'headers': {'X-Amz-Target': 'AWSEC2InstanceConnectService.SendSSHPublicKey', 'Content-Type': 'application/x-amz-json-1.1', 'User-Agent': 'aws-ec2-instance-connect-cli/1.0.3 md/Botocore#1.34.61 ua/2.0 os/macos#23.2.0 md/arch#arm64 lang/python#3.10.13 md/pyimpl#CPython cfg/retry-mode#legacy Botocore/1.34.61'}, 'body': b'{"InstanceId": "i-0b5cb525a46fb21c0", "InstanceOSUser": "ec2-user", "SSHPublicKey": "ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQC110s8kB8tSvFf7siA+JQnnAAxroUi+fxjYDoJdkioxbhcOd9sMTsG0O/6KtIgihKlhp8Q0gvNcauvmFL5Gqk6OJhbIDtWOXKns4JgJtltGNfEPNyHa7G4nPREyc4/89ZYnraXWZXIW52slFS4fulUr/B6/VoZy4lmoCqbTRSHjtb/dJ79ggmRuS3n7nqhacSLuUPYkV7EIB9bjNWycSzCHFUvVgyjPR8HlOcWhSToNglYToGBrd89YN5TeeDEuvamH/CD03CDVF7dD8bvpR37MRLAweMH3UYeOIbNmHLKSs94O5wlfka8Kvk/EvE0V/FW3/iTJCV0m0v2d+NBgRuJ", "AvailabilityZone": "ap-northeast-2a"}', 'url': 'https://ec2-instance-connect.ap-northeast-2.amazonaws.com/', 'context': {'client_region': 'ap-northeast-2', 'client_config': <botocore.config.Config object at 0x10797ed70>, 'has_streaming_input': False, 'auth_type': None}}  
2024-03-14 00:46:19,555 - botocore.hooks - DEBUG - Event request-created.ec2-instance-connect.SendSSHPublicKey: calling handler <bound method RequestSigner.handler of <botocore.signers.RequestSigner object at 0x10797ecb0>>  
2024-03-14 00:46:19,555 - botocore.hooks - DEBUG - Event choose-signer.ec2-instance-connect.SendSSHPublicKey: calling handler <function set_operation_specific_signer at 0x105638160>  
2024-03-14 00:46:19,556 - botocore.auth - DEBUG - Calculating signature using v4 auth.  
2024-03-14 00:46:19,556 - botocore.auth - DEBUG - CanonicalRequest:  
POST  
/  
  
content-type:application/x-amz-json-1.1  
host:ec2-instance-connect.ap-northeast-2.amazonaws.com  
x-amz-date:20240313T154619Z  
x-amz-target:AWSEC2InstanceConnectService.SendSSHPublicKey  
  
content-type;host;x-amz-date;x-amz-target  
fd70080b42932e26454cf3289e4d39a419be8d13eb90972697eeb4bc231f4afb  
2024-03-14 00:46:19,556 - botocore.auth - DEBUG - StringToSign:  
AWS4-HMAC-SHA256  
20240313T154619Z  
20240313/ap-northeast-2/ec2-instance-connect/aws4_request  
5d6e4fa23bf3ec54db94a150cbfef4482b02dfa4392ff1325ba4c4978881c837  
2024-03-14 00:46:19,556 - botocore.auth - DEBUG - Signature:  
1f7ea3e745ebfb6999e548fd01affaafa3cd11bb4c59c29560cb5c05c247076a  
2024-03-14 00:46:19,556 - botocore.hooks - DEBUG - Event request-created.ec2-instance-connect.SendSSHPublicKey: calling handler <function add_retry_headers at 0x10563a170>  
2024-03-14 00:46:19,556 - botocore.endpoint - DEBUG - Sending http request: <AWSPreparedRequest stream_output=False, method=POST, url=https://ec2-instance-connect.ap-northeast-2.amazonaws.com/, headers={'X-Amz-Target': b'AWSEC2InstanceConnectService.SendSSHPublicKey', 'Content-Type': b'application/x-amz-json-1.1', 'User-Agent': b'aws-ec2-instance-connect-cli/1.0.3 md/Botocore#1.34.61 ua/2.0 os/macos#23.2.0 md/arch#arm64 lang/python#3.10.13 md/pyimpl#CPython cfg/retry-mode#legacy Botocore/1.34.61', 'X-Amz-Date': b'20240313T154619Z', 'Authorization': b'AWS4-HMAC-SHA256 Credential=AKIASOLCW3F3CHFHA6HV/20240313/ap-northeast-2/ec2-instance-connect/aws4_request, SignedHeaders=content-type;host;x-amz-date;x-amz-target, Signature=1f7ea3e745ebfb6999e548fd01affaafa3cd11bb4c59c29560cb5c05c247076a', 'amz-sdk-invocation-id': b'a8386582-1b02-4cc6-a5a8-21b60ca11420', 'amz-sdk-request': b'attempt=1', 'Content-Length': '506'}>  
2024-03-14 00:46:19,556 - botocore.httpsession - DEBUG - Certificate path: /Users/dorito/.local/share/mise/installs/python/3.10/lib/python3.10/site-packages/certifi/cacert.pem  
2024-03-14 00:46:20,382 - botocore.parsers - DEBUG - Response headers: {'x-amzn-RequestId': '8f7cb1e5-294b-462b-94c3-2a9c55cfd3be', 'Content-Type': 'application/x-amz-json-1.1', 'Content-Length': '67', 'Date': 'Wed, 13 Mar 2024 15:46:20 GMT'}  
2024-03-14 00:46:20,382 - botocore.parsers - DEBUG - Response body:  
b'{"RequestId":"8f7cb1e5-294b-462b-94c3-2a9c55cfd3be","Success":true}'  
2024-03-14 00:46:20,383 - botocore.hooks - DEBUG - Event needs-retry.ec2-instance-connect.SendSSHPublicKey: calling handler <botocore.retryhandler.RetryHandler object at 0x10797f760>  
2024-03-14 00:46:20,383 - botocore.retryhandler - DEBUG - No retry needed.  
2024-03-14 00:46:20,385 - EC2InstanceConnect - DEBUG - Successfully pushed the public key to i-0b5cb525a46fb21c0  
2024-03-14 00:46:20,385 - EC2InstanceConnect - DEBUG - Generated command: ssh -o "IdentitiesOnly=yes" -i /var/folders/w5/_rv1h9dd455gj82l3yk3yjvc0000gn/T/tmp6qaw75r3 ec2-user@43.201.128.25  
   ,     #_  
```  
  
```  
  
 ssh -v ec2-user@ec2-3-34-140-163.ap-northeast-2.compute.amazonaws.com -i ./dada-ec2.pem  
OpenSSH_9.4p1, LibreSSL 3.3.6  
debug1: Reading configuration data /etc/ssh/ssh_config  
debug1: /etc/ssh/ssh_config line 21: include /etc/ssh/ssh_config.d/* matched no files  
debug1: /etc/ssh/ssh_config line 54: Applying options for *  
debug1: Authenticator provider $SSH_SK_PROVIDER did not resolve; disabling  
debug1: Connecting to ec2-3-34-140-163.ap-northeast-2.compute.amazonaws.com port 22.  
debug1: Connection established.  
debug1: identity file ./dada-ec2.pem type -1  
debug1: identity file ./dada-ec2.pem-cert type -1  
debug1: Local version string SSH-2.0-OpenSSH_9.4  
debug1: Remote protocol version 2.0, remote software version OpenSSH_8.7  
debug1: compat_banner: match: OpenSSH_8.7 pat OpenSSH* compat 0x04000000  
debug1: Authenticating to ec2-3-34-140-163.ap-northeast-2.compute.amazonaws.com:22 as 'ec2-user'  
debug1: load_hostkeys: fopen /Users/dorito/.ssh/known_hosts2: No such file or directory  
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory  
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory  
debug1: SSH2_MSG_KEXINIT sent  
debug1: SSH2_MSG_KEXINIT received  
debug1: kex: algorithm: curve25519-sha256  
debug1: kex: host key algorithm: ssh-ed25519  
debug1: kex: server->client cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none  
debug1: kex: client->server cipher: chacha20-poly1305@openssh.com MAC: <implicit> compression: none  
debug1: expecting SSH2_MSG_KEX_ECDH_REPLY  
debug1: SSH2_MSG_KEX_ECDH_REPLY received  
debug1: Server host key: ssh-ed25519 SHA256:Tt8aXrxkyEz5NRfvOniHuzeErTg920cmotEDFgJ5eds  
debug1: load_hostkeys: fopen /Users/dorito/.ssh/known_hosts2: No such file or directory  
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts: No such file or directory  
debug1: load_hostkeys: fopen /etc/ssh/ssh_known_hosts2: No such file or directory  
debug1: Host 'ec2-3-34-140-163.ap-northeast-2.compute.amazonaws.com' is known and matches the ED25519 host key.  
debug1: Found key in /Users/dorito/.ssh/known_hosts:28  
debug1: rekey out after 134217728 blocks  
debug1: SSH2_MSG_NEWKEYS sent  
debug1: expecting SSH2_MSG_NEWKEYS  
debug1: SSH2_MSG_NEWKEYS received  
debug1: rekey in after 134217728 blocks  
debug1: get_agent_identities: bound agent to hostkey  
debug1: get_agent_identities: ssh_fetch_identitylist: agent contains no identities  
debug1: Will attempt key: ./dada-ec2.pem  explicit  
debug1: SSH2_MSG_EXT_INFO received  
debug1: kex_input_ext_info: server-sig-algs=<ssh-ed25519,sk-ssh-ed25519@openssh.com,ssh-rsa,rsa-sha2-256,rsa-sha2-512,ssh-dss,ecdsa-sha2-nistp256,ecdsa-sha2-nistp384,ecdsa-sha2-nistp521,sk-ecdsa-sha2-nistp256@openssh.com,webauthn-sk-ecdsa-sha2-nistp256@openssh.com>  
debug1: SSH2_MSG_SERVICE_ACCEPT received  
debug1: Authentications that can continue: publickey,gssapi-keyex,gssapi-with-mic  
debug1: Next authentication method: publickey  
debug1: Trying private key: ./dada-ec2.pem  
debug1: Authentications that can continue: publickey,gssapi-keyex,gssapi-with-mic  
debug1: No more authentication methods to try.  
ec2-user@ec2-3-34-140-163.ap-northeast-2.compute.amazonaws.com: Permission denied (publickey,gssapi-keyex,gssapi-with-mic).  
```  
  
  
---  
  
  
  
