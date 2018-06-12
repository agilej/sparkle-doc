# Customized view render

Most of time we want to use template engine in application, but Sparkle does not ship with any built-in template engine. You could customize your own view render with the template engine you choose.

To support customzie a view render, Sparkle provide `ViewRender` registration. All you need to do is creating one implement of `ViewRender` and register it in app configuration with `Config#registerViewRenderClass`.

Here is one example to integrate [Pebble Template Engine](http://www.mitchellbosecke.com/pebble/home) into application.

1. **Implement ViewRender**

   ```java
    class PebbleViewRender implement ViewRender {

      private PebbleEngine engine;

      public PebbleViewRender(){
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

2. **Register your own ViewRender**

   ```java
    class AppConfig implement Application {

      public void config(Config config){
          config.registerViewRenderClass(PebbleViewRender.class);
      }

    }
   ```

3. **Return supported result type from your controller's action method**

   ```java
    @Controller
    class BarController {

      public String index(WebRequest request){
        return "template/user_index";   //map to template file template/user_index.peb
      }

    }
   ```



