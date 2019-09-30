---
title: MissingDefault
summary: The Google Java Style Guide requires that each switch statement includes a default statement group, even if it contains no code. (This requirement is lifted for any switch statement that covers all values of an enum.)
layout: bugpattern
tags: Style
severity: WARNING
---

<!--
*** AUTO-GENERATED, DO NOT MODIFY ***
To make changes, edit the @BugPattern annotation or the explanation in docs/bugpattern.
-->

## The problem
The [Google Java Style Guide §4.8.4.3][style] requires each switch statement to
include a `default` statement group, even if it contains no code.

NOTE: A switch statement for an `enum` type may omit the `default` statement
group, if it includes explicit cases covering all possible values of that type.
See [MissingCasesInEnumSwitch] for more information.

Without a default, the reader does not always know whether execution might
silently "fall out" of the entire block, having executed no code within it. This
is undesirable for most of the same reasons silent fall-through is undesirable.

If the unhandled cases should be impossible, add a `default` clause that throws
`AssertionError`:

```java
switch (state) {
  case READY:
    return true;
  case DONE:
    return false;
  default:
    throw new AssertionError("unexpected state: " + state);
}
```

If having execution fall out of the switch is intentional, add a `default`
clause with a comment:

```java
switch (state) {
  case READY:
    return true;
  case DONE:
    return false;
  default:
    // fall out
}
```

[style]: https://google.github.io/styleguide/javaguide.html#s4.8.4-switch

[MissingCasesInEnumSwitch]: https://errorprone.info/bugpattern/MissingCasesInEnumSwitch

## Suppression
Suppress false positives by adding the suppression annotation `@SuppressWarnings("MissingDefault")` to the enclosing element.