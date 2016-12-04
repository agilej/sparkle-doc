## Route Modules

Sparkle use route module to define mapping between one uri route and controller method.

Route module is represented by interface `RouteModule`, actually this may be the only one thing need for implementing a sparkle web application.

One route module example may be like this:
```
class MyRouteModule implements RouteModule {
  
  void config(Router router) {
    router.mapping("/user/{id}").withGet().to("user#show");
    router.mapping("/user").withPose().to("user#save");
  }
}
```
The above route module defined two route rules, one is http get request with path like `/user/123`  will map to one controller called `user` and action method called `show`. For how to implement one controller please refer to [Implement Controller](conroller.md).

Sparkle framework also support multi route modules, so you can group route rules in different classes, just rember :

* All route modules are auto scanned and installed during framework's startup.
* All route modules are orderless, means you should not design your route rules depend on specific roders.