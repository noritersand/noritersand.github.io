---
layout: post
date: 1970-01-05 00:00:00 +0900
title: '[JavaScript] 이건외이러는걸까요'
categories:
  - javascript
tags:
  - javascript
  - ecmascript
---

이건 이상한모임 슬랙에서 줏어옴:

```js
[] + {} // '[object Object]'
{} + [] // 0
[] + {} === {} + [] // true
```

다음부터는 [jsisweird.com](https://jsisweird.com/)에서 줏어온 거:

#### true + false

According to the [ECMAScript Language Specification](https://262.ecma-international.org/5.1/#sec-11.6), the two boolean values are type coerced into their numeric counterparts.

```js
Number(true); // -> 1
Number(false); // -> 0
1 + 0; // -> 1
```

#### [,,,].length

`[,,,]` outputs an array with three empty slots. The last comma is a [trailing comma](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Trailing_commas).

If you don't think this is weird enough yet, then take a look at this:

```js
[,] + [,]; // -> ""
[] + [] === [,] + [,]; // -> true
[,,,] + [,,,]; // -> ",,,,"
([,,,] + [,,,]).length === [,,,,].length; // -> true
```

To find resources that explain the addition operator with arrays, take a look at the explanation for question 3, directly below this.

#### [1, 2, 3] + [4, 5, 6]

The extremely simplified answer is that the arrays are converted to strings and are then concatenated. See [Denys Dovhan's explanation](https://github.com/denysdovhan/wtfjs#adding-arrays) for how this happens. To learn more about this behavior, visit [this StackOverflow answer](https://stackoverflow.com/questions/9032856/what-is-the-explanation-for-these-bizarre-javascript-behaviours-mentioned-in-the) for a mid-level explanation or this blog post for a more detailed one.

Adding a trailing comma doesn't change anything, by the way:

```js
[1, 2, 3,] + [4, 5, 6]; // -> "1,2,34,5,6"
```

But, I suppose, if you really want to convert your arrays to comma-separated strings and combine them, you could write something stupid like this:

```js
[1, 2, 3] + [, 4, 5, 6]; // -> "1,2,3,4,5,6"
```

Or, even dumber, this:

```js
[1, 2, 3, ""] + [4, 5, 6]; // -> "1,2,3,4,5,6"
```

Probably best not to use the addition operator together with arrays, though. If you do want to combine two arrays for real, this is a better approach:

```js
[...[1, 2, 3], ...[4, 5, 6]];
```

#### 10, 2

The [comma operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Comma_Operator) simply returns the value of the last operand.

```js
10, 2; // -> 2
1, 2, 3, 4; // -> 4
42, "pineapple", true; // -> true
```

#### +!![]

Arrays are truthy, hence the double NOT operator will output true. The plus character then converts true to its numeric representation: 1.

```js
!![]; // -> true
+true; // -> 1
```

This expression might become clearer if written explicitly.

```js
Number(Boolean([])); // -> 1
````

#### true == "true"

According to the rules of [abstract equality comparison](https://262.ecma-international.org/11.0/#sec-abstract-equality-comparison), both of these values are converted to numbers when compared.

```js
Number(true); // -> 1
Number("true"); // -> NaN
1 == NaN; // -> false
```

#### "" - - ""

These two empty strings are both converted to 0.

```js
Number(""); // -> 0
0 - - 0; // -> 0
```

The expression might become a bit clearer if I write it like this:

```js
+"" - -"";
+0 - -0;
```

Please note that, while I put the space between the minus sign and the empty string simply to attempt to confuse you, the space between the minus signs themselves is important:

```js
- -""; // -> 0
--""; // -> SyntaxError
```

#### null + 0

Null converts to its numeric representation: 0.

```js
Number(null); // -> 0
0 + 0; // -> 0
```

This also means that while:

```js
null === false; // -> false
```

this is true:

```js
+null === +false; // -> true
```

#### 1/0 > 10 ** 1000

JavaScript treats both of these values as infinite, and infinity is equal to infinity. [Learn more about infinities](https://en.wikipedia.org/wiki/Floating-point_arithmetic#Infinities).

```js
1/0; // -> Infinity
10 ** 1000; // -> Infinity
Infinity > Infinity; // -> false
```

The exponentiation operator `**` is [basically](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Exponentiation) the same thing as the Math.pow function.

```js
10 ** 1000; // -> Infinity
Math.pow(10, 1000); // -> Infinity
```

#### true++

Our first and only syntax error. I put SyntaxError as an option on a lot of the questions, hoping that some users would find some syntax so bizarre that it could not possibly be allowed. So, I felt that I had to add at least one expression that actually does result in a SyntaxError.

Some global variables and functions won't throw syntax errors, by the way:

```js
true++; // -> SyntaxError
1++; // -> SyntaxError
"x"++; // -> SyntaxError
null++; // -> SyntaxError
undefined++; // -> NaN
$++; // -> NaN
console.log++; // -> NaN
NaN++; // -> NaN
```

And, of course, just to be completely clear, this is valid syntax:

```js
let _true = true;
_true++;
_true; // -> 2
```

#### "" - 1

While the addition operator `+` is used both for numbers and strings, the subtraction operator (-) has no use for strings, so JavaScript interprets this as an operation between numbers. An empty string is type coerced into 0.

```js
Number(""); // -> 0
0 - 1; // -> -1;
```

This would still be true even if the string had a space (or more) inside of it:

```js
" " - 1; // -> -1;
```

However, if we use the addition operator, then string concatenation takes priority:

```js
"" + 1; // -> "1";
```

#### (null - 0) + "0"

null is coerced into 0.

```js
Number(null); // -> 0
0 - 0; // -> 0
0 + "0"; // -> "00"
```

If the question had used only the subtraction operator, the result would have been different:

```js
(null - 0) - "0"; // -> 0
```

#### true + ("true" - 0)

You might suspect that JS is so bananas that it would convert the string literal "true" into the numeric representation of the boolean true. It's not quite that bananas, however. What actually happens is that it tries to convert the string to a number and fails.

```js
Number("true"); // -> NaN
```

#### !5 + !5

Putting a solemn exclamation mark, also known as the logical NOT operator, before a non-Boolean value will convert it to a boolean value opposite of what the Boolean function would convert it into.

5 is truthy:

```js
Boolean(5); // -> true
!!5; // -> true
```

The opposite of true is, of course, false, which in turn is coerced into 0:

```js
!5; // -> false
+false; // -> 0
0 + 0; // -> 0
```

#### [] + []

This question is closely tied to question 3. Again, the extremely simplified answer is that JavaScript converts the arrays to strings. Scroll up to question 3 to find resources that explain this behavior.

```js
[].toString(); // -> ""
"" + ""; // -> ""
```

Also, like I mentioned in the explanation for question 2, these expressions are equal, due to trailing commas:

```js
[] + [] === [,] + [,]; // -> true
```

Even though these arrays are different, they are both converted to empty strings:

```js
[].length; // -> 0
[,].length; // -> 1
[].toString() === [,].toString(); // -> true
```

Of course, this is also true:

```js
Number([]) === Number([,]); // -> true
```

#### 1 + 2 + "3"

JavaScript will execute these operations from left to right. String concatenation will take priority when the number three is added with the string three.

```js
1 + 2; // -> 3
3 + "3"; // -> "33"
```

Since the operations are executed from left to right, the expressions will give a signficantly different output if the first operation were to contain a string:

```js
"1" + 2 + 3; // -> "123"
1 + "2" + 3; // -> "123"
```

#### typeof NaN

The [ECMAScript Language Specification](https://262.ecma-international.org/5.1/#sec-4.3.23) explains NaN as a number value that is a IEEE 754 “Not-a-Number” value. It might seem strange, but this is a common computer science principle.

There are some odd issues surrounding NaN in JavaScript, however. For instance, this is one of the very few instances where the Object.is function disagrees with triple equal.

```js
NaN === NaN; // -> false
Object.is(NaN, NaN); // -> true
```

Another such rare instance can be seen in question 24.

This legacy issue was later remedied with the isNaN function.

```js
isNaN(NaN); // -> true
```

#### undefined + false

While false can be converted to a number, undefined cannot.

```js
Number(undefined); // -> NaN
Number(false); // -> 0
NaN + 0; // -> NaN
```

However, undefined is represented by false:

```js
!!undefined === false; // -> true
```

Which means that we can add undefined and false like so:

```js
!!undefined + false; // -> 0
```

#### "" && -0

The [logical AND operator](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Logical_AND) is usually used with Boolean values in if-statements, but it actually returns the value of one of the operands. If the first expression can be converted to true, then it returns the second. Otherwise, it returns the first. An empty string is falsy, hence it returns the first operand.

```js
"" && -0; // -> ""
-0 && ""; // -> -0
5 && 3; // -> 3
0 && 3; // -> 0
```

#### +!!NaN * "" - - [,]

The finale combines some of the bizarre syntax that I've covered in this quiz. I've explained all of this behavior already in the explanations above, except for the multiplication operator `*`, which will coerce the empty string into its numeric equivalent: 0.

```js
+!!NaN; // -> 0
+""; // -> 0
-[,]; // -> -0
```

Add it all together:

```js
0 * 0 - -0; // -> 0
```
