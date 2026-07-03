# Hashing

Hashing is built on one deceptively simple idea that turns out to be one of the most powerful tools in computing. Here it is in plain language.

## The core idea of hashing

Hashing is about converting some piece of data — a name, a number, a whole string — into a fixed number that acts as its address. Imagine wanting to store items so you can find them instantly. Instead of searching through everything, you run each item through a "hash function" that spits out a location, and you put the item there. Later, to find it, you run the same function, get the same location, and go straight to it — no searching. This is what makes hashing magical: lookups that would otherwise require scanning become nearly instant. The entire field is really about two things: designing hash functions that spread items out evenly, and handling collisions — the unavoidable cases where two different items get sent to the same location.

## Hash Table (chaining & open addressing)

The fundamental structure built on hashing: an array of slots where each item's slot is decided by its hash. Since collisions are inevitable (more possible items than slots), there are two main strategies for handling them.

Chaining handles a collision by letting each slot hold a small list — when multiple items land in the same slot, they simply join that slot's list, and you search the short list to find your item. Open addressing takes a different approach: when an item's slot is taken, it follows a rule to probe for the next available slot elsewhere in the array, storing the item there instead. Chaining is simpler and handles heavy loads gracefully; open addressing keeps everything in one compact array with no extra lists, which can be faster due to how computer memory works. This choice is the fundamental design decision of any hash table.

## Linear / Quadratic Probing, Double Hashing

These are the three ways open addressing decides where to look next when a slot is already taken.

Linear probing simply checks the very next slot, then the next, and so on until it finds a free one — dead simple, but it tends to create clusters where occupied slots bunch together, slowing things down. Quadratic probing jumps in growing steps instead (one away, then four away, then nine away), which spreads items out better and reduces clustering. Double hashing uses a second hash function to decide the jump distance, so different items follow different probing paths, scattering them most effectively of all. The progression from linear to quadratic to double hashing is essentially a series of increasingly clever answers to the clustering problem.

## Rolling Hash

A special hash designed for sliding windows over sequences, letting you update the hash cheaply as the window moves. Normally, hashing a chunk of characters means processing all of them; but if you're sliding one position at a time, recomputing from scratch each time is wasteful. A rolling hash lets you update the hash by removing the contribution of the character that left and adding the one that entered — a quick adjustment instead of a full recompute. This makes it the backbone of fast string-searching (like Rabin-Karp) and anywhere you need to fingerprint many overlapping chunks of a sequence efficiently.

## Perfect Hashing

A technique for a fixed, known set of items where you want to guarantee zero collisions — every item gets its own unique slot, so lookups are always instant with no probing or lists. Since collisions are normally unavoidable, this only works when you know all the items in advance and can carefully design the hash function (often using two layers) to fit them perfectly. It's used for static datasets that never change but must be looked up as fast as physically possible, like reserved keywords in a programming language.

## Consistent Hashing

A specialized hashing scheme for distributing data across multiple servers in a way that survives servers being added or removed gracefully. The problem it solves: with ordinary hashing, if you have N servers and one goes down, nearly all your data would need to be reshuffled to new servers — a catastrophe at scale. Consistent hashing arranges servers and data around a conceptual ring, so that adding or removing a server only reshuffles a small fraction of the data (the portion near the changed server) rather than nearly all of it. It's a cornerstone of large distributed systems, databases, and caching networks — precisely the kind of infrastructure concern that matters when systems scale.

## Cuckoo Hashing

A collision strategy named after the cuckoo bird, which shoves other eggs out of nests. Each item has two possible slots (via two hash functions). When you insert an item and both its slots are somehow occupied, it kicks out an existing item to its alternate slot — and if that slot is taken, the displaced item kicks out its occupant, and so on, until everything settles. The payoff is that lookups are guaranteed fast: any item is in one of just two places, so you check at most two slots. It trades more complex insertions for a strong guarantee on lookup speed.

## Bloom Filter

A brilliant, space-saving structure that answers one question — "have I possibly seen this item before?" — using tiny memory, at the cost of occasional false alarms. Instead of storing items, it records their "fingerprints" by flipping bits in a bit-array using several hash functions. To check membership, it looks at those same bits: if any are off, the item is definitely not present; if all are on, it's probably present (but might be a false positive). Crucially, it never gives false negatives — it never wrongly says "no." This makes it perfect for cheaply filtering out definite non-members before doing expensive lookups, used everywhere from databases skipping disk reads to browsers checking malicious URLs.

## Count-Min Sketch

A close cousin of the Bloom filter that estimates how many times it has seen each item, again using very little memory. Rather than exact counts (which would require storing every distinct item), it maintains a compact grid of counters and, using several hash functions, bumps up counters for each item seen. To estimate an item's count, it checks those counters and takes the smallest (since collisions can only inflate counts, the minimum is the most trustworthy estimate). It may overcount but never undercounts. It's used for tracking frequencies in massive high-speed data streams — like finding the most common search queries or heaviest network traffic — where exact counting is too expensive.

## HyperLogLog

An almost magical structure that estimates how many distinct items it has seen — the count of unique values — using a startlingly tiny amount of memory, even for billions of items. Instead of remembering which items it's seen (which would require enormous storage), it hashes items and observes statistical patterns in the results — specifically, tracking rare patterns that only tend to appear when many distinct items have passed through. From those patterns it infers the approximate number of unique items. It trades exactness for extreme memory efficiency, and it's used at massive scale to count things like unique website visitors or distinct users, where exact counting would be wildly impractical.

The organizing themes worth carrying away. First, everything flows from the basic tension in hashing: turning data into addresses for instant access is easy, but collisions are inevitable, and the classic structures (chaining, open addressing, cuckoo, perfect hashing) are just different philosophies for handling them. Second — and this is the part most relevant to systems work — notice the family of probabilistic structures at the end (Bloom filter, Count-Min sketch, HyperLogLog). They all make the same profound trade: give up exactness in exchange for enormous memory savings, which is exactly what you need when data is too big or too fast to store fully. Third, consistent hashing stands apart as the distributed systems member of the family, solving how to spread data across many machines without chaos when machines come and go.

Given your focus on AI infrastructure and distributed systems, those last four — consistent hashing, Bloom filters, Count-Min sketch, and HyperLogLog — are the ones that show up constantly in real production systems (caching layers, database internals, stream processing, deduplication at scale).
