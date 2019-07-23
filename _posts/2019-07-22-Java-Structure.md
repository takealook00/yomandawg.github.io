---
title: "Java 구조 이해"
date: 2019-03-25 14:18:44 -0400
categories: Github Blog
---

## 컴퓨터 구조의 이해

* RAM - 정보의 휘발성
    - RAM에서 연산 밑 끌어오는 것은 무조건 IO (IO 발생은 RAM)
    - GPU 연산은 예외
    - application을 구동하려면 RAM(메모리) 위로 올라가야 함
      - 그 후 CPU연산 (프로세스라고 함)
      - 즉, application이 RAM 위에서 돌아가서 CPU로 연산하는 것을 process라고 함
      - Java application는 JVM 위에서 돌아가므로 process 상태표시창에서 보면 Java로 뜬다

* OS
    * 프로그래밍을 한다는 것은 OS가 제공하는 범위 내에서 개발을 한다는 것
    - 종류
        - Windows (MS계열)
            - PC or Server 편리함
            - 업데이트 등 불안정성, 비싸서 별로
            - Unix, Linux 환경 호환 불가
        - **Linux (Open source)**
            - 기술지원 힘듦
            - 큰 회사들은 일정 비용 지불로 기술지원
            - 최근에 Unix 거의 따라잡음
        - Unix (상용)
            - 안정적, 고가
            - 탈피중



## Java의 이해
> 초기엔 OS에 따라 새로 개발해야했기 때문에 매우 불편해서 나온 것이 Java (JVM 가상머신 위에서 돌아가는 프로그램)

- 프로그램을 돌릴 때 JVM 통해 명령어를 전달 (JVM이 번역의 역할)
- JVM에 한 번 걸려서 돌아가 성능 저하
  - Android같은 경우 Java 기반 application을 사용하기 때문에 느린 속도를 하드웨어로 커버할 수밖에 없음
- 성능 개선을 위해 점점 복잡해짐
- **Garbage Collector** - 메모리 관리
  - 두 개가 동시에 돈다 - 일반 GC, 프로 GC
  - 성능 저하 & 어떻게 돌아가는지 모른다!
    - 지속적 업데이트 &rarr; 계속 공부해야함; 어려워
- `main` method로 시작하는 틀 == application
  - OS 위에서 돌아가는 모든 프로그램을 application으로 정의
  ```
  Critical! One Source Multi-use 라는 Java 특성상 API 사용이 힘들다.
  아까 말한 OS에서밖에 못돌아가는 컴퓨터 구조상...
  JVM은 OS 위에서 돌아가므로 OS가 제공하는 기능만 사용할 수 있다... 그런데?
  application 특징을 Java로 개발해놓으면 OS마다 기능상 제약이 생김!
  예) Windows에서 제공하는 카메라 어플을 Linux에서 못씀!
  JNI(Java Native Interface) - C 코드 등을 활용함
      - 하지만 이를 사용하면 OS에 맞는 코드를 모두 작성해야함 - 자바의 장점을 포기
  ```
- Java는 native로 개발하므로 기능이나 API 사용이 어려워



## Web Application의 이해
> 분산된 Client에 반복적으로 해야 하는 작업을 하나의 Server로 대체하자

* 기존 C/S program은 데이터를 활용하는 곳과 데이터를 만드는 곳이 나뉘어 있었다.
* 통신 방식은 client &harr; server 이지만 약속 data를 갖고 정보를 주고받는다.
  * 여러가지 약속, 대표적으로 HTTP protocol (데이터 폼)
    * 어떤 업무를 하라는 제목, 상세 내용, 언어 등을 HTML 이라는 형식에 담아서 HTTP로 보냄
    * 웹 서버를 사용한다는 것은 HTTP protocol로 통신한다는 것
* HTTP
  * *W3C* 관장 및 표준화
  * 데이터를 주고 받은 후 다음 연결까지 바로 연결을 끔
    * 어떤 컴퓨터던지 server의 개수 한 개 때문에 연결될 수 있는 동시접속자수가 제한돼있다.
    * HTTP를 사용하는 이유
  * 연결을 하는 것도 반복 작업(비효율)
  * HTTP 2.0 부터는 5번까지 연결 유지
  * HTML 표준을 따라야 하는데 IExplorer만 이를 거부함
    * HTML 문서 안에 각 IExplorer 버젼마다 `if/else`문으로 억지로 제어해야하는 불편함
    * IExploer 11 버젼 이후로는 일부분 통합 지원



## 개발 환경

* IT는 약속 중심
  * 각자 마음대로 개발하면, 통신이든 데이터 처리든 상호작용이 안됨
* **환경변수**
  * global (OS)
  * local (application) - 우선순위
    * JVM 버전을 바꾸면, local &harr; global 환경변수 상호 버전관리가 안돼있어서 버전 인식이 잘못됨
      * global이 버전이 맞더라도 local 환경 변수가 우선으로 인식되기 때문에 변경이 안돼서 나옴
* 형상 관리
  * 하나의 서버에서 소스를 관리해주는 툴 필요
  * 회사에선 *Github*와 같은 open된 환경에서 코드를 공유하거나 관리할 수 없으므로 주로 *SVN* 사용


### Java 개발 환경 구조
> C/C++ 같은 시스템 언어는 코드만 잘 짜면 (메모리 해제만 잘 해주면) 효율적으로 유지 가능하나
> JVM은 GC 최적화 설정을 항상 고려해야 함

* Garbage Collector 기준
  * 해당 변수가 (메모리가) 참조하는 것이 없을 경우 gc list에 감
    * 바로 지워지는 것은 아니다
  * reference가 존재할 경우 해당 객체는 절대 지워지지 않음
    * 잦은 실수 발생 여지
    * 예) `list` 객체 하나를 만들어서 다른 모든 객체를 `add`하면서 사용
      * `list` 객체가 계속 다른 객체들을 참조하므로 `list`가 남아있는 한 gc에 가지 않고 memory overflow 발생
* 초기 메모리 할당 계산할 수 없으므로 테스트 거친 후 메모리 결정
* 코드 수정 후 컴파일(빌드) 후 재테스트
  * 비효율을 제거하기 위해 분석 및 설계 기법이 체계적로 있다
    * *디자인 패턴*
    * *refactoring* - 결과의 변경 없이 코드의 구조를 재조정함 (버그 방지 및 유지보수 용이)
* **Maven** 개발환경 - open source 관리
  * 프로젝트 객체 모델(Project Object Model)이라는 개념을 바탕으로 프로젝트 관리 도구
  * 플러그인 기반으로 소스 코드로부터 배포 가능한 빌드 기능 뿐만 아니라 reporting 및 documentation 작성 등을 제공
  * 단점 - Eclipse와 연동이 되지 않음, 동기화 깨질 때 세팅 잡기가 힘듦
  ```
  ## Maven
  라이브러리 버젼관리, dependancy 관리
  빌드 - 테스트 -배포 - 릴리즈 관리
  ANT와 같은 역할도 해줌
  ```
  * **ANT**
    * 자동화된 소프트웨어 빌드 도구
    * XML 설정으로 쉽게 빌드 가능
    * Maven &harr; ANT 상호 call 가능
* `system.out.println`은 IO를 많이 사용하므로 자제해야함
  * `Logger`를 사용하면 비효율 개선
* `Class not found` error
  * Maven에서 삭제된 것을 Eclipse에서 알지 못하는 경우가 다반사 (호환 잘 안됨)
  * 버그 수정하고자 Maven Clean 이라는 프로그램이 있는데 compile된 Java 소스 날라갈 수 있음


## Web 서비스
> IT는 항상 규칙과 반복 작업에 대해 생각함

* **HTTPS** - 보안화 된 HTTP; 데이터는 같음
* **HTML**
  * Client와 Server 간의 통신에서 HTML 틀은 계속 같지만 요청하는 유저의 데이터만 계속 달라짐
  * 틀 하나만 갖고 계속 쓸 수 있다.
  * JavaScript/CSS 포함
* **WAS** (Web Application Server, Java 기반)
  * 반복 작업을 단순화시키는 작업을 개발 
  * HTTP 요청을 받고 분석해서 요청값을 갖고 logic을 실행해주는 것까지 함
  * 원하는 기능을 WAS 서버에 올리기만 하면 됨 (Web Application)
  * HTTP를 통해 application을 수행해 주는 일종의 미들웨어

```
## 웹 기반의 서비스

client에는 web browser이 있고, server에는 앞단에 WS(Web Server), 뒷단에 WAS
Naver을 예시로 들면 주소(www.naver.com)로 들어가 해당하는 국가 도메인 서버를 찾아가서 등록되어 있는
목록을 갖고 와 요청된 서버까지 도달
아무런 추가 주소 없이 했을 때, "index.html"에 해당하는 welcome 페이지로 reload

WS는 정적인(파일로 존재하는) 페이지만 아무 logic 작업 없이 돌려주는 것
동적 요청을 보내면 WAS 서버에 보내서 logic을 돌려 데이터를 찾아 return함
뒷단(WAS)에서 필요로 하는 logic에 대한 개발만 집중하면 됨
```
```
## jsp

a.jsp 라는 Java기반의 컴파일된 자바 소스가 있다고 하면
(jsp에는 Java, HTML, CSS 등 내용이 모두 들어가있음)
client web browser에서 서버에 요청을 하면 WAS(Java 기반)서버의 jsp(Java 기반) 소스를 실행시켜 return된
HTML 파일을 client에 전송함 (둘 다 Java 기반이니까 동작하는 것!)


## JavaScript

JavaScript 내용을 첨부하여 전송하면 서버에서 보이는 것만 처리하는 실행 데이터만 갖고와서
client에서 처리하게끔 분산처리 (Server이 해야할 일을 Client에서 하는 것)
=> 사용자의 resource를 빌려 쓰는 것 => faster

// 일반적으로 서비스는 3초 이내에 작동해야 함
```

* global 설정보다 local 설정 우선
* Java 폴더 실행 순서: `classes` &rarr; `lib`
  * 임의로 바꿀 수 있긴 함!
