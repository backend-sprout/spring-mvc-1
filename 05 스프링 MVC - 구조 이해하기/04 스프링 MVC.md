μ€νλ§ MVC 
==============    
# π μ€νλ§ MVC - μμνκΈ°   
## π @RequestMapping      
   
* **RequestMappingHandlerMapping**        
* **RequestMappingHandlerAdapter**        
    
μ€νλ§ MVCμμ κ°μ₯ μ°μ μμκ° λμ `νΈλ€λ¬ λ§€ν`κ³Ό `νΈλ€λ¬ μ΄λν°`λ      
**`RequestMappingHandlerMapping`, `RequestMappingHandlerAdapter`** μ΄λ€.   
     
μ΄λ€μ, **@RequestMapping** κΈ°λ°μ μ»¨νΈλ‘€λ¬λ₯Ό μ§μνλ νΈλ€λ¬ λ§€νκ³Ό μ΄λν°λ‘       
μ€λ¬΄μμλ 99.9% μ΄ λ°©μμ μ»¨νΈλ‘€λ¬λ₯Ό μ¬μ©νλ€.       
        
```java
@Controller
public class SpringMemberFormControllerV1 {   

    @RequestMapping("/springmvc/v1/members/new-form")
    public ModelAndView process() {
        return new ModelAndView("new-form");
    }
    
}
```        
* **@Controller** 
    * μ€νλ§μ΄ μλμΌλ‘ μ€νλ§ λΉμΌλ‘ λ±λ‘νλ€.     
      (λ΄λΆμ @Component μ λΈνμ΄μμ΄ μμ΄μ μ»΄ν¬λνΈ μ€μΊμ λμμ΄ λ¨)    
    * μ€νλ§ MVCμμ μ λΈνμ΄μ κΈ°λ° μ»¨νΈλ‘€λ¬λ‘ μΈμνλ€.    
* **@RequestMapping**   
    * μμ²­ μ λ³΄λ₯Ό λ§€ννλ€.      
    * ν΄λΉ URLμ΄ νΈμΆλλ©΄ μ΄ λ©μλκ° νΈμΆλλ€.           
    * μ λΈνμ΄μμ κΈ°λ°μΌλ‘ λμνκΈ° λλ¬Έμ, λ©μλμ μ΄λ¦μ μμλ‘ μ§μΌλ©΄ λλ€.       
* **ModelAndView :**         
    * λͺ¨λΈκ³Ό λ·° μ λ³΄λ₯Ό λ΄μμ λ°ννλ©΄ λλ€.      

**ν΄λμ€μ `@Controller`** λ₯Ό λΆμ΄κ³  **λ©μλ/ν΄λμ€ λ λ²¨μ `@RequestMapping`** μ μ μΈνλ©΄ λλ€.                            
μ΄ κ³Όμ μμ λ μ΄μ `FrontController`λ μμ±νμ§ μμλ λλ€.             

## π RequestMappingHandlerMapping   

```java
    @Override
    protected boolean isHandler(Class<?> beanType) {
        return (AnnotatedElementUtils.hasAnnotation(beanType, Controller.class) || 
                AnnotatedElementUtils.hasAnnotation(beanType, RequestMapping.class));
    }
```

**RequestMappingHandlerMapping**μ `isHandler()`λ₯Ό λ³΄λ©΄ μ¬λ°λμ μ μ μ μλλ°            
**μ€νλ§ λΉ μ€μμ `@RequestMapping` λλ `@Controller`κ° λΆμ ν΄λμ€μ λ§€ν μ λ³΄λ₯Ό μΈμνλ€.**   


**λ°©λ²1**
```java
@Component   
@RequestMapping
public class SpringMemberFormControllerV1 {  

    @RequestMapping("/springmvc/v1/members/new-form")
    public ModelAndView process() {
        return new ModelAndView("new-form");
    }
    
}
```  
     
**λ°©λ²2**   
```java
@RequestMapping
public class SpringMemberFormControllerV1 {  

    @RequestMapping("/springmvc/v1/members/new-form")
    public ModelAndView process() {
        return new ModelAndView("new-form");
    }
    
}
```
```java
@Configuration
public class TestConfiguration {
    
    @Bean
    TestController testController() {
        return new TestController();
    }
    
}
```
μ΄λ‘ μ μμ κ°μ λ°©λ²μΌλ‘λ λΉ λ±λ‘ν Controllerλ‘μ¨ μ¬μ©μ κ°λ₯νμ§λ§      
Best Practiceλ μλμ κ°μ΄ `@Controller`λ₯Ό μ¬μ©ν΄μ ν΄λμ€λ₯Ό κ°νΈνκ² λ§λλ κ²μ΄λ€.   
       
**Best Pratice**          
```java
@Controller
public class SpringMemberFormControllerV1 {   

    @RequestMapping("/springmvc/v1/members/new-form")
    public ModelAndView process() {
        return new ModelAndView("new-form");
    }
    
}
```       
```java
@Controller
public class SpringMemberSaveControllerV1 {
    
    private MemberRepository memberRepository = MemberRepository.getInstance();
    
    @RequestMapping("/springmvc/v1/members/save")
    public ModelAndView process(HttpServletRequest request, HttpServletResponse response) {
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));
        Member member = new Member(username, age);
        
        memberRepository.save(member);
        
        ModelAndView mv = new ModelAndView("save-result");
        mv.addObject("member", member);
        return mv;
    }
}
```

μ°Έκ³ λ‘, μ€νλ§μ΄ μ κ³΅νλ ModelAndView λ₯Ό ν΅ν΄ Model λ°μ΄ν°λ₯Ό μΆκ°ν  λλ    
addObject() λ₯Ό μ¬μ©νλ©΄ λλ€. μ΄ λ°μ΄ν°λ μ΄ν λ·°λ₯Ό λ λλ§ ν  λ μ¬μ©λλ€.  
```java
mv.addObject("member", member)
```
    
# π μ€νλ§ MVC - μ»¨νΈλ‘€λ¬ ν΅ν©        
RequestHandlerMappingμ `@RequestMapping`μ κΈ°μ€μΌλ‘λ§ λμμ νλ€.           
`@RequestMapping`μ μ£Όλ‘ λ©μλ λ¨μμ μ μ©λλλ°          
μ΄λ₯Ό νμ©νλ©΄ **νλμ μ»¨νΈλ‘€λ¬ ν΄λμ€μμ μ¬λ¬ `@RequestMapping` λ©μΈλλ₯Ό κ°μ§ μ μλ€.**        

```java
@Controller
@RequestMapping("/springmvc/v2/members")
public class SpringMemberControllerV2 {
    private MemberRepository memberRepository = MemberRepository.getInstance();

    @RequestMapping("/new-form")
    public ModelAndView newForm() {
        return new ModelAndView("new-form");
    }

    @RequestMapping("/save")
    public ModelAndView save(HttpServletRequest request, HttpServletResponse
            response) {
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));
        Member member = new Member(username, age);
        memberRepository.save(member);
        ModelAndView mav = new ModelAndView("save-result");
        mav.addObject("member", member);
        return mav;
    }

    @RequestMapping
    public ModelAndView members() {
        List<Member> members = memberRepository.findAll();
        ModelAndView mav = new ModelAndView("members");
        mav.addObject("members", members);
        return mav;
    }
}
```
ν΄λμ€ λ λ²¨μμλ `@RequestMapping("/springmvc/v2/members")`μ μ μΈν  μ μλλ°         
μ΄λ΄ κ²½μ° **ν΄λμ€ λ λ²¨ λ§€ν κ²½λ‘**μ **λ©μλ λ λ²¨ λ§€ν κ²½λ‘**κ° μ‘°ν©λλ€.      

**μ‘°ν© κ²°κ³Ό**    
* ν΄λμ€ λ λ²¨ `@RequestMapping("/springmvc/v2/members")`
    * λ©μλ λ λ²¨ `@RequestMapping("/new-form")` -> `/springmvc/v2/members/new-form` 
    * λ©μλ λ λ²¨ `@RequestMapping("/save")` -> `/springmvc/v2/members/save` 
    * λ©μλ λ λ²¨ `@RequestMapping` -> `/springmvc/v2/members`
  
# π μ€νλ§ MVC - μ€μ©μ μΈ λ°©μ
μ€νλ§ MVCλ κ°λ°μκ° νΈλ¦¬νκ² κ°λ°ν  μ μλλ‘ μ λ§μ νΈμ κΈ°λ₯μ μ κ³΅νλ€.       
νΉν **νΈλ€λ¬μ μ μλ λ©μλ νλΌλ―Έν° λ° λ°νκ°μ μ μ°νκ² μ€μ ν  μ μλλ‘ ν΄μ€λ€.**                        
μ°Έκ³ λ‘ SpringMVCμ νΈλ€λ¬λ, `@RequestMapping`μ΄ μ μλ λ©μλλ₯Ό μλ―Ένλ€.(Servletμμλ Controller μμ²΄)        
    
## π λ³κ²½ μ 
```java
@Controller
@RequestMapping("/springmvc/v2/members")
public class SpringMemberControllerV2 {
    private MemberRepository memberRepository = MemberRepository.getInstance();

    @RequestMapping("/new-form")
    public ModelAndView newForm() {
        return new ModelAndView("new-form");
    }

    @RequestMapping("/save")
    public ModelAndView save(HttpServletRequest request, HttpServletResponse
            response) {
        String username = request.getParameter("username");
        int age = Integer.parseInt(request.getParameter("age"));
        Member member = new Member(username, age);
        memberRepository.save(member);
        ModelAndView mav = new ModelAndView("save-result");
        mav.addObject("member", member);
        return mav;
    }

    @RequestMapping
    public ModelAndView members() {
        List<Member> members = memberRepository.findAll();
        ModelAndView mav = new ModelAndView("members");
        mav.addObject("members", members);
        return mav;
    }
}
```   

## π λ³κ²½ ν  
```java
@Controller
@RequestMapping("/springmvc/v3/members")
public class SpringMemberControllerV3 {
    
    private MemberRepository memberRepository = MemberRepository.getInstance();
    
    @GetMapping("/new-form")
    public String newForm() {
        return "new-form";
    }
    
    @PostMapping("/save")
    public String save(
            @RequestParam("username") String username,
            @RequestParam("age") int age, Model model) {
    
       Member member = new Member(username, age);
       memberRepository.save(member);
       model.addAttribute("member", member);
       return "save-result";
    }
    
    @GetMapping
    public String members(Model model) {
        List<Member> members = memberRepository.findAll();
        model.addAttribute("members", members);
        return "members";
    }
}
```
   
* **ModelAndView**      
    * μ€νλ§μ΄ μ κ³΅ν΄μ£Όλ ModelAndView κ°μ²΄λ₯Ό νλΌλ―Έν°λ‘ λ°κ±°λ λ°νκ°μΌλ‘ μ€μ ν  μ μλ€.     
* **Model**      
    * μ€νλ§μ΄ μ κ³΅ν΄μ£Όλ Model κ°μ²΄λ₯Ό νλΌλ―Έν°λ‘ λ°μ μ μλ€.         
    * μ£Όλ‘ `addAttribute("key", value);`λ₯Ό ν΅ν΄ κ°μ request λ²μλ‘ μ μ₯νλ€.
* **View**      
    * λ·° μΈν°νμ΄μ€λ₯Ό κ΅¬νν κ΅¬νμ²΄λ₯Ό νλΌλ―Έν°λ‘ λ°κ±°λ λ°νκ°μΌλ‘ μ€μ ν  μ μλ€.           
* **View μ΄λ¦μ String νμμΌλ‘ μ§μ  λ°ν**   
    * λ·°μ λΌλ¦¬ μ΄λ¦μ String νμμΌλ‘ μ§μ  λ°νν  μ μλ€.  
**@RequestParam μ¬μ©**     
    * HTTP μμ²­ νλΌλ―Έν°λ₯Ό **@RequestParam**μΌλ‘ λ°μ μ μλ€.         
    * `@RequestParam("username")`μ `request.getParameter("username")`μ κ±°μ κ°μ μ½λλ€.        
    * λ¬Όλ‘ , **GET μΏΌλ¦¬ νλΌλ―Έν°**, **POST Form λ°©μ**μ λͺ¨λ μ§μνλ€.        
* **@RequestMapping -> @GetMapping, @PostMapping**   
    * **@RequestMappingμ URLλ§ λ§€μΉ­νλ κ²μ΄ μλλΌ, HTTP Methodλ ν¨κ» κ΅¬λΆν  μ μλ€.**      
        * `@RequestMapping(value = "/new-form", method = RequestMethod.GET)`
    * μ€νλ§ 4.3λΆν° `@GetMapping` , `@PostMapping`μΌλ‘ λ νΈλ¦¬νκ² μ¬μ©ν  μ μλ€.   
    * μ°Έκ³ λ‘ `Get`, `Post`, `Put`, `Delete`, `Patch` λͺ¨λ μ λΈνμ΄μμ΄ μ€λΉλμ΄ μλ€.  
    
    








