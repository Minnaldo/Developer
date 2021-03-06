# Goal  
* Web Service의 기본적인 동작 과정을 이해하기.  
* 사용자 입력을 위한 HTML Form 이해하기.  
* Servlet 을 이해하기.  
  
# Web Service 의 기본적인 동작 과정  
# HTML Form -> Servlet ->HTML Page  
  
<img src="./img/Web_Service.png">  
  
1. 사용자가 웹 페이지 form(HTML Form)을 통해 자신의 정보를 입력한다. (Input)  
2. Servlet의 doGet() 또는 doPost() 메서드는 입력한 form data에 맞게 DB 또는 다른 소스에서 관련된 정보를 검색한다.  
3. 이 정보를 이용하여 사용자의 요청에 맞는 적절한 동적 컨텐츠(HTML Page)를 만들어서 제공한다. (Output)  
  
-------------------------------------------------  
## 1. HTML Form  
: input elements(Ex. 텍스트 상자)가 포함된 웹 페이지의 한 부분(section)  
* 사용자가 입력한 정보(form contents)를 웹 서버로 전송하기 위한 submit element(Ex. 버튼)가 존재한다.  
* action 에는 form을 처리하는 서버 쪽 URL 을 명시한다.  
  
### 클라이언트(browser)가 요청하는 URL 정보  
* 요청을 보낼 서버의 IP 주소 : Port 번호 / App 이름 / 달라고 요청하는 HTML  
- Ex) localhost:8080/FormHandlingServlet/LoginForm.html  
* IP 주소  
- 요청을 보낼 서버의 "위치"를 의미한다.  
- browser와 WAS(Tomcat)가 같은 PC에 있으면 "IP 주소 = localhost"  
* Port 번호  
- 해당 위치 안의 "특정 사람"을 의미한다.  
* HTML 이름  
- i. HTML 이름에 해당하는 HTML 문서를 받아온다.  
- ii. browser는 받아온 HTML 문서 안의 html 태그들(Ex. h2, br 등)을 parsing(기계어로 번역)한다.  
- iii. 마지막으로 이 내용을 browser에서 rendering(그리기)한다.  
  
## LoginForm.html 내용 (간단한 예시)  
<img src="./img/LoginForm.png">  
  
* method="post"  
- 원하는 동작에 따라 HTTP 메서드를 사용한다.  
- method="get"  
* action="loginServlet"  
- URL of the servlet  
- 해당 URL로 request가 간다. 즉, WAS에서의 어떤 Servlet인지 지정하는 것.  
* type="submit"  
- 버튼을 누르면 사용자가 입력한 인자들이 Servlet으로 넘어간다.  
  
## Form Tag 속성  
: Form Tag 속성을 이용하여 어디로, 어떤 방식으로 전송할지 정한다.  
* action : form 을 전송할 서버 쪽 스크립트 파일 지정  
* name : form 을 식별하기 위한 이름 지정  
* accept-charset : form 전송에 사용할 문자 인코딩을 지정  
* target : action 에서 지정한 스크립트 파일을 현재 창이 아닌 다른 위치에서 열리도록 지정  
* method : form 을 서버에 전송하는 방식으로, HTTP 메서드 지정 (GET 또는 POST)  
* enctype : 넘기는 Content의 Type 지정 (주로 파일을 넘길 때 사용)  
- type은 multipart/form-data 로 지정해서 사용  
  
## Form Methods  
: form 을 서버에 전송하는 방식으로, 두 가지 HTTP 메소드 지정할 수 있다.  
1. GET Method  
* 사용자가 입력한 내용(form data)이 URL 뒤에 텍스트 문자열로 추가된다.  
- 크기 제한: 1024 characters  
- data는 ?를 기준으로 action URL과 분리된다.  
- Ex.) http://www.test.com/hello?key1=value1&key2=value2  
* 브라우저에서 웹 서버로 정보를 전달하는 기본 메서드(Default Method)  
- HTTP 메서드를 지정하지 않으면 GET Method를 호출한다.  
* 서버에 전달하는 data에 암호와 같은 민감한 정보가 있는 경우는 GET Method를 사용하지 않는다.  
- URL은 모두에게 노출되는 정보이기 때문에 보안상 적절하지 않다.  
* GET 메서드의 사용  
- Query-Type actions: DB에 영향을 주지 않는 단ㅅ누히 읽기 위주(read operation)의 작업  
- Idempotemt actions: 몇 번이고 같은 연산을 반복해도 같은 값이 나오는 작업  
  
2. POST Method  
* 사용자가 입력한 내용(form data)을 별도의 메시지로 보낸다.  
* Request Body에 data를 추가한다.  
- URL에 직접적으로 data가 노출되지 않기 때문에, GET Method 보다 보안상으로 조금 더 안전하다.  
* POST 메서드의 사용  
- actions with side-effects: DB에 영향을 주는 작업  
  
---------------------------------------------  
  
# 2. Servlet이란  
## Servlet의 개념  
: 웹 기반의 요청에 대한 동적인 처리가 가능한 하나의 클래스  
- Server Side에서 돌아가는 Java Program  
- 개발자가 작성해야 하는 부분  
  
## Servlet Program의 기본적인 동작 과정  
<img src="./img/Web_Program.png">  
  
1. Web Server 는 HTTP request 를 Web Container(Servlet Container)에게 위임한다.  
* 1) web.xml 설정에서 어떤 URL과 매핑되어 있는지 확인.  
* 2) 클라이언트(browser)의 요청 URL을 보고 적절한 Servlet을 실행  
2. Web Container 는 service() 메서드를 호출하기 전에 Servlet 객체를 메모리에 올린다.  
* 1) Web container 는 적절한 Servlet 파일을 컴파일(.class 파일 생성)한다.  
* 2) .class 파일을 메모리에 올려 Servlet 객체를 만든다.  
* 3) 메모리에 로드될 때, Servlet 객체를 초기화하는 init() 메서드가 실행된다.  
3. Web Container 는 Request 가 올 때마다, thread 를 생성하여 처리한다.  
* 각 thread 는 Servlet의 단일 객체에 대한 service() 메서드를 실행한다.  
  
  
### 참고 : Servlet Program에서 Thread의 역할  
* Thread란? : 운영체제로부터 시스템 자원을 할당받는 작업의 단위  
* Servlet Program 에서 thread가 수행할 메서드가 지정/할당되면  
- thread는 생성 후 즉시 해당 메서드만 열심히 수행한다.  
- 해당 메서드가 return하면 thread는 종료되고 제거된다.  
- 즉, 실제로 thread의 역할 : Servlet의 doGet() 또는 doPost() 호출하는 것.  
* Web Container(Servlet Container)는 thread의 생성과 제거를 담당한다.  
- 하지만 thread의 생성과 제거의 반복은 큰 오버헤드를 만든다.  
- 이를 위해 Tomcat(WAS)은 "Thread Pool"(미리 thread를 만들어 놓음) 이라는 적절한 매커니즘을 사용하여 오버헤드를 줄인다.  
* 즉, WAS는 Servlet의 life cycle을 담당한다.  
- 웹 브라우저 클라이언트의 요청이 들어왔을 때, Servlet 객체 생성은 WAS가 알아서 처리한다.  
- WAS 위에서 Servlet이 돌아다니고 개발자는 이 Servlet을 만들어야 한다.  
  

## Servlet Life Cycle  
<img src="./img/Servlet_Life_Cycle.png">  

* 클라이언트의 요청이 들어오면 WAS는 해당 요청에 맞는 Servlet이 메모리에 있는지 확인한다.  
- 만약 메모리에 없다면 해당 Servlet Class 를 메모리에 올린 후 (Servlet 객체 생성) init 메서드 실행  
    - 이후 service 메서드를 실행  
- 메모리에 있다면 바로 service 메서드 실행  
  

* init()  
- 한 번만 수행된다.  
- 클라이언트(browser)의 요청에 따라 적절한 Servlet이 생성되고 이 Servlet이 메모리에 로드될 때 init()  메서드가 호출된다.  
- 역할 :Servlet 객체를 초기화  
  
* service(request, response)  
- 응답에 대한 모든 내용은 service() 메서드에 구현해야 한다.  
- Servlet이 수신한 모든 request에 대해 service() 메서드가 호출된다.  
    - HttpServlet을 상속받은 Servlet 클래스(이하 하위 클래스)에서 service() 메서드를 오버라이드하지 않았다면, 그 붐인 HttpServlet 의 service() 가 호출된다.  
    - HttpServlet의 service() 메서드는 템플릿 메서드 패턴으로 구현되어 있다.  
    - service() 메서드는 request의 type(HTTP Method GET, POST, PUT, DELETE 등)에 따라 적절한 메서드(doGet, doPost, doPut, doDelete 등)를 호출한다.  
    - 즉, 하위 클래스에서 doGet, doPost 등의 메서드를 오버라이드해두면 HttpServlcet의 sercie() 메서드가 요청에 맞는 메서드(하위 클래스에서 오버라이드한 메서드)를 알아서 호출할 수 있게 되는 것이다.  
- 메서드가 return 하면 해당 thread는 제거된다.  
  
* destroy()  
- 한 번만 수행된다.  
- Web Application 이 갱신되거나 WAS 가 종료될 때 호출된다.  
- 역할 : Servlet 객체를 메모리에서 제거  
  
  
## Servlet의 메서드 구현 (간단한 예시)  
<img src="./img/Servlet_Method.png">  
  
* WAS는 웹 브라우저로부터 요청을 받으면  
- i. 요청할 때 가지고 있는 정보를 HttpServletRequest 객체를 생성하여 저장한다.  
- ii. 웹 브라우저에게 응답을 보낼 때 사용하기 위하여 HttpServletResponse 객체를 생성한다.  
- iii. 생성된 HttpServletRequest, HttpServletResponse 객체를 Servlet에게 전달한다.  

* 개발자는 일반적으로 javax.servlet.http.HttpServlet 를 상속받은 Servlet 클래스를 작성한다.  
- HttpServletRequest request 파라미터를 통해 사용자가 입력한 form data 를 읽는다.  
- HttpServletResponse response 파라미터를 통해 출력/결과 Web Page를 생성한다.  

* 개발자는 Servlet 클래스에 doGet() 또는 doPost() 중 적어도 하나를 재정의하여 작성한다.  
- protected doGet()(HttpServletRequest request, HttpServletResponse response){}  
- protected doPost()(HttpServletRequest request, HttpServletResponse response){}  

* HttpServletRequest request 객체  
- 사용자가 HTML Form에 입력한 내용(username과 password)을 request 객체에서 받아온다.  
    - 즉, HTTP 프로토콜의 Request 정보를 Servlet에게 전달  
- 헤더 정보, 파라미터, 쿠키, URI, URL, Body의 Stream 등을 읽어 들이는 메서드가 있다.  
- getHeader("원하는 헤더 이름")  
    - 이 메서드를 통해 원하는 헤더 정보를 확인할 수 있다.  
- getParameter()  
    - 이 메서드를 호출하여 form parameter 값을 가져온다.  
        - 이런 parameter 값은 URL 또는 form 의 input tag를 통해서 넘어올 수 있다.  
    - Ex.  
    <src img="./img/getParameter_Ex.png">  
- getParameterValues()  
    - form parameter가 두 번 이상 나타나고 여러 개의 값을 반환할 때 이 메서드를 호출한다. (Ex. checkbox)  
    - Ex.  
    <img src="./img/getParameterValues_Ex.png">  
      
* HttpServletResponse response 객체  
- 인자의 내용에 맞게 동적인 HTML 코드를 생성하여 response 객체에 담아 반환한다.  
- 인자의 내용에 맞게 동적인 HTML 코드를 생성하여 response 객체에 담아 반환한다.  
- getWriter() 메서드를 호출하여 PrintWriter 객체를 가져온 후 해당 객체에서 print, println 메서드를 실행한다.  
- 즉, form data를 처리한 결과를 Web Page에 생성(view 생성)하여 반환한다.  
- Ex.  
<img src="./img/HttpServletResponse_Ex.png">  
  
## Servlet Concurrency  
<img src="./img/Servlet_Concurrency.png">  

* Java 서블릿 컨테이너 / 웹 서버는 일반적으로 멀티 쓰레드 환경이다.  
- 같은 Servlet에 대한 여러 개의 요청이 동시에 실행될 수 있어 runtime에 따라 결과가 달라질 수 있다.  
- 즉, Servlet은 메모리에 한 번 올라오고 멀티 쓰레드 환경에서 여러 thread는 하나의 Servlet을 공유하기 때문에 Concurrency Control(병행성 제어)가 필요하다.  
  
* Servlet의 service() 메서드 안의 변수  
- 정적 변수, 멤버 변수 : 공유하는 자원이므로 상호배제가 필요  
- 지역 변수 : thread 마다 독립적으로 생성  
  
## Servlet Annotation  
<img src="./img/Servlet_Annotation.png">  

* Servlet API 3.0은 javax.servlet.annotation 이라는 새로운 패키지를 도입했다.  
- Tomcat 7 이상에서 사용 가능.  
* Annotation은 Web Deployment Descriptor 파일(web.xml)의 XML 설정을 대체 할 수 있다.  
- 설정 파일이 너무 길어지면 Annotation 으로 변경한다.  
* Annotation Type  
- @WebServlet : 서블릿 선언  
- @WEBINITPARAM : 초기화 매개 변수 지정  
* 예시  
<img src="./img/Servlet_Annotation_Ex.png">  

* 버전에 따른 Servlet 사용 방법  
- i. Servlet 3.0 spec 이상  
- web.xml 파일을 사용하지 않는다.  
- 자바 어노테이션(annotation)을 사용.  
- @WebServlet("/test")  
    a Servlet 3.0 spec 미만  
- Servlet 을 등록할 때, web.xml 파일에 직접 등록 
```xml SimpleServlet SimpleServlet SimpleServlet/test```  
- servlet-mapping 에서 해당 url이 존재하지 않으면 404 응답 코드를 반환한다.  
- servlet-mapping의 servlet-name과 servlet의 servlet-name 을 동일하게 작성해야 해당 url에 맞는 Servlet을 찾을 수 있다.  
  
