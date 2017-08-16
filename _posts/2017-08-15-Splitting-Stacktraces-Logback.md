---
layout: post
title:  "Print Stacktraces in a different file with Logback"
date:   2017-08-14 08:00:00
categories: en
excerpt: Or "How to hide the mess elsewhere"
---

Sometimes you don't want an ugly stacktrace in your beautiful logs. This is a method on "How to hide the mess elsewhere" with [Logback](https://logback.qos.ch/).

![Exception]({{site.url}}/assets/exception.png)

## First try (it's not gonna work)

The first try is always the same: define two patterns one with [nopex](https://logback.qos.ch/manual/layouts.html#nopex) for ignoring exceptions and another one with the exception. Something like that:

```xml
<property name="noExceptionPattern" value="%nopex %d{dd/MM/yyyy-HH:mm:ss.SSS} [%thread] %-5level %logger{36}:%L - %msg%n" />
<property name="exceptionPattern" value="%nopex %d{dd/MM/yyyy-HH:mm:ss.SSS} [%thread] %-5level %logger{36}:%L - %xException{full}%n" />
```

But as soon as you try that configuration the awfull truth will strike you in the face: it does not work. `exceptionPattern` will print a line every time even if they are no Exception ! :sob:

But it's quite easy to make it work !

## Make it work !

To make it work we have to add something: a [filter](https://logback.qos.ch/manual/filters.html). To show how it works, I'm going to use a [JaninoEventEvaluator](https://logback.qos.ch/manual/filters.html#JaninoEventEvaluator). It takes an arbitrary Java language block returning a boolean value as the evaluation criteria:


```xml
<appender name="stackTracerFileAppender" class="ch.qos.logback.core.rolling.RollingFileAppender">
  <file>&LOG_DIR;/stacktraces.log</file>
  <filter class="ch.qos.logback.core.filter.EvaluatorFilter">
    <evaluator>
      <expression>return (throwable == null);</expression>
    </evaluator>
    <OnMismatch>NEUTRAL</OnMismatch>
    <OnMatch>DENY</OnMatch>
  </filter>
  <encoder>
    <charset>UTF-8</charset>
    <pattern>${exceptionPattern}</pattern>
  </encoder>
</appender>
```

If the exception is null the appender will not print (or in this case write) a line.

To make it work you have to [add Janino to your pom](https://logback.qos.ch/setup.html#janino).

That's it ! Easy !

Stay tuned and See ya !
