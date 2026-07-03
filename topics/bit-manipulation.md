# Bit Manipulation

Bit manipulation works directly with the individual on/off switches (bits) that make up every number inside a computer. It feels low-level and cryptic at first, but it powers some remarkably elegant tricks. Here they are in plain language.

## The core idea of bit manipulation

Every number in a computer is stored as a row of bits — tiny switches that are either on (1) or off (0). Normally we think of numbers by their value, but sometimes it's powerful to think of them as patterns of switches and manipulate those switches directly. This lets you do certain operations extraordinarily fast, pack lots of yes/no information into a single number, and pull off clever shortcuts that would otherwise take loops. The tools are a small set of bitwise operations — AND, OR, XOR, NOT, and shifting bits left or right — and the whole art is combining them cleverly.

## Set / Clear / Toggle / Check bit

These are the four fundamental operations on a single bit at a specific position — the alphabet everything else is built from.

Setting a bit forces it on regardless of its current state. Clearing forces it off. Toggling flips it to its opposite. Checking asks whether a particular bit is currently on or off without changing anything. Each is achieved by combining your number with a carefully constructed "mask" — a number with a 1 in just the position you care about — using the right bitwise operation. These four are the essential vocabulary; every fancier trick below is really just a clever arrangement of these basic moves.

## Count Set Bits (Brian Kernighan's)

This counts how many bits are switched on in a number (also called the "population count"). The naive way checks every single bit position one by one. Brian Kernighan's method is cleverer: it repeatedly removes the lowest set bit in one operation and counts how many times it can do so before the number becomes zero. The elegance is that it only loops once per set bit, not once per total bit — so a number with just a few bits on is counted very quickly, regardless of how large the number is. It's a classic example of a bit trick that beats the obvious approach.

## Power of Two check

A quick test for whether a number is a power of two (like 1, 2, 4, 8, 16). The insight is that a power of two, in binary, is always a single 1 followed by zeros. Subtracting 1 from such a number flips that lone 1 off and turns all the trailing zeros into 1s. So combining the number with its predecessor using AND yields zero only for powers of two. It's a one-line check that replaces what might otherwise be a loop, and it's a favorite for showing how understanding binary patterns leads to slick solutions.

## XOR tricks (single number, missing number, swapping)

XOR (exclusive-or) has a magical property: combining a value with itself cancels out to zero, and combining anything with zero leaves it unchanged. This makes it wonderful for finding "the odd one out."

Single number: if every value in a list appears twice except one, XOR-ing everything together cancels all the pairs, leaving just the lonely value. Missing number: XOR-ing a complete expected set against an incomplete actual set cancels all the present values, revealing the missing one. Swapping: you can exchange two variables' values using XOR without needing a temporary holding variable. All three exploit that same self-canceling behavior, which is why XOR shows up constantly in these elegant puzzles.

## Subset generation via bitmask

This uses the bits of a number to represent which items are chosen from a set. If you have a set of items, each bit position stands for one item — on means "included," off means "excluded." So a single number encodes an entire subset. By simply counting from zero up through all possible numbers of the right bit-width, you automatically generate every possible subset, because every combination of on/off bits corresponds to a different selection. It's a compact, fast way to enumerate all subsets, and it's the foundation of bitmask DP below.

## Fast Exponentiation (binary)

This computes a number raised to a large power quickly by exploiting the binary representation of the exponent. Instead of multiplying the base by itself the full number of times, it looks at the exponent's bits: it repeatedly squares the base, and only multiplies it into the running result at the bit positions that are switched on. Because the exponent's bit-length is tiny compared to its value, this reaches enormous powers in very few steps. It's a bit-manipulation lens on a technique that appears throughout cryptography and math-heavy computing.

## Gray Code

A special way of ordering numbers so that each consecutive value differs from the previous one by exactly one flipped bit. Ordinary counting often flips many bits at once (going from 7 to 8 flips four bits), but Gray code guarantees only a single bit ever changes between neighbors. This matters in hardware and signal systems where flipping multiple bits simultaneously could momentarily produce a garbled in-between value; the single-bit-change property avoids that hazard. Generating it is a neat, small bit trick, and it appears in puzzles, error correction, and physical position sensors.

## Lowest Set Bit (x & -x)

A tiny, famous trick that isolates the rightmost switched-on bit of a number — giving you a value with just that one bit on and everything else off. It works by combining a number with its negative counterpart, which, due to how computers represent negative numbers, cleverly cancels everything except that lowest set bit. It looks cryptic but is extremely useful — notably, it's the mechanism the Fenwick tree (from the tree section) uses to navigate its clever indexing. It's a building block that quietly powers other structures.

## Bitmask DP

This is dynamic programming where the state is a set of items, compactly encoded as the bits of a number (as in subset generation above). It's the tool for problems where you must track exactly which items have been handled — like the travelling salesman problem, where the state is "which cities have I visited so far." Because a bitmask can represent any subset as a single number, you can use it as an index into your DP table, letting you remember and look up answers for specific combinations. It only works when the number of items is small (since the number of subsets explodes quickly), but when it fits, it's the natural way to do DP over sets.

The themes to carry away. First, the four basic operations (set, clear, toggle, check) are the true foundation — every clever trick above is just those primitives arranged thoughtfully, so getting comfortable with masks is the real prerequisite. Second, notice the recurring superpower of XOR — its self-canceling property is behind an entire family of "find the odd one out" tricks, making it the single most useful operation to internalize. Third, the bitmask-as-a-set idea (subset generation and bitmask DP) is the bridge from low-level bit fiddling to serious algorithmic problem-solving, letting you represent and manipulate whole collections as single numbers — which is exactly why it shows up in the harder DP and graph problems you've already seen.
