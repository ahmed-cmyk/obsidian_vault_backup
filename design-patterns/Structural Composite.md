# Structural: Composite

**Composite** is a structural design pattern that lets you treat individual objects and compositions of objects uniformly.

---
## Structure

1. **Component Interface**: Declares common operations for both simple and complex elements.
2. **Leaf**: Represents individual (atomic) objects in the structure.
3. **Composite**: Contains children (leaves or other composites) and implements child-related operations like `add`, `remove`, and `display`.
4. **Client**: Works with all elements through the component interface, without caring whether it’s a leaf or composite.
⠀
This pattern enables **recursive tree structures**, where composites can contain other composites or leaves.

---
## Applicability

Use the Composite pattern when:

* You need to represent **part-whole hierarchies** of objects.
* Clients should treat both simple and complex structures **uniformly**.
* You want to use **recursion** to process nested structures.
* You want to simplify client code that works with complex object trees.

---
## How to Implement

1. Define a common **component interface** for both leaves and composites.
2. Implement **Leaf** classes that perform actual work.
3. Implement **Composite** classes that maintain child components and delegate calls to them.
4. Make sure **client code** only interacts with the component interface.
5. Optionally, add tree traversal, rendering, or aggregation behavior in the composite.
⠀
---
## Pros and Cons

| Pros | Cons |
|---|---|
| Treats individual and composite objects uniformly. | Can make code harder to understand due to recursive structure. |
| Makes tree structures easier to manage. | Can lead to a design where leaves and composites have empty or meaningless implementations for some methods. |
| Supports **Open/Closed Principle**—new components can be added without changing existing code. | Might add overhead if tree is very deep or has many operations. | 

---
## Example Use Cases

* Building a **UI hierarchy** where buttons, labels, and panels are all treated uniformly.
* Representing a **filesystem** of files and folders.
* Creating a **mathematical expression tree**.
* Managing **organization charts**, **task trees**, or **menus**.

---
## Example

### Scenario:

You’re creating a graphic editor where both shapes and groups of shapes must be drawn using the same interface.

#### Step 1: The `Component Interface`

```ts
tsCopyEditinterface Graphic {
  draw(): void;
}
```

#### Step 2: `Leaf` Classes

```ts
tsCopyEditclass Circle implements Graphic {
  draw(): void {
    console.log("Drawing Circle");
  }
}

class Square implements Graphic {
  draw(): void {
    console.log("Drawing Square");
  }
}
```

#### Step 3: The `Composite` Class

```ts
tsCopyEditclass CompoundGraphic implements Graphic {
  private children: Graphic[] = [];

  add(child: Graphic): void {
    this.children.push(child);
  }

  remove(child: Graphic): void {
    this.children = this.children.filter(c => c !== child);
  }

  draw(): void {
    console.log("Drawing CompoundGraphic:");
    for (const child of this.children) {
      child.draw(); // Delegates to child elements
    }
  }
}
```

#### Step 4: `Client` Code

```ts
tsCopyEditconst circle = new Circle();
const square = new Square();

const group = new CompoundGraphic();
group.add(circle);
group.add(square);

const complexDrawing = new CompoundGraphic();
complexDrawing.add(group);
complexDrawing.add(new Square());

complexDrawing.draw();
```

#### Output:

```mathematica
mathematicaCopyEditDrawing CompoundGraphic:
Drawing CompoundGraphic:
Drawing Circle
Drawing Square
Drawing Square
```

---

#design-patterns