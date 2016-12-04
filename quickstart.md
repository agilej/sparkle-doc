# Quick Start

## Choose Runtime

Sparkle framework is designed to be running on different containers or servers, for example you can choose run it on netty server or servlet container. We will choose servlet container in this quick start.

Add dependency to your maven's `pom.xml`

```xml
  <dependency>
    <groupId>org.agilej</groupId>
    <artifactId>sparkle-servlet</artifactId>
    <version>1.0-SNAPSHOT</version>
  </dependency>
```

Other dependency managers such as gradle:

```groovy
  compile "org.agilej:sparkle-servlet:1.0-SNAPSHOT"
```

## Define your first route

Create one `RouteModule` java class, define your route rules.

```java
  public class DemoRouteModule implements RouteModule {
    
      @Override
      public void config(Router router) {
          router.match("/user/{id}").to("user#show");
      }

  }
```

## Implementing controller

Implement `user` controller defined in above `RouteModule`

```java

@Controller("user")
public class UserController{
    
    public JSONModel show(@PathVariable("id") int userId) {
         return e -> {
                    e.expose(userId).withName("user_id");
                };
    }

}
```

## Run

Run with jetty from maven with command `mvn jetty:run`, then open `http://127.0.0.1:8080/user/1` in your browser, you will see the json result.

