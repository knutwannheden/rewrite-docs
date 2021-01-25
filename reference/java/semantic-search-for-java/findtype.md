---
description: List all AST elements that refer by name to a type.
---

# FindType

### Definition

`FindType` searches through a given AST and adds a marker to any elements of the specified type. It then returns a set of those marked elements.
Note that `FindType` does not support listing references to primitive types.

```java
J.CompilationUnit cu = ...

// all the types referred to in a source file
Set<NameTree> anywhereInFile = new FindTypes("java.util.List")
  .visit(cu);
```

Note that this does _not_ include implicit uses of a type. Consider the following class:

```java
import java.util.List; // imports don't match

public class Sample {
  // matches twice, one on each of the List references
  List<List<String>> listOfLists = new ArrayList<>();
  
  // matches on return expression
  List<String> getName() {
  }
}
```

{% hint style="info" %}
Use `SearchResult.PRINTER` to print out the modified AST after running `FindType`. This will insert an arrow before each instance of the given type that was found.
{% endhint %}

Other examples of potential matches include \(not an exhaustive list\):

* `List::add`
* `MyType.genericSignature<List<String>>()`
* `List[]`
* `new List<String>() { }` - An anonymous class declaration
* `(List<String>) collection` - A typecast
* `catch(IllegalArgumentException e)` - Matches on `java.lang.IllegalArgumentException`

