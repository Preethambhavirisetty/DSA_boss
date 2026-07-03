# Arrays & Pointers

## Two Pointers

Instead of using one marker to walk through a list, you use two, and you move them intelligently based on what you find. A common setup places one pointer at each end of a sorted list, moving them toward each other — if the current pair is too big, you pull the right one inward; too small, you push the left one outward. Another setup has both pointers moving the same direction at different speeds. The whole point is that by coordinating two markers, you often solve in one pass what would otherwise take checking every possible pair. It replaces nested loops with a single sweep.

## Sliding Window

This is a specialized two-pointer pattern for problems about contiguous stretches of a list — like "find the best run of consecutive elements." Picture a window framing a section of the list. Instead of recalculating everything each time you consider a new section, you slide the window: add the new item entering on the right, remove the item leaving on the left, and update your running total. You reuse most of the previous work instead of starting over.

In the fixed-size version, the window is always the same width — you're checking every stretch of exactly K items. In the variable-size version, the window grows and shrinks based on a condition — you expand the right edge to include more, and when you've gone too far (say, a sum got too big), you shrink from the left until you're valid again. That expand-then-contract dance is what makes it powerful for "longest/shortest stretch that satisfies some rule" problems.

## Prefix Sum / Suffix Sum

This is about doing preparation once so you can answer many questions instantly. A prefix sum is a new list where each position holds the running total of everything up to that point. Once you've built it, the sum of any stretch of the original list becomes a single subtraction — the total up to the end of the range minus the total up to just before it. Without prefix sums, adding up a range means looping through it every time; with them, it's instant. Suffix sum is the mirror image, accumulating from the right end instead. The trade is a bit of upfront setup and extra memory in exchange for lightning-fast range queries afterward.

## Difference Array

This is prefix sum's clever counterpart, built for the opposite situation: when you need to add a value to many ranges of a list repeatedly. Naively, adding 5 to every element in a big range means touching each one. The difference array trick lets you record just two marks — one where the range starts and one where it ends — no matter how big the range is. After all your range updates are recorded as these lightweight marks, a single prefix-sum pass at the end "resolves" them into the final list. So many range-updates become cheap, and you only pay the full cost once at the very end.

## Kadane's Algorithm

This finds the contiguous stretch with the largest sum, and it's beautifully simple once it clicks. You walk through the list keeping a running sum. At each step you ask one question: "is it better to extend the stretch I've been building, or to abandon it and start fresh from here?" If your running sum has gone negative, it's dragging you down, so you drop it and start over. You keep track of the best total you've seen along the way. That single-pass, one-decision-per-step logic is what makes it elegant — no need to check every possible stretch.

## Dutch National Flag

This sorts a list containing just three distinct kinds of values (classically pictured as three colors, like a flag) in a single pass. You use three pointers to partition the list into three regions: known-low at the front, known-high at the back, and the unsorted middle shrinking as you go. As you examine each middle item, you swap it toward the front, toward the back, or leave it, adjusting the boundaries. It's the efficient answer to "sort an array of 0s, 1s, and 2s" without doing a full general-purpose sort, and the same partitioning idea powers three-way quick sort.

## Boyer-Moore Voting

This finds the majority element — a value that appears more than half the time — using almost no extra memory, which feels almost magical. You keep one candidate and one counter. As you walk the list, if you see your candidate, you bump the counter up; if you see anything else, you bump it down. When the counter hits zero, you adopt the current item as your new candidate. The intuition is that a true majority element is so common it can't be fully "cancelled out" by everything else combined, so it survives to the end. It solves in one pass what looks like it should require counting everything.

## Merge Intervals

This handles problems about overlapping ranges — like combining calendar meetings that overlap into single blocks. The key first step is sorting the intervals by their start points. Once sorted, you sweep through them left to right, and whenever the next interval overlaps the one you're currently building, you merge them by extending the end. When it doesn't overlap, you finalize the current block and start a new one. Sorting first is what makes this a clean single pass instead of comparing every interval against every other.

## Cyclic Sort

This is a specialized, super-efficient sort for a very specific situation: when your list contains numbers from a known continuous range, like 1 through N. The insight is that each number has an obvious "home" position — the number 3 belongs at index 3. So you walk through and, whenever a number isn't in its home, you swap it directly to where it belongs, and repeat until each spot holds its rightful number. Because of this, it's the go-to pattern for problems like "find the missing number" or "find the duplicate" in a range, since after placing everything, whatever's out of place immediately reveals the answer.

## Floyd's Tortoise and Hare

This detects a loop in a sequence using two pointers moving at different speeds — a slow one taking single steps and a fast one taking double steps (hence tortoise and hare). If the sequence has no loop, the fast one simply reaches the end. But if there is a loop, the fast pointer will eventually lap around and collide with the slow one, like two runners on a circular track where the faster inevitably catches the slower from behind. The beauty is that it detects cycles using essentially no extra memory — just two markers — instead of storing everything you've visited to check for repeats. It's famous for finding loops in linked lists and for spotting duplicates framed as cycles.

## Monotonic Stack / Monotonic Deque

A monotonic stack is a stack (a last-in-first-out pile) that you deliberately keep in sorted order — always increasing or always decreasing. The trick is that before adding a new item, you pop off anything that would break the order. This popping isn't wasted work — the moment you pop something, you've often just discovered a meaningful relationship, like "this new item is the first bigger value to the right of the popped one." It's the engine behind a whole family of "find the nearest bigger/smaller element" problems. A monotonic deque is the same idea but using a double-ended structure so you can remove from both ends, which lets it efficiently track the max or min within a sliding window as the window moves.

## Next Greater / Next Smaller Element

This is the classic problem the monotonic stack was made for: for each item in a list, find the first item to its right (or left) that's bigger (or smaller) than it. The brute-force way looks rightward from every element, which is slow. The monotonic stack solves it in one pass: you keep a stack of items still "waiting" to find their next greater element, and the moment a new item arrives that's bigger than what's waiting on top, you've found the answer for those waiting items and pop them off. Each item gets pushed and popped just once, which is what makes it fast. This pattern quietly underlies many harder problems — stock spans, temperatures, histogram areas — so recognizing it is high-leverage.

The unifying theme across all of these: avoid redundant work by remembering something as you go. Two pointers and sliding windows reuse a moving frame; prefix sums and difference arrays precompute once; monotonic stacks and Floyd's remember just enough state to skip re-examining things. Almost every one converts an obvious "check everything against everything" solution into a single smart pass.
