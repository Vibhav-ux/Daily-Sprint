# ⚡ JavaScript — Day 01: Modern JS Foundations
### ES6+ Core | Scoping, Primitives vs Objects, Functions & Arrow Syntax

---

## 📌 1. `var` vs `let` vs `const`

| Feature | `var` | `let` | `const` |
|---|---|---|---|
| **Scope** | Function scope | Block `{}` scope | Block `{}` scope |
| **Hoisting** | Hoisted + initialized as `undefined` | Hoisted but in Temporal Dead Zone (TDZ) | Hoisted in TDZ |
| **Re-assignment** | ✅ Allowed | ✅ Allowed | ❌ TypeError |
| **Re-declaration** | ✅ Allowed (bug prone!) | ❌ SyntaxError | ❌ SyntaxError |

> **Rule of thumb**: Always use `const` by default. Use `let` only when the value must change (e.g. loop counters). Never use `var`.

```javascript
// Temporal Dead Zone Example:
// console.log(greeting); // ❌ ReferenceError: Cannot access 'greeting' before initialization
let greeting = "Hello, Sprint!";
```

---

## 📌 2. Data Types & Type Coercion

### Primitive Types (Stored by value in stack)
* `string`, `number`, `bigint`, `boolean`, `undefined`, `symbol`, `null`

### Reference Types (Stored in heap, pointer in stack)
* `object`, `array`, `function`

### Equality: `==` vs `===`
```javascript
console.log(5 == "5");   // true (loose equality: coerces string to number)
console.log(5 === "5");  // false (strict equality: checks both type and value)
console.log(null == undefined);  // true
console.log(null === undefined); // false
```

---

## 📌 3. Traditional Functions vs Arrow Functions

Arrow functions (`() => {}`) have two critical distinctions from regular `function`:
1. **No `this` binding**: Arrow functions inherit `this` lexically from their enclosing scope.
2. **No `arguments` object**: Use rest parameters `(...args)` instead.

```javascript
// Regular Function
function add(a, b) {
  return a + b;
}

// Arrow Function (concise body with implicit return)
const multiply = (a, b) => a * b;

// Lexical 'this' demonstration:
const counter = {
  count: 0,
  start() {
    setInterval(() => {
      this.count++; // 'this' refers to counter object!
      // If function() {} was used, 'this' would be Window or undefined.
    }, 1000);
  }
};
```

---

## 📌 4. Template Literals & Ternary Operator

```javascript
const user = "Vibhav";
const streak = 5;

// Template Literal (multi-line + variable interpolation)
const statusMessage = `Student ${user} has maintained a ${streak}-day streak.
Status: ${streak >= 3 ? "🔥 On Fire!" : "🌱 Warming up"}`;

console.log(statusMessage);
```
