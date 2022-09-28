# match

A match expression branches on a pattern. The exact form of matching that occurs depends on the pattern. A match expression has a scrutinee expression, which is the value to compare to the patterns. The scrutinee expression and the patterns must have the same type.

A match behaves differently depending on whether or not the scrutinee expression is a place expression or value expression. If the scrutinee expression is a value expression, it is first evaluated into a temporary location, and the resulting value is sequentially compared to the patterns in the arms until a match is found. The first arm with a matching pattern is chosen as the branch target of the match, any variables bound by the pattern are assigned to local variables in the arm's block, and control enters the block.

When the scrutinee expression is a place expression, the match does not allocate a temporary location; however, a by-value binding may copy or move from the memory location. When possible, it is preferable to match on place expressions, as the lifetime of these matches inherits the lifetime of the place expression rather than being restricted to the inside of the match.

An example of a match expression:

```rust,editable
let x = 1;

match x {
    1 => println!("one"),
    2 => println!("two"),
    3 => println!("three"),
    4 => println!("four"),
    5 => println!("five"),
    _ => println!("something else"),
}
```
Multiple match patterns may be joined with the | operator. Each pattern will be tested in left-to-right sequence until a successful match is found.

```rust,editable
let x = 9;
let message = match x {
    0 | 1  => "not many",
    2 ..= 9 => "a few",
    _      => "lots"
};

assert_eq!(message, "a few");

// Demonstration of pattern match order.
struct S(i32, i32);

match S(1, 2) {
    S(z @ 1, _) | S(_, z @ 2) => assert_eq!(z, 1),
    _ => panic!(),
}
```
Note: The 2..=9 is a Range Pattern, not a Range Expression. Thus, only those types of ranges supported by range patterns can be used in match arms.

Every binding in each | separated pattern must appear in all of the patterns in the arm. Every binding of the same name must have the same type, and have the same binding mode.
