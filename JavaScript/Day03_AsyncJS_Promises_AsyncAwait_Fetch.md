# ⚡ JavaScript — Day 03: Asynchronous JavaScript Mastery
### Event Loop, Promises, async/await, Fetch API & Error Handling

---

## 📌 1. The Event Loop Architecture

JavaScript is single-threaded and non-blocking. It achieves concurrency using the **Event Loop**, **Call Stack**, **Web APIs**, **Microtask Queue**, and **Macrotask Queue**.

```
Call Stack (Synchronous Code)
       │
       ▼
   Web APIs (fetch, setTimeout, DOM Events)
       │
       ├───────────────────────────────┐
       ▼                               ▼
Microtask Queue                 Macrotask Queue
(Promises, queueMicrotask)      (setTimeout, setInterval)
   ⭐ HIGHER PRIORITY                ⭐ LOWER PRIORITY
```

### Execution Priority Example
```javascript
console.log("1 - Sync Start");

setTimeout(() => {
  console.log("4 - Macrotask (setTimeout)");
}, 0);

Promise.resolve().then(() => {
  console.log("3 - Microtask (Promise)");
});

console.log("2 - Sync End");

// Output Order:
// 1 - Sync Start
// 2 - Sync End
// 3 - Microtask (Promise)
// 4 - Macrotask (setTimeout)
```

---

## 📌 2. Promises & States

A Promise represents the eventual completion or failure of an asynchronous operation.
States: **`pending` $\rightarrow$ `fulfilled` (with value) OR `rejected` (with reason)**.

```javascript
function fetchUserData(userId) {
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      if (userId > 0) {
        resolve({ id: userId, name: "Vibhav Shukla", role: "Engineer" });
      } else {
        reject(new Error("Invalid User ID"));
      }
    }, 500);
  });
}

// Consuming with .then() / .catch()
fetchUserData(1)
  .then(data => console.log("Fetched:", data))
  .catch(err => console.error("Error:", err.message));
```

---

## 📌 3. Modern `async` / `await` Syntax

Syntactic sugar built on top of Promises that makes asynchronous code read like synchronous sequential code.

```javascript
async function loadDashboard() {
  try {
    console.log("Loading dashboard...");
    const user = await fetchUserData(1);
    console.log(`Welcome back, ${user.name}`);
    return user;
  } catch (error) {
    console.error("Failed to load dashboard:", error.message);
  } finally {
    console.log("Finished attempt.");
  }
}
```

---

## 📌 4. Fetch API & Real-World HTTP Requests

```javascript
async function getGitHubProfile(username) {
  const url = `https://api.github.com/users/${username}`;
  try {
    const response = await fetch(url);
    if (!response.ok) {
      throw new Error(`HTTP Error! Status: ${response.status}`);
    }
    const data = await response.json();
    console.log(`Public Repos: ${data.public_repos}`);
    return data;
  } catch (error) {
    console.error("Network or parsing failed:", error);
  }
}

// Parallel Requests with Promise.all()
async function fetchBatch(usernames) {
  const requests = usernames.map(u => fetch(`https://api.github.com/users/${u}`).then(r => r.json()));
  const results = await Promise.all(requests);
  return results;
}
```
