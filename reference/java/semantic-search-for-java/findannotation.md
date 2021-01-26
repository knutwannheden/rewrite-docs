---
description: Find instances of a given annotation as specified by type and parameters using the AspectJ pointcut syntax.
---

# FindAnnotation

## Definition

`FindAnnotation` searches through a given AST and adds a marker to any annotations matching the specified annotation pattern. It then returns a set of those marked elements.

`FindAnnotation` takes a single parameter, `annotationPattern`, to find matching annotations. Note that `annotationPattern` is expressed using the AspectJ [pointcut syntax](https://www.eclipse.org/aspectj/doc/next/adk15notebook/annotations-pointcuts-and-advice.html).

```java
J.CompilationUnit cu;

FindAnnotation fa = new FindAnnotation("@java.lang.SuppressWarnings");
Set<J.Annotation> annotations = fa.visit(cu);
```

The example above matches on the `@SuppressWarnings` annotation with no parameters. In order to match on single parameter annotations it could be passed `@java.lang.SuppressWarnings("deprecation")`.

Additionally, the order of the paramaters doesn't matter. Consider the following `@Get` annotation:

```java
package myhttp;

public @interface Get {
  String serviceName();
  String path();
}
```

Both`@myhttp.Get(serviceName="payments", path="recentPayments")` and `@myhttp.Get(path="recentPayments", serviceName="payments")` match on both parameters despite the order of their parameters.

