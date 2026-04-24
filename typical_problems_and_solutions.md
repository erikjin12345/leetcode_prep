# Typical Problems and Solutions — Haoxing Quant Assessment Prep

A curated set of problems representative of what you'll see on a Chinese quant-firm LeetCode Enterprise assessment. Each problem includes:

- **Problem statement** (clear and self-contained)
- **Example** (concrete input and output)
- **Approach** (how to think about it before coding)
- **Solution** (clean Python implementation)
- **Complexity analysis** (time and space)
- **Key insight** (the "aha" that separates the right solution from a slow one)

Problems are grouped by pattern and ordered roughly by difficulty within each group.

---

# Table of Contents

1. [Arrays & Hash Maps](#1-arrays--hash-maps)
2. [Two Pointers](#2-two-pointers)
3. [Sliding Window](#3-sliding-window)
4. [Binary Search](#4-binary-search)
5. [Stacks & Monotonic Stacks](#5-stacks--monotonic-stacks)
6. [Heaps / Priority Queues](#6-heaps--priority-queues)
7. [Stock & Trading DP](#7-stock--trading-dp)
8. [General Dynamic Programming](#8-general-dynamic-programming)
9. [Trees & Graphs](#9-trees--graphs)
10. [Probability & Expected Value](#10-probability--expected-value)
11. [Quant-Flavored Problems](#11-quant-flavored-problems)

---

# 1. Arrays & Hash Maps

## Problem 1.1 — Two Sum

**Statement:** Given an array `nums` and integer `target`, return indices `[i, j]` such that `nums[i] + nums[j] == target`. Exactly one solution exists, and you can't use the same element twice.

**Example:**
```
nums = [2, 7, 11, 15], target = 9
Output: [0, 1]  (because 2 + 7 = 9)
```

**Approach:** For each number, check if its complement `target - x` has been seen. Use a hash map `{value: index}` for O(1) lookup.

**Solution:**
```python
def two_sum(nums, target):
    seen = {}
    for i, x in enumerate(nums):
        if target - x in seen:
            return [seen[target - x], i]
        seen[x] = i
    return []
```

**Complexity:** O(n) time, O(n) space.

**Key insight:** One-pass hash lookup beats nested loops. This pattern — "remember what you've seen to avoid quadratic work" — appears everywhere.

---

## Problem 1.2 — Contains Duplicate

**Statement:** Given an integer array, return True if any value appears at least twice.

**Example:**
```
[1, 2, 3, 1] → True
[1, 2, 3, 4] → False
```

**Solution:**
```python
def contains_duplicate(nums):
    return len(set(nums)) < len(nums)
```

**Complexity:** O(n) time, O(n) space.

**Key insight:** Converting to a set deduplicates. If the length shrinks, there were duplicates.

---

## Problem 1.3 — Group Anagrams

**Statement:** Group a list of strings so that anagrams end up in the same group.

**Example:**
```
["eat","tea","tan","ate","nat","bat"]
→ [["eat","tea","ate"], ["tan","nat"], ["bat"]]
```

**Approach:** Anagrams share the same sorted-letter string. Use it as the dict key.

**Solution:**
```python
from collections import defaultdict

def group_anagrams(words):
    groups = defaultdict(list)
    for w in words:
        key = "".join(sorted(w))
        groups[key].append(w)
    return list(groups.values())
```

**Complexity:** O(n · k log k) where k = max word length.

**Key insight:** Hash-map grouping by a canonical form of each item is a go-to pattern.

---

## Problem 1.4 — Subarray Sum Equals K

**Statement:** Count the number of contiguous subarrays whose sum equals `k`.

**Example:**
```
nums = [1, 1, 1], k = 2
Output: 2  (subarrays [1,1] at indices 0-1 and 1-2)
```

**Approach:** A subarray sum from i to j equals `prefix[j+1] - prefix[i]`. We want this = k, so `prefix[i] = prefix[j+1] - k`. Use a hash map of prefix-sum counts seen so far.

**Solution:**
```python
def subarray_sum(nums, k):
    count = 0
    prefix = 0
    seen = {0: 1}  # empty prefix has sum 0
    for x in nums:
        prefix += x
        count += seen.get(prefix - k, 0)
        seen[prefix] = seen.get(prefix, 0) + 1
    return count
```

**Complexity:** O(n) time, O(n) space.

**Key insight:** Prefix sums + hash map turns an O(n²) subarray-sum problem into O(n). This pattern is extremely common.

---

## Problem 1.5 — Longest Consecutive Sequence

**Statement:** Given an unsorted array, find the length of the longest consecutive sequence (e.g., [3,4,5,6]).

**Example:**
```
[100, 4, 200, 1, 3, 2] → 4  (sequence [1,2,3,4])
```

**Approach:** Put all numbers in a set. For each number that starts a sequence (i.e., `n-1` is not in the set), count how long the sequence continues.

**Solution:**
```python
def longest_consecutive(nums):
    s = set(nums)
    best = 0
    for x in s:
        if x - 1 not in s:  # x is a sequence start
            length = 1
            while x + length in s:
                length += 1
            best = max(best, length)
    return best
```

**Complexity:** O(n) time, O(n) space. Each number is visited at most twice (once as a start, once inside a sequence).

**Key insight:** The "only start from the beginning of a sequence" check is what keeps this O(n). Without it, you'd revisit the same sequence from each element and get O(n²).

---

# 2. Two Pointers

## Problem 2.1 — Two Sum on Sorted Array

**Statement:** Given a sorted array, find two indices whose values sum to target.

**Example:**
```
arr = [1, 3, 5, 7, 9], target = 10
Output: (1, 3)  (3 + 7 = 10)
```

**Solution:**
```python
def two_sum_sorted(arr, target):
    i, j = 0, len(arr) - 1
    while i < j:
        s = arr[i] + arr[j]
        if s == target:
            return (i, j)
        elif s < target:
            i += 1
        else:
            j -= 1
    return None
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** On a sorted array, you don't need a hash map. Two pointers exploit the sort order.

---

## Problem 2.2 — 3Sum

**Statement:** Find all unique triplets in the array that sum to zero.

**Example:**
```
nums = [-1, 0, 1, 2, -1, -4]
Output: [[-1, -1, 2], [-1, 0, 1]]
```

**Approach:** Sort. Fix one number, then use two pointers on the rest to find pairs that sum to its negation. Skip duplicates carefully.

**Solution:**
```python
def three_sum(nums):
    nums.sort()
    result = []
    n = len(nums)
    for i in range(n - 2):
        if i > 0 and nums[i] == nums[i-1]:
            continue  # skip duplicate anchor
        j, k = i + 1, n - 1
        while j < k:
            total = nums[i] + nums[j] + nums[k]
            if total == 0:
                result.append([nums[i], nums[j], nums[k]])
                j += 1
                k -= 1
                while j < k and nums[j] == nums[j-1]: j += 1
                while j < k and nums[k] == nums[k+1]: k -= 1
            elif total < 0:
                j += 1
            else:
                k -= 1
    return result
```

**Complexity:** O(n²) time, O(1) extra space (ignoring output).

**Key insight:** Sorting + two pointers reduces 3Sum from O(n³) brute force to O(n²). Skip-duplicate logic is tricky but essential.

---

## Problem 2.3 — Container With Most Water

**Statement:** Given heights `[h_0, h_1, ..., h_{n-1}]`, find two lines that form a container with the maximum water. Area = min(h_i, h_j) × (j - i).

**Example:**
```
heights = [1, 8, 6, 2, 5, 4, 8, 3, 7] → 49
```

**Approach:** Two pointers at both ends. Always move the shorter one inward (moving the taller one can't help: width shrinks and height is still limited by the shorter).

**Solution:**
```python
def max_area(heights):
    i, j = 0, len(heights) - 1
    best = 0
    while i < j:
        area = min(heights[i], heights[j]) * (j - i)
        best = max(best, area)
        if heights[i] < heights[j]:
            i += 1
        else:
            j -= 1
    return best
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** The greedy choice (move the shorter line) works because moving the taller one is always dominated by the current configuration.

---

# 3. Sliding Window

## Problem 3.1 — Longest Substring Without Repeating Characters

**Statement:** Find the length of the longest substring with all unique characters.

**Example:**
```
"abcabcbb" → 3  (substring "abc")
"bbbbb" → 1
"pwwkew" → 3  (substring "wke")
```

**Approach:** Slide a window. Expand `right`. When `s[right]` was seen inside the window, jump `left` past the previous occurrence.

**Solution:**
```python
def longest_unique(s):
    last_seen = {}
    left = 0
    best = 0
    for right, c in enumerate(s):
        if c in last_seen and last_seen[c] >= left:
            left = last_seen[c] + 1
        last_seen[c] = right
        best = max(best, right - left + 1)
    return best
```

**Complexity:** O(n) time, O(min(n, alphabet_size)) space.

**Key insight:** Both `left` and `right` only move forward, so the total work is O(n), not O(n²).

---

## Problem 3.2 — Minimum Size Subarray Sum

**Statement:** Given positive integers `nums` and `target`, find the minimum length of a contiguous subarray with sum ≥ target. Return 0 if none.

**Example:**
```
target = 7, nums = [2, 3, 1, 2, 4, 3] → 2  (subarray [4, 3])
```

**Solution:**
```python
def min_subarray_len(target, nums):
    left = 0
    total = 0
    best = float('inf')
    for right, x in enumerate(nums):
        total += x
        while total >= target:
            best = min(best, right - left + 1)
            total -= nums[left]
            left += 1
    return 0 if best == float('inf') else best
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** Expand right to reach the target; contract left to minimize length. Works because all numbers are positive (shrinking monotonically decreases sum).

---

## Problem 3.3 — Longest Continuous Subarray with Absolute Diff ≤ Limit

**Statement:** Find the longest contiguous subarray where max − min ≤ limit.

**Example:**
```
nums = [8, 2, 4, 7], limit = 4 → 2  (e.g., [2, 4] or [4, 7])
nums = [10, 1, 2, 4, 7, 2], limit = 5 → 4  (subarray [2, 4, 7, 2])
```

**Approach:** Monotonic deques track the window's max and min in O(1).

**Solution:**
```python
from collections import deque

def longest_subarray(nums, limit):
    max_dq, min_dq = deque(), deque()
    left = 0
    best = 0
    for right, x in enumerate(nums):
        while max_dq and nums[max_dq[-1]] <= x: max_dq.pop()
        max_dq.append(right)
        while min_dq and nums[min_dq[-1]] >= x: min_dq.pop()
        min_dq.append(right)
        while nums[max_dq[0]] - nums[min_dq[0]] > limit:
            left += 1
            if max_dq[0] < left: max_dq.popleft()
            if min_dq[0] < left: min_dq.popleft()
        best = max(best, right - left + 1)
    return best
```

**Complexity:** O(n) — each index pushed and popped at most once per deque.

**Key insight:** A monotonic-decreasing deque gives you the window max at its front; monotonic-increasing gives the min. This is the standard trick for "range max/min in a sliding window."

---

# 4. Binary Search

## Problem 4.1 — Search in Rotated Sorted Array

**Statement:** A sorted array was rotated at some unknown pivot. Find the index of target (or -1 if absent).

**Example:**
```
nums = [4, 5, 6, 7, 0, 1, 2], target = 0 → 4
```

**Approach:** At each step, one half is sorted. Determine which half, then decide where target could be.

**Solution:**
```python
def search_rotated(nums, target):
    lo, hi = 0, len(nums) - 1
    while lo <= hi:
        mid = (lo + hi) // 2
        if nums[mid] == target:
            return mid
        if nums[lo] <= nums[mid]:  # left half sorted
            if nums[lo] <= target < nums[mid]:
                hi = mid - 1
            else:
                lo = mid + 1
        else:  # right half sorted
            if nums[mid] < target <= nums[hi]:
                lo = mid + 1
            else:
                hi = mid - 1
    return -1
```

**Complexity:** O(log n) time.

**Key insight:** Even with rotation, you can always identify a sorted half and check if target lies in its range.

---

## Problem 4.2 — Koko Eating Bananas (Binary Search on the Answer)

**Statement:** Koko has `n` banana piles. She eats at speed `k` bananas/hour, starting a new pile after finishing the current (time per pile = ⌈pile/k⌉ hours). Find the minimum integer `k` that finishes all piles in ≤ `h` hours.

**Example:**
```
piles = [3, 6, 7, 11], h = 8 → 4
```

**Approach:** "Can finish in h hours at speed k" is monotonic in k. Binary search the smallest k where it's True.

**Solution:**
```python
def min_eating_speed(piles, h):
    def feasible(k):
        return sum((p + k - 1) // k for p in piles) <= h
    
    lo, hi = 1, max(piles)
    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1
    return lo
```

**Complexity:** O(n log(max_pile)).

**Key insight:** "Binary search on the answer" turns an optimization problem into a decision problem. Recognize when you see "minimize the maximum" or "smallest X such that...".

---

## Problem 4.3 — Capacity to Ship Packages Within D Days

**Statement:** Given package weights and a deadline of `D` days, find the minimum ship capacity that ships all packages in order within the deadline.

**Example:**
```
weights = [1,2,3,4,5,6,7,8,9,10], days = 5 → 15
```

**Solution:**
```python
def ship_capacity(weights, days):
    def feasible(cap):
        d, load = 1, 0
        for w in weights:
            if load + w > cap:
                d += 1
                load = w
            else:
                load += w
        return d <= days
    
    lo, hi = max(weights), sum(weights)
    while lo < hi:
        mid = (lo + hi) // 2
        if feasible(mid):
            hi = mid
        else:
            lo = mid + 1
    return lo
```

**Complexity:** O(n log(sum(weights))).

**Key insight:** Same "binary search on the answer" pattern. The feasibility function is a greedy simulation.

---

# 5. Stacks & Monotonic Stacks

## Problem 5.1 — Valid Parentheses

**Statement:** Given a string with characters from `"(){}[]"`, decide if brackets are balanced.

**Example:**
```
"()[]{}" → True
"(]" → False
"([)]" → False
```

**Solution:**
```python
def is_valid(s):
    stack = []
    pairs = {')': '(', ']': '[', '}': '{'}
    for c in s:
        if c in "([{":
            stack.append(c)
        else:
            if not stack or stack[-1] != pairs[c]:
                return False
            stack.pop()
    return not stack
```

**Complexity:** O(n) time, O(n) space.

**Key insight:** Stacks naturally model nested structures — the most recent opener must be matched first.

---

## Problem 5.2 — Daily Temperatures (Monotonic Stack)

**Statement:** For each day, return the number of days until a warmer temperature. 0 if no warmer day ahead.

**Example:**
```
[73, 74, 75, 71, 69, 72, 76, 73] → [1, 1, 4, 2, 1, 1, 0, 0]
```

**Solution:**
```python
def daily_temperatures(temps):
    n = len(temps)
    ans = [0] * n
    stack = []  # indices with decreasing temps
    for i, t in enumerate(temps):
        while stack and temps[stack[-1]] < t:
            j = stack.pop()
            ans[j] = i - j
        stack.append(i)
    return ans
```

**Complexity:** O(n) — each index enters and exits the stack at most once.

**Key insight:** A monotonic-decreasing stack of pending indices lets each warmer day resolve all prior cooler days in one sweep. Pattern: "next greater element" problems.

---

## Problem 5.3 — Trapping Rain Water

**Statement:** Given elevations, compute total trapped rainwater.

**Example:**
```
[0,1,0,2,1,0,1,3,2,1,2,1] → 6
```

**Approach (two pointers, O(1) space):** Water at index i = min(max_left, max_right) − height[i]. Maintain running max from both ends.

**Solution:**
```python
def trap(heights):
    i, j = 0, len(heights) - 1
    left_max = right_max = 0
    total = 0
    while i < j:
        if heights[i] < heights[j]:
            left_max = max(left_max, heights[i])
            total += left_max - heights[i]
            i += 1
        else:
            right_max = max(right_max, heights[j])
            total += right_max - heights[j]
            j -= 1
    return total
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** Whichever side is lower determines the water level for that column. This lets you process columns greedily from both ends.

---

# 6. Heaps / Priority Queues

## Problem 6.1 — Kth Largest Element in an Array

**Statement:** Find the kth largest element (not distinct).

**Example:**
```
[3, 2, 1, 5, 6, 4], k = 2 → 5
```

**Solution:**
```python
import heapq

def find_kth_largest(nums, k):
    heap = []
    for x in nums:
        heapq.heappush(heap, x)
        if len(heap) > k:
            heapq.heappop(heap)
    return heap[0]
```

**Complexity:** O(n log k) time, O(k) space.

**Key insight:** Min-heap of size k: after processing all numbers, the smallest in the heap is the kth largest overall. Better than sorting (O(n log n)) when k ≪ n.

---

## Problem 6.2 — Top K Frequent Elements

**Statement:** Return the k most frequent elements.

**Example:**
```
nums = [1,1,1,2,2,3], k = 2 → [1, 2]
```

**Solution:**
```python
from collections import Counter
import heapq

def top_k_frequent(nums, k):
    freq = Counter(nums)
    return heapq.nlargest(k, freq.keys(), key=freq.get)
```

**Complexity:** O(n log k).

**Key insight:** `heapq.nlargest(k, iterable, key)` does top-k in one call. Cleaner than manually managing a heap.

---

## Problem 6.3 — Find Median from Data Stream

**Statement:** Design a data structure supporting `addNum(x)` and `findMedian()`.

**Approach:** Two heaps. Max-heap `low` for the lower half; min-heap `high` for the upper half. Keep sizes balanced.

**Solution:**
```python
import heapq

class MedianFinder:
    def __init__(self):
        self.low = []   # max-heap (negated values)
        self.high = []  # min-heap

    def addNum(self, num):
        heapq.heappush(self.low, -num)
        # move the biggest of low to high
        heapq.heappush(self.high, -heapq.heappop(self.low))
        # rebalance if high got too big
        if len(self.high) > len(self.low):
            heapq.heappush(self.low, -heapq.heappop(self.high))

    def findMedian(self):
        if len(self.low) > len(self.high):
            return -self.low[0]
        return (-self.low[0] + self.high[0]) / 2
```

**Complexity:** O(log n) per add, O(1) per median query.

**Key insight:** Two heaps "meeting in the middle" keep the median at the heap tops. This is a classic quant-style question.

---

# 7. Stock & Trading DP

## Problem 7.1 — Best Time to Buy and Sell Stock (one transaction)

**Statement:** Given prices, maximize profit from a single buy/sell.

**Example:**
```
[7, 1, 5, 3, 6, 4] → 5  (buy at 1, sell at 6)
```

**Solution:**
```python
def max_profit(prices):
    min_price = float('inf')
    best = 0
    for p in prices:
        min_price = min(min_price, p)
        best = max(best, p - min_price)
    return best
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** Track the running min; at each point, the best profit "selling today" is `today - min_so_far`.

---

## Problem 7.2 — Best Time to Buy and Sell Stock II (unlimited transactions)

**Statement:** Unlimited buy/sell, but you can only hold one share at a time.

**Example:**
```
[7, 1, 5, 3, 6, 4] → 7  (buy 1 sell 5, buy 3 sell 6)
```

**Solution:**
```python
def max_profit_ii(prices):
    profit = 0
    for i in range(1, len(prices)):
        if prices[i] > prices[i-1]:
            profit += prices[i] - prices[i-1]
    return profit
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** Sum every positive daily delta. Any upward run contributes its full height regardless of how you slice the buys and sells.

---

## Problem 7.3 — Best Time to Buy and Sell Stock with Cooldown

**Statement:** After selling, you must wait one day before buying again.

**Example:**
```
[1, 2, 3, 0, 2] → 3  (buy 1 sell 2, cooldown, buy 0 sell 2)
```

**Approach:** State machine with three states each day — holding, just sold (cooldown), resting.

**Solution:**
```python
def max_profit_cooldown(prices):
    if not prices: return 0
    hold = -prices[0]  # holding a share
    sold = 0            # just sold
    rest = 0            # not holding, not in cooldown
    for p in prices[1:]:
        prev_sold = sold
        sold = hold + p
        hold = max(hold, rest - p)
        rest = max(rest, prev_sold)
    return max(sold, rest)
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** When problems have "states" that depend on the immediately preceding state, a state-machine DP gives clean O(1) space. This pattern generalizes to problems with fees, multiple transactions, etc.

---

## Problem 7.4 — Best Time to Buy and Sell Stock with Fee

**Statement:** Each transaction costs a fixed fee. Maximize profit.

**Example:**
```
prices = [1,3,2,8,4,9], fee = 2 → 8
```

**Solution:**
```python
def max_profit_fee(prices, fee):
    hold = -prices[0]  # own a share
    cash = 0           # no share
    for p in prices[1:]:
        cash = max(cash, hold + p - fee)
        hold = max(hold, cash - p)
    return cash
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** The fee is subtracted when selling. Two states suffice: holding vs not.

---

# 8. General Dynamic Programming

## Problem 8.1 — House Robber

**Statement:** Rob houses in a row to maximize loot; can't rob two adjacent ones.

**Example:**
```
[2, 7, 9, 3, 1] → 12  (rob 2, 9, 1)
```

**Solution:**
```python
def rob(nums):
    prev2, prev1 = 0, 0
    for x in nums:
        prev2, prev1 = prev1, max(prev1, prev2 + x)
    return prev1
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** `dp[i] = max(dp[i-1], dp[i-2] + nums[i])`. Either skip this house (inherit previous) or rob it (add to two-back).

---

## Problem 8.2 — Coin Change

**Statement:** Minimum coins to make amount; -1 if impossible.

**Example:**
```
coins = [1, 2, 5], amount = 11 → 3  (5+5+1)
```

**Solution:**
```python
def coin_change(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for a in range(1, amount + 1):
        for c in coins:
            if c <= a:
                dp[a] = min(dp[a], dp[a - c] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1
```

**Complexity:** O(amount × len(coins)) time.

**Key insight:** Unbounded knapsack. For each amount, try every coin. Iterate amount outer, coin inner.

---

## Problem 8.3 — Longest Increasing Subsequence

**Statement:** Length of the longest strictly increasing subsequence (not necessarily contiguous).

**Example:**
```
[10, 9, 2, 5, 3, 7, 101, 18] → 4  ([2, 3, 7, 101])
```

**Solution (O(n log n) via patience sorting):**
```python
from bisect import bisect_left

def length_of_lis(nums):
    tails = []
    for x in nums:
        i = bisect_left(tails, x)
        if i == len(tails):
            tails.append(x)
        else:
            tails[i] = x
    return len(tails)
```

**Complexity:** O(n log n) time, O(n) space.

**Key insight:** `tails[i]` = smallest possible tail of an increasing subsequence of length `i+1`. Binary-search insertion keeps it sorted.

---

## Problem 8.4 — Edit Distance

**Statement:** Minimum operations (insert, delete, replace) to convert word1 into word2.

**Example:**
```
"horse" → "ros" = 3
```

**Solution:**
```python
def edit_distance(w1, w2):
    m, n = len(w1), len(w2)
    dp = [[0] * (n+1) for _ in range(m+1)]
    for i in range(m+1): dp[i][0] = i
    for j in range(n+1): dp[0][j] = j
    for i in range(1, m+1):
        for j in range(1, n+1):
            if w1[i-1] == w2[j-1]:
                dp[i][j] = dp[i-1][j-1]
            else:
                dp[i][j] = 1 + min(dp[i-1][j], dp[i][j-1], dp[i-1][j-1])
    return dp[m][n]
```

**Complexity:** O(m × n).

**Key insight:** The three operations correspond to three DP transitions. Characters matching = no cost for this pair.

---

## Problem 8.5 — Maximum Product Subarray

**Statement:** Contiguous subarray with the largest product.

**Example:**
```
[2, 3, -2, 4] → 6  (subarray [2, 3])
```

**Approach:** A negative number flips sign — track both max and min so far.

**Solution:**
```python
def max_product(nums):
    best = cur_max = cur_min = nums[0]
    for x in nums[1:]:
        if x < 0:
            cur_max, cur_min = cur_min, cur_max
        cur_max = max(x, cur_max * x)
        cur_min = min(x, cur_min * x)
        best = max(best, cur_max)
    return best
```

**Complexity:** O(n) time, O(1) space.

**Key insight:** A big negative becomes a big positive when multiplied by another negative. So the min matters too.

---

# 9. Trees & Graphs

## Problem 9.1 — Maximum Depth of Binary Tree

**Statement:** Return the maximum depth (root to deepest leaf).

**Solution:**
```python
def max_depth(root):
    if not root: return 0
    return 1 + max(max_depth(root.left), max_depth(root.right))
```

**Complexity:** O(n) time, O(h) space for recursion (h = height).

**Key insight:** Tree recursion = base case (null) + combine child results.

---

## Problem 9.2 — Validate Binary Search Tree

**Statement:** Determine whether a binary tree is a valid BST.

**Approach:** Pass down allowed (min, max) bounds. Every node must lie strictly within its bounds.

**Solution:**
```python
def is_valid_bst(root):
    def check(node, low, high):
        if not node: return True
        if not (low < node.val < high): return False
        return check(node.left, low, node.val) and check(node.right, node.val, high)
    return check(root, float('-inf'), float('inf'))
```

**Complexity:** O(n) time, O(h) space.

**Key insight:** Checking just `left < node < right` locally is wrong. BST requires every ancestor's bound. Pass bounds down the recursion.

---

## Problem 9.3 — Number of Islands

**Statement:** Count connected components of 1s in a 2D grid (1=land, 0=water).

**Example:**
```
[["1","1","0","0","0"],
 ["1","1","0","0","0"],
 ["0","0","1","0","0"],
 ["0","0","0","1","1"]]  → 3
```

**Solution:**
```python
def num_islands(grid):
    if not grid: return 0
    m, n = len(grid), len(grid[0])
    count = 0
    
    def dfs(i, j):
        if i < 0 or i >= m or j < 0 or j >= n: return
        if grid[i][j] != '1': return
        grid[i][j] = '0'  # mark visited in place
        for di, dj in [(1,0),(-1,0),(0,1),(0,-1)]:
            dfs(i+di, j+dj)
    
    for i in range(m):
        for j in range(n):
            if grid[i][j] == '1':
                count += 1
                dfs(i, j)
    return count
```

**Complexity:** O(m × n).

**Key insight:** Each unvisited 1 starts a new island; DFS floods the rest of it.

---

## Problem 9.4 — Course Schedule (Topological Sort / Cycle Detection)

**Statement:** Given prerequisite pairs `[course, prereq]`, can you finish all courses?

**Approach:** Build a directed graph. A valid schedule exists iff the graph has no cycle.

**Solution (Kahn's algorithm):**
```python
from collections import defaultdict, deque

def can_finish(num_courses, prerequisites):
    graph = defaultdict(list)
    indeg = [0] * num_courses
    for c, p in prerequisites:
        graph[p].append(c)
        indeg[c] += 1
    q = deque(i for i in range(num_courses) if indeg[i] == 0)
    taken = 0
    while q:
        p = q.popleft()
        taken += 1
        for c in graph[p]:
            indeg[c] -= 1
            if indeg[c] == 0:
                q.append(c)
    return taken == num_courses
```

**Complexity:** O(V + E).

**Key insight:** Repeatedly remove nodes with no remaining dependencies. If you can remove all, the graph is a DAG.

---

# 10. Probability & Expected Value

## Problem 10.1 — Knight Probability in Chessboard

**Statement:** A knight on an N×N board at (r, c) makes k moves, each uniform over the 8 knight moves (even if off-board — off-board = game ends). Probability it's still on the board?

**Example:**
```
n = 3, k = 2, r = 0, c = 0 → 0.0625
```

**Solution:**
```python
def knight_probability(n, k, r, c):
    moves = [(2,1),(2,-1),(-2,1),(-2,-1),(1,2),(1,-2),(-1,2),(-1,-2)]
    P = [[0]*n for _ in range(n)]
    P[r][c] = 1.0
    for _ in range(k):
        new_P = [[0]*n for _ in range(n)]
        for i in range(n):
            for j in range(n):
                if P[i][j] == 0: continue
                for di, dj in moves:
                    ni, nj = i+di, j+dj
                    if 0 <= ni < n and 0 <= nj < n:
                        new_P[ni][nj] += P[i][j] / 8
        P = new_P
    return sum(sum(row) for row in P)
```

**Complexity:** O(k × n²).

**Key insight:** Evolve the probability distribution forward step by step. Each cell aggregates inbound probability mass from its 8 knight-predecessors.

---

## Problem 10.2 — Expected Sum Until Exceeding N (Custom)

**Statement:** Roll a fair 6-sided die repeatedly until the running sum first exceeds `N`. Compute the expected value of the final sum.

**Example:**
```
N = 0 → 3.5  (one roll)
N = 10 → ~12.17
```

**Approach:** Backward DP. For `s > N`, `E[s] = s`. Otherwise, `E[s] = (1/6) · Σ E[s+k]` for `k = 1..6`.

**Solution:**
```python
def expected_final_sum(N):
    E = [0.0] * (N + 7)
    for s in range(N + 6, N, -1):
        E[s] = s
    for s in range(N, -1, -1):
        E[s] = sum(E[s + k] for k in range(1, 7)) / 6
    return E[0]
```

**Complexity:** O(N) time and space.

**Key insight:** Compute expected values from "past the end" backward to the start. This is the classic stopping-time DP.

---

## Problem 10.3 — New 21 Game (Probability DP)

**Statement:** Alice starts with 0 points. She draws a random integer uniform in `[1, maxPts]`, adding to her total. She stops when her total ≥ k. Return the probability her total is ≤ n when she stops.

**Approach:** Let `f(x)` = probability the final score is ≤ n given current score is x. For x ≥ k, `f(x) = 1 if x ≤ n else 0`. For x < k, `f(x) = (1/maxPts) · Σ f(x + i)` for i=1..maxPts.

**Solution (with sliding window for efficiency):**
```python
def new21_game(n, k, maxPts):
    if k == 0 or n >= k + maxPts: return 1.0
    dp = [0.0] * (n + 1)
    dp[0] = 1.0
    window = 1.0  # running sum of the last maxPts dp values (those that contribute)
    answer = 0.0
    for i in range(1, n + 1):
        dp[i] = window / maxPts
        if i < k:
            window += dp[i]
        else:
            answer += dp[i]
        if i - maxPts >= 0:
            window -= dp[i - maxPts]
    return answer
```

**Complexity:** O(n).

**Key insight:** Sliding window maintains the sum of contributing dp values in O(1) per step. Without it, each dp[i] would take O(maxPts) to compute.

---

## Problem 10.4 — Generate Random Index by Weight

**Statement:** Given a list of positive weights, return an index i with probability weights[i] / sum(weights).

**Example:**
```
weights = [1, 3] → 0 with probability 0.25, 1 with probability 0.75
```

**Solution:**
```python
import random
from bisect import bisect_left
from itertools import accumulate

class Solution:
    def __init__(self, w):
        self.prefix = list(accumulate(w))
        self.total = self.prefix[-1]
    def pickIndex(self):
        r = random.random() * self.total
        return bisect_left(self.prefix, r)
```

**Complexity:** O(n) setup, O(log n) per pick.

**Key insight:** Prefix sums + binary search. A random number in [0, total) lands in an index's "slot" with probability proportional to its weight.

---

# 11. Quant-Flavored Problems

## Problem 11.1 — Currency Arbitrage Detection

**Statement:** Given exchange rates between currencies as a directed graph, detect whether arbitrage is possible: a cycle where multiplying rates gives > 1.

**Approach:** Take −log of each rate. Arbitrage becomes a negative cycle. Use Bellman-Ford.

**Solution:**
```python
import math

def has_arbitrage(n, rates):
    # rates: list of (from, to, rate)
    edges = [(a, b, -math.log(r)) for a, b, r in rates]
    dist = [0.0] * n  # any start, since we only care about cycles
    for _ in range(n - 1):
        for a, b, w in edges:
            if dist[a] + w < dist[b]:
                dist[b] = dist[a] + w
    # One more pass: if anything relaxes, there's a negative cycle
    for a, b, w in edges:
        if dist[a] + w < dist[b]:
            return True
    return False
```

**Complexity:** O(V × E).

**Key insight:** Multiplicative cycles in rate-space become additive cycles in log-space. Negative cycle detection is exactly what Bellman-Ford does.

---

## Problem 11.2 — Online Stock Span

**Statement:** For each price in a stream, return the span: number of consecutive previous days (including today) where the price was ≤ today's.

**Example:**
```
prices = [100, 80, 60, 70, 60, 75, 85]
spans  = [1,   1,  1,  2,  1,  4,  6]
```

**Solution (monotonic stack):**
```python
class StockSpanner:
    def __init__(self):
        self.stack = []  # (price, span)
    def next(self, price):
        span = 1
        while self.stack and self.stack[-1][0] <= price:
            span += self.stack.pop()[1]
        self.stack.append((price, span))
        return span
```

**Complexity:** O(1) amortized per call.

**Key insight:** Aggregate pending spans into the popped entry — lets each price be processed once over all calls.

---

## Problem 11.3 — Moving Average from Data Stream

**Statement:** Compute the moving average of the last `size` values as they arrive.

**Solution:**
```python
from collections import deque

class MovingAverage:
    def __init__(self, size):
        self.size = size
        self.q = deque()
        self.total = 0
    def next(self, val):
        self.q.append(val)
        self.total += val
        if len(self.q) > self.size:
            self.total -= self.q.popleft()
        return self.total / len(self.q)
```

**Complexity:** O(1) per call.

**Key insight:** Maintain a running sum + deque to avoid recomputing on every call. Essential pattern for streaming statistics.

---

## Problem 11.4 — Maximum Profit with K Transactions

**Statement:** At most `k` buy/sell pairs. Maximize profit.

**Example:**
```
prices = [3, 2, 6, 5, 0, 3], k = 2 → 7
```

**Solution:**
```python
def max_profit_k(k, prices):
    n = len(prices)
    if n < 2 or k == 0: return 0
    # If k is large enough, just take all upward moves
    if k >= n // 2:
        return sum(max(0, prices[i] - prices[i-1]) for i in range(1, n))
    
    # dp[j] = (max_hold, max_sold) for j transactions
    hold = [-float('inf')] * (k + 1)
    sold = [0] * (k + 1)
    for p in prices:
        for j in range(1, k + 1):
            hold[j] = max(hold[j], sold[j-1] - p)
            sold[j] = max(sold[j], hold[j] + p)
    return sold[k]
```

**Complexity:** O(n × k) time, O(k) space.

**Key insight:** State = (day, transactions, holding?). The inner loop iterates transactions; the outer iterates days. If k ≥ n/2, transactions are unlimited in effect.

---

## Problem 11.5 — Streaming Correlation (Custom quant-flavored)

**Statement:** Given two streams of numbers, maintain the Pearson correlation coefficient of the last `w` pairs.

**Formula:**
```
ρ(X, Y) = (n·ΣXY − ΣX·ΣY) / √((n·ΣX² − (ΣX)²)(n·ΣY² − (ΣY)²))
```

**Approach:** Maintain five rolling sums: ΣX, ΣY, ΣXY, ΣX², ΣY². Use a deque of (x, y) pairs for the sliding window.

**Solution:**
```python
from collections import deque
import math

class StreamingCorrelation:
    def __init__(self, window):
        self.w = window
        self.q = deque()
        self.sx = self.sy = self.sxy = self.sxx = self.syy = 0.0
    
    def add(self, x, y):
        self.q.append((x, y))
        self.sx += x; self.sy += y
        self.sxy += x * y
        self.sxx += x * x; self.syy += y * y
        if len(self.q) > self.w:
            ox, oy = self.q.popleft()
            self.sx -= ox; self.sy -= oy
            self.sxy -= ox * oy
            self.sxx -= ox * ox; self.syy -= oy * oy
    
    def correlation(self):
        n = len(self.q)
        if n < 2: return None
        num = n * self.sxy - self.sx * self.sy
        den2 = (n * self.sxx - self.sx ** 2) * (n * self.syy - self.sy ** 2)
        if den2 <= 0: return None
        return num / math.sqrt(den2)
```

**Complexity:** O(1) per add and query.

**Key insight:** All the ingredients for correlation are rolling sums. You never need to store more than the window's worth of raw pairs. This pattern generalizes to any sliding-window statistic (variance, covariance, beta, etc.).

---

# Final Notes

## How to use this problem set

1. **Don't just read the solutions.** For each problem: read the statement, close this file, set a 25-minute timer, and try to solve it. Open the solution only after you've attempted or timed out.

2. **Type the solutions from scratch** after reading. Seeing isn't knowing. Typing it builds the muscle memory.

3. **Re-solve a week later.** Spaced repetition is how these patterns stick.

4. **On test day**, recognize the pattern first. Say to yourself: "This is sliding window" or "This is probability DP". That recognition is most of the battle.

## What to prioritize

If you have limited time, focus on these sections in order:

1. Stock & Trading DP (section 7) — most likely given the firm.
2. Probability & Expected Value (section 10) — your strong suit.
3. Sliding Window (section 3) — ubiquitous warm-up problems.
4. Hash Maps / Prefix Sums (section 1) — fast wins.
5. Quant-Flavored Problems (section 11) — domain-specific bonus.

The rest is useful but lower-priority if you're time-constrained.

Good luck. 加油！
