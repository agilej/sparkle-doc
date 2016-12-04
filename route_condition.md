## Route Condition

Route condition is used to define specific route rule from request info and mapping to controller method.

One route rule condition example may like this:
```
class MyRouteModule implements RouteModule {
  
  void config(Router router) {
    router.mapping("/user/{id}").withGet().params("a!=1").header("X-ARGU").to("user#show");
  }
}
```

The above route module defined one route with some specifications:

* The http method is get
* The request has one parameter named `a` and its value is not equals to `1`
* The request has one header named `X-ARGU` and its value is not null

Only all these conditions a mateched for one request, the controller method will be called.

### Supported conditions.

Currently Sparkle framework support following conditions:

* http method condition
* parameter condition
* header condition

All parameter and header condition use same logic expressions

* Present logic: just use `a`
* Equals logic: just use `a==1`
* Not equals logic: just use `a != 1`

Multi conditions can be combined use `&` such as `a!=1 & b == 2`

