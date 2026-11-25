# Hashing Fundamentals in DSA – Beginner Notes

## 1. What is Hashing?

**Hashing is a technique used to store and retrieve data efficiently using a hash function.**

Instead of searching through every element one by one, hashing helps us locate data quickly.

### Real-life Example

Think about a library:
- Every book has an identification number.
- The librarian uses that number to locate the book.
- Instead of searching the entire library, the librarian knows where to look.

### Hashing Terminology

| Hashing Term | Library Example |
|---|---|
| Key | Book identification number |
| Value | Book information |
| Hash Function | Method to determine shelf |
| Hash Table | Library storage |
| Bucket | Particular shelf |

### Why Do We Use Hashing?

- Fast searching.
- Fast insertion.
- Fast deletion.
- Counting frequencies.
- Finding duplicates.
- Checking whether an element exists.

### Time Complexity

| Operation | Average Case | Worst Case |
|---|---|---|
| Search | O(1) | O(n) |
| Insertion | O(1) | O(n) |
| Deletion | O(1) | O(n) |

**Remember:** O(1) means constant time on average.

---

## 2. What is a Hash Function?

A **Hash Function** is a function that takes a key as input and produces a hash value.

In simple words, it decides where the data should be stored in a hash table.

### Example

Suppose we have an array of 10 positions.

Hash function:

```text
h(key) = key % 10
```

Calculations:

| Key | Calculation | Index |
|---|---|---|
| 12 | 12 % 10 | 2 |
| 25 | 25 % 10 | 5 |
| 37 | 37 % 10 | 7 |
| 41 | 41 % 10 | 1 |
| 53 | 53 % 10 | 3 |

For example:

```text
h(25) = 25 % 10 = 5
```

Therefore, 25 is stored at index 5.

Similarly:

```text
h(37) = 37 % 10 = 7
```

Therefore, 37 is stored at index 7.

### Characteristics of a Good Hash Function

1. It should be fast to calculate.
2. It should distribute keys uniformly.
3. It should minimize collisions.
4. It should produce the same result for the same key under the same conditions.

### Java Example

```java
public class Main {
    public static void main(String[] args) {

        int key = 25;
        int size = 10;

        int index = key % size;

        System.out.println(index);
    }
}
```

Output:

```text
5
```

**Remember:** A hash function converts a key into a hash value, which helps determine its storage location.

---

## 3. What is a Collision?

A **Collision** occurs when two different keys produce the same index in a hash table.

### Example

Hash function:

```text
h(key) = key % 10
```

Consider two keys:

- 25
- 35

Calculations:

```text
h(25) = 25 % 10 = 5

h(35) = 35 % 10 = 5
```

Both keys produce index 5.

Therefore, a collision occurs.

### Why Do Collisions Happen?

Because the number of possible keys can be much larger than the number of available positions in a hash table.

For example:

- Possible keys: 1 to 1,000,000.
- Available indices: 0 to 9.

Many keys must share the same index.

### Important Interview Question

**Can we completely avoid collisions?**

For a general-purpose hash table with a large key space and limited slots, collisions cannot be completely avoided.

Therefore, we need collision-handling techniques.

---

## 4. Collision Handling

**Collision Handling** means deciding what to do when two or more keys produce the same index.

There are two major techniques:

1. Chaining.
2. Open Addressing.

### A. Chaining

In Chaining, each index maintains a linked list of elements.

Example:

Hash function:

```text
h(key) = key % 10
```

Insert:

```text
25, 35, 45
```

All three produce index 5.

Instead of replacing the previous element, we store all three elements in a linked list.

```text
Index 5

25 → 35 → 45 → null
```

### How Does Searching Work?

Suppose we want to search for 35.

1. Calculate its hash index: 35 % 10 = 5.
2. Go to index 5.
3. Traverse the linked list.
4. Find 35.

**Important:** Chaining allows multiple elements to be stored in the same bucket.

### B. Open Addressing

In Open Addressing, all elements are stored directly inside the hash table.

If the calculated position is occupied, we search for another empty position.

There are three common techniques:

1. Linear Probing.
2. Quadratic Probing.
3. Double Hashing.

#### 1. Linear Probing

If the calculated index is occupied, check the next index.

Example:

```text
h(key) = key % 10
```

Insert:

```text
25, 35, 45
```

**Step 1: Insert 25**

```text
25 % 10 = 5
```

Store 25 at index 5.

**Step 2: Insert 35**

```text
35 % 10 = 5
```

Index 5 is occupied.

Check index 6.

Store 35 at index 6.

**Step 3: Insert 45**

```text
45 % 10 = 5
```

Index 5 is occupied.

Check index 6: Occupied.

Check index 7: Empty.

Store 45 at index 7.

Final hash table:

| Index | Value |
|---|---|
| 0 | Empty |
| 1 | Empty |
| 2 | Empty |
| 3 | Empty |
| 4 | Empty |
| 5 | 25 |
| 6 | 35 |
| 7 | 45 |
| 8 | Empty |
| 9 | Empty |

**Remember:** Linear Probing checks the next available position until it finds an empty slot.

#### 2. Quadratic Probing

In Quadratic Probing, instead of checking the next consecutive position, we use quadratic intervals.

The general formula is:

```text
newIndex = (hash(key) + i²) % tableSize
```

Where:
- `i` starts from 0.
- `i` increases after each collision.
- `tableSize` is the size of the hash table.

Example:

If the original index is 5:

```text
i = 0 → (5 + 0²) % 10 = 5

i = 1 → (5 + 1²) % 10 = 6

i = 2 → (5 + 2²) % 10 = 9

i = 3 → (5 + 3²) % 10 = 4
```

We check positions 5, 6, 9, 4, and so on.

#### 3. Double Hashing

Double Hashing uses two different hash functions.

The second hash function determines the step size when a collision occurs.

General formula:

```text
newIndex = (h1(key) + i * h2(key)) % tableSize
```

Where:
- `h1(key)` is the primary hash function.
- `h2(key)` is the secondary hash function.
- `i` is the probing attempt number.

It helps reduce clustering compared with Linear Probing.

---

## 5. Chaining vs Open Addressing

| Chaining | Open Addressing |
|---|---|
| Uses linked lists or other bucket structures | Stores elements directly in the hash table |
| Multiple elements can occupy one bucket | One element per slot |
| Can store more elements than table slots | Requires an empty slot for insertion |
| Requires extra memory for links | Does not require linked-list pointers |

### Comparison of Collision-Handling Techniques

| Technique | Working |
|---|---|
| Chaining | Stores colliding elements in a linked list |
| Linear Probing | Checks the next slot |
| Quadratic Probing | Checks slots using quadratic intervals |
| Double Hashing | Uses a second hash function to determine the next position |

---

## 6. How Hashing Is Used in Java

Java provides built-in classes that use hashing internally.

The two most important classes for DSA are:

### A. HashMap

HashMap stores data in key-value pairs.

Example:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {

        HashMap<String, Integer> map = new HashMap<>();

        map.put("Rahul", 85);
        map.put("Ajay", 90);
        map.put("Priya", 75);

        System.out.println(map.get("Rahul"));
    }
}
```

Output:

```text
85
```

### B. HashSet

HashSet stores unique elements.

Example:

```java
import java.util.*;

public class Main {
    public static void main(String[] args) {

        HashSet<Integer> set = new HashSet<>();

        set.add(10);
        set.add(20);
        set.add(10);

        System.out.println(set);
    }
}
```

Possible Output:

```text
[20, 10]
```

The order may vary.

Notice that duplicate 10 is not stored twice.

### HashMap vs HashSet

| HashMap | HashSet |
|---|---|
| Stores key-value pairs | Stores unique elements |
| Uses `put()` | Uses `add()` |
| Uses `get()` to retrieve values | Uses `contains()` to check elements |
| Keys are unique | Elements are unique |

**Important:** Java HashMap and HashSet use hashing internally. Their implementations also include additional collision-handling mechanisms.

---

## 7. Important Concepts – Quick Revision

| Concept | Meaning |
|---|---|
| Hashing | Technique to store and retrieve data efficiently |
| Hash Function | Converts a key into a hash value |
| Hash Table | Data structure that stores hashed data |
| Collision | Two different keys map to the same index |
| Collision Handling | Technique to handle collisions |
| Chaining | Stores colliding elements in the same bucket |
| Open Addressing | Finds another empty position |
| Linear Probing | Checks the next available position |
| Quadratic Probing | Checks positions using quadratic intervals |
| Double Hashing | Uses a second hash function |

---

## 8. Important Interview Questions

1. What is Hashing?
2. What is a Hash Function?
3. What is a Hash Table?
4. What is a Collision?
5. Why do collisions occur?
6. Can collisions be completely avoided?
7. What is Collision Handling?
8. What is Chaining?
9. What is Open Addressing?
10. What is Linear Probing?
11. What is Quadratic Probing?
12. What is Double Hashing?
13. What is the difference between Chaining and Open Addressing?
14. What is the average time complexity of HashMap operations?
15. What is the difference between HashMap and HashSet?

---

## 9. What to Learn Next for DSA Placements

Follow this learning order:

- [ ] Hashing Fundamentals
- [ ] Java HashMap
- [ ] Java HashSet
- [ ] HashMap and HashSet important methods
- [ ] Different ways of iterating over HashMap and HashSet
- [ ] Frequency Counting Pattern
- [ ] Duplicate Detection Pattern
- [ ] Two Sum Pattern
- [ ] Array Intersection Pattern
- [ ] Character Frequency Pattern
- [ ] Subarray Sum Pattern
- [ ] Placement-level Hashing Problems

### Recommended Practice Order

| Level | Problems |
|---|---|
| Beginner | Count frequency of elements |
| Beginner | Find duplicate elements |
| Beginner | Find unique elements |
| Beginner | Find the first repeating element |
| Beginner | Find the first non-repeating character |
| Easy | Two Sum |
| Easy | Check whether two arrays are equal |
| Easy | Find common elements in two arrays |
| Easy | Check whether two strings are anagrams |
| Medium | Longest subarray with sum K |
| Medium | Count subarrays with sum K |

---

## Final Revision

Remember these five important points:

1. Hashing helps us store and retrieve data efficiently.
2. A Hash Function determines the hash value for a key.
3. A Collision occurs when different keys map to the same index.
4. Chaining and Open Addressing are two major collision-handling techniques.
5. In Java, HashMap and HashSet are the most important hashing-based collections for DSA.

**My recommendation:** Understand HashMap and HashSet in Java thoroughly before starting advanced Hashing problems. They are essential for solving DSA questions in service-based company placements.