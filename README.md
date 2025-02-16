# aws-architect

## Basic

1. EC2 SSH Connect

    - ACL 은 Stateful 방화벽, 서브넷 단위
    - SG 는 Stateless 방화벽, 인스턴스 단위
    - 인터넷 통한 인스턴스 접근 위해서는 IGW 생성후 Routing Table 규칙 생성 필요 (0.0.0.0 to IGW)

![](/images/ec2-ssh.svg) 