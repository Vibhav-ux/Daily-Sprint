# ⚡ JavaScript — Day 02: Arrays, Objects & Modern ES6+
### Functional Iteration, Destructuring, Rest/Spread & Immutability

---

## 📌 1. Essential Functional Array Methods

Modern JavaScript emphasizes pure, functional array transformations:

### `map()` — Transform every element into a new array
```javascript
const prices = [100, 250, 400];
const pricesWithTax = prices.map(p => p * 1.18);
// [118, 295, 472]
```

### `filter()` — Keep elements that satisfy condition
```javascript
const scores = [45, 82, 91, 60, 33];
const passing = scores.filter(s => s >= 50);
// [82, 91, 60]
```

### `reduce()` — Accumulate array into a single value
```javascript
const cart = [
  { item: "Laptop", price: 65000 },
  { item: "Mouse", price: 1500 },
  { item: "Keyboard", price: 3500 }
];

const totalCost = cart.reduce((acc, curr) => acc + curr.price, 0);
// 70000
```

### Method Chaining (Idiomatic React/JS pattern)
```javascript
const highValueTotal = cart
  .filter(p => p.price > 2000)
  .map(p => p.price * 0.9) // 10% discount
  .reduce((acc, p) => acc + p, 0);
```

---

## 📌 2. Destructuring Assignment

Unpack values from arrays or properties from objects into distinct variables:

```javascript
// Object Destructuring with default values and renaming
const developer = {
  name: "Vibhav Shukla",
  role: "Full Stack Engineer",
  experienceYears: 2,
  address: { city: "Lucknow", state: "UP" }
};

const { name, role: title, address: { city }, country = "India" } = developer;
console.log(title); // "Full Stack Engineer"
console.log(city);  // "Lucknow"
console.log(country); // "India"

// Array Destructuring
const coordinates = [26.8467, 80.9462];
const [latitude, longitude] = coordinates;
```

---

## 📌 3. Spread (`...`) & Rest Operators

### Spread Operator (Expanding collections)
```javascript
// Clone and extend arrays
const baseSkills = ["Python", "DSA"];
const updatedSkills = [...baseSkills, "JavaScript", "React"];

// Clone and override object properties (Immutability pattern for React state)
const user = { id: 1, name: "Vibhav", role: "Student" };
const promotedUser = {
  ...user,
  role: "Lead Engineer",
  updatedAt: new Date().toISOString()
};
```

### Rest Parameter (Collecting leftovers)
```javascript
function sumAll(...numbers) {
  return numbers.reduce((sum, n) => sum + n, 0);
}
console.log(sumAll(10, 20, 30, 40)); // 100
```

---

## 📌 4. Shallow Copy vs Deep Copy

* **Shallow Copy (`{ ...obj }`, `Object.assign()`):** Only copies top-level properties; nested objects still share the same reference!
* **Deep Copy (`structuredClone(obj)`):** Deep clones all nested levels.

```javascript
const original = { a: 1, nested: { b: 2 } };
const shallow = { ...original };
shallow.nested.b = 99;
console.log(original.nested.b); // 99 (mutated because nested reference was shared!)

// Deep clone safe approach:
const deep = structuredClone(original);
```
