# Bit Manipulation

Bit manipulation means working directly with binary bits.

A bit is either `0` or `1`.

Every integer is stored as bits inside the computer.

Most of the time we use normal arithmetic, but some problems become cleaner with bit operations.

## Binary Basics

Decimal number `5` is binary:

```txt
101
```

That means:

```txt
1 * 4 + 0 * 2 + 1 * 1 = 5
```

Decimal `6` is:

```txt
110
```

Bits represent powers of two.

## Common Operators

AND:

```js
a & b
```

A bit is `1` only if both bits are `1`.

OR:

```js
a | b
```

A bit is `1` if either bit is `1`.

XOR:

```js
a ^ b
```

A bit is `1` if the bits are different.

NOT:

```js
~a
```

Flips bits.

Left shift:

```js
a << 1
```

Moves bits left.

This often multiplies by 2.

Right shift:

```js
a >> 1
```

Moves bits right.

This often divides by 2 for non-negative integers.

## AND

AND is useful for checking whether a bit is set.

```js
function isBitSet(num, bit) {
  return (num & (1 << bit)) !== 0;
}
```

If the result is not zero, that bit was set.

## OR

OR is useful for turning a bit on.

```js
function setBit(num, bit) {
  return num | (1 << bit);
}
```

This keeps all existing bits and turns on the selected bit.

## XOR

XOR is extremely useful.

Rules:

```txt
x ^ x = 0
x ^ 0 = x
```

This solves Single Number.

If every number appears twice except one number, XOR all values.

Pairs cancel out.

The unique number remains.

```js
function singleNumber(nums) {
  let result = 0;

  for (const num of nums) {
    result ^= num;
  }

  return result;
}
```

Time is `O(n)`.

Space is `O(1)`.

## Checking Even or Odd

The last bit tells if a number is odd.

```js
function isOdd(num) {
  return (num & 1) === 1;
}
```

If the last bit is `1`, the number is odd.

If the last bit is `0`, the number is even.

## Power of Two

A power of two has exactly one bit set.

Examples:

```txt
1  -> 0001
2  -> 0010
4  -> 0100
8  -> 1000
```

The trick:

```js
function isPowerOfTwo(n) {
  return n > 0 && (n & (n - 1)) === 0;
}
```

Why?

`n - 1` flips the rightmost set bit and everything after it.

For powers of two, `n & (n - 1)` becomes zero.

## Bitmasking

A bitmask uses bits to represent choices.

If there are 3 items:

```txt
bit 0 -> item 0
bit 1 -> item 1
bit 2 -> item 2
```

Mask `101` means choose item 0 and item 2.

You can generate all subsets using masks from `0` to `2^n - 1`.

```js
function subsets(nums) {
  const result = [];
  const total = 1 << nums.length;

  for (let mask = 0; mask < total; mask++) {
    const subset = [];

    for (let i = 0; i < nums.length; i++) {
      if ((mask & (1 << i)) !== 0) {
        subset.push(nums[i]);
      }
    }

    result.push(subset);
  }

  return result;
}
```

Time complexity is `O(n * 2^n)`.

That is expected because there are `2^n` subsets.

## Common Use Cases

Bit manipulation appears in:

- Single Number.
- Counting bits.
- Power of two.
- Subset generation.
- Bitmask DP.
- Permissions and flags.
- Low-level optimization.

## Common Mistakes

The first mistake is using bit tricks without understanding them.

Bit manipulation should make the solution clearer, not more mysterious.

The second mistake is forgetting operator precedence.

Use parentheses.

The third mistake is not considering negative numbers.

Negative numbers can behave differently because of two’s complement representation.

The fourth mistake is using bitmasking when `n` is too large.

Subsets are `2^n`, so this grows fast.

## Interview Explanation

A strong explanation sounds like this:

“I use XOR because equal numbers cancel out. Since x XOR x is 0 and x XOR 0 is x, all duplicate pairs disappear and the unique number remains. The algorithm scans once, so time is O(n), and it uses O(1) extra space.”

## Final Understanding

Bit manipulation is about seeing numbers as binary.

It is powerful for flags, masks, parity, cancellation, and subsets.

Do not memorize tricks blindly.

Understand what each bit operation changes.

