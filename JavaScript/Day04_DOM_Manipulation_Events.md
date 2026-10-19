# ⚡ JavaScript — Day 04: DOM Manipulation
### Selecting Elements · Events · Creating/Removing Nodes · Event Delegation

---

> 📌 Create `day4_dom.html` and `day4_dom.js` and open in browser.

---

## PART 1 — Selecting Elements

The DOM (Document Object Model) is the browser's tree representation of your HTML.
JavaScript lets you query and manipulate this tree in real time.

```javascript
// ── Best Practice: Always prefer querySelector / querySelectorAll ──

const heading    = document.querySelector("h1");           // first <h1>
const submitBtn  = document.querySelector("#submit-btn");  // by ID
const allCards   = document.querySelectorAll(".card");     // NodeList of all .card

// ── Older alternatives (still common in codebases) ──
const byId   = document.getElementById("submit-btn");
const byTag  = document.getElementsByTagName("h1")[0];
const byClass = document.getElementsByClassName("card");  // HTMLCollection (live!)

// ── Traversal ──
const container = document.querySelector(".container");
console.log(container.children);          // direct child elements
console.log(container.parentElement);     // parent element
console.log(container.firstElementChild); // first child element
console.log(heading.nextElementSibling);  // next sibling
```

---

## PART 2 — Reading & Modifying Elements

```javascript
const card = document.querySelector(".card");

// ── Reading ──
console.log(card.textContent);    // all text content (including hidden)
console.log(card.innerHTML);      // HTML string inside the element
console.log(card.id);             // attribute value

// ── Modifying content ──
card.textContent = "Day 4 — DOM Study";      // safe: treats as plain text
card.innerHTML = "<strong>Day 4</strong>";   // ⚠️ can run scripts (XSS risk!)

// ── Modifying attributes ──
const link = document.querySelector("a");
link.setAttribute("href", "https://github.com/Vibhav-ux");
link.getAttribute("href");        // read attribute
link.removeAttribute("target");   // remove attribute

// ── Modifying styles ──
card.style.backgroundColor = "#1e1e2e";
card.style.borderRadius = "12px";
card.style.padding = "1rem";

// ── ClassList API (the RIGHT way to toggle styles) ──
card.classList.add("active");
card.classList.remove("hidden");
card.classList.toggle("highlighted");     // add if absent, remove if present
card.classList.contains("active");        // true / false
```

---

## PART 3 — Creating & Removing DOM Nodes

```javascript
// ── Create a new element ──
const newCard = document.createElement("div");
newCard.classList.add("card");
newCard.textContent = "Newly created card!";
newCard.dataset.id = "42";   // data attributes: data-id="42"

// ── Append to DOM ──
const container = document.querySelector(".container");
container.appendChild(newCard);           // add at end
container.prepend(newCard);               // add at start
container.insertBefore(newCard, container.firstElementChild);

// ── Modern: insertAdjacentElement ──
container.insertAdjacentElement("beforeend", newCard);

// ── Remove elements ──
newCard.remove();                         // remove itself
container.removeChild(container.lastElementChild);
```

---

## PART 4 — Events & Event Handling

```javascript
const btn = document.querySelector("#submit-btn");

// ── addEventListener (preferred way) ──
btn.addEventListener("click", (event) => {
    console.log("Clicked!", event.target);
    event.preventDefault();   // stop default browser behavior
    event.stopPropagation();  // stop event bubbling up to parents
});

// ── Common Event Types ──
// click, dblclick, mouseover, mouseout, mouseenter, mouseleave
// keydown, keyup, keypress
// submit, change, input, focus, blur
// DOMContentLoaded, load, scroll, resize

// ── keyboard events ──
document.addEventListener("keydown", (e) => {
    console.log(e.key, e.code, e.ctrlKey, e.shiftKey);
    if (e.key === "Escape") closeModal();
});

// ── input event (fires on every keystroke) ──
const searchInput = document.querySelector("#search");
searchInput.addEventListener("input", (e) => {
    console.log("Search value:", e.target.value);
});
```

---

## PART 5 — Event Delegation (Performance Pattern)

Instead of adding listeners to every child, add ONE listener to the parent.
This is critical for dynamically created elements.

```javascript
// ❌ Bad: individual listeners for every button
document.querySelectorAll(".delete-btn").forEach(btn => {
    btn.addEventListener("click", handleDelete);
});
// Breaks for dynamically added buttons!

// ✅ Good: single listener on parent using event delegation
document.querySelector(".task-list").addEventListener("click", (e) => {
    if (e.target.classList.contains("delete-btn")) {
        const taskItem = e.target.closest(".task-item");
        taskItem.remove();
    }
    if (e.target.classList.contains("complete-btn")) {
        e.target.closest(".task-item").classList.toggle("done");
    }
});
```

---

## 💡 Day 04 Challenge: Build a Live Task Manager

```html
<!-- day4_dom.html -->
<!DOCTYPE html>
<html lang="en">
<head><title>Task Manager</title></head>
<body>
  <input id="task-input" type="text" placeholder="Enter a task...">
  <button id="add-btn">Add Task</button>
  <ul id="task-list"></ul>
  <script src="day4_dom.js"></script>
</body>
</html>
```

```javascript
// day4_dom.js
const input    = document.querySelector("#task-input");
const addBtn   = document.querySelector("#add-btn");
const taskList = document.querySelector("#task-list");

function addTask() {
    const text = input.value.trim();
    if (!text) return;
    const li = document.createElement("li");
    li.innerHTML = `${text} <button class="delete-btn">🗑</button>`;
    taskList.appendChild(li);
    input.value = "";
}

addBtn.addEventListener("click", addTask);
input.addEventListener("keydown", (e) => { if (e.key === "Enter") addTask(); });

// Event delegation for delete buttons
taskList.addEventListener("click", (e) => {
    if (e.target.classList.contains("delete-btn")) {
        e.target.closest("li").remove();
    }
});
```
