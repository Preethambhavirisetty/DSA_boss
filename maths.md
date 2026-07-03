# Maths

Number theory is the mathematics of whole numbers — primes, divisibility, remainders — and it shows up constantly in cryptography, hashing, and competitive programming. Here they are in plain language.

## The core theme: primes and remainders

Two ideas thread through almost everything here. First, prime numbers (numbers divisible only by themselves and 1) are the building blocks of all integers, so a lot of number theory is about finding them, testing for them, and breaking numbers into them. Second, modular arithmetic — working with remainders after division, like how clocks wrap around at 12 — is everywhere, because it keeps numbers manageable and underlies cryptography. Many algorithms below exist to do arithmetic efficiently within this wrap-around world.

## Sieve of Eratosthenes

An ancient, elegant method for finding all prime numbers up to some limit. Rather than testing each number individually, you list all numbers and then systematically cross out the multiples of each prime: cross out every multiple of 2, then every multiple of 3, then 5, and so on. Whatever survives uncrossed is prime. It's efficient because it eliminates composites in bulk by their factors rather than checking each number in isolation. It's the standard first tool whenever you need many primes at once.

## Segmented Sieve

A variation for when the limit is so enormous that you can't hold the whole list in memory at once. Instead of sieving the entire range in one go, it processes the range in manageable segments, using the small primes found early to cross out composites within each segment separately. This keeps memory use low while still finding primes across a huge span. It's the practical adaptation of the sieve for very large ranges that wouldn't otherwise fit.

## Euclidean Algorithm (GCD)

A beautifully simple, ancient method for finding the greatest common divisor — the largest number that divides two numbers evenly. The insight is that the GCD of two numbers equals the GCD of the smaller number and the remainder when you divide them. By repeatedly replacing the pair with (smaller number, remainder), the numbers shrink rapidly until one becomes zero — and the other is your answer. It's remarkably fast and is one of the oldest algorithms still in everyday use.

## Extended Euclidean Algorithm

An enhancement that finds the GCD and something more: it also determines how to combine the two original numbers (with appropriate multipliers) to actually produce that GCD. This extra information is the key that unlocks modular inverses (below) and solving certain equations. So while the plain version answers "what's the GCD," the extended version answers "and here's exactly how to build it from the originals" — which turns out to be essential for cryptographic math.

## Modular Exponentiation

Computing a number raised to a large power, but keeping only the remainder against some modulus — done efficiently. Cryptography routinely raises numbers to astronomically large powers within modular arithmetic, and computing that directly would produce impossibly huge intermediate numbers. Modular exponentiation combines the fast binary-exponentiation trick (squaring repeatedly) with taking the remainder at every step, keeping numbers small throughout. It's a workhorse of encryption systems like RSA.

## Modular Inverse (Fermat's Little Theorem)

In ordinary arithmetic, dividing is straightforward, but in the wrap-around world of modular arithmetic, there's no direct division — instead you multiply by a modular inverse, the value that plays the role division would. Finding it can be done via the extended Euclidean algorithm, or, when the modulus is prime, via a neat shortcut from Fermat's Little Theorem (a classical result about how numbers behave under prime moduli). This is essential because so many number-theory computations, especially combinatorics under a modulus, require "dividing" in modular space.

## Chinese Remainder Theorem

A classical result for reconstructing a number from its remainders against several different divisors. If you know that some unknown number leaves remainder 2 when divided by 3, remainder 3 when divided by 5, and so on, this theorem lets you piece together the unique number satisfying all those conditions at once (as long as the divisors share no common factors). It's useful for breaking one hard modular computation into several smaller, independent ones and then recombining — a divide-and-conquer for modular arithmetic.

## Miller-Rabin Primality Test

A fast way to check whether a large number is prime — far quicker than trying to divide it by everything. It's a probabilistic test: it applies clever mathematical checks that a prime would always pass, and repeatedly tries them with different witnesses. A composite number will almost certainly fail one of these checks, so after enough rounds, passing them all makes primality overwhelmingly likely. For truly gigantic numbers where exact testing is impractical, this near-certain probabilistic answer is the standard, and it's central to generating the large primes cryptography depends on.

## Pollard's Rho (factorization)

A clever method for factoring a number — breaking it into its prime components — that's much faster than trial division for large numbers with non-trivial factors. It uses a pseudo-random sequence and cycle-detection (the tortoise-and-hare idea from earlier) to stumble upon a factor efficiently. Factoring large numbers is famously hard (the difficulty is exactly what keeps RSA encryption secure), and Pollard's Rho is one of the practical tools for tackling moderately large numbers where brute force would be hopeless.

## Euler's Totient Function

This counts, for a given number, how many smaller numbers share no common factor with it (are "coprime" to it). This count has deep significance in number theory and cryptography — it governs how numbers behave under modular exponentiation and is a crucial ingredient in RSA's mathematics. It can be computed efficiently from the number's prime factorization. It's less an algorithm and more a fundamental quantity that many cryptographic and modular results depend on.

## Fast Fourier Transform (FFT)

A landmark algorithm that dramatically speeds up multiplying large polynomials (or large numbers), among many other uses. Multiplying two polynomials directly is slow, but the FFT cleverly transforms them into a different representation (based on evaluating them at special points), where multiplication becomes simple point-by-point multiplication, then transforms the result back. This transform-multiply-untransform trip is far faster than the direct approach for large inputs. Beyond math, it's foundational to signal processing, audio and image compression, and much of modern engineering — one of the most important algorithms ever devised.

## Number Theoretic Transform (NTT)

Essentially the FFT adapted to work purely with integers under a modulus, avoiding the fractional numbers the standard FFT uses. Those fractional numbers can introduce tiny rounding errors, which is unacceptable when you need exact answers (like in cryptography or precise big-number multiplication). The NTT performs the same transform-multiply-untransform magic but entirely in modular integer arithmetic, giving exact results. It's the go-to when you need FFT's speed but demand perfect precision.

## Matrix Exponentiation

A technique for computing certain sequences or repeated-transformation results extremely fast by raising a matrix (a grid of numbers) to a high power — using the same fast binary-exponentiation trick applied to matrices. Its most famous use is computing far-off terms of sequences like Fibonacci almost instantly: you express the sequence's step as a matrix, then raise it to a huge power quickly by repeated squaring, rather than iterating one step at a time. Any process that advances by a fixed linear rule can often be leaped forward this way.

## Catalan Numbers

A special sequence of numbers that surprisingly answers a whole family of counting problems: how many ways to correctly nest parentheses, how many distinct binary tree shapes exist, how many ways to triangulate a polygon, and more. These seemingly unrelated problems all turn out to have the same count, given by this sequence. Recognizing that a problem is "a Catalan number in disguise" instantly gives you the answer via a known formula. It's a beautiful example of deep unity between problems that look nothing alike on the surface.

## Combinatorics (nCr with modular arithmetic)

Combinatorics is the math of counting arrangements and selections, and "nCr" — the number of ways to choose r items from n — is its most common quantity. The twist here is computing these counts under a modulus, which competitive programming and cryptography frequently require because the raw numbers grow astronomically large. This demands combining factorial computations with modular inverses (since the formula involves division, which becomes modular-inverse multiplication in modular space). It ties together several earlier tools — modular arithmetic, inverses, Fermat's theorem — into the practical skill of counting huge quantities while keeping numbers bounded.

The organizing themes. First, notice how much of this serves cryptography — modular exponentiation, modular inverses, Miller-Rabin, Pollard's Rho, and Euler's totient are essentially the mathematical machinery behind systems like RSA, where the security rests on primes being easy to find but products being hard to factor. Second, several tools are about doing arithmetic efficiently within the wrap-around world of remainders, because keeping numbers small is what makes huge computations feasible. Third, the FFT (and its integer cousin NTT) stands apart as a genuinely profound algorithm whose reach extends far beyond number theory into all of signal processing and engineering. And running underneath it all is the fast-exponentiation-by-squaring idea, which you've now seen recur for plain numbers, modular numbers, and even matrices — a single trick with remarkable range.
