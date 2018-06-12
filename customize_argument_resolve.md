# Customized argument resolver

Sometimes it's reasonable to pass customized argument type to action method, Sparkle also provide `ArgumentResolver` registration to support it. All you need to do is creating one implement of `ArgumentResolver` and register it in app configuration with `Config#registerArgumentResolver`.

Here is one example with supporting customized argument type `Foo`

1. **Define your argument class**

   ```java
    class Foo { 
      //... 
    }
   ```

2. **Implement ArguemntResolver**

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

3. **Register FooArgumentResolver**

   ```java
    class AppConfig implement Application {

      public void config(Config config){
          FooArgumentResolver foorArgumentResolver = new FooArgumentResolver();
          config.registerArgumentResolver(foorArgumentResolver);
      }

    }
   ```

4. **Use it in your controller's action method**

   ```java
    @Controller
    class BarController {

      public String index(WebRequest request, Foo foo){
        // here you can use foo  
      }

    }
   ```



