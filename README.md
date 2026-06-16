[![Review Assignment Due Date](https://classroom.github.com/assets/deadline-readme-button-22041afd0340ce965d47ae6ef1cefeee28c7c493a6346c4f15d667ab976d596c.svg)](https://classroom.github.com/a/LUMU1T0W)
# Java Practice Problem — The Shape Area Calculator

**Course:** Object-Oriented Pattern Design Lab
**Course Code:** ICT 2208
**Learning Topics:** Abstract Classes · Method Overriding · Early Binding · Downcasting · `toString()`

---

## The Story

Rafi is a first-year student at MBSTU who just started learning Java. His professor gives the class a small project:

> "Build a shape calculator for a geometry app. The app must support **rectangles** and **circles**. Every shape has a color and can be filled or unfilled. Each shape must be able to report its **area**, its **perimeter**, and a text summary of itself."

The professor adds one important rule:

> "You must design this so that any new shape — triangle, pentagon, anything — can be added later without rewriting the calculator code."

Rafi realizes he needs an **abstract class** called `Shape` that enforces a contract: every shape *must* implement `getArea()` and `getPerimeter()`. `Rectangle` and `Circle` will extend `Shape` and provide their own formulas.

---

## UML Class Diagram

<img width="1472" height="1138" alt="image" src="https://github.com/user-attachments/assets/2ae8136e-44c8-4297-86e2-c7ff7207fecd" />


Alternative UML: (if not visible above)
```
+------------------------------------------+
|              <<abstract>>                |
|                 Shape                    |
+------------------------------------------+
| - color  : String                        |
| - filled : boolean                       |
+------------------------------------------+
| + Shape(color, filled)                   |
| + getArea()      : double  <<abstract>>  |
| + getPerimeter() : double  <<abstract>>  |
| + toString()     : String                |
+------------------------------------------+
              ^               ^
              |               |
         extends         extends
              |               |
+---------------------------+   +---------------------------+
|        Rectangle          |   |          Circle           |
+---------------------------+   +---------------------------+
| - width  : double         |   | - radius : double         |
| - length : double         |   +---------------------------+
+---------------------------+   | + Circle(color,filled,    |
| + Rectangle(color,filled, |   |          radius)          |
|             width,length) |   | + getArea()      : double |
| + getArea()      : double |   | + getPerimeter() : double |
| + getPerimeter() : double |   | + toString()     : String |
| + toString()     : String |   +---------------------------+
+---------------------------+
```

**Formulas:**

```
Rectangle  →  area = width × length           perimeter = 2 × (width + length)
Circle     →  area = π × radius²              circumference = 2 × π × radius
```

---

## Concepts You Will Practice

| Concept | Where It Appears |
|---|---|
| Abstract class | `Shape` cannot be instantiated directly |
| Abstract method | `getArea()` and `getPerimeter()` declared but not implemented in `Shape` |
| Method overriding | Each subclass provides its own area and perimeter formulas |
| Early binding | `toString()` on a `Shape` reference — compile-time type governs accessibility, runtime type governs which version runs |
| Downcasting | Casting a `Shape` reference back to `Rectangle` to call `getWidth()` |
| `toString()` | Each class formats itself as a readable one-block string |

---

## Project Structure

Your GitHub Classroom repository must have exactly this layout:

```
src/
  shapes/
    Shape.java
    Rectangle.java
    Circle.java
    ShapeMain.java
```

The autograder compiles from the `src/` root and runs `shapes.ShapeMain`.

---

## Starter Code

### `Shape.java`

```java
package shapes;

public abstract class Shape {

    private String  color;
    private boolean filled;

    public Shape(String color, boolean filled) {
        this.color  = color;
        this.filled = filled;
    }

    public abstract double getArea();
    public abstract double getPerimeter();

    @Override
    public String toString() {
        return "Color : " + color + "\n" +
               "Filled: " + (filled ? "Yes" : "No");
    }

    public String  getColor()           { return color;  }
    public boolean isFilled()           { return filled; }
    public void    setColor(String c)   { this.color  = c; }
    public void    setFilled(boolean f) { this.filled = f; }
}
```

### `Rectangle.java`

```java
package shapes;

public class Rectangle extends Shape {

    private double width;
    private double length;

    public Rectangle(String color, boolean filled, double width, double length) {
        super(color, filled);
        this.width  = width;
        this.length = length;
    }

    @Override
    public double getArea() {
        return width * length;
    }

    @Override
    public double getPerimeter() {
        return 2 * (width + length);
    }

    @Override
    public String toString() {
        return "[ Rectangle ]\n" +
               super.toString()  + "\n" +
               "Width : " + width  + "\n" +
               "Length: " + length + "\n" +
               String.format("Area      : %.2f", getArea())      + "\n" +
               String.format("Perimeter : %.2f", getPerimeter());
    }

    public double getWidth()  { return width;  }
    public double getLength() { return length; }
}
```

### `Circle.java`

```java
package shapes;

public class Circle extends Shape {

    private double radius;

    public Circle(String color, boolean filled, double radius) {
        super(color, filled);
        this.radius = radius;
    }

    @Override
    public double getArea() {
        return Math.PI * radius * radius;
    }

    @Override
    public double getPerimeter() {
        return 2 * Math.PI * radius;
    }

    @Override
    public String toString() {
        return "[ Circle ]\n" +
               super.toString() + "\n" +
               "Radius: " + radius + "\n" +
               String.format("Area         : %.2f", getArea())      + "\n" +
               String.format("Circumference: %.2f", getPerimeter());
    }

    public double getRadius() { return radius; }
}
```

---

## Your Task — `ShapeMain.java`

Read input from `System.in` using a `Scanner`. The program will receive a series of shape descriptions and must print each shape's full summary followed by a blank line.

### Input format

```
<numberOfShapes>
For each shape:
  <shapeType>          ← RECTANGLE or CIRCLE
  <color> <filled>     ← e.g.  Red true
  <dimensions>         ← width length  (Rectangle)  OR  radius  (Circle)
```

### Output format

Print each shape's `toString()` output followed by one blank line.  
All decimal values must be formatted to **exactly 2 decimal places**.

---

## Sample Input and Output

Here is the formatted content optimized for a GitHub repository **README.md**. It utilizes clean markdown components, code blocks, and clear hierarchies to make the grading criteria easy for students to read.

---

```markdown
## ⚙️ Autograding Test Suites

This assignment is automatically graded out of **5 marks** using GitHub Classroom Input/Output tests. Below are the exact test configurations and sample cases used to evaluate your submission.

---

### 🧪 Test Case 1: Simple Mix Test (5 Marks)
This test case passes exactly **1 Rectangle** and **1 Circle** to verify that both subclasses, text formatting, and explicit downcasting logic work correctly.

* **Test Name:** `Mix Shapes Test`
* **Setup Command:** `javac shapes/Shape.java shapes/Rectangle.java shapes/Circle.java shapes/ShapeMain.java`
* **Run Command:** `java shapes.ShapeMain`
* **Comparison Type:** `Exact match`
* **Total Points:** `5`

#### Expected Console Interaction

**📥 Sample Input:**
```text
2
RECTANGLE
Red true
4.0 5.0
CIRCLE
Blue false
3.0

```

**📤 Expected Output:**

```text
[ Rectangle ]
Color : Red
Filled: Yes
Width : 4.0
Length: 5.0
Area      : 20.00
Perimeter : 18.00

[ Circle ]
Color : Blue
Filled: No
Radius: 3.0
Area         : 28.27
Circumference: 18.85

--- Downcast Check ---
Rectangle width=4.0 length=5.0
Circle radius=3.0

```

---

### 🧪 Test Case 2: Alternate Parameters Test (5 Marks)

This case tests dynamic variable tracking and ensures decimal values round correctly according to your `String.format()` structures.

* **Test Name:** `Dynamic Math Test`
* **Setup Command:** `javac shapes/Shape.java shapes/Rectangle.java shapes/Circle.java shapes/ShapeMain.java`
* **Run Command:** `java shapes.ShapeMain`
* **Comparison Type:** `Exact match`
* **Total Points:** `5`

#### Expected Console Interaction

**📥 Sample Input:**

```text
2
CIRCLE
Green true
1.5
RECTANGLE
Yellow false
2.5 3.5

```

**📤 Expected Output:**

```text
[ Circle ]
Color : Green
Filled: Yes
Radius: 1.5
Area         : 7.07
Circumference: 9.42

[ Rectangle ]
Color : Yellow
Filled: No
Width : 2.5
Length: 3.5
Area      : 8.75
Perimeter : 12.00

--- Downcast Check ---
Circle radius=1.5
Rectangle width=2.5 length=3.5

```

```

```

---

## `ShapeMain.java` Skeleton

```java
package shapes;

import java.util.Scanner;

public class ShapeMain {

    public static void main(String[] args) {

        Scanner sc = new Scanner(System.in);

        // ── TASK 1 ────────────────────────────────────────────
        // Read the number of shapes from input.
        //
        int n = /* YOUR CODE */;

        // ── TASK 2 ────────────────────────────────────────────
        // Declare a Shape array of size n.
        //
        Shape[] shapes = /* YOUR CODE */;

        // ── TASK 3 ────────────────────────────────────────────
        // Loop n times. For each iteration:
        //   a) Read the shape type  (RECTANGLE or CIRCLE)
        //   b) Read color and filled on the same line
        //   c) Read dimensions (width + length OR radius)
        //   d) Create the correct object and store it in shapes[i]
        //      Use the Shape reference type for the array element.
        //
        for (int i = 0; i < n; i++) {
            String type   = /* YOUR CODE */;
            String color  = /* YOUR CODE */;
            boolean filled = /* YOUR CODE */;

            if (type.equals("RECTANGLE")) {
                double width  = /* YOUR CODE */;
                double length = /* YOUR CODE */;
                shapes[i] = /* YOUR CODE */;

            } else if (type.equals("CIRCLE")) {
                double radius = /* YOUR CODE */;
                shapes[i] = /* YOUR CODE */;
            }
        }

        // ── TASK 4 ────────────────────────────────────────────
        // Loop through the shapes array.
        // For each shape: print it (toString() is called automatically)
        // then print a blank line.
        //
        for (Shape s : shapes) {
            /* YOUR CODE */
        }

        // ── TASK 5 — Downcasting ─────────────────────────────
        // After printing all shapes, loop through the array again.
        // If the shape is a Rectangle, downcast it and print:
        //   "Rectangle width=" + width + " length=" + length
        // If the shape is a Circle, downcast it and print:
        //   "Circle radius=" + radius
        //
        System.out.println("--- Downcast Check ---");
        for (Shape s : shapes) {
            if (s instanceof Rectangle) {
                Rectangle r = /* YOUR CODE */;
                System.out.println("Rectangle width="  + r.getWidth()
                                 + " length=" + r.getLength());
            } else if (s instanceof Circle) {
                Circle c = /* YOUR CODE */;
                System.out.println("Circle radius=" + c.getRadius());
            }
        }

        // ── TASK 6 — Abstract class (written answer) ──────────
        // What error does the compiler give if you write:
        //     Shape s = new Shape("Red", true);
        //
        // Write your answer here as a comment:
        // ANSWER: _______________________________________________

        sc.close();
    }
}
```

---

## Updated Expected Output (with Downcast Check)

The autograder tests `ShapeMain` end-to-end including the downcast section.  
Below is the full expected output for **Test 3**:

```
[ Rectangle ]
Color : Green
Filled: Yes
Width : 4.0
Length: 9.0
Area      : 36.00
Perimeter : 26.00

[ Circle ]
Color : Yellow
Filled: No
Radius: 3.5
Area         : 38.48
Circumference: 21.99

[ Rectangle ]
Color : Black
Filled: No
Width : 10.0
Length: 2.5
Area      : 25.00
Perimeter : 25.00

--- Downcast Check ---
Rectangle width=4.0 length=9.0
Circle radius=3.5
Rectangle width=10.0 length=2.5
```

---

## Grading Rubric

| Test | Description | Points |
|---|---|---|
| Test 1 | Single rectangle — area, perimeter, toString | 2 |
| Test 2 | Single circle — area, circumference, toString | 2 |
| Test 3 | Mixed rectangle + circle + downcast check | 2 |
| Test 4 | Rectangles only — downcast reports both | 2 |
| Test 5 | Large decimal values formatted correctly | 2 |
| **Total** | | **10** |

---

## Concept Explanation Notes

### Abstract class

`Shape` is declared `abstract`, so `new Shape(...)` causes a **compile-time error**:

```
error: Shape is abstract; cannot be instantiated
```

Its job is to define the contract — every shape must provide `getArea()` and `getPerimeter()` — without specifying how to compute them.

### Method overriding

When `Rectangle` writes `@Override public double getArea()`, it replaces `Shape`'s declaration with its own formula. Rules for a valid override:

- Same method name and parameter list.
- Same or covariant return type.
- Cannot reduce visibility (`public` cannot become `private`).

The `@Override` annotation is optional but strongly recommended — the compiler catches signature typos.

### Early binding vs late binding

`static`, `final`, and `private` methods use **early (static) binding** — resolved at compile time.  
All other methods use **late (dynamic) binding** — the JVM checks the actual object type at runtime.

```java
Shape s = new Rectangle("Red", true, 5, 8);
s.getArea();      // calls Rectangle.getArea() at runtime — late binding
s.toString();     // calls Rectangle.toString() at runtime — late binding
```

The declared type `Shape` controls what method *names* are accessible.  
The runtime type `Rectangle` controls which *version* runs.

### Downcasting

```java
Shape s1 = new Rectangle("Red", true, 5.0, 8.0);

// s1.getWidth();          ← compile error: Shape has no getWidth()

if (s1 instanceof Rectangle) {
    Rectangle r = (Rectangle) s1;   // downcast
    r.getWidth();                    // now accessible
}
```

Always guard with `instanceof` before casting. An unguarded cast to the wrong type throws `ClassCastException` at runtime.

### Why `toString()` in each class?

`System.out.println(shape)` automatically calls `toString()` on the object. By overriding it in every subclass (and calling `super.toString()` to reuse the parent's color/filled lines), each shape can print a complete, neatly formatted summary of itself with no extra code in `main`.

---

## Common Mistakes to Watch For

| Mistake | What happens |
|---|---|
| `new Shape("Red", true)` | Compile error — cannot instantiate abstract class |
| Forgetting `@Override` on `getArea()` | Silently creates a new method; the abstract method remains unimplemented — compile error |
| `(Rectangle) s2` where `s2` holds a `Circle` | `ClassCastException` at runtime |
| Calling `getWidth()` on a `Shape` reference | Compile error — not declared in `Shape` |
| `toString()` not calling `super.toString()` | Output loses `color` and `filled` lines — test fails |
| Extra space or wrong decimal places | Autograder uses exact match — `40.0` fails where `40.00` is expected |

---

## Submission Checklist

- [ ] `src/shapes/Shape.java` — abstract class, two abstract methods, `toString()`
- [ ] `src/shapes/Rectangle.java` — overrides `getArea()`, `getPerimeter()`, `toString()`
- [ ] `src/shapes/Circle.java` — overrides `getArea()`, `getPerimeter()`, `toString()`
- [ ] `src/shapes/ShapeMain.java` — reads input, fills array, prints shapes, prints downcast check
- [ ] All 5 autograder tests pass (100 / 100)
- [ ] Written answer for Task 6 is filled in as a comment
