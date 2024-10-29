# 一、

Spring 是一个开源的 Java 应用框架，它提供了一种轻量级的、非侵入式的编程模型，用于构建企业级应用程序。Spring 提供了众多功能和组件，包括依赖注入、面向切面编程、事务管理、Web 开发等，使开发者能够更轻松地构建可扩展、可维护和松耦合的应用程序。

下面是对 Spring 的详细描述，并附带一个简单的示例代码：

1. 依赖注入（Dependency Injection）：
   Spring 的核心特性之一是依赖注入，它通过将对象之间的依赖关系外部化，实现了松耦合的设计。在 Spring 中，你可以使用注解或 XML 配置来声明依赖关系，并由 Spring 容器负责将相关对象注入到需要它们的地方。

2. 面向切面编程（Aspect-Oriented Programming，AOP）：
   Spring 提供了 AOP 的支持，使开发者能够将横切关注点（如日志记录、事务管理等）与核心业务逻辑分离。通过使用切面、连接点和通知等概念，Spring AOP 可以在不修改原始代码的情况下，动态地将切面逻辑织入到应用程序中。

3. 容器（Container）：
   Spring 容器是 Spring 框架的核心部分，它负责创建和管理应用程序中的对象。Spring 容器根据配置文件或注解，创建对象、注入依赖关系，并提供对这些对象的访问。常见的 Spring 容器有 ApplicationContext 和 BeanFactory。

4. 声明式事务管理（Declarative Transaction Management）：
   Spring 提供了声明式事务管理的功能，使开发者能够以声明的方式管理数据库事务。通过将事务逻辑与业务逻辑分离，并使用事务切面，Spring 能够自动管理事务的开始、提交或回滚，并提供更好的事务控制和异常处理。

5. Spring MVC：
   Spring MVC 是 Spring 框架的 Web 模块，它提供了用于开发 Web 应用程序的 MVC（Model-View-Controller）架构。Spring MVC 基于前端控制器（Front Controller）模式，通过控制器、视图解析器和处理器映射器等组件，帮助开发者构建灵活且可扩展的 Web 应用程序。

下面是一个简单的示例代码，展示了 Spring 的依赖注入和 Spring MVC 的基本用法：

```java
// 定义一个服务接口
public interface GreetingService {
    String greet();
}

// 实现服务接口
public class GreetingServiceImpl implements GreetingService {
    @Override
    public String greet() {
        return "Hello, Spring!";
    }
}

// 定义一个控制器类
@Controller
public class GreetingController {
    @Autowired
    private GreetingService greetingService;

    @RequestMapping("/greet")
    @ResponseBody
    public String greet() {
        return greetingService.greet();
    }
}

// 配置 Spring 容器
@Configuration
@ComponentScan("com.example")
@EnableWebMvc
public class AppConfig extends WebMvcConfigurerAdapter {
    @Bean
    public GreetingService greetingService() {
        return new GreetingServiceImpl();
    }

    @Override
    public void configureDefaultServletHandling(DefaultServletHandlerConfigurer configurer) {
        configurer.enable();
    }
}

// 启动 Spring 应用程序
public class Application {
    public static void main(String[] args) {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);
        GreetingController greetingController = context.getBean(GreetingController.class);
        System.out.println(greetingController.greet());
        context.close();
    }
}
```

在上述示例中，我们首先定义了一个 `GreetingService` 接口和实现类 `GreetingServiceImpl`，它们代表了一个简单的服务和具体实现。接着，我们创建了一个 `GreetingController` 控制器类，并在其中使用 `@Autowired` 注解将 `GreetingService` 作为依赖注入到控制器中。

然后，我们通过 `@Configuration` 注解定义了一个配置类 `AppConfig`，其中使用 `@ComponentScan` 扫描了 `com.example` 包下的组件，并使用 `@EnableWebMvc` 启用了 Spring MVC。在配置类中，我们还通过 `@Bean` 注解定义了 `GreetingService` 的实例。

最后，在 `Application` 类中，我们使用 `AnnotationConfigApplicationContext` 创建了 Spring 容器，并从容器中获取 `GreetingController` 的实例，并调用其 `greet()` 方法进行输出。

这只是一个简单的示例，展示了 Spring 的基本用法和特性。你可以根据实际需求和复杂度，进一步深入学习和探索 Spring 框架的更多功能和组件。





在使用 Maven 等构建工具管理项目依赖时，可以通过以下方式指定 Spring 的依赖坐标：

Maven 依赖坐标：
```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.3.9</version>
</dependency>
```

在上述示例中，我们以 Spring 的 `spring-context` 模块为例，指定了版本号为 `5.3.9`。你可以根据项目需要选择合适的 Spring 模块和对应的版本号，然后在项目的构建配置文件中添加这些依赖坐标。

# 二、spring中为什么bean默认为单例

在Spring框架中，Bean默认为单例是出于性能和资源管理的考虑。当一个Bean被定义为单例时，Spring容器会在第一次创建Bean的实例后将其缓存起来，并在后续请求该Bean时直接返回缓存的实例，而不是每次都创建新的实例。这样做可以带来以下好处：

1. **资源节约**：单例模式避免了频繁创建和销毁对象的开销，尤其是那些初始化较为复杂或者占用大量资源的对象。如果每次请求都创建一个新的实例，会增加系统的负担，降低性能。

2. **全局状态共享**：在某些情况下，需要多个组件或类共享相同的状态。通过单例模式创建的Bean，可以确保在整个应用程序范围内只有一个实例，这样所有引用该Bean的组件都会共享同一个状态，避免了因为状态不一致而导致的问题。

3. **依赖注入的一致性**：在依赖注入场景下，默认为单例可以确保所有使用该Bean的地方都引用同一个实例，不会出现因为多个实例而导致的依赖注入问题。

虽然单例模式有很多好处，但也有一些需要注意的地方：

1. **线程安全**：由于单例Bean在整个应用程序中只有一个实例，因此需要特别注意确保该Bean的线程安全性，避免多线程并发访问时出现问题。

2. **非延迟加载**：默认单例Bean会在容器启动时创建并初始化，无论是否真正被使用。如果某些Bean在应用启动时初始化会导致性能问题，可以考虑使用懒加载来延迟初始化。

如果在某些情况下需要非单例的Bean，Spring也提供了其他作用域，例如原型（prototype）作用域，它会在每次请求时都创建一个新的实例。你可以通过在Bean的声明中明确指定作用域来实现不同的实例化行为。例如：

```xml
<bean id="myBean" class="com.example.MyBean" scope="prototype"/>
```

总的来说，**Spring默认将Bean定义为单例是为了提高性能和方便管理**，但在设计应用程序时，仍需根据具体情况选择合适的Bean作用域。

