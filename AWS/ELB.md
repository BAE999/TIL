# ELB
AWS에서 제공하는 트래픽 분산 서비스

- 사용자가 EC2에 접근하려면 모든 EC2 IP 주소를 알아야 함
  - 만약 EC2 한대가 종료된다면?
  - 만약 사용자가 관리 할 수 없을 정도로 EC2가 많다면?  
-> ELB

## Elastic Load Balancer
ELB는 들어오는 어플리케이션 트래픽을 Amazon 인스턴스, 컨테이너, IP 주소, 람다 함수와 같은 여러 대상에 자동 분산 시킴.

ELB는 단일 가용영역, 여러 가용 영역에서 다양한 어플리케이션 부하를 처리 가능

ELB는 제공하는 3가지 로드밸런서는 모두 어플리케이션의 내 결합성에 필요한 고가용성, 자동 확장/축소, 강력한 보안을 갖추고 있음

## 특징
다수의 서비스에 트래픽을 분산 시켜주는 서비스

Health Check
- 직접 트래픽을 발생시켜 Instance가 살아있는지 체크

AutoScaling과 연동 가능

여러 가용영역에 분산 가능

지속적으로 IP 주소가 바뀌며 IP 고정 불가능
- 항상 도메인 기반으로 사용

## 종류
Application Load Balancer
- 똑똑함
- 트래픽을 모니터링하여 라우팅 가능
- image.test.com -> 이미지 서버로, web.test.com -> 웹 서버로 트래픽 분산

Network Load Balancer
- 빠름
- TCP 기반 빠른 트래픽 분산
- Elastic IP 할당 가능

Classic Load Balancer
- 옛날
- 지금 잘 사용하지 않음

### ALB - Target Group
타겟 그룹
- ALB가 트래픽을 분산시킬 대상 (Target)을 논리적으로 묶어놓은 그룹
  
타겟 그룹 기준으로 요청 분배

EC2 인스턴스, IP 주소, Lambda 함수, ELB 타겟 그룹 가능

역할
- ALB -> 라우팅 규칙에 따라 타겟 그룹으로 요청 전달
- 타겟 그룹은 내부에서 등록된 인스턴스 중 헬스체크를 통과한 대상에게 트래픽 전달
- 타겟 그룹별로 포트, 프로토콜, 헬스체크 설정 가능

<!-- omit in toc\

 -->
#### ALB 구성
![](https://i.imgur.com/TlUTZac.png)