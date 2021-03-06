# 클라우드 컴퓨팅

### 기존 서버의 방식

전산실에 컴퓨터를 배치하고, 인터넷을 연결하여 서비스를 제공

서버 수용 능력이 한계에 도달하면 컴퓨터 수를 늘리거나, 컴퓨터의 성능을 높이는 방식을 사용

**[기존 방식의 한계]**

- **주기적인 유지 관리가 필요하다.**
  고장나거나, 인터넷 연결이 안되는 컴퓨터를 위한 인력 및 비용 발생
- **공간의 한계가 있다.**
  서버실에 필요할 때마다 추가적인 컴퓨터를 배치해 수용 능력을 높였는데, 공간이 부족하여 컴퓨터를 더 배치할 수 없는 문제에 직면할 수 있다.

⇒ 서버의 컴퓨터 성능을 높이고, 부피를 줄여 추가적으로 서버 증설을 한다.

⇒ 일부 거대 기업이 **데이터 센터**라는 건물을 세우고, 데이터 센터의 유휴 자원을 대여하는 서비스를 제공

⇒ 즉, 서버의 자원과 공간, 네트워크 환경의 제공을 빌려 사용하는 **클라우드 컴퓨팅**이 시작!

( 데이터 센터의 공간과 자원, 네트워크 환경을 빌려 사용하는 것 ⇒ **온프레미스**)

### Cloud(클라우드)

**가상화** 기술의 발전으로, 앞서 말한 데이터 센터와 비슷하지만, 물리적인 컴퓨터가 아닌, **가상 컴퓨터**를 대여하는 **클라우드 컴퓨팅!** (네트워크를 기반으로 한다. 인터넷이 좋은 환경에 있을 것이라고 생각하고 만든 것)

ex) 네이버 오피스, Google DOCS(Google), Work Space(Microsoft), Acrobat(Adobe)

**[장점]**

- 필요할 때마다 **컴퓨팅 능력을 유연하게 조절**할 수 있다.
- **사용한 만큼의 요금만 지급**하면 된다. (온프레미스는 고정적인 비용이 들어간다.)
- 컴퓨터의 스냅샷('이미지'라고 부른다.)을 이용해 **다른 컴퓨터로 즉시 이주(migration)가 가능**하다.

**[단점]**

- **클라우드 서비스에 종속된다.**
  운영 환경 자체가 클라우드 서비스에 종속되면 클라우드 서비스에 문제가 생기면, 내가 배포하고 관리하는 환경에도 영향을 미치고, 백엔드 구성 자체가 특정 회사의 기술로만 구성해야 하는 경우가 발생한다.

## 클라우드의 서비스 형태

예전 IT 인프라의 여러 구성 요소 중 예전에는 모두 사용자가 관리해야만 했지만, 이제는 일정 부분을 클라우드에서 내려받는다.

얼마만큼 사용자가 관리하고, 얼마만큼 클라우드에서 제공받는가에 따라 나누어진다.

- **Packaged Software**
  - 물리적인 장치, 하드웨어를 직접 모두 구매해야 한다.
  - 직접 OS를 설치한다
  - 네트워크 환경을 직접적으로 구성한다.
  - 서버 관리를 직접적으로 해야한다.
  - 이 모든 것들을 직접 사용자가 다 준비해야하기 때문에 매우 큰 시간과 돈을 소비하게 된다.
- **IaaS (Infra as a Service)** - 네트워크, 하드웨어
  - 기업이 준비해놓은 환경에서 우리가 선택할 수 있다.
  - OS와 어플리케이션을 직접 관리 해야한다.
  - 관리 측면에서 개발자와 인프라 관리자의 역할을 분담시킬 수 있다.
  - ex) **AWS의 EC2**
- **PaaS (Platform as a Service)** - 네트워크, 하드웨어, 운영체제, 플랫폼/데이터베이스
  - 개발자가 **응용 프로그램을 작성**할 수 있는 플랫폼 및 환경을 제공하는 모델
  - 운영팀이 인프라를 모니터링 할 필요가 없다.
  - 사용자는 OS, Server 하드웨어, 네트워크 등을 고려할 필요가 없다.
  - 사용자가 어플리케이션 자체에만 집중할 수 있다.
  - 개발자는 빠르게 어플리케이션을 개발하고, 서비스가 가능하게 할 수 있다.
  - **IaaS**는 아마존과 같은 서비스가 VM을 제공하는 IaaS라면, **PaaS**는 node.js, Java와 같이 런타임을 미리 깔아놓고, 거기에 소스 코드를 넣어서 돌리는 구조이다. 즉 우리는 소스코드만 넣어서 빌드하는 것이고, 컴파일은 클라우드에서 하여 결과만 가져오는 것이다.
  - PaaS의 제공 업체는 `Heroku, Google App Engine, IBM Bluemix, OpenShift, SalesForce` 가 있다.
- **SaaS (Software as a Service)** - 네트워크, 하드웨어, 운영체제, 플랫폼/데이터베이스, 어플리케이션
  - 클라우드 제공자가 당장 사용이 가능한 소프트웨어를 제공하는 경우 ⇒ SaaS
  - 모든 것을 기업(클라우드)가 제공하므로, 사용자는 별도의 설치나 부담이 필요없이 SW를 사용할 수 있다.
  - 웹브라우저에 접속하면 사용할 수 있고, 최신 SW 업데이트를 빠르게 제공받을 수 있다.
  - 다만 인터넷 접속이 없으면 사용할 수 없고, 외부의 데이터 노출에 대한 위험이 있다.
  - ex) 웹 메일, 구글 클라우드, 네이버 클라우드, MS 오피스 365, 드롭박스 등

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/0867c643-5331-47da-8ac8-e30b804a062c/Untitled.png)

### 클라우드 vs 온프레미스

|                    | 클라우드                      | 온프레미스             |
| ------------------ | ----------------------------- | ---------------------- |
| 비용               | 사용한만큼 + 초기비용이 없다. | 초기 비용 + 유지비용   |
| 확장성             | 유리하다.                     | 불리하다. 공간적 제약. |
| 구축에 걸리는 시간 | 클릭 몇번이면 할 수 있다.     | 오래 걸린다.           |

언제 온프레미스를 사용할까?

- 안정적인 서비스
- 보안이 중요할 때
- 클라우드에 종속되고 싶지 않을 때

최근은 섞어서 사용하는 추세이다..

---

## Deployment

배포 ⇒ 개발한 서비스를 사용자가 이용 가능하게 하는 과정이다.

기본적으로 4단계를 거쳐서 개발한 서비스를 배포한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/494452e6-6b46-42b7-9107-1d15c61fa169/Untitled.png)

배포 할 때는 개발 환경과 다르므로, 환경 설정을 코드와 분리하는 것이 중요하다.

작성된 코드가 다른 환경에서 잘 작동하게 하려면?

- 절대 경로 대신 **상대 경로**를 사용
- 환경에 따라 포트를 분기할 수 있도록 **환경 변수** 설정 (envvars 혹은 env)
- Docker와 같은 개발 환경 자체를 통일 시키는 솔루션 사용 ?

---

# AWS

## EC2 (Elastic Compute Cloud)

EC2는 아마존 웹 서비스에서 제공하는 클라우드 컴퓨팅 서비스이다. 비용, 성능, 용량 면에서 탄력적인 클라우드 컴퓨터를 제공한다.

> ❓ **클라우드 컴퓨팅**: 인터넷(클라우드)를 통해 서버, 스토리지, 데이터베이스 등 컴퓨팅 서비스를 제공하는 것

즉 아마존에서 가상의 컴퓨터를 한 대 빌리는 것과 같다고 볼 수 있다. 이렇게 **빌린 컴퓨터를 `Instance`**라고 한다.

인스턴스는 1대의 컴퓨터를 의미하는 단위, AWS에서 컴퓨터를 빌리는 것을 인스턴스를 생성한다고 한다.

**[장점]**

- **구성하는 데 필요한 시간이 짧다.**
  - pc를 구매하면 구매에서 배송까지 시간이 걸리는데, EC2 서비스는 몇 번의 클릭만으로 PC를 구성할 수 있다.
- **AMI를 통해서 다양한 운영체제, CPU, RAM, 용량을 쉽게 선택해서 구성할 수 있다.**
- **컴퓨터로 할 수 있는 모든 일을 할 수 있다.**
  인스턴스(빌린 컴퓨터)는 아마존이 전세계에 만들어 놓은 데이터 센터에 만들어져 있기 때문에, 네트워크를 통해서 컴퓨터를 제어해야 한다. 이러한 차이점을 제외하면, 일반적인 컴퓨터와 다른 점이 없다.

### AMI (Amazon Machine Image)

소프트웨어 구성이 기재된 템플릿

이미지 종류로 단순히 운영체제(윈도우, 우분투 리눅스 등) 만 깔려있는 템플릿을 선택할 수도 있고, 아예 특정 런타임이 설치되어 있는 템플릿이 제공되는 경우도 있다. (우분투 + node.js, 윈도우+JVM)

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/7eac87bf-3b91-4b74-b031-5b86a5124703/Untitled.png)

### RDS (Relational Database Service)

AWS에서 제공하는 **관계형 데이터베이스 서비스**

**RDS를 사용하는 이유**

어? 그냥 EC2 인스턴스에서 MySQL 설치해서 사용하면 굳이 RDS를 사용할 필요가 없지 않나? 왜 굳이 데이터베이스만 따로 분리해서 서비스를 이용해야 하지?

- EC2 인스턴스
  - EC2 인스턴스는 데이터베이스와 관련해서 자동으로 관리를 담당하는 부분이 매우 적다.
    ⇒ 사용자가 일일이 시간을 투자하여 데이터베이스 엔진의 설치, 버전 관리, 데이터 백업을 해야 한다.
- RDS 사용
  - 데이터베이스의 유지 보수와 관련된 일을 RDS에서 전적으로 자동 관리해준다.
  - 사용자는 초기 설정을 제외하고, 데이터베이스에 저장된 데이터를 관리하는 일 밖에 없으므로, 큰 편의성을 느낄 수 있다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/ca5ab8db-ff39-466e-a984-de289a7134aa/Untitled.png)

그리고 RDS를 사용하면 다양한 데이터베이스 엔진 선택지를 제공한다. 필요와 목적에 맞는 데이터베이스 엔진을 선택하여 효율성을 높일 수 있다.

## S3 (Simple Storage Service)

**클라우드 스토리지**

인터넷 공간에 데이터를 저장하는 저장소. 구글 드라이브, 네이버 mybox, 마이크로소프트의 Onedrive와 같은 서비스가 있다.

- 뛰어난 접근성으로, 웹 환경이라면 언제 어디서나 저장된 파일에 접근할 수 있다.
- 컴퓨터가 아닌, 웹이 접속 가능한 다른 전자기기를 활용하여 클라우드 스토리지에 저장된 데이터에 접속할 수 있다.

**S3**

AWS 에서 제공하는 클라우드 스토리지 서비스

[장점]

- **뛰어난 접근성**
- **높은 확장성**
  - 많은 시간과 수고를 들이지 않고, 스토리지 규모를 확장/축소할 수 있다.
  - 스토리지 용량을 무한히 확장할 수 있다.
  - 사용한 만큼만 비용을 지불하면 되므로, 비용적인 측면에서 매우 효율적이다.
- **강력한 내구성**
  - 내구성이 높아 저장된 파일을 유실할 가능성이 적어진다.
- **높은 가용성**
  - 스토리지에 저장된 파일을 정상적으로 사용할 수 있는 시간이 길어진다. ⇒ **리전** 때문에 가능!
- **다양한 스토리지 클래스 제공**
  - 저장소를 어떤 목적으로 사용할 건지에 따라 효율적으로 선택할 수 있는 스토리지 클래스가 달라짐
    - Standard 클래스
      ⇒ 범용적인 목적. 데이터에 빠르게 접근, 데이터 액세스 요청 처리가 빠름, 단 보관 비용이 높아 오래 보관하기에는 효율적이지 않다.
    - Glacier 클래스
      ⇒ 데이터의 장기 보관 목적. 데이터 보관 비용이 매우 저렴하다. 단 데이터 액세스 속도가 느리다.
    주로 이 2개의 클래스를 사용하며, 그 외에도 여러 스토리지 클래스가 존재한다.
- **정적 웹사이트 호스팅이 가능**
  - **정적 파일**: 서버의 개입 없이 클라이언트에 제공될 수 있는 파일
    ( ↔ 동적 파일 : 클라가 서버에 요청을 보내면, 서버가 요청에 맞추어 그 자리에서 생성한 파일)
  - **웹 호스팅**: 서버의 한 공간을 빌려주어 웹 사이트의 배포, 운영이 가능하게 만들어주는 서비스
  - S3에서 **버킷**을 통해 정적 웹 사이트 호스팅이 가능하다.

### ⭐ 리전 (region)

AWS에서 클라우드 서비스를 제공하기 위해서 운영하는 물리적인 서버의 위치를 말한다.

![Untitled](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/8bec1158-3378-4acd-8786-a0548aec2b73/Untitled.png)

주황색 동그라미 안의 숫자 ⇒ 리전에 위치한 **가용 영역 (Availability Zone)**

가용 영역은 각 리전 안에 존재하는 데이터 센터(IDC)로 각각 개별적인 위치에 떨어져서 존재한다. (보통 2~4개까지 가용 영역이 나누어져 있다.)

그래서 한 곳의 가용 영역이 재난, 사고로 인해 가동이 불가능해져도, 다른 가용 영역에서 백업해놓은 데이터를 활용해 문제 없이 서버가 가동되도록 한다.

### 버킷

- S3에 저장되는 파일들이 담기는 바구니. **파일을 저장하는 최상위 디렉터리**
- **S3에서 저장되는 모든 파일은 버킷 안에 저장**되어야 하고, 버킷에는 무한한 양의 파일을 저장할 수 있다.
- 각각의 버킷은 이름을 가지고 있으며, **버킷의 이름**은 버킷이 속한 리전(버킷이 생성된 지역)에서 **유일**해야 한다.
- **버킷 정책**을 생성하여, 해당 버킷에 대한 **다른 유저의 접근 권한을 수정**할 수 있다.

### 객체

- 버킷에 담기는 파일
- 파일과 메타데이터로 구성. 파일은 키-값 형식으로 데이터를 저장한다.
- 파일의 값은 실제 데이터로, S3 객체의 값으로 저장될 수 있는 데이터 최대의 크기는 5TB이다.
- 모든 객체는 고유한 키를 가지며, 키는 각각의 객체를 고유하게 만들어주는 식별자 역할을 한다.
- 모든 객체는 고유한 URL 주소를 가지며, URL 주소로 객체에 접근 가능하다.
- URL 주소 ⇒ http://[버킷의 이름].S3.amazonaws.com/[객체의 키]

VPC (virtual private cloud)
