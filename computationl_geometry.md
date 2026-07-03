# Computational Geometry

Computational geometry is about solving problems involving points, lines, and shapes in space — the math behind graphics, mapping, robotics, and collision detection. Here they are in plain language.

## The core idea of computational geometry

This field takes geometric questions we grasp intuitively by looking — "which points are on the outer edge?", "do these two lines cross?", "is this point inside that shape?" — and turns them into precise procedures a computer can follow using coordinates and arithmetic. The recurring challenge is that what's obvious to human eyes must be reduced to careful calculations, often boiling down to one fundamental question asked over and over: given three points, does the path turn left, turn right, or go straight? That simple "which way does it turn" test is the atom from which much of this field is built.

## Convex Hull (Graham Scan, Jarvis March, Andrew's Monotone Chain)

The convex hull is the smallest shape that wraps around a set of scattered points — imagine stretching a rubber band around a scatter of nails and letting it snap tight; the outline it forms is the hull. Finding it is a foundational geometry task, and there are several methods.

Jarvis March (also called gift-wrapping) mimics the intuitive idea directly: start at an extreme point and repeatedly "wrap" to the next outermost point, like wrapping paper around the outside, until you circle back. It's simple but slower when many points lie on the hull. Graham Scan first sorts points by angle around a reference point, then walks through them keeping only those that maintain the outward turn, discarding any that would cave inward. Andrew's Monotone Chain is a tidy variant that sorts points left-to-right and builds the hull's top and bottom edges separately, then joins them. All three find the same hull; they differ in strategy and efficiency, with the sorting-based ones generally faster.

## Line Segment Intersection

Determining whether (and where) two line segments cross. This sounds trivial but has subtle cases — segments that merely touch at an endpoint, or lie along the same line partially overlapping. The clean way to test it uses the "which way does it turn" orientation check: two segments cross if the endpoints of each lie on opposite sides of the other. This basic test scales up into detecting intersections among many segments at once (using the sweep line below) and is fundamental to graphics, mapping overlaps, and collision detection.

## Closest Pair of Points

Finding the two points nearest each other among many scattered points. Checking every possible pair is slow, so the efficient approach uses divide and conquer (which you saw earlier): split the points into left and right halves, find the closest pair within each half, then carefully check the narrow strip straddling the dividing line where a closer cross-boundary pair might hide. The clever insight that only a few points in that strip need comparing is what keeps it fast. It's a showcase of applying a general algorithmic strategy to a geometric problem.

## Point in Polygon (ray casting)

Determining whether a given point lies inside or outside a shape. The elegant "ray casting" method: imagine shooting a ray outward from the point in any direction and counting how many times it crosses the polygon's edges. An odd number of crossings means the point is inside; an even number means outside. The intuition is that each crossing flips you between inside and outside, so odd crossings leave you trapped within. It's the standard technique behind "is this click inside this region?" in maps and graphics.

## Sweep Line Algorithm

A powerful general strategy rather than a single algorithm. You imagine a line sweeping across the plane (say, left to right), and instead of considering all geometric objects at once, you only track what the sweep line currently touches, processing events as the line encounters them — like a point entering or leaving. This transforms a two-dimensional problem into a sequence of simpler one-dimensional snapshots handled in order. It's the engine behind efficiently finding all intersections among many segments, and it applies broadly to problems about overlaps, closest pairs, and coverage. It's one of the most important paradigms in the whole field.

## Rotating Calipers

A technique for problems involving the extremes of a convex shape — like finding the two farthest-apart points, or the shape's width. Picture two parallel calipers (like a measuring tool's jaws) gripping the convex hull from opposite sides, then rotating them all the way around the shape. As they rotate, the contact points sweep through all the "opposing" pairs, letting you efficiently find things like the maximum distance across the shape or the tightest bounding rectangle. It's an elegant way to answer extreme-measurement questions about convex shapes in a single rotation.

## Area of Polygon (Shoelace formula)

A neat formula for computing the area of any polygon given its corner coordinates. Called the "shoelace" formula because the calculation involves cross-multiplying coordinates in a crisscross pattern that resembles lacing a shoe. You walk around the polygon's vertices in order, accumulating these cross-products, and the result gives the enclosed area. It works for any simple polygon regardless of shape, and it's remarkably simple for how general it is — a small, satisfying tool that turns a list of corners directly into an area.

## Bresenham's Line Algorithm

A classic algorithm for drawing a straight line on a pixel grid — deciding which pixels to light up to best approximate the line. The challenge is that a mathematical line passes through continuous space, but a screen has discrete pixels, so you must choose the closest pixels. Bresenham's cleverness is doing this using only simple integer addition and subtraction — no slow division or fractions — making it extremely fast, which mattered enormously in early computer graphics and still underlies low-level rendering. It's a beautiful example of translating a continuous ideal into efficient discrete steps.

The themes worth carrying away. First, notice how much rests on that one primitive — the orientation test ("do three points turn left, right, or straight?"). It's the foundation of convex hulls, segment intersection, and point-in-polygon, so internalizing that single idea unlocks much of the field. Second, the sweep line is the standout general strategy: whenever a geometry problem feels overwhelming because everything interacts with everything, imagining a line sweeping across and processing events in order is often the key to making it efficient. Third, several of these (closest pair via divide-and-conquer, the convex hull methods) show geometry borrowing the same algorithmic strategies you've seen throughout — sorting, divide and conquer, incremental building — just applied to points in space rather than numbers in a list.
