# View Resolve

A ViewRender is a core component of Sparkle Framework, translating action result in @Controller to actual view implementations. In one sparkle application, there are many implementations of built-in ViewRender to choose from, and you also can register your own ViewResolver to support more view solutions. 

Sparkle treat all action result as one view render, it maybe one jsp page render or json output or other output.When controller finished executing action method and returned one result object, sparkle framework will iterate all registered ViewRenders (from built-in or created by application), use the first matched one to resolve the result object and render result.

Since `Json` is getting more and more popular today, sparkle support various json view resolve solution, from jackson to jsonty support.

Sparkle has following built-in ViewResolvers

* TextViewRender
* RedirectViewRender
* JSONObjectViewRender
* JSONTextViewRender
* JsontyViewRender

# ViewRenders

## RedirectViewRender

`RedirectViewResolver` is used for doing request redirecting if sparkle find one action result object match
* is `String` type
* the string is start with `redirect:`

``` java

public String create(WebRequest request) {
  // ...
  return "redirect:/dashboard";
}

```

## JSONTextViewRender

If you already have some json string and want to output it directly, you can return one `JSONText` instance as action result.

``` java

public JSONText output(WebRequest request) {
  String json = getJsonFromsomeapi();
  return new JSONText(json);
}

```

## JSONModelViewRender

If you want to use [jsonty](jsonty) to build json result, just return `JSONModel` instance as action result.

``` java

public JSONModel output(WebRequest request) {
  return e -> {
    e.exposeWithName("100", "count");
  }
}

```

## JSONObjectViewRender

If you want to use [jackson](jsonty) to build json result, annotate your action method with `@Json`, now sparkle will use Jackson to generate json result

``` java

public List<Users> output(WebRequest request) {
  List<User> users = this.userService.findAll();
  return users;
}

```
