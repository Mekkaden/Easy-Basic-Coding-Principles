**DSA Notes: Understanding Stack and Multi-Stack (with Helicopter & Building Analogy)**

---

### ✅ **1. What is a Stack?**

* A **stack** is a linear data structure that follows the **LIFO (Last In, First Out)** principle.
* You can imagine it like a stack of plates: the last plate you put on the top is the first one you'll take off.
* In implementation, we usually use:

  * An **array** to store the elements
  * A **`top` pointer** to keep track of the index of the topmost element

**Common operations:**

* **Push:** Add an element to the top
* **Pop:** Remove the top element
* **Peek/Poke:** View the top element without removing
* **Display/Print:** Show all elements in the stack

---

### ✅ **2. What is a Multi-Stack?**

* A **multi-stack** is when **two stacks share the same array**.
* Purpose: Efficiently use memory when you want to maintain two stacks in a single array.
* Also known as **Two Stacks in an Array**.

---

### 🏗️ **3. Analogy to Understand Multi-Stack (Building & Helicopter)**

Imagine the array as a **vertical building** with numbered floors (indices).

#### ▶ **Stack 1: The Building Stacker (Ground-Up)**

* People (A, B, C, D...) come in through the **main door** and climb up the building from **floor 0 upward**.
* This stack grows from **left to right**.
* Initially: `top1 = -1` (no element)
* Each `push1` moves top1 upward

  * A goes to index 0
  * B goes to index 1
  * C to 2, and so on

#### ▶ **Stack 2: The Helicopter Stacker (Top-Down)**

* People (X, Y, Z...) **land on the roof via helicopter** and come down from **top floor downward**.
* This stack grows from **right to left**.
* Initially: `top2 = size` (beyond last index)
* Each `push2` moves top2 downward

  * X goes to index size - 1
  * Y to size - 2
  * Z to size - 3, and so on

#### ⚡ Rule:

* Both stacks grow **toward each other**.
* Overflow occurs if `top1 + 1 == top2`

---

### ✉️ **4. Stack Operations**

#### ▶ For Stack 1 (Building Stack):

```cpp
void push1(int x) {
  if (top1 + 1 == top2) throw Overflow;
  top1++;
  arr[top1] = x;
}

void pop1() {
  if (top1 == -1) throw Underflow;
  top1--;
}

int peek1() {
  if (top1 == -1) throw Empty;
  return arr[top1];
}

void display1() {
  for (int i = top1; i >= 0; i--) cout << arr[i] << " ";
}
```

#### ▶ For Stack 2 (Helicopter Stack):

```cpp
void push2(int x) {
  if (top1 + 1 == top2) throw Overflow;
  top2--;
  arr[top2] = x;
}

void pop2() {
  if (top2 == size) throw Underflow;
  top2++;
}

int peek2() {
  if (top2 == size) throw Empty;
  return arr[top2];
}

void display2() {
  for (int i = top2; i < size; i++) cout << arr[i] << " ";
}
```

---

### ✨ **5. Summary Table**

| Feature        | Stack 1 (Building) | Stack 2 (Helicopter) |
| -------------- | ------------------ | -------------------- |
| Grows from     | Left to right      | Right to left        |
| Initial top    | -1                 | size                 |
| First element  | index 0            | index size - 1       |
| Overflow check | `top1 + 1 == top2` | `top1 + 1 == top2`   |
| Visual Flow    | Bottom-up          | Top-down             |

---

### 🔧 **6. Pro Tip**

Memorize it like this:

* **"Stack 1 builds up like floors"**
* **"Stack 2 drops down like airdrops from helicopters"**

If you ever get confused: draw a building ⛳ and label index 0 at the bottom, index N at the top.
Then visualize top1 going up and top2 coming down. No way to forget it.

---

