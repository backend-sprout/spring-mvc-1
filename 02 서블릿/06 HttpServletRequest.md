HttpServletRequest
=====================
# ๐ HttpServletRequest ๊ฐ์ 
## ๐ HttpServletRequest ์ญํ 
์๋ธ๋ฆฟ์ **`HTTP ์์ฒญ ๋ฉ์์ง๋ฅผ ํ์ฑ`ํ๊ณ  ๊ทธ ๊ฒฐ๊ณผ๋ฅผ `HttpServletRequest ๊ฐ์ฒด`์ ๋ด์์ ์ ๊ณตํ๋ค.**       
       
![http-message-get](https://user-images.githubusercontent.com/50267433/126497276-f0ddfed0-9b91-46fa-a26f-51056feec165.PNG)   

```http
POST /save HTTP/1.1                                     # START LINE
Host: localhost:8080                                    # HEADER
Content-Type: application/x-www-form-urlencoded         # HTML FORM ์ผ๋ก ์ ์ก  

username=kim&age=20                                     # BODY
```
์ฆ, ์๋ธ๋ฆฟ์ ์์ ๊ฐ์ `HTTP ์์ฒญ ๋ฉ์์ง` ํ์ฑํด์ `HttpServletRequest ๊ฐ์ฒด`๋ฅผ ๋ง๋ค๊ณ   
`HttpServletRequest ๊ฐ์ฒด`๋ฅผ ํตํด ์ฐ๋ฆฌ๋ ์๋์ ๊ฐ์ HTTP ์์๋ค์ ํธ๋ฆฌํ๊ฒ ์กฐํํ  ์ ์๋ค.
       
* **START LINE**   
    * HTTP ๋ฉ์๋
    * URL
    * ์ฟผ๋ฆฌ ์คํธ๋ง
    * ์คํค๋ง, ํ๋กํ ์ฝ
* **ํค๋**   
    * ํค๋ ์กฐํ
* **๋ฐ๋**   
    * form ํ๋ผ๋ฏธํฐ ํ์ ์กฐํ   
    * message body ๋ฐ์ดํฐ ์ง์  ์กฐํ   

`HttpServletRequest`, `HttpServletResponse`๋              
HTTP ์์ฒญ ๋ฉ์์ง, HTTP ์๋ต ๋ฉ์์ง๋ฅผ ํธ๋ฆฌํ๊ฒ ์ฌ์ฉํ๋๋ก ๋์์ฃผ๋ ๊ฐ์ฒด๋ค.        
๋ฐ๋ผ์ ๊น์ด์๋ ์ดํด๋ฅผ ํ๋ ค๋ฉด HTTP ์คํ์ด ์ ๊ณตํ๋ ์์ฒญ, ์๋ต ๋ฉ์์ง ์์ฒด๋ฅผ ์ดํดํด๋ฉด ์ข๋ค.     
     
์ฐธ๊ณ ๋ก, `HttpServletRequest` ๊ฐ์ฒด๋ ์ถ๊ฐ๋ก ์ฌ๋ฌ๊ฐ์ง ๋ถ๊ฐ๊ธฐ๋ฅ๋ ํจ๊ป ์ ๊ณตํ๋ค.   
    
1. ์์ ์ ์ฅ์ ๊ธฐ๋ฅ     
2. ์ธ์ ๊ด๋ฆฌ ๊ธฐ๋ฅ        

### ๐ ์์ ์ ์ฅ์ ๊ธฐ๋ฅ    
ํด๋น HTTP ์์ฒญ์ด ์์๋ถํฐ ๋๋  ๋ ๊น์ง ์ ์ง๋๋ ์์ ์ ์ฅ์ ๊ธฐ๋ฅ์ ๊ฐ์ง๊ณ  ์๋ค.  
์ฌ์ฉ ๋ฐฉ๋ฒ์ `Session` ๊ณผ ๋น์ทํ๋ค.   
        
* **์ ์ฅ :** request.setAttribute(name, value)    
* **์กฐํ :** request.getAttribute(name)    
           
### ๐ ์ธ์ ๊ด๋ฆฌ ๊ธฐ๋ฅ  
`request`๋ก๋ถํฐ ์ธ์ ์์ฑ๊ณผ ๊ด๋ จ๋ ๊ธฐ๋ฅ์ ์ ๊ณตํด์ค๋ค.   
   
```java
request.getSession(create: true)
```
`request.getSession()`์ ๊ธฐ์กด์ ์ด๋ฏธ ์ฐ๊ฒฐ๋ ์ธ์์ด ์๋ค๋ฉด ํด๋น ์ธ์์ ๋ฐํํด์ค๋ค.  
๋ง์ฝ, ์์ผ๋ฉด ์๋์ ๊ฐ์ ๋์์ ํ๋ค.   

* `request.getSession(true);` : ์๋ก ๋ฐํ 
* `request.getSession(false);` : null ๋ฐํ 

# ๐ HttpServletRequest - ๊ธฐ๋ณธ ์ฌ์ฉ๋ฒ
```java
    @WebServlet(name = "requestHeaderServlet", urlPatterns = "/request-header")
    public class RequestHeaderServlet extends HttpServlet {
        @Override
        protected void service(HttpServletRequest request, HttpServletResponse
                response)
                throws ServletException, IOException {
            printStartLine(request);
            printHeaders(request);
            printHeaderUtils(request);
            printEtc(request);
            response.getWriter().write("ok");
        }
    }
```
## ๐ START LINE 
```java
    private void printStartLine(HttpServletRequest request) {
        // http://localhost:8080/request-header?username=hello
        System.out.println("--- REQUEST-LINE - start ---");
        System.out.println("request.getMethod() = " + request.getMethod());             // GET
        System.out.println("request.getProtocal() = " + request.getProtocol());         // HTTP / 1.1
        System.out.println("request.getScheme() = " + request.getScheme());             // http
        
        System.out.println("request.getRequestURL() = " + request.getRequestURL());     // http://localhost:8080/request-header?username=hello 
        System.out.println("request.getRequestURI() = " + request.getRequestURI());     // /request-header 
        System.out.println("request.getQueryString() = " + request.getQueryString());   // username=hello
        System.out.println("request.isSecure() = " + request.isSecure());               // https ์ฌ์ฉ ์ ๋ฌด
        System.out.println("--- REQUEST-LINE - end ---");
        System.out.println();
    }
```
```sh
--- REQUEST-LINE - start ---
request.getMethod() = GET
request.getProtocal() = HTTP/1.1
request.getScheme() = http
request.getRequestURL() = http://localhost:8080/request-header
request.getRequestURI() = /request-header
request.getQueryString() = username=hello
request.isSecure() = false
--- REQUEST-LINE - end ---
```

## ๐ HEADER

```java
    //Header ๋ชจ๋  ์ ๋ณด
    private void printHeaders(HttpServletRequest request) {
        System.out.println("--- Headers - start ---");
/*
        Enumeration<String> headerNames = request.getHeaderNames();
        while (headerNames.hasMoreElements()) {
            String headerName = headerNames.nextElement();
            System.out.println(headerName + ": " + request.getHeader(headerName));
        }
*/
        request.getHeaderNames().asIterator()
                .forEachRemaining(headerName -> System.out.println(headerName + ":
                        " + request.getHeader(headerName)));
                        System.out.println("--- Headers - end ---");
        System.out.println();
    }
```
```sh
--- Headers - start ---
host: localhost:8080
connection: keep-alive
cache-control: max-age=0
sec-ch-ua: "Chromium";v="88", "Google Chrome";v="88", ";Not A Brand";v="99"
sec-ch-ua-mobile: ?0
upgrade-insecure-requests: 1
user-agent: Mozilla/5.0 (Macintosh; Intel Mac OS X 11_2_0) AppleWebKit/537.36
(KHTML, like Gecko) Chrome/88.0.4324.150 Safari/537.36
accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/
webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.9
sec-fetch-site: none
sec-fetch-mode: navigate
sec-fetch-user: ?1
sec-fetch-dest: document
accept-encoding: gzip, deflate, br
accept-language: ko,en-US;q=0.9,en;q=0.8,ko-KR;q=0.7
--- Headers - end ---
```

## ๐ HEADER ํธ๋ฆฌํ ์กฐํ
```java
    //Header ํธ๋ฆฌํ ์กฐํ
    private void printHeaderUtils(HttpServletRequest request) {
        System.out.println("--- Header ํธ์ ์กฐํ start ---");
        System.out.println("[Host ํธ์ ์กฐํ]");
        System.out.println("request.getServerName() = " +
                request.getServerName()); //Host ํค๋
        System.out.println("request.getServerPort() = " +
                request.getServerPort()); //Host ํค๋
        System.out.println();
        System.out.println("[Accept-Language ํธ์ ์กฐํ]");
        request.getLocales().asIterator()
                .forEachRemaining(locale -> System.out.println("locale = " +
                        locale));
        System.out.println("request.getLocale() = " + request.getLocale());
        System.out.println();
        System.out.println("[cookie ํธ์ ์กฐํ]");
        if (request.getCookies() != null) {
            for (Cookie cookie : request.getCookies()) {
                System.out.println(cookie.getName() + ": " + cookie.getValue());
            }
        }
        System.out.println();
        System.out.println("[Content ํธ์ ์กฐํ]");
        System.out.println("request.getContentType() = " +
                request.getContentType());
        System.out.println("request.getContentLength() = " +
                request.getContentLength());
        System.out.println("request.getCharacterEncoding() = " +
                request.getCharacterEncoding());
        System.out.println("--- Header ํธ์ ์กฐํ end ---");
        System.out.println();
    }
```
```sh
--- Header ํธ์ ์กฐํ start ---
[Host ํธ์ ์กฐํ]
request.getServerName() = localhost
request.getServerPort() = 8080
[Accept-Language ํธ์ ์กฐํ]
locale = ko
locale = en_US
locale = en
locale = ko_KR
request.getLocale() = ko
[cookie ํธ์ ์กฐํ]
[Content ํธ์ ์กฐํ]
request.getContentType() = null
request.getContentLength() = -1
request.getCharacterEncoding() = UTF-8
--- Header ํธ์ ์กฐํ end ---
```

## ๐ ๊ธฐํ ์กฐํ
```java
    //๊ธฐํ ์ ๋ณด
    private void printEtc(HttpServletRequest request) {
        System.out.println("--- ๊ธฐํ ์กฐํ start ---");
        System.out.println("[Remote ์ ๋ณด]");
        System.out.println("request.getRemoteHost() = " +
                request.getRemoteHost()); //
        System.out.println("request.getRemoteAddr() = " +
                request.getRemoteAddr()); //
        System.out.println("request.getRemotePort() = " +
                request.getRemotePort()); //
        System.out.println();
        System.out.println("[Local ์ ๋ณด]");
        System.out.println("request.getLocalName() = " +
                request.getLocalName()); //
        System.out.println("request.getLocalAddr() = " +
                request.getLocalAddr()); //
        System.out.println("request.getLocalPort() = " +
                request.getLocalPort()); //
        System.out.println("--- ๊ธฐํ ์กฐํ end ---");
        System.out.println();
    }
```
```sh
--- ๊ธฐํ ์กฐํ start ---
[Remote ์ ๋ณด]
request.getRemoteHost() = 0:0:0:0:0:0:0:1
request.getRemoteAddr() = 0:0:0:0:0:0:0:1
request.getRemotePort() = 54305
[Local ์ ๋ณด]
request.getLocalName() = localhost
request.getLocalAddr() = 0:0:0:0:0:0:0:1
request.getLocalPort() = 8080
--- ๊ธฐํ ์กฐํ end ---
```
   
**์ฐธ๊ณ **
> ๋ก์ปฌ์์ ํ์คํธํ๋ฉด IPv6 ์ ๋ณด๊ฐ ๋์ค๋๋ฐ, IPv4 ์ ๋ณด๋ฅผ ๋ณด๊ณ  ์ถ์ผ๋ฉด ๋ค์ ์ต์์ VM options์๋ฃ์ด์ฃผ๋ฉด ๋๋ค.   
> -Djava.net.preferIPv4Stack=true      
