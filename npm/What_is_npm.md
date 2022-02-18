#  npm

> Node Package Manager

- 자바스크립트 프로그래밍 언어를 위한 패키지 관리자
- Node.js ( 자바스크립트 런타임 환경 ) 의 기본 패키지 관리자



## Package? manager?



### package?

- 라이브러리와 유사한 개념으로서,
  라이브러리가 **코드의 작성**을 위해서 사용되는 코드의 묶음 이라면,
  패키지는 **코드의 배포**를 위해서 사용되는 코드의 묶음이다.
- 패키지는 경우에 따라서 라이브러리를 포함할 수도 있다.
  일반적으로 라이브러리 나 실행파일을 포함한다.
- 패키지는 3가지 정보를 가지고 있는 코드의 배포 단위이다.
  1. 컴파일한 소프트웨어의 바이너리 (binary)
  2. 환경설정 ( configuration ) 에 관련한 정보
  3. 의존 ( dependency ) 에 관련한 정보



### Dependency?

- 많은 패키지들이 제대로 동작하기 위해서는 다른 패키지들을 필요로 한다.
  기존 패키지들을 제대로 동작시키기 위해서 필요한 다른 패키지들을 **dependency** 라고 한다.
- 개발을 하기 위해서 특정 패키지를 사용하기 위해서는 해당하는 패키지의 dependency 에 해당하는 다른 패키지들을 전부 설치해야한다.
- 중요한 점은 특정 패키지를 설치하기 위한 dependency를 위한 dependency 를 설치해야한다는 것이다.
  수동으로 모든 dependency 를 해결하는 것은 매우 힘든 일이다.
  따라서 패키지 관리자 ( package manager ) 를 통해서 dependency 를 효율적으로 관리하는 것이 좋다.



### Package Manger 의 역할

- 패키지의 dependency 를 관리한다.
- 패키지의 보완관리를 한다. ( 신뢰, 손상 관련 보장 )
- 여러 패키지를 기능에 따라 그룹으로 묶어서 정리한다.
- 패키지 압축을 해제해준다.
- Software repository 로 부터 패키지를 찾고, 다운로드하고, 설치하고, 업데이트 한다.



## npm 의 구성요소

- 웹사이트
  - 웹사이트를 사용해서 패키지를 검색하고, 프로필을 설정할 수 있다.
- CLI ( Command Line Interface )
  - 터미널에서 실행된다.
- 레지스트리
  - javaScipt 소프트웨어와 이를 둘러싼 메타 정보의 대규모 공개 데이터베이스





# 출처

- https://docs.npmjs.com/about-npm
- https://ko.wikipedia.org/wiki/Npm_(%EC%86%8C%ED%94%84%ED%8A%B8%EC%9B%A8%EC%96%B4)
- https://hellominchan.tistory.com/10
- https://aahc.tistory.com/14

