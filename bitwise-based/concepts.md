## 1. **XOR Properties - The Most Important Concept**

### Key Properties:
- `a ^ a = 0` (any number XOR itself is 0)
- `a ^ 0 = a` (any number XOR 0 is itself)
- XOR is **commutative** and **associative**: `a ^ b ^ c = c ^ a ^ b`
- `a ^ b ^ a = b` (cancellation property)

### Applications in Your Code:

#### **Finding the Unique Element** (`XorOfPairs.java`)
```java
// When every element appears twice except one
int ans = 0;
for (int num : array) {
    ans ^= num;  // Pairs cancel out, unique remains
}
```

#### **Finding Two Unique Numbers** (`TwoUniqueNumbersXor.java`)
**Problem**: Array has 2n+2 elements, all appear twice except two numbers.

**Strategy**:
1. XOR all elements → result is `a ^ b` (the two unique numbers)
2. Find any set bit in the result (this bit differs between a and b)
3. Partition numbers into two groups based on this bit
4. XOR each group separately to get the two numbers

```java
int xorval = 0;
for (int num : array) xorval ^= num;  // Get a ^ b

// Find a bit that's set (differs between a and b)
int sh = 0;
while ((xorval & (1 << sh)) == 0) sh++;

// Partition and XOR separately
int group1 = 0, group2 = 0;
for (int num : array) {
    if ((num & (1 << sh)) != 0) group1 ^= num;
    else group2 ^= num;
}
```

---

## 2. **Bit Masking for Character Frequency**

### The Technique:
Use an integer as a bitmask where each bit represents whether a character appears an odd or even number of times.

```java
int mask = 0;
for (char ch : string.toCharArray()) {
    mask ^= (1 << (ch - 'a'));  // Toggle bit for this character
}
```

### Applications:

#### **Check if String Can Form Palindrome** (`checkIfFormAPalintrome.java`)
**Logic**: A palindrome can have at most ONE character with odd frequency.

```java
// After building mask:
if (mask == 0 || (mask & (mask - 1)) == 0) {
    // mask == 0: all even frequencies
    // mask & (mask-1) == 0: exactly one bit set (one odd frequency)
    System.out.println("Yes");
}
```

#### **Count Palindrome Pairs** (`CheckIfNStringsPalindrome.java`)
**Problem**: Count pairs of strings that can be rearranged together to form a palindrome.

**Strategy**:
1. Build XOR mask for each string
2. Count pairs where masks are:
   - Identical (XOR = 0, all characters paired)
   - Differ by exactly 1 bit (one character appears odd times)

```java
HashMap<Integer, Long> maskCount = new HashMap<>();

// Count identical masks
for (long count : maskCount.values()) {
    pairs += count * (count - 1) / 2;  // C(n, 2)
}

// Count masks differing by 1 bit
for (int mask : maskCount.keySet()) {
    for (int bit = 0; bit < 26; bit++) {
        int other = mask ^ (1 << bit);  // Flip one bit
        if (other > mask && maskCount.containsKey(other)) {
            pairs += maskCount.get(mask) * maskCount.get(other);
        }
    }
}
```

#### **Check if String Contains All Letters** (`checkString.java`)
```java
int flag = 0;
for (char ch : s.toCharArray()) {
    flag |= (1 << (ch - 'a'));  // Set bit for this letter
}
// All 26 bits set?
if (flag == (1 << 26) - 1) // 0b11111111111111111111111111
```

---
