HttpServletResponse
======================  
# π HttpServletResponse κ°μ
## π HttpServletResponse μ­ν   
`HttpServletResponse` λ μλ΅κ³Ό κ΄λ ¨λ μ²λ¦¬λ₯Ό ν  μ μμΌλ©°       
`μλ΅ λ©μμ§λ₯Ό μμ±` λ° `νΈμ κΈ°λ₯μ μ κ³΅`ν΄μ€λ€.        
   
**HTTP μλ΅ λ©μμ§ μμ±**  
* HTTP μλ΅μ½λ μ§μ  
* ν€λ μμ±
* λ°λ μμ±
   
**νΈμ κΈ°λ₯ μ κ³΅**
* Content-Type
* μΏ ν€  
* Redirect   

## π HttpServletResponse κΈ°λ³Έ μ¬μ©λ²   
### π μΌλ°μ μΈ μμ± λ°©λ² 
```java
@WebServlet(name = "responseHeaderServlet", urlPatterns = "/response-header")
public class ResponseHeaderServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //[status-line]
        response.setStatus(HttpServletResponse.SC_OK); //200
        
        //[response-headers]
        response.setHeader("Content-Type", "text/plain;charset=utf-8");
        response.setHeader("Cache-Control", "no-cache, no-store, mustrevalidate");
        response.setHeader("Pragma", "no-cache");
        response.setHeader("my-header", "hello");
        
        //[message body]
        PrintWriter writer = response.getWriter();
        writer.println("ok");
    }
}
```  
μλ΅μ μμ²­κ³Ό λ€λ₯΄κ² λ°μ΄ν°λ₯Ό λ§€ννλ μμμ μκ³        
κ°λ¨ν `μλ΅μ½λ` + `ν€λ μ€μ ` + `λ°λ(λ°μ΄ν°)`μ μμλ§ μννλ©΄ λλ€.          
λ¬Όλ‘ , `ν€λ μ€μ ` μμ `μΊμ μ€μ `, `μ½νμΈ  νμ μ€μ `μ κ°μ μ€μ λ€μ λ£μ μ μλ€.      
    
### π Header νΈμ λ©μλ   
```java
@WebServlet(name = "responseHeaderServlet", urlPatterns = "/response-header")
public class ResponseHeaderServlet extends HttpServlet {
    @Override
    protected void service(HttpServletRequest request, HttpServletResponse response) throws ServletException, IOException {
        //[status-line]
        response.setStatus(HttpServletResponse.SC_OK); //200
        
        //[Header νΈμ λ©μλ]
        content(response);
        cookie(response);
        redirect(response);
        
        //[message body]
        PrintWriter writer = response.getWriter();
        writer.println("ok");
    }
}

private void content(HttpServletResponse response) {
    //Content-Type: text/plain;charset=utf-8
    //Content-Length: 2
    
    response.setContentType("text/plain");
    response.setCharacterEncoding("utf-8");
    response.setContentLength(2);                     // κΈΈμ΄ λ§νΌμ λ°μ΄ν°λ§ μλ΅, μλ΅μ κΈΈμ΄μ λ§λ κ°μΌλ‘ μλ μμ±
}

private void cookie(HttpServletResponse response) {
    //Set-Cookie: myCookie=good; Max-Age=600;
    //response.setHeader("Set-Cookie", "myCookie=good; Max-Age=600");
    
    Cookie cookie = new Cookie("myCookie", "good");   // Key-Value λ°μ΄ν°  
    cookie.setMaxAge(600);                            // MaxAge 600μ΄
    response.addCookie(cookie);                       // μΏ ν€λ₯Ό λ΄λλ€.   
}

private void redirect(HttpServletResponse response) throws IOException {
    //Status Code 302
    //Location: /basic/hello-form.html
    //response.setStatus(HttpServletResponse.SC_FOUND);         // 302
    //response.setHeader("Location", "/basic/hello-form.html"); // 302 μλ΅μ½λμ κ°μ΄ μ¬μ©μ μ΄λ url νν  
    
    response.sendRedirect("/basic/hello-form.html");            // ν΄λΉ urlλ‘ λ¦¬λ€μ΄λ νΈλ₯Ό μν¨λ€.  
}
```
`Header νΈμ λ©μλ`λ₯Ό ν΅ν΄        
`setHeader()` λ§κ³  μλ―Έμλ μ΄λ¦μ λ©μλλ₯Ό μ¬μ©ν  μ μλ€.       
  

