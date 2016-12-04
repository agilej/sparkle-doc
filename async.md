
## Asynchronous Support In Sparkle

Sparkle has two ways in async support.

### Sparkle Managed Async Task

This way means you submit a sync task and Sparkle will run it with it's internal Thread Pool.

For this scene, Sparkle provide two ways to do it

#### Annotation @Async

If you annotated one action method with @Async, this method will wrapped in one Callable by Sparkle and submit it to  internal thread pool and render result.

```
//example 

@Async
public List<User> index(){
  List<User> users = remoteUserService.listUsers();
  return users;
}

```

#### Callable return type

If you return one Callable instance in action method, Sparkle will then submit this task to it's internal thread pool and render result asynchrously.

```java
//example 

public Callable index(){
  return new Callable(){
    Object call(){
      List<User> users = remoteUserService.listUsers();
      return users;
    }
  };
}
```

### Application Managed Async Task

Some times you want to controll your async task manually(with your own asynchronous model), when task finish ask Sparkle to render the result. 

For this scene, Sparkle provide one `DeferredResult` class. If you return a `DeferredResult` instance from one action method, Sparkle knows you want to do some task and waiting for rendering. After you call `DeferredResult#setResult`, Sparkle will continue rendering with the real result.

```
//example

public DeferredResult index(){
  DeferredResult deferredResult = new DeferredResult();
  executeWithYourOwnAsyncModel(deferredResult);
  return deferredResult;
}

private void executeWithYourOwnAsyncModel(DeferredResult deferredResult) {
  // execute code with your async model
  execute({
    List<User> users = remoteUserService.listUsers();
    deferredResult.setResult(users);
  })
}

```

## Examples

* Example with @Async annotation

```
@Async
public List<User> index(){
  List<User> users = remoteUserService.listUsers();
  return users;
}

```

* Example with `Callable` result.

```
public Callable index(){
  return new Callable(){
    Object call(){
      List<User> users = remoteUserService.listUsers();
      return users;
    }
  };
}

```

* Example with `DeferredResult`.

```
public DeferredResult index(){
  DeferredResult deferredResult = new DeferredResult();
  executeWithYourOwnAsyncModel(deferredResult);
  return deferredResult;
}

private void executeWithYourOwnAsyncModel(DeferredResult deferredResult) {
  // execute code with your async model
  execute({
    List<User> users = remoteUserService.listUsers();
    deferredResult.setResult(users);
  })
}

```