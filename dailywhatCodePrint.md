Good morning! Here's your coding interview problem for today.

This problem was asked by Google.

What will this code print out?

def make_functions():
    flist = []

    for i in [1, 2, 3]:
        def print_i():
            print(i)
        flist.append(print_i)

    return flist

functions = make_functions()
for f in functions:
    f()
How can we make it print out what we apparently want?


===========================================================

### **Problem Analysis**

The code creates a list of functions (`flist`) inside a loop and then calls them. However, the behavior might not be what the programmer expects because of **late binding** in Python closures.

### **Key Concept: Late Binding in Closures**
When `print_i` is defined inside the loop, it captures a reference to the variable `i`, not its current value. By the time the functions are executed, the loop has completed, and `i` is set to its final value (`3`). Therefore, all functions in `flist` will print `3`.

### **What the Code Prints**
The output will be:
```
3
3
3
```

---

### **How to Fix It**

To make the functions print the intended values (`1`, `2`, and `3`), we need to bind the current value of `i` at each iteration. This can be done in multiple ways:

---

#### **1. Use Default Arguments**

Default arguments in Python evaluate when the function is defined, capturing the current value of `i`:

```python
def make_functions():
    flist = []

    for i in [1, 2, 3]:
        def print_i(i=i):  # Default argument captures the current value of `i`
            print(i)
        flist.append(print_i)

    return flist

functions = make_functions()
for f in functions:
    f()
```

**Output:**
```
1
2
3
```

---

#### **2. Use a Lambda Inside a Function**

Create a function that returns a lambda or function, which explicitly captures the value of `i`:

```python
def make_functions():
    flist = []

    for i in [1, 2, 3]:
        def make_print_i(val):
            def print_i():
                print(val)
            return print_i

        flist.append(make_print_i(i))

    return flist

functions = make_functions()
for f in functions:
    f()
```

**Output:**
```
1
2
3
```

---

#### **3. Use a Function Instead of a Loop**

If the loop isn't essential, create each function directly:

```python
def make_functions():
    return [
        lambda: print(1),
        lambda: print(2),
        lambda: print(3),
    ]

functions = make_functions()
for f in functions:
    f()
```

**Output:**
```
1
2
3
```

---

### **Why These Fixes Work**
- **Default Arguments:** Bind the value of `i` at the time of function definition.
- **Explicit Closure:** Pass the value of `i` explicitly to a nested function, avoiding late binding.
- **Direct Function Creation:** Avoid the issue entirely by explicitly creating each function.

Using **default arguments** is the most Pythonic and concise solution in this case.
