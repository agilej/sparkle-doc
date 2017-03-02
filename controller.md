## Implement controller

Controller is the real place your action code will be executed, and you map request to controller's one action method from route rules defined in `RouteModule`.

Suppose you have the following route rule: 

```java
  router.match("/user/{id}").to("user#show");
```

This will map one http request like `/user/1` to one controller named `user` and the action method is `show`.

Sparkle use `@Controller` annotation to define controller.

```java
@Controller("user")
public class UserController{

    public ModelAndView show(@PathVariable("id") int userId) {
         //...
    }

}
```

The above code define `user` controller and has a `show` action method, so it's fit for route rule `router.match("/user/{id}").to("user#show")`.

### Action method

In sparkle, one action method must meet two requirements:

* `public`, it must mark as public.
*  Has exactly same name with route rules, so you can't Java overload for action method.

Action method can have various arguments and different return type, you will see it in the following part.

