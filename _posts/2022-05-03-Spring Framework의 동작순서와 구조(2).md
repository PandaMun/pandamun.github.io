---
layout: post
title: Spring Framework의 동작순서와 구조(2)
subtitle: Spring Framework의 동작순서와 구조(2)
category: web
---

# Spring MVC 구조

![SpringMVC.png](/img/post/SpringMVC.png))

**Spring MVC 구조**

DispatcherServlet  : FrontController패턴를 구현한 FrontController

DispatcherServlet(org.springframework.web.servlet.DispatcherServlet)은 스프링 MVC의 핵심입니다. DispatcherServlet도 부모 클래스에서 ‘HttpServlet’을 상속 받아서 사용하고, 서블릿으로 동작한다.

- DispatcherServlet → FrameworkServlet → HttpServletBean → HttpServlet

스프링 부트는 DispatcherServlet을 서블릿으로 자동으로 등록하면서 **모든경로(’urlParttern = “ / ”)**에 대해서 매핑합니다.

- 참고 : 더 자세한 경로를 매핑한 서블릿이 우선순위가 더 높습니다. 그래서 기존에 등록한 서블릿도 함께 동작합니다.

**요청 흐름**

- 서블릿이 호출되면 HttpServlet이 제공하는 service() 가 호출됩니다.
- 스프링 MVC는 DispatcherServlet의 부모인 FrameworkServlet에서 service()를 오버라이드 해두었습니다.
- FrameworkServlet.service()를 시작으로 여러 메서드가 호출되면서 DispatcherServlet.doDispatch()가 호출됩니다.

doDispatch()

```java
/* doDispatcher() */
protected void doDispatch(HttpServletRequest request, HttpServletResponse
  response) throws Exception {
    HttpServletRequest processedRequest = request;
    HandlerExecutionChain mappedHandler = null;
    ModelAndView mv = null;
		// 1. 핸들러 조회
		mappedHandler = getHandler(processedRequest);
		if (mappedHandler == null) {
			    noHandlerFound(processedRequest, response);
					return;
				}

		//2.핸들러 어댑터 조회-핸들러를 처리할 수 있는 어댑터
		HandlerAdapter ha = getHandlerAdapter(mappedHandler.getHandler());

		// 3. 핸들러 어댑터 실행 -> 4. 핸들러 어댑터를 통해 핸들러 실행 -> 5. ModelAndView 반환
		mv = ha.handle(processedRequest, response, mappedHandler.getHandler());
	  processDispatchResult(processedRequest, response,
																mappedHandler, mv, dispatchException);
		}

/* processDispatchResult() */
private void processDispatchResult(HttpServletRequest request,
																	HttpServletResponse response,
																	HandlerExecutionChain mappedHandler,
																	ModelAndView mv,
																	Exception exception) throws Exception {
		// 뷰 렌더링 호출
		render(mv, request, response);
		}

/* render() */
protected void render(ModelAndView mv, HttpServletRequest request,
												HttpServletResponse response) throws Exception {
		 View view;
		 String viewName = mv.getViewName();
		 //6. 뷰 리졸버를 통해서 뷰 찾기,7.View 반환
	   view = resolveViewName(viewName, mv.getModelInternal(), locale, request);
		// 8. 뷰 렌더링
		view.render(mv.getModelInternal(), request, response);
		}
```

![스크린샷 2022-05-01 오후 10.13.18.png](Spring%20MVC%20%E1%84%80%E1%85%AE%E1%84%8C%E1%85%A9%202cb2acf51d3c459f924d6012af710463/%E1%84%89%E1%85%B3%E1%84%8F%E1%85%B3%E1%84%85%E1%85%B5%E1%86%AB%E1%84%89%E1%85%A3%E1%86%BA_2022-05-01_%E1%84%8B%E1%85%A9%E1%84%92%E1%85%AE_10.13.18.png)

**동작순서**

1. 핸들러 조회 : 핸들러 매핑을 통해 요청 URL에 매핑된 핸들러(컨트롤러)를 조회한다.
2. 핸들러 어댑터 조회 : 핸들러를 실행할수 있는 핸들러 어댑터를 조회한다.
3. 핸들러 어댑터 실행 : 핸들러 어댑터를 실행한다.
4. ModelAndView 반환 : 핸들러 어댑터는 핸들러가 변환하는 정보를 ModelAndView로 변환 해서 반환한다.
5. ViewResolver 호출 : 뷰 리졸버를 찾고 실행한다.
6. View 반환 : 뷰 리졸버는 뷰의 논리 이름을 물리 이름으로 바꾸고, 렌더링 역할을 담당하는 뷰 객체를 반환한다.

그전에는 HttpServlet을 상속받아 원하는 기능으로 변경하고 확장하였지만 스프링 MVC에서는 웬만한 기능이 구현되어 있기 때문에 코드의 변경없이 원하는 기능을 변경하거나 확장할수 있습니다.

**주요 인터페이스 목록**

- 핸들러 매핑: org.springframework.web.servlet.HandlerMapping
- 핸들러 어댑터: org.springframework.web.servlet.HandlerAdapter
- 뷰 리졸버: org.springframework.web.servlet.ViewResolver
- 뷰: org.springframework.web.servlet.View

### HandlerMapping and HandlerAdapter

- HandlerMapping(핸들러 매핑)
    - 핸들러 매핑에서 이 컨트롤러를 찾을수 있어야합니다.
- HandlerAdapter(핸들러 어댑터)
    - 핸들러 매핑을 통해서 찾은 핸들러를 실행할수 있는 핸들러 어댑터가 필요하다.

스프링은 이미 필요한 핸들러 패핑과 핸들러 어댑터를 대부분 구현해 두었습니다.

개발자가 직접 핸들러 매핑과 핸들러 어댑터를 만드는 일은 거의 없습니다.

**스프링 부트가 자동 등록하는 핸들러 매핑과 핸들러 어댑터**

HandlerMapping

- 0 순위 : RequestMappingHandlerMapping : 어노테이션 기반의 컨트롤러인 @RequestMapping에서 사용
- 1 순위 : BeanNameUrlHandlerMapping : 스프링 빈의 이름으로 핸들러를 찾습니다.
    - ex)  @Component(”/springmvc/controller”)

HandlerAdapter

- 0 순위 : RequestMappingHandlerAdapter : 어노테이션 기반의 컨트롤러인 @RequestMapping에서 사용
- 1 순위 : HttpRequestHandlerAdapter : HttpRequestHandler 처리
- 2 순위 : SimpleControllerHandlerAdapter : Controller 인터페이스(애노테이션X, 과거에 사용) 처리

예시)

```java
@Component("/springmvc/old-controller")
public class OldController implements Controller {
    @Override
    public ModelAndView handleRequest(HttpServletRequest request, HttpServletResponse response) throws Exception {
        System.out.println("OldController.handleRequest");
        return null;
    }
}
```

이경우 OldController는 "/springmvc/old-controller"이름을 가진 스프링 빈 객체로 등록이 되며

1. Handler Mapping으로 핸들러 조회
    1. HandlerMapping을 순서대로 실행해서 핸들러를 찾습니다.
    2. 이경우 Bean의 이름으로 핸들러를 찾아야하기 때문에 그대로 빈 이름으로 핸들러를 찾아주는 BeanNameUrlHandlerMapping이 실행에 성공하고 핸들러인 OldController를 반환한다.
2. Handler Adapter 조회
    1. HandlerAdapter의 supports()를 순서대로 호출합니다.
    2. SimpleControllerHandlerAdapter가 Controller 인터페이스를 지원하므로 대상이된다.
3. Handler Adpater 실행
    1. DispatcherServlet이 조회한 SimpleControllerHandlerAdapter을 실행하면서 핸들러 정보도 함께 넘겨줍니다.
    2. SimplerControllerHandlerAdapter는 핸들러인 OldController를 내부에서 실행하고, 그결과를 반환한다.

정리 - OldController  핸들러 매핑, 어댑터

OldController 를 실행하면서 사용된 객체는 다음과 같다.

- HandlerMapping = BeanNameUrlHandlerMapping
- HandlerAdapter = SimpleControllerHandlerAdapte

@RequestMapping

가장 우선순위가 높은 핸들러 매핑과 핸들러 어댑터는 RequestMappingHandlerMapping,  RequestMappingHandlerAdapter입니다. @RequestMapping의 앞글자를 따서 만든 이름으로써 현재 99.9퍼센트로 이 방식의 컨트롤러를 사용합니다.

### View Resolver

ViewResovle는 Controller가 View의 논리적 이름을 반환하여 줄때 View 이름으로부터 사용할 오브젝트를 매핑하여 반환하는 역할을 하고 있습니다.

스프링 부트가 자동 등록하는 ViewResolver

1.  BeanNameViewResolver : 빈 이름으로 뷰를 찾아서 반환한다 (예 : 엑셀 파일 생성 기능에 사용)
2. InternalResourceViewResolver : properties에 저장한 prefix와 suffix을 가지고 처리할수 있는 뷰를 반환합니다.

예제

```java

/*
prefix : "/WEB-INF/views/"
suffix : ".jsp"
*/
@Controller
  public class SpringMemberFormControllerV1 {
      @RequestMapping("/springmvc/v1/members/new-form")
      public ModelAndView process() {
          return new ModelAndView("new-form");
      }
          }
```

1. 핸들러 어댑터 호출

핸들러 어댑터를 통해 new-form 이라는 논리 뷰 이름을 획득한다.

2. ViewResolver 호출

- new-form 이라는 뷰 이름으로 viewResolver를 순서대로 호출한다.
- BeanNameViewResolver 는 new-form 이라는 이름의 스프링 빈으로 등록된 뷰를 찾아야 하는데 없다.
- InternalResourceViewResolver 가 호출된다.

3. InternalResourceViewResolver

- 이 뷰 리졸버는 InternalResourceView 를 반환한다.

4. 뷰 - InternalResourceView

- InternalResourceView 는 JSP처럼 포워드 forward() 를 호출해서 처리할 수 있는 경우에 사용한다.

5. view.render()

- view.render() 가 호출되고 InternalResourceView 는 forward() 를 사용해서 JSP를 실행한다.

**참고**

InternalResourceViewResolver 는 만약 JSTL 라이브러리가 있으면 InternalResourceView 를 상속받은 JstlView 를 반환한다. JstlView 는 JSTL 태그 사용시 약간의 부가 기능이 추가된다.

**참고**

다른 뷰는 실제 뷰를 렌더링하지만, JSP의 경우 forward() 통해서 해당 JSP로 이동(실행)해야 렌더링이 된다. JSP를 제외한 나머지 뷰 템플릿들은 forward() 과정 없이 바로 렌더링 된다.

**참고**

Thymeleaf 뷰 템플릿을 사용하면 ThymeleafViewResolver 를 등록해야 한다. 최근에는 라이브러리만 추가하면 스프링 부트가 이런 작업도 모두 자동화해준다.

## Reference
- [스프링 MVC 1편 - 김영한](https://www.inflearn.com/course/%EC%8A%A4%ED%94%84%EB%A7%81-mvc-1)

### 목차
- [Spring Framework의 동작순서와 구조(1)][https://pandamun.github.io/2022-03-21-Spring-Framework%EC%9D%98-%EB%8F%99%EC%9E%91%EC%88%9C%EC%84%9C%EC%99%80-%EA%B5%AC%EC%A1%B0(1)/]
- Spring Framework의 동작순서와 구조(2)
