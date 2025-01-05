# Shifting_Letters
2381. Shifting Letters

Here’s a step-by-step explanation and solution to solve **2381. Shifting Letters II**:

### Approach

1. **Understanding the Shifting Logic**:  
   Each operation specifies a range of indices `[start, end]` in the string `s` and a direction:
   - If `direction = 1`, shift forward (e.g., 'a' → 'b', 'z' → 'a').
   - If `direction = 0`, shift backward (e.g., 'a' → 'z', 'b' → 'a').

2. **Efficient Range Updates Using a Difference Array**:  
   Instead of iterating over the string for every operation, use a **difference array** to accumulate the effect of shifts:
   - Increment `diff[start]` by `1` for forward shifts and decrement `diff[start]` by `1` for backward shifts.
   - Decrement `diff[end + 1]` to mark the end of the effect.

3. **Cumulative Sum for Net Shifts**:  
   Calculate the cumulative sum of the difference array to determine the net shift for each character.

4. **Apply the Shifts**:  
   Compute the new character after applying the net shift. Use modular arithmetic to handle wrapping around the alphabet.

---

### Solution Code

```python
def shiftingLetters(s: str, shifts: list[list[int]]) -> str:
    n = len(s)
    # Step 1: Create a difference array
    diff = [0] * (n + 1)

    # Step 2: Apply the shifts to the difference array
    for start, end, direction in shifts:
        shift_value = 1 if direction == 1 else -1
        diff[start] += shift_value
        diff[end + 1] -= shift_value

    # Step 3: Calculate the cumulative sum
    cumulative_shift = [0] * n
    current_shift = 0
    for i in range(n):
        current_shift += diff[i]
        cumulative_shift[i] = current_shift

    # Step 4: Apply the cumulative shifts to the string
    result = []
    for i in range(n):
        original_pos = ord(s[i]) - ord('a')
        new_pos = (original_pos + cumulative_shift[i]) % 26
        result.append(chr(new_pos + ord('a')))

    return ''.join(result)

# Example Usage
s1 = "abc"
shifts1 = [[0, 1, 0], [1, 2, 1], [0, 2, 1]]
print(shiftingLetters(s1, shifts1))  # Output: "ace"

s2 = "dztz"
shifts2 = [[0, 0, 0], [1, 1, 1]]
print(shiftingLetters(s2, shifts2))  # Output: "catz"
```

---

### Explanation of the Code

1. **Difference Array**:
   - Initialize a `diff` array of size `n + 1` to track shifts.
   - For each operation `[start, end, direction]`, update `diff[start]` and `diff[end + 1]`.

2. **Cumulative Sum**:
   - Traverse the `diff` array to compute the cumulative shift for each index in the string.

3. **Character Transformation**:
   - Convert the character to its position in the alphabet (`ord(s[i]) - ord('a')`).
   - Add the cumulative shift and wrap around using modulo `26`.
   - Convert the position back to a character (`chr(new_pos + ord('a'))`).

4. **Build Result**:
   - Append the transformed characters to a result list and return the joined string.

---

### Complexity

1. **Time Complexity**:  
   - Processing the shifts: \(O(m)\), where \(m\) is the number of operations.
   - Calculating the cumulative sum: \(O(n)\), where \(n\) is the length of the string.
   - Applying the shifts: \(O(n)\).  
   **Total**: \(O(n + m)\).

2. **Space Complexity**:  
   - Difference array and cumulative shift array: \(O(n)\).  
   **Total**: \(O(n)\).

---

### Test Cases

#### Example 1:
```plaintext
Input: s = "abc", shifts = [[0, 1, 0], [1, 2, 1], [0, 2, 1]]
Output: "ace"
```

#### Example 2:
```plaintext
Input: s = "dztz", shifts = [[0, 0, 0], [1, 1, 1]]
Output: "catz"
```

#### Example 3:
```plaintext
Input: s = "aaa", shifts = [[0, 2, 1], [0, 1, 0]]
Output: "aab"
```

This solution is efficient and handles all edge cases like overlapping shifts and wrapping around the alphabet.
