---
layout: post
title: 싸피 최종 프로젝트를 마무리하며
subtitle: 싸피 최종 프로젝트 소개 및 회고
categories: etc
---


저번달 중순에 SSAFY 8기를 수료하였습니다.

사실 최종 프로젝트는 그보다 이른 수료 한달전에 마무리 되었지만 그동안 코딩테스트와 면접준비로 인하여 프로젝트를 회고하는것이 좀 늦어졌습니다.

늦어진 만큼 프로젝트를 진행하면서 발생했던 문제와 해결방안 그리고 앞으로의 방향성에 대해 깊게 정리하는 시간을 가질수 있게 된것 같습니다.

회고에 들어가기전 진행했던 프로젝트에 대해 먼저 소개하겠습니다.

## 프로젝트 개요

---

![ssafy_final_1.png](/img/post/ssafy_final_1.png)

- 프로젝트 명 : 니땅 내땅
- 기간 : 2023.04.10 ~ 2023.05.19
- 인원 : 6명
- Github Repository : [https://github.com/yourgroundmyground/ygmg](https://github.com/yourgroundmyground/ygmg)
- 간단 설명 : 땅따먹기와 달리기를 결합한 런닝앱
- 기획 의도 : 러닝과 땅따먹기를 결합한 컨텐츠를 개발하여 MZ세대의 건강 트렌드인 '헬시 플레저'를 추구하는 서비스를 만들었습니다. ‘니땅내땅’은 매일 한 번씩 게임을 플레이하면서 자신의 영토를 확장하고, 다른 사용자의 영토를 뺏을 수 있는 요소를 포함하고 있습니다. 게임 참여를 통해 사용자는 건강한 즐거움을 느끼고, 지역의 러닝인들과 함께 달리기를 즐길 수 있습니다.

프로젝트 기획 단계은 저번 프로젝트에서 느낀 부분을 반영하여 프로젝트가 시작하기전 미리 팀원들과 프로젝트의 주제를 어느정도 fix하여 기획단계에 사용되는 시간을 많이 단축시킬수 있었습니다.

사용 기술 스택으로는 아래와 같습니다.

## 기술 스택 및 환경

---

- Spring Boot 2.7.10
- Spring Batch
- AWS RDS
- AWS API Gateway
- Redis
- AWS Lightsail
- Docker 23.0.1
- TeamCity 2022.04.2
- nginx/1.18.0
- RabbitMQ

위와 같은 기술스택을 선택한 이유를 하나씩 하나씩 선택하여 이야기 해보도록 하겠습니다.

![ssafy_final_2.png](/img/post/ssafy_final_2.png)

첫번째 Backend의 주요 기술로는 Spring Boot를 사용하였고, 런닝 + 게임이라는 주제에 맞게 주마다 사람들과 런닝을 통해 땅을 먹는 기능을 주 기능으로 잡았기 때문에 일주일 단위로 Batch Job을 실행시켜 랭킹을 매기고, 해당 런닝데이터를 처리하여 자신의 마이페이지에서 확인할수 있도록 하기 위하여 Spring Batch 기술을 사용하였습니다.

![ssafy_final_3.png](/img/post/ssafy_final_3.png)

다음으로 사용된 기술은 Amazon Aws에서 제공하는 관계형 데이터베이스 AWS RDS를 사용하였고  DB엔진으로는 MySQL을 사용하였습니다.

![ssafy_final_4.png](/img/post/ssafy_final_4.png)

그리고 런닝, 회원, 게임등의 여러 서비스을 효율적으로 관리하고 보안기능으로 클라이언트의 요청을 안전하게 처리하기위하여 AWS API Gateway를 사용하였으며

![ssafy_final_5.png](/img/post/ssafy_final_5.png)

실시간 게임 랭킹을 저장하고, 회원 서비스에서 Refresh 토큰을 안전하게 저장하고 갱신하는등의 사용자 인증과 보안을 효율적으로 관리하기 위하여 사용하였습니다.

![ssafy_final_6.png](/img/post/ssafy_final_6.png)

그리고 AWS 에서 제공하는 클라우드 컴퓨팅 서비스인 LightSail을 사용하였습니다.

아 여기서 EC2를 사용하지 않고 LightSail을 사용한 이유는 … SSAFY에서 서버를 제공받았기 때문에 잘모르겠습니다…. (요금 때문인가?..)

이왕 LightSail 서버를 SSAFY로부터 제공 받았기 때문에 EC2 서버와 어떠한 차이가 있는지 확인해보겠습니다.

|   | Amazon Lightsail | Amazon EC2 |
| --- | --- | --- |
| 사용 | 사용자 지정 코드 및 일반 CMS를 포함한 간단한 웹 애플리케이션 및 웹 사이트에 사용됩니다. | HPC, 빅 데이터 및 분석 워크로드와 같은 소규모 내지 엔터프라이즈 애플리케이션에 사용됩니다. |
| 성능 | 소규모에서 중간 규모에 이르는 워크로드가 있는 애플리케이션에 사용됩니다. | 복잡한 아키텍처에서 소규모 이상의 워크로드에 사용됩니다. |
| 편의성 | 몇 번의 클릭만으로 Lightsail에서 애플리케이션을 배포할 수 있습니다.올인원 경험을 제공합니다. | Amazon EC2에서 애플리케이션을 배포하는 것은 애플리케이션 유형, 사용된 구성 요소 유형 등과 같은 여러 요인에 따라 달라집니다.각 구성 요소에는 해당 콘솔에서 수정할 수 있는 고유한 특성과 기능이 있습니다. |
| 관리 지원 | Lightsail에서는 시스템 관리자와 시스템 아키텍트의 수고를 덜 수 있습니다. | 환경 유형에 따라 관리에 필요한 노력이 달라집니다. EC2의 대부분의 서비스는 구성 요소에 대한 철저한 이해가 필요합니다. |
| 네트워크 | AWS에서 관리합니다. 고객은 Lightsail 방화벽에 규칙을 추가할 수 있습니다. | VPC 및 관련 구성 요소를 사용하여 고객이 관리합니다. |
| 서브넷 | Lightsail에는 프라이빗 서브넷에 대한 개념이 없습니다. | 고객은 애플리케이션 요구 사항에 따라 퍼블릭 또는 프라이빗 서브넷을 생성할 수 있습니다. |
| 확장성 | Lightsail에서 자동 인스턴스 확장성은 지원되지 않습니다.시작 후에는 인스턴스를 수정할 수 없습니다. 플랜을 변경하려면 새 인스턴스를 시작해야 합니다. | Amazon EC2 Auto Scaling 그룹을 사용하여 인스턴스를 자동으로 크기 조정할 수 있습니다.EC2 인스턴스는 새로운 유형 또는 새로운 가상화로 수정할 수 있습니다. |
| 리소스 관리의 유연성 | 네트워크, 하드 디스크, 로드 밸런서 등과 같은 리소스 관리의 유연성을 최소화합니다. | 고객은 애플리케이션 요구 사항에 따라 모든 관련 구성 요소를 관리할 수 있습니다. |
| 탄력적 볼륨 | 지원되지 않음 | 지원됨 |
| 리소스 관리 | 모든 리소스는 동일한 대시보드에서 관리됩니다. | 각 리소스에는 자체 콘솔과 옵션이 있습니다. |
| 요금 | 가격이 저렴하고 고정 가격 모델이 있습니다. | 요금은 사용한 만큼 지불하는 모델을 따릅니다. |
| 로드 밸런싱 | Lightsail 인스턴스에서는 Lightsail 로드 밸런서를 사용할 수 있습니다. | 여러 유형의 로드 밸런서를 사용할 수 있습니다. |
| 모니터링 | 모니터링을 사용할 수 있지만 몇 가지 옵션으로 제한됩니다. | 세부 모니터링 옵션은 Amazon Cloudwatch를 사용하여 이용할 수 있습니다. |
| 백업 | Lightsail 스냅샷을 사용하여 백업을 할 수 있습니다. | 스냅샷 및 AMI로 백업을 할 수 있습니다. |
| 암호화 | 암호화는 기본적으로 활성화되어 있으며 AWS에서 관리합니다. | 고객은 암호화 사용 또는 사용 중지를 선택할 수 있습니다. |
| 프리 티어 | 프리 티어는 가입 당일부터 3개월 동안 사용할 수 있습니다. | 프리 티어는 가입 당일부터 12개월 동안 사용할 수 있습니다. |
| 지원 | 지원은 AWS Support 팀에서 제공합니다. 애플리케이션 수준의 문제를 해결하는 범위는 제한되어 있습니다. | 지원은 AWS Support 팀에서 제공합니다. 애플리케이션 수준의 문제를 해결하는 범위는 제한되어 있습니다. |

**참고**

[https://inpa.tistory.com/entry/AWS-📚-Lightsail-vs-EC2-비교-어느게-좋을까](https://inpa.tistory.com/entry/AWS-%F0%9F%93%9A-Lightsail-vs-EC2-%EB%B9%84%EA%B5%90-%EC%96%B4%EB%8A%90%EA%B2%8C-%EC%A2%8B%EC%9D%84%EA%B9%8C)

![ssafy_final_7.png](/img/post/ssafy_final_7.png)

그리고 CI/CD툴로 Teamcity를 선택한 이유로는 이전 이동욱님의 블로그에서 [Teamcity에 관한 글](https://jojoldu.tistory.com/448)을 보게 되었습니다.

많은 사람들이 사용하는 CI/CD 툴인 Jenkins의 UI/UX가 불편하며, 자바 기반으로 만들어져서 거의 모든 OS를 지원하며, 무료 버전으로도 작은 스타트업도 충분한 성능을 가지고 있다. 라는 포스팅 내용을 보면서 Jenkins말고 이번 프로젝트에서는 Teamcity를 사용해보자라는 생각을 가지게 되었고 팀에 적극적으로 어필하여 해당 기술을 사용하게 되었습니다.

프로젝트에서 위 기술을 사용하여 CI/CD 체인을 구성한 내용은 따로 정리하여 포스팅 해보겠습니다.

![ssafy_final_8.png](/img/post/ssafy_final_8.png)

그리고 NGINX입니다.

처음에는 NGINX가 아닌 AWS ELB(Elastic LoadBalance)를 사용할 예정이였습니다.

그래서 해당 기술을 사용하기 위하여 AWS IAM을 통해 권한을 양도받을 생각이였지만 제공 받은 서버가 Lightsail이라는걸 알게되었고 LightSail은 AWS IAM을 통한 권한 양도가 불가능하기 때문에 NGINX를 사용하는방법으로 아키텍처를 변경하였습니다.

그렇게 탄생한 아키텍처 구성도는 아래와 같습니다.

![ssafy_final_9.png](/img/post/ssafy_final_9.png)

## 프로젝트 회고

---

### 맡았던 부분

---

- 아키텍처 설계
- 런닝 서비스 API 작성
- RabbitMQ(이벤트 버스)를 활용하여 이벤트 처리를 구현
- Teamcity를 활용한 CI/CD 체인 구축
- NGINX, AWS api gateway 구축

### 좋았던점

---

- 첫번째 SSAFY 프로젝트를 진행하면서 일정과 팀원간의 소통이 잘 맞지 않았고 이런부분을 최대한 개선하기위하여 이번 프로젝트에서는 일정공유나 팀원들간의 다양한 소통을 위해 노력하여 프로젝트를 일정내에 마무리 할수 있었습니다.
- 해당 프로젝트는 RabbitMQ(이벤트 버스)를 사용한 이벤트 기반 아키텍처를 지향하였습니다. 여러 서비스 간의 느슨한 결합과 서비스간의 비동기적인 처리를 위하여 여러 블로그나 책과 같은 자료를 참고하여 구현하였습니다.
- 실제 서비스를 제공한다는 생각으로 AWS API Gateway와 NGINX를 활용하여 2번의 SSL 인증과 트래픽 처리를 위한 로드밸런싱을 도입하였습니다.
- 사용자의 위치 정보를 받아와 서버에서 해당 좌표를 기반으로 면적을 계산하고 처리하였으며, 사용자들끼리 먹은 땅이 겹칠경우 해당 부분을 연산해서 랭킹에 다시 반영하는 로직을 처리하였습니다. 이러한 로직을 처리하는데 자바 JTS 라이브러리를 주로 사용하였습니다.

### 부족한점

---

- 이 프로젝트는 주된 기능으로 게임을 완료한 후 해당 게임에서 생성된 런닝 데이터를 마이페이지에서 확인할 수 있도록, 게임 서비스와 런닝 서비스 간의 이벤트를 통해 데이터 처리가 이루어지도록 구성되었습니다. 그러나 저희는 이벤트 기반 아키텍처를 중요시 여겨, 프로젝트 아키텍처를 설계하고 구현하도록 노력하였지만 위에 내용과 같은 한정된 부분에만 그러한 이벤트 처리 방식을 구현한 부분이 아쉬움으로 남습니다.
- 또한 이벤트 기반 아키텍처를 구현하면서 우아한 테크 블로그의 [이벤트 기반 아키텍처 구축하기](https://techblog.woowahan.com/7835/)라는 글을 많이 참고 하였는데 해당 포스트에서도 볼수 있듯이 이벤트 기반 아키텍처를 구현하면서 발생할수 있는 문제점이 되게 많지만 이에 대해 크게 생각하지 않고 프로젝트를 진행하여 많은 사용자가 발생했을때 문제가 발생할수 있는 여지를 많이 남겼다는점(유지보수 비용이 많이 발생할 가능성이 크다)이 매우 큰 아쉬움을 남겼습니다.
- 이번 프로젝트는 테스트 코드를 거의 작성하지 않고 프로젝트를 진행했다는 부분이 가장 큰 아쉬움으로 남습니다. 아직은 테스트 코드를 작성하는데 익숙하지 않아 시간이 오래 걸리며, 프로젝트 마감기한이 짧다는 이유로 테스트 코드를 작성하는데 소홀했던 점이 너무나 아쉽습니다.

가장 크게 느낀점은 서비스에 맞는 아키텍처가 있고 그러한 아키텍처를 잘 설계하는것이 중요하다는것입니다.

저희 팀은 더 많은 기술과 다양한 경험을 쌓기 위하여 여러 아키텍처를 공부하고 도입하였던 것이였지만 실제로 프로젝트를 마무리하면서 이번 프로젝트를 MSA 방식으로 구현한것이 맞는것인가? 또는 저희가 구현한 이벤트 기반 아키텍처가 아닌 다른 아키텍처를 도입하였으면 어땟을까? 라는 의문을 남기게 된거같습니다.

### 개선할점

---

- 이전 프로젝트를 진행할때 안전하게 AWS S3 버킷을 사용하는법이라는 [우아한 테크 블로그의 글](https://techblog.woowahan.com/6217/)을 읽고 그러한 내용을 반영하고자 하였으나 아쉽게도 시간이 부족해서 그러한작업을 하지 못하였습니다. 시간이 될때 이 프로젝트를 리팩토링을 진행하면서 해당 부분도 같이 진행해야한다고 생각합니다.
- 또한 위에서 말한 내용과 같이 이벤트 기반 아키텍처를 지향하며 프로젝트를 설계 및 구현하였기 때문에 이러한 아키텍처를 구현했을때 발생할수 있는 문제점에 대해 생각하며 리팩토링을 진행해야한다고 생각하며, 테스트 코드를 작성하여 그러한 오류가 무엇인지 확인하는 작업도 병행해야한다고 생각합니다.
- Batch의 코드는 제가 맡았던 부분은 아니지만 같이 확인하며 수정이 필요하다고 생각하여 이부분에 대해서도 같이 더 깊게 공부하여 수정하는 작업을 진행해야 한다고 생각합니다.
