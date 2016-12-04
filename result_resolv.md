
## result annotated with @Text

## result annotated with @Json

Sparkle will transform the any result to json string if the action method is annotated with `@Json`.

For transforming the result to json, sparkle will use different tools to do it. If the runtime returned result is type `JSONModel`, sparkle will use built-in json transform; otherwise it will use google `gson` to transform object to json.

## result with type `JSONModel`

Sparkle use `Jsonty` library as it's json support. If you return a result with type `JSONModel`, sparkle will use `Jsonty` to transform the result to json string.

## result with type JSONText

For some specific scene, you can directly return a `JSONText` object as result, it means you want to output some string you setted to JSONText as json string directly, For example you can render some file content as json.

Please note `JSONText` result don't need the action method annotated with `@Json`.

## result with type `String`

If the result is `String`, it can be processed differently, all depend on the view renders in your application. Sparkle has a built-in view render `RedirectViewRender` which accept a `String` result and redirect user to another request if the `String` is started with `redirect:`.
