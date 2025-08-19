# eGovFrame Standard Project

전자정부프레임워크(eGovFrame) 기반 표준 프로젝트 구조 및 설명입니다.  
Spring MVC + MyBatis + 공통 컴포넌트 규약을 따릅니다.

---

## 📂 디렉토리 구조

```
egov-project/
├── pom.xml                     # Maven 설정 파일 (라이브러리 의존성 관리)
├── src/
│   ├── main/
│   │   ├── java/
│   │   │   └── egovframework/
│   │   │       ├── example/    # 사용자 비즈니스 패키지
│   │   │       │   ├── web/       # Controller 계층
│   │   │       │   ├── service/   # Service 인터페이스 & VO
│   │   │       │   │   └── impl/  # Service 구현체, DAO 클래스
│   │   │       │   └── mapper/    # (선택) MyBatis Mapper 인터페이스
│   │   │       └── common/        # 공통 유틸, 보안, AOP, 예외 처리
│   │   │
│   │   ├── resources/
│   │   │   ├── egovframework/
│   │   │   │   ├── spring/        # Spring 설정 XML
│   │   │   │   │   ├── context-common.xml      # 공통 Bean 정의, component-scan
│   │   │   │   │   ├── context-datasource.xml  # DataSource, MyBatis 연동
│   │   │   │   │   ├── context-mapper.xml      # MyBatis Mapper XML 스캔
│   │   │   │   │   ├── context-transaction.xml # 트랜잭션 AOP 설정
│   │   │   │   │   └── dispatcher-servlet.xml  # Spring MVC DispatcherServlet
│   │   │   │   └── message/       # 다국어 메시지 (*.properties)
│   │   │   └── mapper/            # MyBatis Mapper XML
│   │   │       └── user/          # ex) UserMapper.xml
│   │   │
│   │   ├── webapp/
│   │   │   ├── WEB-INF/
│   │   │   │   ├── views/         # JSP 뷰 파일
│   │   │   │   └── web.xml        # 서블릿/JSP 설정
│   │   │   └── static/            # 정적 리소스 (css, js, img)
│   │   │
│   │   └── resources-log/         # 로그 설정 (log4j.xml, logback.xml 등)
│   │
│   └── test/
│       └── java/                  # JUnit 테스트 코드
│
└── target/                        # 빌드 결과물 (WAR 등)
```

---

## 📌 주요 디렉토리 설명

- **web/**  
  - `Controller` 클래스 위치 (`@Controller`)
  - 요청 → Service 호출 → JSP/JSON 반환

- **service/**  
  - `Service` 인터페이스와 VO(Value Object, DTO)
  - `impl/` : `@Service` 구현체 + DAO(`@Repository`)

- **mapper/**  
  - MyBatis Mapper 인터페이스 (선택)
  - SQL은 `/resources/mapper/**/*.xml`에 작성

- **resources/egovframework/spring/**  
  - `context-common.xml` → 공통 Bean 설정, component-scan
  - `context-datasource.xml` → DB 연결 정보
  - `context-mapper.xml` → MyBatis Mapper XML 스캔
  - `context-transaction.xml` → 트랜잭션 AOP
  - `dispatcher-servlet.xml` → Spring MVC Front Controller 설정

- **resources/egovframework/message/**  
  - 다국어 지원 properties 파일

- **webapp/WEB-INF/views/**  
  - JSP 뷰 템플릿

- **webapp/WEB-INF/web.xml**  
  - 서블릿, 필터, 리스너 설정

---

## 📌 요청 처리 흐름

1. **클라이언트 요청** → DispatcherServlet  
2. **Controller** (`@RequestMapping`)  
3. **Service** (비즈니스 로직)  
4. **DAO** (`EgovAbstractMapper` 기반)  
5. **Mapper XML** (실제 SQL 실행)  
6. **결과 반환** → Service → Controller → JSP/JSON 응답
