# Customized view resolver

Most of time we want to use template engine in application, but Sparkle does not ship with any built-in template engine. You could customize your own view resolver with the template engine you choose. 

To support customzie a view resolver, Sparkle provide `ViewRender` registration. All you need to do is creating one implement of `ViewRender` and register it in app configuration with `Config#registerViewRenderClass`.

Here is one example to integrate [Pebble Template Engine](http://www.mitchellbosecke.com/pebble/home) into application.

1. __Implement ViewResolver__

    ```java
    class PebbleViewResolver implement ViewRender {

      private PebbleEngine engine;

      public PebbleViewResolver(){
        this.engine = new PebbleEngine();
      }

      public boolean supportActionMethod(ActionMethod actionMethod, Object actionMethodResult)  {
        return actionMethodResult.getClass().equals(String.class);
      }

      public renderView(Object result, Object controller, WebRequest request) 
        String templateName = result.toString();
        this.engine.getTemplate(template).evaluate(ctx, request.getResponse().getWriter(), request.getLocale());
      }

    }
    ```

3. __Register your own ViewRender

    ```java
    class AppConfig implement Application {

      public void config(Config config){
          config.registerViewRenderClass(PebbleViewResolver.class);
      }

    }
    ```

4. __Return supported result type from your controller's action method__

    ```java
    @Controller
    class BarController {

      public String index(WebRequest request){
        return "template/user_index";   //map to template file template/user_index.peb
      }

    }

    ```
