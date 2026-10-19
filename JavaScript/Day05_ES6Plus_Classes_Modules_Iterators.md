# ⚡ JavaScript — Day 05: ES6+ Modern Features
### Modules · Classes · Symbols · WeakMap/WeakSet · Iterators · Generators

---

> 📌 Run with Node.js or a browser bundler (Vite). Use `.mjs` extension for ES Modules in Node.

---

## PART 1 — ES Modules (`import` / `export`)

Modules let you split code across files with explicit dependency declarations.
This replaced the older CommonJS `require()` pattern in modern JS.

```javascript
// ─── math.js ─── (named exports)
export const PI = 3.14159;
export function add(a, b) { return a + b; }
export function subtract(a, b) { return a - b; }

// Default export (one per file)
export default function multiply(a, b) { return a * b; }

// ─── main.js ─── (consuming the module)
import multiply, { PI, add, subtract } from "./math.js";

// Import all named exports under a namespace
import * as MathUtils from "./math.js";
console.log(MathUtils.add(2, 3));  // 5

// Dynamic import (lazy loading — loads only when needed)
async function loadHeavyModule() {
    const module = await import("./heavy-computation.js");
    module.run();
}
```

---

## PART 2 — ES6 Classes (Syntactic Sugar Over Prototypes)

```javascript
class Animal {
    #name;      // Private field (ES2022) — truly private, unlike _name convention
    #sound;

    constructor(name, sound) {
        this.#name  = name;
        this.#sound = sound;
    }

    speak() {
        return `${this.#name} says ${this.#sound}!`;
    }

    get name() { return this.#name; }       // getter property
    set name(v) { this.#name = v.trim(); }  // setter property

    // Static method: belongs to class, NOT instances
    static create(name, sound) {
        return new Animal(name, sound);
    }
}

class Dog extends Animal {
    #breed;
    constructor(name, breed) {
        super(name, "Woof");    // must call super() first!
        this.#breed = breed;
    }

    describe() {
        return `${this.name} is a ${this.#breed}. ${this.speak()}`;
    }
}

const dog = new Dog("Max", "Labrador");
console.log(dog.describe());
// dog.#name;  // ❌ SyntaxError — truly private!
```

---

## PART 3 — Symbol: Truly Unique Identifiers

`Symbol()` creates a globally unique primitive value. Two `Symbol("desc")` calls are NEVER equal.

```javascript
const id1 = Symbol("id");
const id2 = Symbol("id");
console.log(id1 === id2);   // false (always unique)

// Use case: Adding properties without collision risk
const SECRET_KEY = Symbol("secretKey");
const user = {
    name: "Vibhav",
    [SECRET_KEY]: "abc123"   // symbol-keyed (hidden from for...in, JSON.stringify)
};

console.log(user[SECRET_KEY]);   // "abc123"
console.log(Object.keys(user));  // ["name"] — symbol key not included!
```

---

## PART 4 — WeakMap & WeakSet (Memory-Safe Storage)

Unlike regular `Map`/`Set`, `WeakMap` and `WeakSet`:
- Only accept **objects** as keys/values (not primitives)
- Hold **weak references** → if the object is garbage collected, the entry is automatically removed
- Not iterable (no `.forEach`, no `size`)

**Primary use case:** Caching computed data tied to DOM elements or objects.

```javascript
const cache = new WeakMap();

function processElement(element) {
    if (cache.has(element)) {
        return cache.get(element);    // return cached result
    }
    const result = heavyComputation(element);
    cache.set(element, result);       // cache it
    return result;
}
// When element is removed from DOM → garbage collected → cache entry auto-removed
```

---

## PART 5 — Iterators & Generators

### Custom Iterator Protocol
An object is iterable if it has a `[Symbol.iterator]()` method that returns an iterator.

```javascript
// Making a custom range iterable
function range(start, end, step = 1) {
    return {
        [Symbol.iterator]() {
            let current = start;
            return {
                next() {
                    if (current <= end) {
                        const value = current;
                        current += step;
                        return { value, done: false };
                    }
                    return { value: undefined, done: true };
                }
            };
        }
    };
}

for (const num of range(1, 10, 2)) {
    console.log(num);   // 1  3  5  7  9
}

console.log([...range(0, 5)]);  // [0, 1, 2, 3, 4, 5]
```

### Generator Functions
```javascript
function* fibonacci() {
    let [a, b] = [0, 1];
    while (true) {
        yield a;
        [a, b] = [b, a + b];
    }
}

const fib = fibonacci();
console.log(fib.next().value);  // 0
console.log(fib.next().value);  // 1
console.log(fib.next().value);  // 1
console.log(fib.next().value);  // 2
console.log(fib.next().value);  // 3

// Take first 10 Fibonacci numbers
const first10 = Array.from({ length: 10 }, () => fib.next().value);
```

---

## 💡 Day 05 Challenge

Create an `EventEmitter` class using private fields (`#listeners`) that supports:
- `on(event, callback)` — subscribe
- `emit(event, ...args)` — trigger all subscribers
- `off(event, callback)` — unsubscribe
