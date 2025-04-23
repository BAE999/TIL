# 목차
- [목차](#목차)
- [Auto Scaling](#auto-scaling)
  - [Scaling](#scaling)
    - [Scale-Up, Vertical Scaling](#scale-up-vertical-scaling)
    - [Scale-Out, Horizontal Scaling](#scale-out-horizontal-scaling)
  - [Auto Scaling](#auto-scaling-1)
    - [구성요소](#구성요소)

# Auto Scaling
## Scaling
애플리케이션 또는 시스템의 성능을 높이기 위해 컴퓨팅 리소스를 확장하거나 축소하는 것

요청이 많아질 때 처리할 수 있도록 서버를 키우는 것

### Scale-Up, Vertical Scaling

![](https://i.imgur.com/BcYhbmS.png)

장점
- 단순하고 설정이 쉬움

단점
- 물리적인 한계 존재 (더 이상 업그레이드 불가), 100배 가능?!
- 재시작 필요성 있음
- 하나의 서버에만 의존 -> 장애 발생 시 위험

### Scale-Out, Horizontal Scaling

![](https://i.imgur.com/cWWGTYz.png)

장점
- 무중단 확장 가능

단점
- 복잡한 아키텍쳐, Load Balancer(ELB) 등의 구성 필요

## Auto Scaling
트래픽이 언제 늘고 줄지, 사람이 항상 직접 조절?

Auto Scaling
- 트래픽 상황에 맞춰 서버 수를 자동으로 늘리거나 줄여주는 AWS 서비스
  - 트래픽 증가 시 자동으로 인스턴스 증가
  - 사용량 감소시 자동으로 인스턴스 축소
  - 리소스 비용 절감 + 고가용성 유지

### 구성요소
<!-- omit in toc -->
### Launch Template
새 인스턴스를 생성할 대 필요한 설정을 담고 있는 설계도

어떤 AMI(이미지)로 만들지

인스턴스 타입(t3.micro 등)

키 페어, 보안 그룹

UserData 스크립트 (초기 셋팅 자동화)

<!-- omit in toc -->
### ASG
인스턴스를 묶어서 관리하는 단위

최소 / 최대 / 원하는 (Desired) 인스턴스 수 설정

실제 인스턴스 수를 계속 모니터링하고 자동 조절

Availability Zone 간 분산 가능

<!-- omit in toc -->
### Scaling Policy
언제, 어떻게 서버 수를 조절할지에 대한 규칙

Target Tracking
- 평균 CPU 60% 유지처럼 목표 설정
- Step Scaling
  - CPU 70~80% -> 1대, 80% 이상 -> + 2대
- Scheduled Scaling
  - 특정 시간에 확장 -> 매일 오후 6시

<!-- omit in toc -->
### CloudWatch
지표를 감시하다가 스케일링 정책을 실행하는 역할

CPU 사용률, 네트워크 트래픽, 메모리 등 모니터링

조건 충족 시 Scaling Policy 트리거

실시간 알람 + 로그 기록 가능