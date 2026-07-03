# Linear Search

You look at each item one by one, from start to finish, until you find what you want. That's it. It works on anything because it doesn't care whether the list is organized. The downside is that a big list means a lot of looking. This is what you do when the data is a mess with no order to take advantage of.

## Binary Search

This only works if the list is already sorted. You check the middle item. If your target is smaller, you know it's in the left half, so you ignore the right half entirely. If it's bigger, you ignore the left half. You keep cutting what's left in half each time, so even a huge list gets solved in just a few steps. The whole trick is that one look lets you throw away half the list.

## Binary Search on Answer

Here you're not searching a list — you're searching through possible answers. Imagine you're guessing a number, and after each guess someone tells you "too low" or "too high." As long as answers split cleanly into "works" and "doesn't work" with a single dividing line, you can keep guessing the middle and cutting the possibilities in half. It's great for problems like "what's the smallest size that still gets the job done" — you're binary searching, but over answers instead of over an array.

## Ternary Search

Instead of cutting the range in two, you cut it into three parts using two test points. This is mainly for finding the top of a hill or the bottom of a valley — a shape that goes up then down, or down then up. By comparing your two test points, you can tell which third of the range definitely doesn't contain the peak, and toss it. For plain sorted lists it's actually worse than binary search, so you only reach for it when you're hunting for a high point or low point.

## Exponential Search

Use this when the list is sorted but you don't know how long it is, or the thing you want is probably near the front. You check position 1, then 2, then 4, then 8, doubling each time until you overshoot your target. Now you know it's somewhere in that last stretch, so you do a normal binary search inside just that stretch. The doubling quickly boxes in the right area, then binary search finishes it off.

## Interpolation Search

This is binary search that makes a smart guess about where to look instead of always checking the exact middle. It's like a phone book: to find "Zach," you flip toward the back, not the center. It works beautifully when the values are spread out evenly, because your guesses land close to the target. But if the values are lumpy or uneven, the guesses go wrong and it slows down. Great under the right conditions, unreliable otherwise.

## Jump Search

A compromise between the slow one-by-one method and full binary search, for sorted lists. You leap forward in fixed-size chunks — skipping ahead until you pass your target — then walk backward through just that one chunk to find it. The best chunk size is around the square root of the list length. It's handy when jumping forward is cheap but jumping around freely is expensive, since it only ever backtracks within one chunk.

## Fibonacci Search

This is like binary search, but instead of splitting the range in half, it splits it using Fibonacci numbers (that sequence where each number is the sum of the two before it). The special thing is that it only needs addition and subtraction to figure out where to look — no division. That mattered on old machines where division was slow, and it still helps in cases where reaching nearby items is cheaper than reaching far-off ones.

## Meta Binary Search

Instead of narrowing down a range, this builds the answer digit by digit in binary — deciding each bit from the biggest down to the smallest. It ends up being just as fast as regular binary search but thinks about the problem as "constructing the answer one piece at a time." It's mostly a niche trick for competitive programming, useful because it avoids calculating midpoints.

## Lower Bound / Upper Bound

These are versions of binary search that find edges instead of just "is it here or not." Lower bound finds the first spot where a value is not smaller than your target — basically the leftmost place it belongs. Upper bound finds the first spot just past your target. Put them together and you can count how many times a value appears, or find exactly where a new item should be inserted, all still in that fast few-steps way. It's the same halving idea, just tuned to land on the border between two zones rather than stopping at the first match. Most real-world binary search is actually this.

The big idea is called monotonicity, and once you see it, half of these algorithms turn into the same algorithm wearing different outfits.

## The core picture: a clean dividing line

Imagine a long line of light switches, and somewhere along that line, every switch to the left is OFF and every switch to the right is ON. There's exactly one spot where OFF turns into ON, and it never flips back. That's monotonicity: the property only changes once, in one direction.

Now here's the key question — if you're standing at some random switch and you check it, what do you learn?

If you land on an ON switch, you instantly know the dividing line is somewhere to your left, so everything to your right is useless to check. If you land on an OFF switch, the line is to your right, so everything to your left is useless. Either way, one look lets you throw away half the switches. That's binary search. It's not really about numbers or sorting — it's about that single, one-time flip from "no" to "yes."

## Why sorted arrays are just a special case

When people first learn binary search, they think the rule is "the array must be sorted." But sorting is just one way to create that OFF-to-ON pattern. Ask the question "is this element greater than or equal to my target?" In a sorted array, the answer is NO, NO, NO... then YES, YES, YES — it flips exactly once. The sorting is only there to guarantee the flip happens once and never reverses. The flip is the real thing you're searching for.

## How this unifies the list

Once you stop thinking "search for a value" and start thinking "find where the answer flips," the connections appear:

Lower bound and upper bound are just asking about the flip point with slightly different questions. Lower bound asks "where does 'less than the target' turn into 'not less than'?" Upper bound asks "where does 'less than or equal' turn into 'greater'?" Same switch-line, two different questions about where things flip. That's why they find the edges of a group of equal values — you're locating boundaries, which is literally what the flip is.

Binary search on answer is the same idea applied to a made-up line of switches you never actually build. Instead of an array, picture every possible answer laid out in order, and for each one ask "does this work?" If bigger answers always keep working once one works, you've got NO, NO, NO... YES, YES, YES again — a clean flip — so you binary-search for it. The array is imaginary, but the switch-line is real.

Where ternary search breaks the pattern (and why that matters)

Ternary search is the odd one out, and understanding why deepens the whole idea. Binary search needs a one-time flip. But a hill — going up, reaching a peak, then coming down — doesn't flip once. It's YES it's rising... YES... then NO it's falling... NO. That's two changes of direction, not one, so plain binary search can't handle it.

But notice there's still hidden monotonicity if you look at it right: to the left of the peak, things are always rising, and to the right, they're always falling. Each side is clean on its own. Ternary search uses two probes to figure out which side of the peak you're on, so it can shave off a chunk that definitely doesn't contain the top. So it's the same spirit — exploit a property that changes in a predictable, one-directional way — just adapted for a shape with a single turning point instead of a single flip.

## The takeaway

Don't memorize these as separate tricks. Ask one question: "Is there something about this problem that changes exactly once, cleanly, in one direction?" If yes, you can cut the search space in half every step. Sorted values, feasibility of an answer, the boundary of a group, the slope of a hill — they're all just different flavors of that same one-time change. The moment you can spot the flip, you know binary search (or its cousin) applies, even when there's no array in sight.

That last skill — spotting an invisible flip in a problem that doesn't look like a search problem — is what separates people who know binary search from people who can actually use it under interview pressure.
