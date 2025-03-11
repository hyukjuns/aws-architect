# AWS Architecture Practice
Practice for AWS Architecture

VPC, Subnet, Routing Table

- Subnet에 명시적인 Routing Table Association이 없으면, VPC 메인 Routing Table과 연결됨
- Auto-assign public IP는 서브넷 단위

Network Firewall 비교
- NACL(Network ACL)
    - 서브넷 대상 적용, L4 계층 동작(TCP,UDP)
    - Stateless, In/Out 모두 열어줘야 통신 가능
    - 번호 순서대로 평가 하며 허용과 거부 규칙 가능
    - VPC 생성시 기본 NACL 함께 생성됨

- SG(Security Group)
    - ENI 대상 적용, L3,L4 계층(IP, 포트, 프로토콜)
    - Stateful, Inbound 혹은 Outbound 1개만 열어도 응답 패킷 허용
    - 모든 규칙 확인 후 허용된 것만 적용
    - EC2 생성시 ENI에 연결할 기본 SG 를 함께 생성 가능

- Azure의 네트워크 방화벽은 모두 Stateful 방화벽임 (NSG, FW 모두)


Network In/Out Flow
- Internet Gateway(IGW): 클라우드 외부 인터넷과 연결이 필요한 인스턴스의 In/Out bound Access를 가능하게 함, 서브넷의 Routing Table에서 IGW로 명시적인 Route 처리가 필요함 (0.0.0.0/0 to IGW)
- NAT Gateway: 인터넷에서 Inbound Access는 필요 없지만, VPC 내부에서 Outbound Access 가 필요할 경우 사용, NAT Gateway도 결국 외부 인터넷 접근시 IGW를 통해서 나가게됨
    
    | 기능 | **Internet Gateway (IGW)** | **NAT Gateway (NGW)** |
    | --- | --- | --- |
    | **주요 목적** | **퍼블릭 서브넷의 리소스를 인터넷과 연결** | **프라이빗 서브넷의 리소스가 인터넷에 접속하도록 허용 (반대로 인터넷에서 접근은 차단)** |
    | **사용 위치** | 퍼블릭 서브넷에 있는 인스턴스가 인터넷과 통신할 때 사용 | 프라이빗 서브넷에 있는 인스턴스가 인터넷과 통신할 때 사용 |
    | **트래픽 방향** | Inbound + Outbound 가능 | Outbound만 가능 (Inbound 차단) |
    | **Public IP 필요 여부** | EC2 인스턴스에 Public IP 필요 | EC2 인스턴스는 Private IP만 사용 가능 |
    | **예제** | 인터넷에 직접 노출된 웹 서버 (예: 프론트엔드 서버) | 인터넷 연결이 필요하지만 외부에서 접근 불가능해야 하는 서버 (예: DB, 내부 API) |
- EKS
- VPC Peering, GW
- EC2 Availabilty Options
- EC2 SKU and Disk Type
- IAM and RBAC
- Resource Management

---

### 참고

- **AWS 기초** **개념**
    - AWS를 사용한다면 반드시 알아야 할 네트워크 기초 지식
        
        [https://youtu.be/vCNexbgYmQ8](https://kor01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fyoutu.be%2FvCNexbgYmQ8&data=05%7C02%7Chyukjun%40cloocus.com%7Cebd41020b48648bfb3ef08dcce1b1807%7C355deae4a1d64d5ebe340ad0c20aaa0f%7C0%7C0%7C638611862984930759%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C0%7C%7C%7C&sdata=%2FSoFmANuXORvVRv1SPvP8Zk1McNhXJG%2FMVHQnubNxXo%3D&reserved=0)
        
    - Amazon EC2부터 서버리스 컴퓨팅까지, AWS 컴퓨팅 서비스 알아보기
        
        [https://www.youtube.com/watch?v=nljFw7Y_Uc0](https://kor01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3DnljFw7Y_Uc0&data=05%7C02%7Chyukjun%40cloocus.com%7Cebd41020b48648bfb3ef08dcce1b1807%7C355deae4a1d64d5ebe340ad0c20aaa0f%7C0%7C0%7C638611862984937248%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C0%7C%7C%7C&sdata=kjkZvSOKFvY9F%2BL1WRuGKjceFeQqM0pGv2ofVPSGBUA%3D&reserved=0)
        
    - AWS의 스토리지, 데이터베이스 개념 잡기
        
        [https://youtu.be/hAUjFd0BfAs](https://kor01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fyoutu.be%2FhAUjFd0BfAs&data=05%7C02%7Chyukjun%40cloocus.com%7Cebd41020b48648bfb3ef08dcce1b1807%7C355deae4a1d64d5ebe340ad0c20aaa0f%7C0%7C0%7C638611862984943405%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C0%7C%7C%7C&sdata=1lRiQ3Jqseei5hZ41L9mUBjhFn0OMZKfISFa1WJ02lg%3D&reserved=0)
        
- **Storage 관련** **기초** **개념**
    - 모든 기업을 위한 AWS 스토리지 서비스 쉽게 알아보기
        
        [https://youtu.be/erhyRR6mVNo](https://kor01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fyoutu.be%2FerhyRR6mVNo&data=05%7C02%7Chyukjun%40cloocus.com%7Cebd41020b48648bfb3ef08dcce1b1807%7C355deae4a1d64d5ebe340ad0c20aaa0f%7C0%7C0%7C638611862984949533%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C0%7C%7C%7C&sdata=QcRw%2Fzy7GpgVIF%2Fxapgb1W4USgGYaIp63jd5BTXQefg%3D&reserved=0)
        
    - 스마트한 클라우드 스토리지 비용 관리 전략
        
        [https://www.youtube.com/watch?v=4FZysarv7KY](https://kor01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3D4FZysarv7KY&data=05%7C02%7Chyukjun%40cloocus.com%7Cebd41020b48648bfb3ef08dcce1b1807%7C355deae4a1d64d5ebe340ad0c20aaa0f%7C0%7C0%7C638611862984955505%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C0%7C%7C%7C&sdata=Xp6qPvkRBg2T%2BQZGmnCJytMO%2BRR9zKbqWOrQKOnsDh0%3D&reserved=0)
        
    - AWS 스토리지 마이그레이션 서비스 및 대규모 데이터 전송 사례
        
        [https://www.youtube.com/watch?v=1TpgFbUa3fY](https://kor01.safelinks.protection.outlook.com/?url=https%3A%2F%2Fwww.youtube.com%2Fwatch%3Fv%3D1TpgFbUa3fY&data=05%7C02%7Chyukjun%40cloocus.com%7Cebd41020b48648bfb3ef08dcce1b1807%7C355deae4a1d64d5ebe340ad0c20aaa0f%7C0%7C0%7C638611862984961503%7CUnknown%7CTWFpbGZsb3d8eyJWIjoiMC4wLjAwMDAiLCJQIjoiV2luMzIiLCJBTiI6Ik1haWwiLCJXVCI6Mn0%3D%7C0%7C%7C%7C&sdata=dp%2FUaKcAcBAk7n3%2FWY5ytsZQEzAwZusZgeJOFRGHr1Q%3D&reserved=0)
## Architecture Samples
VPC, EC2 SSH Connections

![](/images/ec2-ssh.svg) 

VPC, IGW, NGW (public, Private Subnets)

![](/images/igw_ngw.png)

EC2 Fileshare, S3 Static Web hostirng

![](/images/ec2-s3-efs.png)

- EC2 High Availablity (Set/Zone/Region)
- L4 Load Balancer for Web
- L7 Load Balancer for Web
- Web - WAS - DB 3 Tier
- VPC Peering and VPC GW
- VPC Hub and Spoke
- AWS VPC to Azure Vnet 
- S3를 활용한 정적 웹사이트
    - S3에 HTML/CSS 파일 업로드하고 정적 웹사이트 호스팅
    - 퍼블릭 액세스 및 버킷 정책 설정 학습
- RDS와 EC2 연결
    - RDS에서 MySQL/PostgreSQL DB 생성 후 EC2에서 연결
    - 보안 그룹과 IAM 역할 개념 익히기
- Lambda와 API Gateway를 활용한 서버리스
    - 간단한 Python Lambda 함수 작성 및 API Gateway로 API 생성
    - 서버 없이 코드만으로 API를 운영하는 개념 학습
- CloudFormation과 Terraform으로 인프라 자동화
    - CloudFormation 템플릿을 사용해 EC2, S3, RDS 생성
    - Terraform을 활용해 코드 기반 인프라 관리

## Ref
- [EKS 설명](https://www.youtube.com/watch?v=E49Q3y9wsUo)