# section01 - 오리엔테이션

### Firebase의 강력한 대항마, Supabase

**Firebase (2011 ~)**
- 서버 없이 빼르게 앱을 출시할 수 있도록 도와주는 플랫폼

장점
- 데이터베이스, 인증, 파일 스토리지, 호스팅, 모니터링 등 다양한 기능을 제공
- 적용이 쉽고 문서화가 잘 되어있다.
- 커뮤니티가 활발하고 오픈소스 프로젝트가 많다.
- 앱, 웹에서 단순하게 사용할 수 있는 NoSQL 기반

단점 
- 오픈소스가 아니다 (Vender Lock-In)
- 유저가 많아졌을 때 비용이 많이 든다.
- 복잡한 쿼리 불가 (NoSQL 기반이라서 테이블 조인이나 레인지 쿼리 작성이 복잡해짐)
- 앱 개발에는 월등히 좋으나 웹 개발에 최적은 X.

<br>

**Supabase**

장점
- 오픈소스 프로젝트 (자체 서버구축 가능)
- Firebase 대비 저렴
- PostgreSQL 기반 (관계형 DB 장점을 살릴 수 있다, MySQL과 비슷)
- 다양한 연동방식 지원 (+ GraphQL, API, SDK, DB Connection)

단점
- 커뮤니티 발달 X
- 부족한 문서와, 한글 문서 부족
- 비교적 적은 기능, 서비스 연동 지원
- Firebase 보다 높은 러닝커브

<br>

**Firebase vs Supabase**

- 개인 또는 소규모 팀이 풀스택 개발을 하는데 필요한 대부분의 것들이 갖춰져있다.
- 스타트업 or 개인 프로젝트 특성상 복잡한 요구사항이 생기기 쉬운데, 이를 대응하기가 훨씬 용이하다
- 보안상의 이슈로 직접 서버구축을 해야할 떄, Supabase는 비교적 쉽게 이전이 가능하다 Vendor Lock-In 이슈 (오픈소스)

<br>
<br>

### 초기설정 - VSCode, Git, nvm, Typescript

1. **VSCode 세팅**
- extensions 확장프로그램 설치
  - Prettier - Code Formatter : 코드 정리 기능
    - 중간에 Cmd + alt + F 로 코드 정리하는 습관이 있어서 설치 X
  - Tailwind CSS IntelliSense : Tailwind CSS 자동완성 기능
  - GitHub Copilot : 코드 자동완성 기능
    - Cursor 사용으로 설치 X

2. **Git 설치**
3. **nvm(노드 버전 관리) 또는 node 설치**
4. **Typescript 설치**
  - npm 전역으로 설치하면 오류나서 brew로 설치

    >$ brew install typescript

