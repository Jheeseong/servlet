# v1.0 2/14
# servlet
## 웹 서버(Web Server)
- HTTP 기반으로 동작
- 정적 리소스 제공, 기타 부가기능
- 정적(파일) HTML, CSS, JS, 이미지, 영상 등  
![image](https://user-images.githubusercontent.com/96407257/153800154-d758a60e-7e75-4ef4-aef9-dbb227d9e65f.png)

## 웹 어플리케이션 서버(WAS - Web Application Server)
- HTTP 기반으로 동작
- 웹 서버 기능 포함 + (정적 리소스 제공 기능)
- 프로그램 코드를 실행해서 애플리케이션 로직 수행
  - 동적 HTML, HTTP API(JSON)
  - 서블릿, JSP, 스프링 MVC  

![image](https://user-images.githubusercontent.com/96407257/153800302-61e21707-aa85-4b86-903a-d78cf79c8fec.png)

## 웹 서버, WAS 차이
- 웹 서버는 정적 리소스, WAS는 애플리케이션 로직
- 사실 둘 경계는 모호
  - 웹 서버도 프로그램을 실행하는 기능을 포함하기도 함
  - WAS도 웹 서버의 기능을 제공
- 자바는 서블릿 컨테이너 기능을 제공하면 WAS
- WAS는 애플리케이션 코드를 실행하는데 특화

## 웹 시스템 구성 - WAS,DB
![image](https://user-images.githubusercontent.com/96407257/153800624-f668077c-9e93-44b3-810f-1ab9de4856a0.png)  
- WAS, DB로만 시스템이 구성
- WAS는 정적 리소스, 애플리케이션 로직 모두 제공
- WAS가 너무 많은 역할을 담당, 서버 과부하 가능성 존재
- 비싼 애플리케이션 로직이 정적 리소스 떄문에 수행 어려움 존재
- WAS 장애 시 오류 화면 노출 불가능
-> WEB, WAS, DB 구성을 통해 해결

## 해결법 : 웹 시스템 구성 - WEB, WAS, DB
![image](https://user-images.githubusercontent.com/96407257/153800821-f60b160b-130a-4f6c-9c8a-fc2b562e2fbe.png)  
- 정적 리소스는 웹 서버가 처리
- 웹 서버는 애플리케이션 로직같은 동적인 처리가 필요할 경우 WAS에 위임
- WAS는 애플리케이션 로직 처리 전담
- 효율적인 리소스 관리
  - 정적 리소스가 많이 사용되면 WEB 서버 증설
  - 애플리케이션 리소스가 많이 사용되면 WAS 증설
- 웹 서버는 서버 과부하 가능성이 적음
- WAS는 서버 과부하 가능성이 높음
- WAS, DB 장애 시 WEB 서버가 오류 화면을 제공

# 서블릿
- urlPatterns의 URL이 호출되면 서블릿 코드가 실행
- HTTP 요청 정보를 편리하게 사용할 수 있는 HttpServletRequest
- HTTP 응답 정보를 편리하게 제공할 수 있는 HttpServletResponse
- 개발자는 HTTP 스펙을 편리하게 사용 가능  
![image](https://user-images.githubusercontent.com/96407257/153801017-b860ef95-c749-4332-a446-4becd539c290.png)  
- HTTP 요청 시
  - WAS는 Request, Response 객체를 새로 만들어서 서블릿 객체 호출
  - 개발자는 Request 객체에서 HTTP 요청 정보를 편리하게 꺼내서 사용
  - 개발자는 Response 객체에 HTTP 응답 정보를 편리하게 입력
  - WAS는 Response 객체에 담겨있는 내용으로 HTTP 응답 정보를 생성

# 서블릿 컨테이너
- 서블릿 컨테이너는 톰캣처럼 서블릿을 지원하는 WAS
- 서블릿 컨테이너는 서블릿 객체를 생성, 초기화, 호출, 종료하는 생병주기를 관리
- 서블릿 객체는 싱글톤으로 관리
  - 고객 요청마다 객체를 생성하는 것은 비효율
  - 최초 로딩 시점에 서블릿 객체를 미리 만들고 재활용
  - 모는 고객 요청은 동일한 서블릿 객체 인스턴스에 접근
  - **공유 변수 사용 주의**
  - 서블릿 컨테이너 종료 시 함께 종료
- JSP도 서블릿으로 변환 되어서 사용
- 동시 요청을 위한 **멀티 쓰레드** 처리 지원

# 멀티 쓰레드
## 쓰레드
- 애플리케이션 코드를 하나하나 순차적으로 실행하는 것은 쓰레드
- 자바 메인 메스드를 처음 실행 시 main이라는 이름의 쓰레드가 실행
- 쓰레드가 없다면 자바 애플리케잇녀 실행이 불가능
- 쓰레드는 한번에 하나의 코드 라인만 수행
- 동시 처리가 필요하면 쓰레드를 추가로 생성

![image](https://user-images.githubusercontent.com/96407257/153801840-d5d8aa86-69b8-4911-9da0-c004695471e8.png)

![image](https://user-images.githubusercontent.com/96407257/153801827-2bd2494f-3abc-46b2-bfac-5c0216d8b5ce.png)

![image](https://user-images.githubusercontent.com/96407257/153801862-3ca3f137-9a4f-4219-a945-88f9e3f44991.png)

## 요청 마다 쓰레드 생성
- 장점
  - 동시 요청 처리 가능
  - 리소스(CPU, 메모리)가 허용할 떄까지 처리 가능
  - 하나의 쓰레드가 지연되어도, 나머지 쓰레드는 정상 동작
- 단점
  - 쓰레드는 생성 비용이 비쌈
  - 고객 요청마다 쓰레드를 생성 시, 응답 속도가 늦어짐
  - 스레드는 **컨텍스트 스위칭 비용**(대기와 실행이 번갈아가며 작동하며 생기는 비용)이 발생
  - 쓰레드 생성에 제한이 없어 서버 과부하의 가능성이 존재

## 쓰레드 풀
![image](https://user-images.githubusercontent.com/96407257/153802354-667ea96c-2069-405a-8b70-ada408350d47.png)

- 특징
  - 필요한 쓰레드를 쓰레드 풀에 보관하고 관리
  - 쓰레드 풀에 생성 가능한 쓰레드의 최대치를 관리(기본 200개 설정)
- 사용
  - 쓰레드가 필요하면, 이미 생성되어 있는 쓰레드를 쓰레드 풀에서 꺼내어 사용
  - 사용을 종료하면 쓰레드 풀에 해당 쓰레드를 반납
  - 최대 쓰레드가 모두 사용 중이여서 쓰레드가 없을 시 기다리는 요청을 거절 혹은 대기하도록 설정
- 장점
  - 쓰레드가 미리 생성되어 있어서, 쓰레드를 생성하고 종료하는 비용이 절약, 응답시간이 빠름
  - 생성 가능한 쓰레드의 최대치가 있어서 너무 많은 요청이 들어와도 안전하게 처리가 가능
- 실무 팁
  - WAS의 주요 튜닝 포인트는 최대 쓰레드 수 이다
  - 값이 너무 낮을 떄 동시 요청이 많으면 서버 리소스는 여유롭지만, 클라이언트는 응답 지연
  - 값이 너무 높을 때 동시 요청이 많으면 CPU, 메모리 리소스 임계점 초과로 서버 다운
  - 장애 발생 할 때 클라우드 경우 서버를 늘리고 튜닝
  - 성능 테스트 : 아파치ab, 제이미터, nGrinder
- 멀티 스레드 부분은 WAS가 처리
- 개발자는 멀티 쓰레드 관련 코드 신경 X
- 싱글 쓰렏 ㅡ프로그래밍 하듯 소스 코드 개발하면 됨

# 정적 리소스
- 고정된 HTML 파일, CSS, JS, 이미지, 영상 등을 제공
- 주로 웹 브라우저

# HTML 페이지
- 동적으로 필요한 HTML 파일을 생성해서 전달
- 웹 브라우저 : HTML 해석

# HTML API
![image](https://user-images.githubusercontent.com/96407257/153807096-910e6f5a-451f-491f-bc01-109125828bf1.png)

- HTML이 아니라 데이터를 전달
- 주로 JSON 형식 사용
- 다양한 시스템에서 호출
- UI 클라리언트 접점
  - 앱 클라이언트(아이폰, 안드로이드, PC앱)
  - 웹 브라우저에서 자바스크립트를 통한 HTTP API 호출
  - React, Vue.js 같은 웹 클라이언트
- 서버 to 서버
  - 주문 서버 -> 결제 서버
  - 기업 간 데이터 통신

# SSR - 서버 사이드 렌더링
- HTML 최종 결과를 서버에서 만들어서 웹 브라우저에 전달
- 주로 정적인 홤녀에 사용
- 관련기술 : JPS, 타임리프 -> 백엔드 개발자  
![image](https://user-images.githubusercontent.com/96407257/153809975-007b4af9-b8f2-47f3-b3e6-55d2ec73fb3b.png)

# CSR - 클라이언트 사이드 렌더링
- HTML 결과를 자바스크립트를 사용해 웹 브라우저에서 동적으로 생성해서 적용
- 주로 동적인 화면에 사용, 웹 환경을 마치 앱 처럼 필요한 부분부분 변경 가능
- ex) 구글 지도, Gmail, 구글 캘린더
- 관련 기술 : React, Vue,js -> 웹 프론트 개발자

![image](https://user-images.githubusercontent.com/96407257/153810129-43b3a212-fe05-4907-bddd-4d5e28639ac6.png)

# 자바 웹 기술 역사
- 서블릿 - 1997
  - HTML 생성이 어려움
- JSP - 1999
  - HTML 생성은 편리하지만, 비지니스 로직까지 너무 많은 역할을 담당
- 서블릿, JSP 조합 MVC 패턴 사용
  - Model, View, COntroller로 역할을 나누어 개발
- MVC 프레임워크 - 2000년 초 ~ 2010년 초
  - MVC 패턴 자동화, 복잡한 웹 기술을 편리하게 사용할 수 있는 다양한 기능 지원
  - 스트릿츠, 웹워크. 스프링MVC
- 애노테이션 기반의 스프링MVC - 현재 
  - @Controller
  - 스프링 부트 등장
  - 과거 서버에 WAS를 직접 설치하고, 소스는 War 파일을 만들어 설치한 WAS에 배포
  - 스프링 부트는 빌드 결과(Jar)에 WAS 서버 포함 -> 빌드 배포 단순화

## 최신 기술 - 스프링 웹 플럭스(WebFlux)
- 비동기 넌 블러킹 처리
- 최소 쓰레드로 최대 성능 - 쓰레드 컨텍스트 스위칭 비용 효율화
- 함수형 스타일로 개발 - 동시처리 코드 효율화
- 서블릿 기술 사용 X
- 웹 플럭스는 기술적 난이도가 높음
- RDB 지원 부족

# 자바 뷰 템플릿 역사
- JSP
  - 속도 느림, 기능 부족
- 프리마커(Freemarker),, Velocity(밸로시티)
  - 속도 문제 해결, 다양한 기능
- 타임리프(Thymeleaf)
  - 스프링 MVC와 강력한 기능 통합

# v1.1 2/15
# 스프링 부트 서블릿 환경 구성
**ServletApplication**

    @ServletComponentScan
    @SpringBootApplication
    public class ServletApplication {

	    public static void main(String[] args) {
	      	SpringApplication.run(ServletApplication.class, args);
      	}

    }
    
**HelloServlet**

    @WebServlet(name = "helloServlet", urlPatterns = "/hello")
    public class HelloServlet extends HttpServlet {

      @Override
      protected void service(HttpServletRequest request, HttpServletResponse response) throws
      ServletException, IOException {

          System.out.println("HelloServlet.service");
          System.out.println("request = " + request);
          System.out.println("response = " + response);

          String username = request.getParameter("username");
          System.out.println("username = " + username);

          response.setContentType("text/plain");
          response.setCharacterEncoding("utf-8");
          response.getWriter().write("helle " + username);
          }
        }

- @ServletComponentScan
  - 스프링 부트는 서블릿을 직접 등록해서 사용할 수 있도록 지원
- @WebWervlet : 서블릿 애노테이션
  - name : 서블릿 이름
  - urlPatterns : URL 매핑
- HTTP 요청을 통해 매핑된 URL이 호출되면 서블릿 컨테이너는 다음 메서드를 실행
  - Protected void service(HttpServletRequest request, HttpServletResponse reponse

## 서블릿 컨테이너 동작 방식 설명
- **내장 톰캣 서버 생성**  
![image](https://user-images.githubusercontent.com/96407257/154015172-8c1134d4-2c7a-4487-b2ac-d18f96b2f2e5.png)

# HttpServletRequest
- 서블릿은 개발자가 HTTP 요청 메세지를 편리하게 사용할 수 있도록 개발자 대신에 HTTP 요청 메세지를 파싱 후 그 결과를 HttpServletRequest 객체에 담아서 제공
- HttpServletRequest, HttpServletResponse는 HTTP 요청 메세지, HTTP 응답 메세지를 편리하게 사용하도록 도와주는 객체

## HttpServletRequest - 기본 사용법

    @WebServlet(name = "requestHeaderServlet", urlPatterns = "/request-header")
    public class RequestHeaderServlet extends HttpServlet {

    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {

        printStartLine(request);
        printHeaders(request);
        printHeaderUtils(request);
        printEtc(request);
    }
    
**printStartLine 정보**

    private void printStartLine(HttpServletRequest request) {
        System.out.println("--- REQUEST-LINE - start ---");
        System.out.println("request.getMethod() = " + request.getMethod()); //GET
        System.out.println("request.getProtocal() = " + request.getProtocol()); //HTTP/1.1
        
        ....
        
    }
    
**printHeaders - 헤더 정보**

    private void printHeaders(HttpServletRequest request) {
        System.out.println("--- Headers - start ---");
        request.getHeaderNames().asIterator()
                .forEachRemaining(headerName -> System.out.println(headerName + ": " + request.getHeader(headerName)));
                        System.out.println("--- Headers - end ---");
        System.out.println();
    }
    
**printHeaderUtils - 편의 조회**

    private void printHeaderUtils(HttpServletRequest request) {
        System.out.println("--- Header 편의 조회 start ---");
        System.out.println("[Host 편의 조회]");
        System.out.println("request.getServerName() = " +
                request.getServerName()); //Host 헤더
        System.out.println("request.getServerPort() = " +
                request.getServerPort()); //Host 헤더
        System.out.println();
        System.out.println("[Accept-Language 편의 조회]");
        request.getLocales().asIterator()
                .forEachRemaining(locale -> System.out.println("locale = " +
                        locale));
        System.out.println("request.getLocale() = " + request.getLocale());
        System.out.println();
        System.out.println("[cookie 편의 조회]");
        if (request.getCookies() != null) {
            for (Cookie cookie : request.getCookies()) {
                System.out.println(cookie.getName() + ": " + cookie.getValue());
            }
        }
        System.out.println();
        System.out.println("[Content 편의 조회]");
        System.out.println("request.getContentType() = " +
                request.getContentType());
        System.out.println("request.getContentLength() = " +request.getContentLength());
        System.out.println("request.getCharacterEncoding() = " +
                request.getCharacterEncoding());
        System.out.println("--- Header 편의 조회 end ---");
        System.out.println();
    }
    
**printEtc- 기타 정보**
    
    private void printEtc(HttpServletRequest request) {
        System.out.println("--- 기타 조회 start ---");
        System.out.println("[Remote 정보]");
        System.out.println("request.getRemoteHost() = " +
                request.getRemoteHost());
        
        ....
               
    }
    
# HTTP 요청 데이터 - 개요
- **GET- 쿼리 파라미터**
  - /url?username=hello&age=20
  - 메세지 바디 없이, URL의 쿼리 파라미터에 데이터를 포함해서 전달
  - ex) 검색, 필터, 페이징 등에서 사용하는 방식
- **POST - HTML Form**
  - content Type : application/x-www-form-urlencoded
  - 메세지 바디에 쿼리 파라미터 형식으로 전달 username=hello&age=20
  - ex) 회원 가입, 상품 주문, HTML From 사용
- **HTTP message body에 데이터를 직접 담아서 요청**
  - HTTP API에서 주로 사용, JSON, XML, TEXT
- 데이터 형식은 주로 JSON 사용
  - POST, PUT, PATCH

# v1.2 2/16
# HTTP 요청 데이터 - GET 쿼리 파라미터
- 쿼리 파라미터는 URL에 ?를 시작으로 보낼 수 있음. 추가 파라미터는 &로 구분 가능
  - ex) http://localhost:8080/request-param?username=hello&age=20
- 서버에서는 HttpServletRequest가 제공하는 다음 메서드를 통해 쿼리 파라미터를 편하게 조회 가능

      @WebServlet(name = "requestParamServlet", urlPatterns = "/request-param")
      public class RequestParamServlet extends HttpServlet {

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            System.out.println("RequestParamServlet.service");

            System.out.println("[전체 파라미터 조회] - start");
            req.getParameterNames().asIterator()
                    .forEachRemaining(paramName -> System.out.println(paramName + "= " + req.getParameter(paramName)));
            System.out.println("[전체 파라미터 조회] - end");

            System.out.println("[단일 파라미터 조회]");
            String username = req.getParameter("username");
            String age = req.getParameter("age");

            System.out.println("age = " + age);
            System.out.println("username = " + username);

            System.out.println("[이름이 같은 복수 파라미터 조회]");
            String[] usernames = req.getParameterValues("username");
            for (String s : usernames) {
                System.out.println("s = " + s);
            }

            resp.getWriter().write("ok");
         }
	}
	
- **복수 파라미터에서 단일 파라미터 조회**
  - 하나의 파라미터 이름에는 request.getParameter()을 사용
  - 복수의 파라미터 이름에는 request.getParameterValues()를 사용
  - 복수일 때 request.getParameter()을 사용 시 첫 번쨰 값이 반환

# HTTP 요청 데이터 - POST HTML Form
- content type : appllacation/x-www-form-urlencoded
- 메세지 바디에 쿼리 파라미터 형식으로 데이터를 전달 - username=helle&age=20
- **application/x-www.form-urlencoded 형식**
  - GET에서 사용한 쿼리 파라미터 형식과 같음, 따라서 쿼리 파라미터 조회 메서드를 그대로 사용
  - request.getParameter()로 편리하게 구분없ㄷ이 조회 가능
  -> request.getParameter()는 GET 형식과 POST HTML form 형식 둘 다 지원.
  
##  GET 쿼리 파라미터 형식과 POST HTML Form 형식 비교
- GET 쿼리 파라미터 형식
  - 클라이언트에서 서버로 데이터 전달 시 HTTP 메세지 바디를 사용하지 않기 떄문에 Content-type이 X
- POST HTML Form 형식
  - 데이터 전달 시 HTTP 메세지 바디에 해당 데이터를 포함하여 전송하기 떄문에 바디에 포함된 데이터가 어떤 형식인지 content-type을 꼭 지정!!

# v1.3 2/17
# HTTP 요청 데이터 - API 메세지 바디 - 단순 텍스트
- HTTP Message Body에 데이터를 직접 담아서 요청
  - HTTP API 에서 주로 사용, JSON, XML, TEXT
  - 데이터 형식은 주로 JSON 사용
  - POST, PUT, PATCH

**RequestBodyStringServlet**

    @WebServlet(name = "RequestBodyStringServlet", urlPatterns = "/request-body-string")
    public class RequestBodyStringServlet extends HttpServlet {

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            ServletInputStream inputStream = req.getInputStream();
            String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);

            System.out.println("messageBody = " + messageBody);

            resp.getWriter().write("ok");
        }
    }
    
# HTTP 요청 데이터 - API 메세지 바디 - JSON
- JSON 형식 전송
  - content -type : application/json
  - message body : {"username": "hello:, "age": 20}
  - 결과 : messageBody = {"username": "hello", "age": 20}

**RequestBoddyJsonServlet**

    @WebServlet(name = "RequestBodyJsonServlet", urlPatterns = "/request-body-json")
    public class RequestBodyJsonServlet extends HttpServlet {

        private ObjectMapper objectMapper = new ObjectMapper();
        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            ServletInputStream inputStream = req.getInputStream();
            String messageBody = StreamUtils.copyToString(inputStream, StandardCharsets.UTF_8);

            System.out.println("messageBody = " + messageBody);

            HelloData helloData = objectMapper.readValue(messageBody, HelloData.class);

            System.out.println("helloData.getUsername() = " + helloData.getUsername());
            System.out.println("helloData.getAge() = " + helloData.getAge());

            resp.getWriter().write("ok");
        }
    }

- JSON 결과를 파싱해서 사용할 수 이쓴ㄴ 자바 객체로 변환하려면 Jackson, Gson 같은 JSON 변환 라이브러리 를 추가해서 사용
- 스프링 부트로 Spring MVC 사용시 기본으로 Jackson 라이브러리를 사용

# HttpServletResponse - 기본 사용법
- HTTP 응답 메세지 생성
  - HTTP 응답코드 지정
  - 헤더 생성
  - 바디 생성

**ResponseHeaderServlet**

    @WebServlet(name = "ResponseHeaderServlet", urlPatterns = "/response-header")
    public class ResponseHeaderServlet extends HttpServlet {

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            resp.setStatus(HttpServletResponse.SC_OK);

            resp.setHeader("Content-Type", "text/plain;charset=utf-8");
            resp.setHeader("Cache-Control", "no-cache, no-store, must-revalidate");
            resp.setHeader("Pragma", "no-cache");
            resp.setHeader("my-header", "hello");

        content(resp);
        cookie(resp);
        redirect(resp);

        PrintWriter writer = resp.getWriter();
        writer.println("안녕하세요");
    }

**Content 편의 메서드**

    private void content(HttpServletResponse response) {

        response.setContentType("text/plain");
        response.setCharacterEncoding("utf-8");
        //response.setContentLength(2); //(생략시 자동 생성)
    }

**쿠키 편의 메서드**

    private void cookie(HttpServletResponse response) {
        //Set-Cookie: myCookie=good; Max-Age=600;
        //response.setHeader("Set-Cookie", "myCookie=good; Max-Age=600");
        Cookie cookie = new Cookie("myCookie", "good");
        cookie.setMaxAge(600); //600초
        response.addCookie(cookie);
    }

**redirect 편의 메서드**

    private void redirect(HttpServletResponse response) throws IOException {
        //Status Code 302
        //Location: /basic/hello-form.html
	
        //response.setStatus(HttpServletResponse.SC_FOUND); //302
        //response.setHeader("Location", "/basic/hello-form.html");
        response.sendRedirect("/basic/hello-form.html");
    }

# v1.4 2/18
# HTTP 응답 데이터 - 단순 텍스트, HTML
## HttpServletResponse - HTML 응답
**ResponseHtmlServlet**
    
    @WebServlet(name = "ResponseHtmlServlet", urlPatterns = ("/response-html"))
    public class ResponseHtmlServlet extends HttpServlet {

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            resp.setContentType("text/html");
            resp.setCharacterEncoding("utf-8");

            PrintWriter writer = resp.getWriter();
            writer.println("<html>");
            writer.println("<body>");
            writer.println("  <div>안녕?</div>");
            writer.println("</body>");
            writer.println("</html>");
        }
    }
    
- HTTP 응답으로 HTML을 반환할 시 content-type을 text/html로 지정
- 페이지 소스보기 사용 시 결과 HTML을 println 한 것과 동일하게 확인 가능.

# HTTP 응답 데이터 - API JSON
**ResponseJsonServlet**

    @WebServlet(name = "ResponseJsonServlet", urlPatterns = ("/response-json"))
    public class ResponseJsonServlet extends HttpServlet {

        ObjectMapper objectMapper = new ObjectMapper();
        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            resp.setContentType("application/json");
            resp.setCharacterEncoding("utf-8");

            HelloData helloData = new HelloData();
            helloData.setUsername("kim");
            helloData.setAge(20);

            String result = objectMapper.writeValueAsString(helloData);
            resp.getWriter().write(result);
        }
    }
    
- HTTP 응답으로 JSON을 반환 할 때 content-type은 application/json으로 지정
- objectMapper.writeValueAsString() 사용 시 객체를 JSON 문자로 변경 가능.

# v1.5 2/19
# 서블릿을 통한 웹 애플리케이션
**MemberFormServlet**

    @WebServlet(name = "memberFormServlet", urlPatterns = "/servlet/members/new=form")
    public class MemberServletForm extends HttpServlet {

        private MemberRepository memberRepository = MemberRepository.getInstance();

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            resp.setContentType("text/html");
            resp.setCharacterEncoding("utf-8");

            PrintWriter w = resp.getWriter();
            w.write("<!DOCTYPE html>\n" +
                "<html>\n" +
                "<head>\n" +
		
                ....
		
                "</html>\n");
	}
    }
    
- MemberFormServlet은 회원 정보 입력이 가능한 HTML Form을 만들어서 응답.
- 자바코드를 통한 HTML 제공

**MemberSaveServlet**

    @WebServlet(name = "memberSaveServlet", urlPatterns = "/servlet/members/save")
    public class MemberSaveServlet extends HttpServlet {

        private MemberRepository memberRepository = MemberRepository.getInstance();

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            System.out.println("MemberSaveServlet.service");
            String username = req.getParameter("username");
            int age = Integer.parseInt(req.getParameter("age"));

            Member member = new Member(username, age);
            System.out.println("member = " + member);
            memberRepository.save(member);

            resp.setContentType("text/html");
            resp.setCharacterEncoding("utf-8");

            PrintWriter w = resp.getWriter();
            w.write("<html>\n" +
                "<head>\n" +
		
                ....
		
                "</html>");
        }
    }
    
- MemberSaveServlet 순서
  - 1.파라미터 조회 후 Member 객체 생성
  - 2.Member 객체를 MemberRepository를 통해 저장
  - 3.Member 객체를 사용해 HTML 동적 화면을 만들어서 응답

**MemberListServlet**

    @WebServlet(name = "MemberListServlet", urlPatterns = "/servlet/members")
    public class MemberListServlet extends HttpServlet {

        private MemberRepository memberRepository = MemberRepository.getInstance();

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            resp.setContentType("text/html");
            resp.setCharacterEncoding("utf-8");

            List<Member> members = memberRepository.findAll();

            PrintWriter w = resp.getWriter();

            w.write("<html>");
	    
	    ....
	    
            w.write(" <tbody>");

            for (Member member : members) {
                w.write(" <tr>");
                w.write(" <td>" + member.getId() + "</td>");
                w.write(" <td>" + member.getUsername() + "</td>");
                w.write(" <td>" + member.getAge() + "</td>");
                w.write(" </tr>");
            }
            w.write(" </tbody>");
            w.write("</table>"); w.write("</body>");
            w.write("</html>");
        }
    }
    
- MemberListServlet 순서
  - memberRepository.findAll()을 통해 모든 회원 조회
  - For 루프를 통해 외원 수만큼 동적으로 HTML에 생성 및 응답

**HTML을 통한 웹 애플리케이션 단점**
- 서블릿과 자바 코드만으로 HTML을 만들어 복잡, 비효율적 -> 자바 코드를 통해 HTML 코드를 작성해야하기 때문
- 차라리 HTML 문서에 동적 변경이 필요한 부분만 자바 코드를 넣는다면 더 효율적이고 편리
- 이 부분 문제 해결을 위해 템플릿 엔진(JSP, Thymeleaf, Freemarker, velocity)이 나오게 됨.
