# FrontController

작성자: 전영태
날짜: 2025년 6월 5일
카테고리: JSP & Servlet
주차: 3주차
담당자: 전영태

## **1. Front Controller**

- **모든 요청을 하나의 서블릿(FrontControllerServlet)이 받아서** 분기 처리하는 구조
- 웹 애플리케이션의 **진입점(Entry Point)** 역할
- **공통 로직(로그인, 인코딩, 예외처리)**을 한 곳에 모아 유지보수성 향상

---

## **2. 구조 이해**

```
[클라이언트 요청]
     ↓
[FrontControllerServlet]
     ↓
[get/post Map에서 요청 처리 객체(Command) 찾기]
     ↓
[Controller 메서드 실행 → 처리]
     ↓
[View 이름 반환 → forward or redirect]
```

---

## **3. 핵심 설계 포인트**

| **구성 요소** | **설명** |
| --- | --- |
| **DispatcherServlet** | 요청을 받고 실행 흐름을 분기하는 서블릿 (부모 역할) |
| **FrontControllerServlet** | 애플리케이션 별로 실제 요청 처리를 등록하는 자식 서블릿 |
| **Command 인터페이스** | 모든 요청 처리를 execute()로 통일 |
| **컨트롤러 클래스** (ex: TodoController) | 각 요청에 대한 로직 수행 (getList, postCreate 등) |
| **JSP 뷰 파일** | 클라이언트에게 보여질 화면 반환 |

---

## **4. 요청 흐름 요약**

| **단계** | **설명** |
| --- | --- |
| ① 요청 수신 | URL: /todo/list, /todo/create 등 |
| ② URI 분석 | requestURI - contextPath 로 경로 추출 |
| ③ 요청 맵핑 | getMap, postMap에 등록된 메서드 실행 |
| ④ 처리 결과 반환 | 문자열 viewName 반환 (todo/list, redirect:/...) |
| ⑤ 뷰 처리 | prefix/suffix 붙여 JSP로 forward 또는 redirect 처리 |

---

## **5. 예제 코드**

- **DispatcherServlet.java (부모)**
    
    ```
    public class DispatcherServlet extends HttpServlet {
        Map<String, Command> getMap = new HashMap<>();
        Map<String, Command> postMap = new HashMap<>();
        String prefix = "/WEB-INF/views/";
        String suffix = ".jsp";
    
        public void init() {
            createMap(getMap, postMap);
        }
    
        protected void createMap(Map<String, Command> getMap, Map<String, Command> postMap) {}
    
        protected void doGet(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException {
            Command command = getMap.get(getCommandName(req));
            execute(command, req, resp);
        }
    
        protected void doPost(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException {
            Command command = postMap.get(getCommandName(req));
            execute(command, req, resp);
        }
    
        private String getCommandName(HttpServletRequest req) {
            return req.getRequestURI().substring(req.getContextPath().length());
        }
    
        private void execute(Command command, HttpServletRequest req, HttpServletResponse resp)
                throws IOException, ServletException {
            if (command == null) {
                req.getRequestDispatcher(prefix + "404" + suffix).forward(req, resp);
                return;
            }
    
            String viewName = command.execute(req, resp);
            if (viewName.startsWith("redirect:")) {
                resp.sendRedirect(viewName.substring("redirect:".length()));
            } else {
                req.getRequestDispatcher(prefix + viewName + suffix).forward(req, resp);
            }
        }
    }
    ```
    

---

- **FrontControllerServlet.java (자식)**
    
    ```
    @WebServlet("/")
    public class FrontControllerServlet extends DispatcherServlet {
        @Override
        protected void createMap(Map<String, Command> getMap, Map<String, Command> postMap) {
            TodoController todo = new TodoController();
    
            getMap.put("/todo/list", todo::getList);
            getMap.put("/todo/view", todo::getView);
            getMap.put("/todo/create", todo::getCreate);
            postMap.put("/todo/create", todo::postCreate);
        }
    }
    ```
    

---

- **Command 인터페이스**
    
    ```
    public interface Command {
        String execute(HttpServletRequest req, HttpServletResponse resp) throws IOException, ServletException;
    }
    ```
    

---

## **6. 특징**

| **장점** | **단점** |
| --- | --- |
| 모든 흐름을 한 곳에서 관리 → 유지보수 편리 | 초심자에겐 구조가 복잡해 보일 수 있음 |
| URL 매핑 → 메서드 호출 → 뷰 전달 분리 | 구조가 무너지면 전체 로직 흐름도 흔들릴 수 있음 |
| 공통 처리(인코딩, 로그인 검사 등) 통합 | URL과 Command 매핑 실수가 있으면 404 |

---

## **+) 전체 흐름 구조를 정리한다면..**

```
[ 1. 사용자의 웹 요청 ]
         │
         ▼
[ 2. FrontControllerServlet ]
   (요청을 전부 이곳으로 받음)
         │
         ▼
[ 3. 요청 경로 추출 ]
   ex) /todo/list, /todo/create
         │
         ▼
[ 4. Command 매핑(Map<String, Command>) 조회 ]
         │
         ▼
[ 5. Command 객체의 execute() 실행 ]
         │
         ▼
[ 6. viewName 반환 ("todo/list" 또는 "redirect:/todo/list") ]
         │
         ▼
[ 7. View 처리 ]
   └─ forward: /WEB-INF/views/todo/list.jsp
   └─ redirect: /todo/list
         │
         ▼
[ 8. 사용자에게 응답 ]
```

---

## **흐름 키워드**

| **단계** | **구성 요소** | **역할** |
| --- | --- | --- |
| 1 | 사용자 요청 | ex) /todo/create, /todo/list |
| 2 | FrontControllerServlet | 모든 요청의 진입점 |
| 3 | URI 파싱 | ContextPath 제거 → 경로 추출 |
| 4 | Map 조회 | 요청에 맞는 Command 객체 찾기 |
| 5 | Command 실행 | 요청 처리 로직 수행 |
| 6 | viewName 리턴 | 뷰 이름 문자열 반환 |
| 7 | 뷰 이동 | forward or redirect 처리 |
| 8 | 최종 응답 | JSP 결과 화면 사용자에게 전달 |

---

## **시나리오 예시**

> 사용자가 /todo/list로 접속했을 때:
> 
1. GET /todo/list 요청
2. FrontControllerServlet에서 "/todo/list" 추출
3. getMap.get("/todo/list") → TodoController.getList()
4. getList() 내부에서 할 일 목록 조회
5. "todo/list" 반환
6. View prefix/suffix 붙임 → /WEB-INF/views/todo/list.jsp
7. JSP로 forward
8. 사용자가 화면에서 할 일 목록을 확인함

---