# Greedy

A greedy algorithm makes the best local choice at each step.

It does not try every possibility.

It chooses what looks best right now and moves forward.

Greedy can be very efficient, but it only works when the local best choice is safe.

## The Core Question

For every greedy problem, ask:

If I make this choice now, can it block a better answer later?

If the answer is yes, greedy may fail.

If the answer is no and you can explain why, greedy may work.

Greedy is not about guessing.

It needs a reason.

## Simple Example

Suppose you want to attend the maximum number of meetings.

Each meeting has a start and end time.

The greedy idea is:

Always choose the meeting that ends earliest.

Why?

Because it leaves the most room for future meetings.

Sorting by end time makes the safe choice visible.

## Interval Scheduling

```js
function maxMeetings(intervals) {
  intervals.sort((a, b) => a[1] - b[1]);

  let count = 0;
  let lastEnd = -Infinity;

  for (const [start, end] of intervals) {
    if (start >= lastEnd) {
      count++;
      lastEnd = end;
    }
  }

  return count;
}
```

Sorting takes `O(n log n)`.

The scan takes `O(n)`.

Total time is `O(n log n)`.

## Why Sorting Often Appears

Greedy choices are easier when data is ordered.

Sorting can reveal the correct decision order.

Examples:

- Sort intervals by end time.
- Sort jobs by deadline or profit.
- Sort numbers before two-pointer greedy.
- Sort costs before choosing cheapest options.

Sorting is often the setup step for greedy.

## Greedy vs Dynamic Programming

Greedy makes one choice and does not revisit it.

Dynamic programming explores choices through states and remembers answers.

If a local choice can lead to a bad future, DP may be needed.

If a local choice is always safe, greedy is better and simpler.

## Counterexample Thinking

To test greedy, try to break it.

Example: coin change.

For US coins, choosing the largest coin first often works.

But for arbitrary coin systems, greedy can fail.

Coins:

```txt
1, 3, 4
```

Amount:

```txt
6
```

Greedy chooses `4 + 1 + 1`, which uses 3 coins.

Best is `3 + 3`, which uses 2 coins.

So greedy is not always safe.

## Common Greedy Problems

Greedy appears in:

- Jump Game.
- Merge intervals.
- Meeting rooms.
- Gas station.
- Assign cookies.
- Non-overlapping intervals.
- Partition labels.
- Minimum arrows to burst balloons.

## Complexity

Greedy algorithms are often:

- `O(n)` if no sorting is needed.
- `O(n log n)` if sorting is needed.

Space is often `O(1)` or depends on sorting/output.

## Common Mistakes

The first mistake is using greedy without proof.

The second mistake is sorting by the wrong field.

The third mistake is assuming local maximum always leads to global maximum.

The fourth mistake is ignoring counterexamples.

The fifth mistake is using DP when a simple greedy rule is enough.

## Interview Explanation

A strong explanation sounds like this:

“I use a greedy strategy because choosing the earliest ending interval is safe. It never reduces the number of future intervals we can choose; it only leaves more room. After sorting by end time, I scan once and take every compatible interval. Sorting dominates the time, so the complexity is O(n log n).”

The proof is the important part.

## Final Understanding

Greedy is simple code with deep reasoning.

The code is often short.

The hard part is proving the choice is safe.

