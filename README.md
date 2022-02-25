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
  - 1.memberRepository.findAll()을 통해 모든 회원 조회
  - 2.For 루프를 통해 외원 수만큼 동적으로 HTML에 생성 및 응답

**HTML을 통한 웹 애플리케이션 단점**
- 서블릿과 자바 코드만으로 HTML을 만들어 복잡, 비효율적 -> 자바 코드를 통해 HTML 코드를 작성해야하기 때문
- 차라리 HTML 문서에 동적 변경이 필요한 부분만 자바 코드를 넣는다면 더 효율적이고 편리
- 이 부분 문제 해결을 위해 템플릿 엔진(JSP, Thymeleaf, Freemarker, velocity)이 나오게 됨.

# v1.6 2/20
# JSP를 통한 웹 애플리케이션 개발
**build.gradle에 추가**

    implementation 'org.apache.tomcat.embed:tomcat-embed-jasper'
    implementation 'javax.servlet:jstl'
    
**new-form,jsp - 멤버 등록 폼**

    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <html>
    <head>
      <title>Title</title>
    </head>
    <body>
    <form action="/jsp/members/save.jsp" method="post">
     username: <input type="text" name="username" />
     age: <input type="text" name="age" />
     <button type="submit">전송</button>
    </form>
    </body>
    </html>

- <%@ page contentType="text/html;charset=UTF-8" language="java" %>
  - JSP 문서 선언 후 JSP 문서 작성 시작
- 서버 내부에서 서블릿으로 변환하여 이전의 memberFormServlet과 유사한 모습으로 변환

**save,jsp - 회원 저장**

    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%@ page import="hello.servlet.basic.member.Member" %>
    <%@ page import="hello.servlet.basic.member.MemberRepository" %>
    <%
        //request, response 사용 가능
        MemberRepository memberRepository = MemberRepository.getInstance();

        System.out.println("MemberSaveServlet.service");
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));

        Member member = new Member(username, age);
        memberRepository.save(member);

    %>
    <html>
     ....
    <ul>
        <li>id=<%=member.getId()%></li>
        <li>username=<%=member.getUsername()%></li>
        <li>age=<%=member.getAge()%></li>
    </ul>
    ....
    </html>

- JSP는 자바 코드를 그래도 사용 가능
- <%@ page import="hello.servlet.domain.member.MemberRepository" %>
  - JAVA의 import문과 동일
- <% ~~ %>
  - JAVA 코드 입력 및 출력 가능
- JSP는 서블릿 코드와 동일하나, HTML을 중심으로 설계 후, 자바 코드를 부분 입력.

**members.jsp - 회원 목록**

    <%@ page contentType="text/html;charset=UTF-8" language="java" %>
    <%@ page import="hello.servlet.basic.member.Member" %>
    <%@ page import="hello.servlet.basic.member.MemberRepository" %>
    <%@ page import="java.util.List" %>
    <%
     MemberRepository memberRepository = MemberRepository.getInstance();
     List<Member> members = memberRepository.findAll();
    %>
    <html>
    ....
     <tbody>
    <%
     for (Member member : members) {
     out.write(" <tr>");
     out.write(" <td>" + member.getId() + "</td>");
     out.write(" <td>" + member.getUsername() + "</td>");
     out.write(" <td>" + member.getAge() + "</td>");
     out.write(" </tr>");
     }
     %>
     ....
     </html>

## 서블릿과 JSP의 한계
- 서블릿
  - 뷰(view) 화면을 위한 HTML을 만드는 작업이 복잡, 비효율적
- JSP
  - 뷰(view)를 생성하는 HTML 작업 및 부분 자바 코드 작성은 간단, 효율적.
- 그러나, JSP의 상위 절반 코드는 비지니스 로직, 나머지 코드는 뷰(view) 화면을 위한 HTML 코드.
- 비지니스 로직과 뷰 코드는 분리가 필요.
- **MVC 패턴 등장**
  - MVC 패턴을 통해 비지니스 로직과 뷰 코드를 분리 시켜 JSP는 뷰(view) 화면에만 집중.


# v1.7 2/21
# MVC 패턴 - 개요
- 역할 분리
  - 하나의 서블릿 혹은 JSP가 비지니스 로직과 뷰 렌더링까지 모두 처리 시, 많은 역할을 하게 되고 결과적으로 유지보수가 어려움
  - UI를 변경할 경우, 비지니스 로직이 함께 있는 파일을 수정해야 함
- 변경의 라이프 사이클
  - 둘 사이의 변경 라이프 사이클이 다름. 예를 들어 UI를 일부 수정하는 일과 비지니스 로직을 수정하는 일은 각각 다르게 발생할 가능성이 높고 서로 영향을 주지 않음.
  - 서로 라이프 사이클이 다를 시 유지 보수가 어려움
- 기능 특화
  - JSP 같은 뷰 템플릿은 화면 렌더링에 특화 되어 있기 떄문에 업무를 분리해 주는 것이 효과적
- Model View Contoroller
  - 컨트롤러(contoroller)와 뷰(view) 영역을 나눠서 설계
  - 컨트롤러(Controller) : HTTP 요청을 받아서 파라미터를 검증하고, 비지니스 로직을 실행. 그 후 뷰에 전달할 결과 데이터를 조회해서 모델에 전송
  - 모델(Model) : 뷰에 출력할 데이터를 담아둠. 뷰가 필요한 데이터를 모두 모델에 담아 전달해주는 역할을 하며, 뷰와 역할을 분리함
  - 뷰(View) : 모델에 담긴 데이터를 이용하여 화면에 렌더링. 대부분 HTML을 생성하는 부분

![image](https://user-images.githubusercontent.com/96407257/154931174-b60ca4e4-82ba-47a6-be0e-0d860c812356.png)

# MVC 패턴 - 적용
**MvcMemberFormServlet - 회원 등록 폼(컨트롤러)**

    @WebServlet(name = "MvcMemberFormServlet", urlPatterns = "/servlet-mvc/members/new-form")
    public class MvcMemberFormServlet extends HttpServlet {

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            String viewPath = "/WEB-INF/views/new-form.jsp";
            RequestDispatcher dispatcher = req.getRequestDispatcher(viewPath);
            dispatcher.forward(req,resp);
        }
    }
    
- Model은 HttpServletRequest 객체를 사용.
- request는 내부에 데이터 저장소를 가지고 있고, requset.setAttribute(), request.getAttribute()를 사용 시 데이터를 보관 및 조회가 가능
- /WEB-INF 결로 안에 JSP가 있을 시 외부에서 직접 JSP를 호출 불가능
- redirect vs forward
  - 리다이렉트는 실제 클라이언트에 응답이 나갔다가, 클라이언트가 redirect경로로 다시 요청하기 때문에 클라이언트가 인지가 가능하며, URL 결로도 실제로 변경
  - 포워드는 서버 내부에서 일어나는 호출로 클라이언트가 인지 불가능
- JSP에서 form의 action은 상대 경로로 설정. 상대 경로 설정 시 현재 URL이 속한 계층 경로 _ save가 호출

**MvcMemberSaveServlet - 회원 저장(컨트롤러)**

    @WebServlet(name = "MvcMemberSaveServlet", urlPatterns = "/servlet-mvc/members/save")
    public class MvcMemberSaveServlet extends HttpServlet {

        private MemberRepository memberRepository = MemberRepository.getInstance();

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            String username = req.getParameter("username");
            int age = Integer.parseInt(req.getParameter("age"));

            Member member = new Member(username, age);
            System.out.println("member = " + member);
            memberRepository.save(member);

            req.setAttribute("member", member);

            String viewPath = "/WEB-INF/views/save-result.jsp";
            RequestDispatcher dispatcher = req.getRequestDispatcher(viewPath);
            dispatcher.forward(req,resp);

        }
    }
    
- HttpServletRequest를 Model로 사용
- request가 제공하는 setAttribute() 사용 시 requst 객체에 데이터를 보관하여 뷰에 전달
- 뷰는 request.getAttribute()를 사용하여 데이터를 이용

**MvcMemberListServlet - 회원 목록 조회(컨트롤러)**

    @WebServlet(name = "MvcMemberListServlet", urlPatterns = "/servlet-mvc/members")
    public class MvcMemberListServlet extends HttpServlet {

        private MemberRepository memberRepository = MemberRepository.getInstance();
        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            System.out.println("MvcMemberListServlet.service");
            List<Member> members = memberRepository.findAll();

            req.setAttribute("members", members);

            String viewPath = "/WEB-INF/views/members.jsp";
            RequestDispatcher dispatcher = req.getRequestDispatcher(viewPath);
            dispatcher.forward(req,resp);
         }
     }
     
- request 객체를 사용하여 members를 모델에 보관

# MVC 패턴 - 한계
- 포워드 중복
  - view로 이동하는 코드가 항상 중복 호출 됨. 메서드를 공통화해도 되지만 해당 메서드로 항상 직접 호출이 필요
       
        RequestDispatcher dispatcher = request.getRequestDispatcher(viewPath);
        dispatcher.forward(request, response);
        
- ViewPath 중복
  - /WEB-INF/views/ 가 계속해서 중복
  - 그리고 JSP 가 아닌 다른 뷰로 변경 시 전체 코드 변경이 필요
- 사용하지 않는 코드
  - request는 사용하지만, response 코드는 사용되지 않음
- 공통 처리의 어려움
  - 기능이 복잡할수록 컨트롤러에서 공통으로 처리해야 하는 부분이 증가.
- 해결점
  - 컨트롤러 호출 전에 먼저 공통 기능 처리가 필요.
  - 프론트 컨트롤러(front controller)패턴을 통해 문제를 해결 가능.
  - 스프링 MVC 역시 프론트 컨트롤러 패턴을 사용하여 해결.

# v1.8 2/22
# 프론트 컨트롤러 패턴
![image](https://user-images.githubusercontent.com/96407257/155145288-24ab5324-0f90-4db9-afc9-0f9895c88600.png)
- 프론트 컨트롤러 서블릿 하나로 클라이언트의 요청을 수신
- 프론트 컨트롤러가 요청에 맞는 컨트롤러를 찾아 호출
- 공통 처리 가능
- 프론트 컨트롤러를 제외한 컨트롤러는 서블릿을 사용 X
- 스프링 웹 MVC 역시 **프론트 컨트롤러**이며, 스프링 웹 MVC의 DispatcherServlet 이 프론트 컨트롤러 패턴으로 구현

# 프론트 컨트롤러 - V1
![image](https://user-images.githubusercontent.com/96407257/155145846-b5893ec2-bea7-4125-a619-bfc5ad18fc0a.png)

**ControllerV1**

    public interface ControllerV1 {

        void process(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException;
    }
    
- 서블릿과 비슷한 모양의 컨트롤러 인터페이스를 도입.
- 각 컨트롤러는 controller interface를 호출하며 로직의 일관성 성립 가능.

- 내부 로직 **MemberFormControllerV1**, **MemberSaveControllerV1**, **MemberListControllerV1**을 생성
    
**FrontContorllerServletV1 - 프론트 컨트롤러**

    @WebServlet(name = "frontControllerServletV1", urlPatterns = "/front-controller/v1/*")
    public class FrontControllerServletV1 extends HttpServlet {

        private Map<String, ControllerV1> controllerMap = new HashMap<>();

        public FrontControllerServletV1() {
            controllerMap.put("/front-controller/v1/members/new-form", new MemberFormControllerV1());
            controllerMap.put("/front-controller/v1/members/save", new MemberSaveControllerV1());
            controllerMap.put("/front-controller/v1/members", new MemberListControllerV1());
        }

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            System.out.println("FrontControllerServletV1.service");
            String requestURI = req.getRequestURI();

            ControllerV1 controller = controllerMap.get(requestURI);
            if (controller == null) {
                resp.setStatus(HttpServletResponse.SC_NOT_FOUND);
                return;
            }

            controller.process(req, resp);
        }
    }
    
- **urlpatterns** = "/front-controller/v1/" : "/front-controller/v1" 를 포함한 하위 모든 요청을 서블릿에서 받아들임
- **Map<String, ControllerV1> controllerMap** : String 은 매핑 URL, Value : 호출될 컨트롤러
- **service()** : 먼저 requestURI를 초회해서 실제 호출될 컨트롤러를 contorollerMap에서 찾음 -> 없을 시 404 상태 코드 반환 -> 컨트롤러를 찾은 후 controller.process(request,response)를 호출하여 해당 컨트롤러를 실행

# v1.9 2/23
# 프론트 컨트롤러(View 분리) - V2
![image](https://user-images.githubusercontent.com/96407257/155277685-b16dd9e2-a579-4f0c-896c-56daee7f487c.png)
- 모든 컬트롤러에서 뷰로 이동하는 부분의 코드 중복을 제거

**MyView**

    public class MyView {

        private String viewPath;

        public MyView(String viewPath) {
            this.viewPath = viewPath;
        }

        public void render(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            RequestDispatcher dispatcher = req.getRequestDispatcher(viewPath);
            dispatcher.forward(req, resp);
        }
    }
    
**ControllerV2**

    public interface ControllerV2 {

        MyView process(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException;
    }
    
**MemberFormControllerV2**, **MemberSaveControllerV2**, **MemberListControllerV2**
- dispatcher.forward()를 MyView에 전가함으로서 코드 작성 필요가 없음.
- 단순히 MyView 객체 생성 후 뷰 이름을 넣고 반환

**FrontControllerServletV2**

    @WebServlet(name = "frontControllerServletV2", urlPatterns = "/front-controller/v2/*")
    public class FrontControllerServletV2 extends HttpServlet {

        private Map<String, ControllerV2> controllerMap = new HashMap<>();

        public FrontControllerServletV2() {
            controllerMap.put("/front-controller/v2/members/new-form", new MemberFormControllerV2());
            controllerMap.put("/front-controller/v2/members/save", new MemberSaveControllerV2());
            controllerMap.put("/front-controller/v2/members", new MemberListControllerV2());
        }

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            System.out.println("FrontControllerServletV2.service");
            String requestURI = req.getRequestURI();

            ControllerV2 controller = controllerMap.get(requestURI);
            if (controller == null) {
                resp.setStatus(HttpServletResponse.SC_NOT_FOUND);
                return;
            }

            MyView view = controller.process(req, resp);
            view.render(req,resp);
        }
    }
    
- ControllerV2의 반환 타입이 MyView이므로 프론트 컨트롤러는 MyView를 반환.
- 그 후 view.render()를 호출하면 forward 로직을 수행하여 JSP를 실행.

# v1.10 2/24
# 프론트 컨트롤러(Model 추가) - V3
![image](https://user-images.githubusercontent.com/96407257/155527682-6f08baac-d66f-49d8-81a9-a90df3a9a932.png)
- 서블릿 종속성 제거
  - 요청 파라미터 정보는 자바의 Map으로 대신 넘기도록 하면 지금 구조에서는 컨트롤러가 서블릿 기술을 몰라도 동작 가능
- 뷰 이름 중복 제거
  - 중복 되는 뷰 이름을 제거하고 동적인 이름만 코드 작성
- **Modelview**
  - 이전까지는 컨트롤러에서 서블릿에 종속적인 HttpServletRequest를 사용하였고, Model도 request.setAttribute()를 통해 데이터를 저장하고 뷰에 전달.

**ModelView**

    @Getter @Setter
    public class ModelView {
        private String viewName;
        private Map<String, Object> model = new HashMap<>();

        public ModelView(String viewName) {
            this.viewName = viewName;
        }
    }
    
**ControllerV3**

    public interface ControllerV3 {

        ModelView process(Map<String, String> paramMap);
    }
    
**MemberFormController**, **MemberSaveController**, **MemberListcontroller**
- paramMap.get("username");
  - 파라미터 정보는 map에 담겨있다. map에서 필요한 요청 파라미터를 조회
- mv.getModel().put("member", member);
  - 모델은 단순한 map이므로 모델에 뷰에서 필요한 member객체를 담고 반환

**FrontControllerServletV3**

    @WebServlet(name = "frontControllerServletV3", urlPatterns = "/front-controller/v3/*")
    public class FrontControllerServletV3 extends HttpServlet {

        private Map<String, ControllerV3> controllerMap = new HashMap<>();

        public FrontControllerServletV3() {
            controllerMap.put("/front-controller/v3/members/new-form", new MemberFormControllerV3());
            controllerMap.put("/front-controller/v3/members/save", new MemberSaveControllerV3());
            controllerMap.put("/front-controller/v3/members", new MemberListControllerV3());
        }

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            System.out.println("FrontControllerServletV3.service");
            String requestURI = req.getRequestURI();

            ControllerV3 controller = controllerMap.get(requestURI);
            if (controller == null) {
                resp.setStatus(HttpServletResponse.SC_NOT_FOUND);
                return;
            }

            Map<String, String> paramMap = CreateParamMap(req);
            ModelView mv = controller.process(paramMap);

            String viewName = mv.getViewName();
            MyView view = viewResolver(viewName);

            view.render(mv.getModel(),req,resp);
        }

        private MyView viewResolver(String viewName) {
            return new MyView("/WEB-INF/views/" + viewName + ".jsp");
        }

        private Map<String, String> CreateParamMap(HttpServletRequest req) {
            Map<String, String> paramMap = new HashMap<>();
            req.getParameterNames().asIterator()
                    .forEachRemaining(paramName -> paramMap.put(paramName, req.getParameter(paramName)));

            return paramMap;
        }
    }
    
- view.render(mv.getModel(), request, response) 를 MyView 객체에 필요한 메서드를 추가.
- createParamMap()을 HttpServletRequest에서 파라미터 정보를 꺼내서 Map으로 변환

**뷰 리졸버**
- MyView view = viewResolver(viewName)
  - 컨트롤러가 반환한 논리 뷰 이름을 실제 물리 뷰 경로로 변경. 그리고 Myview객체를 반환
- 논리 뷰 이름 : members
- 물리 뷰 결로 : : /WEB-INF/views/members.jsp
- view.render(mv.getModel(), request, resopnse)
  - 뷰 객체를 통해서 HTML 화면을 렌더링
  - 뷰 객체의 render()는 모델 정보도 함께 받음
  - JSP는 request.getAttribute()로 데이터를 조회, 모델의 데이터를 꺼내서 request.setAttribute()로 담음
  - JSP로 포워드 해서 JSP를 렌더링

**My View**

    public class MyView {

        private String viewPath;

        public MyView(String viewPath) {
            this.viewPath = viewPath;
        }

        public void render(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            RequestDispatcher dispatcher = req.getRequestDispatcher(viewPath);
            dispatcher.forward(req, resp);
        }

        public void render(Map<String, Object> model, HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {
            modelToRequestAttribute(model, req);
            RequestDispatcher dispatcher = req.getRequestDispatcher(viewPath);
            dispatcher.forward(req,resp);
        }

        private void modelToRequestAttribute(Map<String, Object> model, HttpServletRequest req) {
            model.forEach((key, value) -> req.setAttribute(key, value));
        }
    }

# v1.11 2/25
# 프론트 컨트롤러(단순, 실용적) - V4
![image](https://user-images.githubusercontent.com/96407257/155648318-79797286-cd7d-43d0-a2c0-d8e963ba2a26.png)
- 구조는 V3와 같으나, ModelView를 반환하지 않고, ViewName만 반환

**ControllerV4**

    public interface ControllerV4 {

        String process(Map<String, String> paramMap, Map<String, Object> model);
    }
    
- 인터페이스에 ModelView가 없고 뷰의 이름만 반환

**MemberFormControllerV4**, **MemberSaveControllerV4**, **MemberListControllerV4**
- 뷰의 논리 이름만 반환.
- 모델을 직접 생성하지 않고, 파라미터로 전달
  - model.put("member",member)

**FrontControllerServletV4**

    @WebServlet(name = "frontControllerServletV4", urlPatterns = "/front-controller/v4/*")
    public class FrontControllerServletV4 extends HttpServlet {

        private Map<String, ControllerV4> controllerMap = new HashMap<>();

        public FrontControllerServletV4() {
            controllerMap.put("/front-controller/v4/members/new-form", new MemberFormControllerV4());
            controllerMap.put("/front-controller/v4/members/save", new MemberSaveControllerV4());
            controllerMap.put("/front-controller/v4/members", new MemberListControllerV4());
        }

        @Override
        protected void service(HttpServletRequest req, HttpServletResponse resp) throws ServletException, IOException {

            System.out.println("FrontControllerServletV3.service");
            String requestURI = req.getRequestURI();

            ControllerV4 controller = controllerMap.get(requestURI);
            if (controller == null) {
                resp.setStatus(HttpServletResponse.SC_NOT_FOUND);
                return;
            }

            Map<String, String> paramMap = CreateParamMap(req);
            Map<String, Object> model = new HashMap<>();

            String viewName = controller.process(paramMap, model);
            MyView view = viewResolver(viewName);

            view.render(model,req,resp);
        }

        private MyView viewResolver(String viewName) {
            return new MyView("/WEB-INF/views/" + viewName + ".jsp");
        }

        private Map<String, String> CreateParamMap(HttpServletRequest req) {
            Map<String, String> paramMap = new HashMap<>();
            req.getParameterNames().asIterator()
                    .forEachRemaining(paramName -> paramMap.put(paramName, req.getParameter(paramName)));

            return paramMap;
        }
    }
    
- 모델 객체 전달
  - Map<String, Object> model = new HashMap<>()
  - 모델 객체를 프론트 컨트롤러에서 생성 후 넘김. 컨트롤러에서 모델 객체에 값을 담으면 여기에 담겨있게 됨.
- 뷰 논리 이름을 직접 반환
  - String viewName = controller.process(paramMap, model);  
    Myview view = viewResolver(viewName);  
- 기존 구조에서 모델을 파라미터로 넘기고, 뷰의 논리 이름을 반환하는 컨트롤러
