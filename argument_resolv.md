Sparkle support several argument type for your action method.

# Built-In types

Sparkle support these built-in parameter types

## String

Pass string parameters from request.

__You should use `@Param` or `@PathVariable` when use `String` as parameter type__, because currently we can't get parameter name from java method directly(though java 8 has parameter in reflection, but it's disabled by default)

## Primitive type and wrap type

You can pass `int`, `double`, `float`, or their wrap type `Integer`, `Float`, `Double`. Sparkle will automatically convert param from string to correct type.

## WebRequest

The sparkle's core interface `WebRequest` can be passed as parameter

## WebResponse

The sparkle's core interface `WebResponse` can be passed as parameter

## Params

`Params` represent all params from current request.

## Cookies

`Cookies` can be used to manage cookies write and read.

# Vendor types

You can also pass some vendor supported types as parameter.

## HttpServletRequest

If you run sparkle with a servlet container, you can pass `HttpServletRequest` as parameter.

## HttpServletResponse

If you run sparkle with a servlet container, you can pass `HttpServletResponse` as parameter.

## FullHttpRequest

If you run sparkle with a netty container, you can pass netty's `FullHttpRequest` as parameter.

# Customize Argument Resolver

For customizing your argument resolver ownly, please refer to [Customize Arugment Resolver](customize_argument_resolve.md)