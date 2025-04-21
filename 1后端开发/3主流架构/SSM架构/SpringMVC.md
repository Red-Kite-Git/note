# SpingMVC

## 一、Spring MVC 概述

### 1.Spring MVC 的架构原理  

### 2.Spring MVC 的核心组件（`DispatcherServlet`、`HandlerMapping`、`Controller` 等）

### 3.Spring MVC 的优势和应用场景

## 二、Spring MVC 配置

* `web.xml` 中 `DispatcherServlet` 的配置

* Spring MVC 配置文件spring-mvc.xml的配置

  * 视图解析器的配置（如 `InternalResourceViewResolver`）
  * 静态资源处理的配置

## 三、Controller 开发

* `@Controller` 注解的使用
* 请求映射注解（`@RequestMapping`、`@GetMapping`、`@PostMapping` 等）的使用
* 请求参数的绑定（基本数据类型、对象类型参数绑定）
* 响应数据的返回（视图跳转、JSON 数据返回等）

## 四、拦截器和异常处理

* 拦截器的概念和作用
* 自定义拦截器的开发和配置
* 全局异常处理器的开发和配置