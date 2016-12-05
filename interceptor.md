# Interceptor

Sparkle's execution mechanism includes interceptors, which are useful when you want to apply specific functionality to certain requests, for example, checking for authentication.

Interceptors  implement `Interceptor`. This interface defines two methods: `preHandle(..)` is called before the actual controller-action is executed; `afterHandle(..)` is called after the ontroller-action is executed. These two methods should provide enough flexibility to do all kinds of preprocessing and postprocessing.

The `preHandle(..)` method returns a boolean value. You can use this method to break or continue the processing of the execution chain. When this method returns true, the ontroller-action execution chain will continue; when it returns false, the Sparkle framework assumes the interceptor itself has taken care of requests (and, for example, rendered an appropriate view) and does not continue executing the other interceptors and the actual ontroller-action in the execution chain.

All the interceptors you created should be registered from `Config#registerInterceptor` method.

Here is the example for how to apply a login-check interceptor:

1. __Define Interceptor__

    ```java
    public LoginCheckInterceptor implements Interceptor {
      
      public boolean preHandle(WebRequest request, WebResponse response){
        if(request.session().get("uid") != null) {
          return true;
        }
        reponse.sendRedirect("/login");
        return false;
      }

      public void afterHandle(WebRequest request, WebResponse response){
      }

    }

    ```
2. __Register Interceptor__
    
    ```java
    public AppConfig implements Application {
      
      void config(Config config){
        config.registerInterceptor(new LoginCheckInterceptor());
      }

    }

    ```
