## Installation

Sparkle framework is designed to be running on different containers or servers, so the first thing you need to do is choose the runtime.

### Install for servlet runtime

If you use maven, add dependency:

``` xml
  <dependency>
    <groupId>org.agilej</groupId>
    <artifactId>sparkle-servlet</artifactId>
    <version>{sparkle.version}</version>
  </dependency>
```

For gradle user, use

``` groovy
  compile "org.agilej:sparkle-servlet:{sparkle.version}"
```

### Install for netty runtime

If you use maven, add dependency:

``` xml
  <dependency>
    <groupId>org.agilej</groupId>
    <artifactId>sparkle-netty</artifactId>
    <version>{sparkle.version}</version>
  </dependency>
```

For gradle user, use

``` groovy
  compile "org.agilej:sparkle-netty:{sparkle.version}"
```