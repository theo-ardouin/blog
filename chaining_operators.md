# Chaining operator '>'

The most objective tier-list of programming languages based on the following expression: `3 > 2 > 1`.

It might _even_ extend to most - _if all_! - comparative operators chaining. What a treat!

Without context, here it is:

| Tier | Languages                               |
| ---- | --------------------------------------- |
| S    | [Rust], [Swift]                         |
| A    | [Python]                                |
| B    | [Java], [C#], [TypeScript], [Go], [Lua] |
| C    | [Kotlin], [PHP], [Ruby]                 |
| D    | [C], [C++]                              |
| F    | [Javascript]                            |

_You, dear reader, should definitely use this metric when choosing which language to use in your startup._

## S

The obvious _crème de la crème_.

Those languages know what they're doing and they're doing it right.
If you're doing something pretty stupid, they will let you know!

They are clear, helpful, strong, independent and dress nicely.
Somehow more importantly, they know better to stay out of cold, cold math's ways.

## A

The sneaky math enthusiasts.

Those languages always think highly of themselves. They might be a bit right, though.

They like to have more math in their math.[1]
As such, they usually are formal, logical and wear white socks and sandals.

## B

The goody-two-shoes.

Those are the good students. They care about the rules and type-safety.[2]

If they were more creative, they could have noticed the syntax didn't make that much sense...
But hey, what matter is the result. And after all, they are correct, the best kind of correct.[3]

Them have the brain. _I sure don't._

## C

The average ones.

It definitly works but somehow, it feels mediocre and lacks polish.

They might be trying their best, but ultimately, they aren't as helpful as we would want them to be.

They can be good-natured, muddled and with those hopeful big, round open eyes.
You just don't want them to be sad, even if they are a bit pitiful...

## D

The epitome of _"Not my problem, we told you so if you listened carefully"_.

Those languages don't care if you do it _right_. It only matters to them if you _can_ do it.

They are usually arrogant and self-destructive.
They might have excuses because they're getting old, but go and get some therapy, _grandpa_!

## F

The worst offenders.

Those languages are unstable, unclear and most of all, have daddy issues.

At best, their design is questionable. At worst, they're a plague on our shattered world.
Please take some time to rethink your choices if you think they're somehow good to you...

---

## In depth

### Rust

```rust
println!(3 > 2 > 1);
error: comparison operators cannot be chained
 --> src/main.rs:2:16
  |
2 |     println!(3 > 2 > 1);
  |                ^   ^
  |
help: split the comparison into two
  |
2 |     println!(3 > 2 && 2 > 1);
  |                    ++++
```

So helpful, so nice <3

### Swift

```swift
/main.swift:3:9: error: adjacent operators are in non-associative precedence group 'ComparisonPrecedence'
print(3 > 2 > 1)
        ^   ~
/main.swift:3:9: error: cannot convert value of type 'Bool' to expected argument type 'Int'
print(3 > 2 > 1)
```

### Python

```python
>>> 3 > 2 > 1
True
```

One of the few languages where you can chain operators.[4]

However don't look a gift horse in the mouth, as in:

```python
>>> (3 > 2) > 1
False
```

As `bool` inherits from `int`[5] and `False` is `0`, the code above is technically correct. Sadly.

### Java

```java
System.out.print(3 > 2 > 1);
Main.java:6: error: bad operand types for binary operator '>'
        System.out.print(3 > 2 > 1);
                               ^
  first type:  boolean
  second type: int
```

(Kudos on giving that much details!)

### C#

```c#
Compilation error (line 7, col 21): Operator '>' cannot be applied to operands of type 'bool' and 'int'
```

### TypeScript

```typescript
Operator '>' cannot be applied to types 'boolean' and 'number'.
```

### Go

```go
invalid operation: 3 > 2 > 1 (mismatched types untyped bool and untyped int)
```

### Lua

```lua
> print(3 > 2 > 1)
stdin:1: attempt to compare number with boolean
stack traceback:
        stdin:1: in main chunk
        [C]: ?
```

### Kotlin

```kotlin
The integer literal does not conform to the expected type Boolean
```

### PHP

```php
print(3 > 2 > 1);
PHP Parse error:  syntax error, unexpected token ">"
```

Definitly did not expect a somewhat correct response,
considering its long, long history of fucking things up.[6]

### Ruby

```ruby
(file): undefined method `>' for true (NoMethodError)
```

### C

Of course it runs. Of course, you need to `-Wall -Wextra -Werror` at the very least to avoid those issues.

```c
test.c: In function ‘main’:
test.c:5:31: warning: comparisons like ‘X<=Y<=Z’ do not have their mathematical meaning [-Wparentheses]
    5 |         printf("%d\n", (3 > 2 > 1));
```

Same ol' story about order of operation here.
First `3 > 2` gets executed and returns `1`, as boolean values didn't exist in C.
Then, `1 > 1` returns `0`.

To its defence, the boolean type wasn't that necessary.
Mostly because they had bigger things to focus on.[7]

Moreover, the processor already handles `0` as `false` for comparison, why bother?

### C++

Due to its heavy `C` legacy, it behaves the same way.

Just don't forget those brackets within that stream or else you'll be in a world of ~~template~~ pain.

```c++
test.c: In function ‘int main(void)’:
test.c:5:25: warning: comparisons like ‘X<=Y<=Z’ do not have their mathematical meaning [-Wparentheses]
    5 |         std::cout << (3 > 2 > 1) << std::endl;
```

### Javascript

```js
>> 3 > 2 > 1
< false
```

Lot to unpack here. This really, really terrible result is due to:
- the order of operations
- implicit casting to anything and everything

More or less, it expands to the following operations:
```js
(3 > 2) > 1 // Operation order
true > 1    // Evaluate (3 > 2)
1 > 1       // Implicit cast of true to 1
= false
```

Simple and yet, so wrong.

[Rust]: #rust
[Swift]: #swift
[Python]: #python
[Java]: #java
[C#]: #c
[TypeScript]: #typescript
[Go]: #go
[Lua]: #lua
[Kotlin]: #kotlin
[PHP]: #php
[Ruby]: #ruby
[C]: #c-1
[C++]: #c-2
[Javascript]: #javascript

[1]: https://en.wikipedia.org/wiki/Boolean_algebra
[2]: https://en.wikipedia.org/wiki/Type_safety
[3]: https://www.youtube.com/watch?v=hou0lU8WMgo
[4]: https://docs.python.org/3/reference/expressions.html#comparisons
[5]: https://peps.python.org/pep-0285
[6]: https://eev.ee/blog/2012/04/09/php-a-fractal-of-bad-design
[7]: http://csapp.cs.cmu.edu/3e/docs/chistory.html
