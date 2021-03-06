class: middle, center

# Replace Primitive With Object

#### A Refactoring by Martin Fowler and Kent Beck

---

### Goal

The goal is to refactor a "concept" that is represented by a primitive value
(in this case a string) into an object.

```ts
orders.filter(order => order.priority === 'high' ||
                        order.priority === 'rush');
```

<span class="arrow center">&#8681;</span>

```ts
orders.filter(order => 
    order.priority.higherThan(new Priority('high'))
);
```
???

At the top, the order `priority` is represented here by a `string`.

The goal is to refactor the concept of order priority from a string to an object.

As you can see in the second example, using the `Priority` class allows 
the code to be very expressive.

In some circumstances, using a primitive data type might be perfectly acceptable. 

However, I would argue as software matures it rarely ever turns out to be 
the right choice. 

---

### Refactoring Steps

1. Apply Encapsulate Variable if it isn't already
2. Create a simple value class for the data value. It should take the existing value in its constructor and provide a getter for that value.
3. Run static checks
4. Change the setter to create a new instance of the value class and store that in the field, changing the type of the field if present.
5. Change the getter to return the result of invoking the getter of the new class.
6. Test.
7. Consider using Rename Function on the original accessors to better reflect what they do.
---
layout: true
class: columns

---

.left-column[

### Steps

- Apply Encapsulate Variable
]

.right-column[

This is refering to creating an `Order` class if it isn't already.

```ts
class Order {
    get priority(): string {
        return this.#priority;
    }

    set priority(priority: string) {
        this.#priority = priority;
    }
}
```

]

???

### 1. Apply Encapsulate Variable if it isn't already
2. Create a simple value class for the data value. It should take the existing value in its constructor and provide a getter for that value.
3. Run static checks
4. Change the setter to create a new instance of the value class and store that in the field, changing the type of the field if present.
5. Change the getter to return the result of invoking the getter of the new class.
6. Test.
7. Consider using Rename Function on the original accessors to better reflect what they do.

---

.left-column[

### Steps

- Apply Encapsulate Variable
- Create a simple value class for the data value.
]

.right-column[

```ts
class Priority {
    public constructor(value) {
        this.#value = value;
    }

    public toString(): string {
        return this.#value;
    }
}
```

]

???

1. Apply Encapsulate Variable if it isn't already
### 2. Create a simple value class for the data value. It should take the existing value in its constructor and provide a getter for that value.
3. Run static checks
4. Change the setter to create a new instance of the value class and store that in the field, changing the type of the field if present.
5. Change the getter to return the result of invoking the getter of the new class.
6. Test.
7. Consider using Rename Function on the original accessors to better reflect what they do.

The `toString` method could alternatively be a getter 

```ts
class Priority {
    // ...snip
    public get value(): string {
        return this.#value;
    }
}
```

---

.left-column[

### Steps

- Apply Encapsulate Variable
- Create a simple value class for the data value.
- Run static checks
]

.right-column[

Run the TypeScript compiler to check for errors.

]

???

1. Apply Encapsulate Variable if it isn't already
2. Create a simple value class for the data value. It should take the existing value in its constructor and provide a getter for that value.
### 3. Run static checks
4. Change the setter to create a new instance of the value class and store that in the field, changing the type of the field if present.
5. Change the getter to return the result of invoking the getter of the new class.
6. Test.
7. Consider using Rename Function on the original accessors to better reflect what they do.

---

.left-column[

### Steps

- Apply Encapsulate Variable
- Create a simple value class for the data value.
- Run static checks
- Update Order setter
]

.right-column[

The `priority` setter we initially set up for the `Order` class now needs to be updated.

```ts
class Order {

    set priority(priority: string) {
        this.#priority = 
            new Priority(priority);
    }
}
```

]

???

1. Apply Encapsulate Variable if it isn't already
2. Create a simple value class for the data value. It should take the existing value in its constructor and provide a getter for that value.
3. Run static checks
### 4. Change the setter to create a new instance of the value class and store that in the field, changing the type of the field if present.
5. Change the getter to return the result of invoking the getter of the new class.
6. Test.
7. Consider using Rename Function on the original accessors to better reflect what they do.


---

.left-column[

### Steps

- Apply Encapsulate Variable
- Create a simple value class for the data value.
- Run static checks
- Update Order setter
- Change the getter to return the result of invoking the getter of the new class.
]

.right-column[

```ts
class Order {
    get priority(): string {
        return this.#priority.toString();
    }
}
```

]

???

1. Apply Encapsulate Variable if it isn't already
2. Create a simple value class for the data value. It should take the existing value in its constructor and provide a getter for that value.
3. Run static checks
4. Change the setter to create a new instance of the value class and store that in the field, changing the type of the field if present.
### 5. Change the getter to return the result of invoking the getter of the new class.
6. Test.
7. Consider using Rename Function on the original accessors to better reflect what they do.


---

.left-column[

### Steps

- Apply Encapsulate Variable
- Create a simple value class for the data value.
- Run static checks
- Update Order setter
- Change the getter to return the result of invoking the getter of the new class.
- Test.
]

.right-column[

Run the test runner to check for any errors.

]

???

1. Apply Encapsulate Variable if it isn't already
2. Create a simple value class for the data value. It should take the existing value in its constructor and provide a getter for that value.
3. Run static checks
4. Change the setter to create a new instance of the value class and store that in the field, changing the type of the field if present.
5. Change the getter to return the result of invoking the getter of the new class.
### 6. Test.
7. Consider using Rename Function on the original accessors to better reflect what they do.

---

.left-column[

### Steps

- Apply Encapsulate Variable
- Create a simple value class for the data value.
- Run static checks
- Update Order setter
- Change the getter to return the result of invoking the getter of the new class.
- Test.
- Rename Function on accessors
]

.right-column[

This is an optional step.

```ts
class Order {
    get priorityString(): string {
        return this.#priority.toString();
    }

    get priority(): Priority {
        return this.#priority;
    }
}
```
]

???

1. Apply Encapsulate Variable if it isn't already
2. Create a simple value class for the data value. It should take the existing value in its constructor and provide a getter for that value.
3. Run static checks
4. Change the setter to create a new instance of the value class and store that in the field, changing the type of the field if present.
5. Change the getter to return the result of invoking the getter of the new class.
6. Test.
### 7. Consider using Rename Function on the original accessors to better reflect what they do.

We may also want to provide a getter for direct access to the priority object.


---

layout: false

## Additional functionality

```ts
class Priority {
    private static _validValues = 
        new Set(['low', 'normal', 'high', 'rush']);

    public constructor(value: string) {
        if (!Priority._validValues.has(value)) {
            throw new Error(`${value} is an invalid Priority!`);
        }
        this.#value = value;
    }

    public toString(): string {
        return this.#value;
    }

    // ... snip
}
```
???

The `Priority` class now gives us a single place to encapsulate logic 
associated to priorities.

---

## Additional functionality

```ts
class Priority {
    // ... snip

    private get _priorityLevel(): number {
        return Array.from(Priority._validValues.values())
            .indexOf(this.#value);
    }

    public isEqual(other: Priority): boolean {
        return this.#value === other.#value;
    }

    public higherThan(other: Priority): boolean {
        return this._priorityLevel > other._priorityLevel;
    }

    public lessThan(other: Priority): boolean {
        return this._priorityLevel < other._priorityLevel;
    }
}
```

???

Consider what this logic would look like if the priority was still represented by a string

It would be hard to read, difficult to maintain, and prone to bugs. 

---

### Example in IS

```ts
{
    // ...snip
    BBoxMax: "32000 0 67200",
    Pos: "-15520 140000 8000"
}
```

Here the Bounding Box and Position are represented by strings. In order to work with these values
you have to parse it first.

???

Parsing the string itself is simple, but it opens the software up to many potential bugs.

What if you use the first value as the 'z' value instead of the 'x' value?

What if you incorrectly get the screen position based on the parsed values?

If the Bounding Box was represented here with an object, it could have all the required behavior
easily accessible without having to re-write the needed functionality. 
---

### The Refactor Result

```ts
class BoundingBox {
*   public static initFromString(bb: string): BoundingBox {}
    public constructor(bb: { x: number, y: number, z: number }) {}
}
const boundingBox1 = new BoundingBox({ x: 32000, y: 0, z: 67200 });
// or
const boundingBox2 = BoundingBox.initFromString("32000 0 67200");
```

The BoundingBox object gives you a single place to encapsulate all logic
associated to the Bounding Box. 

---

### Summary

In some circumstances, using a primitive data type might be perfectly acceptable. 

However, I would argue as software matures it rarely ever turns out to be 
the right choice. 


---

class: middle, center

# Questions