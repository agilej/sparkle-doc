# Customized argument resolver

Sometimes it's reasonable to pass customized argument type to action method, Sparkle also provide `ArgumentResolver` registration to support it. All you need to do is creating one implement of `ArgumentResolver` and register it in app configuration with `Config#registerArgumentResolver`.

Here is one example with supporting customized argument type `Foo`

__Define your argument class__

```java
class Foo { 
  //... 
}
```

__Implement ArguemntResolver__

```java
class FooArgumentResolver implement ArgumentResolver {
  
  public boolean support(ActionMethodParameter actionMethodParameter) {
    return actionMethodParameter.getClass().equals(Foo.class);
  }

  public Object resolve(ActionMethodParameter actionMethodParameter, WebRequest request){
    Foo foo = new Foo();
    //set foo from request.
    //...
    return foo;
  }

}
```

__Register FooArgumentResolver__

```java
class AppConfig implement Application {

  public void config(Config config){
      FooArgumentResolver foorArgumentResolver = new FooArgumentResolver();
      config.registerArgumentResolver(foorArgumentResolver);
  }

}
```

__Use it in your controller's action method__

```java
@Controller
class BarController {
  
  public String index(WebRequest request, Foo foo){
    // here you can use foo  
  }
  
}

```



