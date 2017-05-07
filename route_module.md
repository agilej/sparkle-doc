## Route Modules

Sparkle use route module to define route rules (which is the mapping from http request to one controller's action method).

Route module is represented by interface `RouteModule`, actually this may be the only one thing need for implementing a sparkle web application. 

One route module can define a set of route fules, for example it may like this:

```java
class MyRouteModule implements RouteModule {
  
  void config(Router router) {
    router.mapping("/user/{id}").withGet().to("user#show");
    router.mapping("/user").withPose().to("user#save");
  }
}
```

The above route module defined two route rules, one is http get request with path like `/user/123`  will map to one controller called `user` and action method called `show`; the other one define one http request post to `/user` will map to `user` controller and `save` action method.

For how to implement a controller please refer to [Implement Controller](conroller.md).

Sparkle framework also support multi route modules, so you can group route rules in different classes, just create multi `RouteModule` implementation classes, but you should rember:

* All route modules are automatically scanned and installed during framework's startup.
* All route modules are orderless, means you should not design your route rules depend on specific roders.

### Route's path template

As you have seen above, you can define path variables with '{}' syntax for one route, these named parameters will be accessible through annotation `@PathVariable` or `WebRequest#pathVariable`

Just remember, every path variable name must match:

1. Start with a-z or A-Z
2. Can only contains `a-z` `A-Z` `0-9` and `_`
