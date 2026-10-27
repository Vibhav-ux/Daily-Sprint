# ⚡ JavaScript — Day 06: React Basics
### Components · JSX · Props · useState · useEffect · Conditional Rendering

---

> 📌 **Setup:**
> ```bash
> npm create vite@latest my-react-app -- --template react
> cd my-react-app && npm install && npm run dev
> ```

---

## PART 1 — The Component Mental Model

In React, your UI is a **tree of components** — reusable, self-contained pieces of UI.
Think of components like LEGO blocks: each block does one thing, and you combine them to build the full UI.

```
<App>
  <Header />
  <Main>
    <StudentCard name="Vibhav" score={92} />
    <StudentCard name="Alice"  score={85} />
  </Main>
  <Footer />
</App>
```

---

## PART 2 — JSX: HTML Inside JavaScript

JSX compiles to `React.createElement()` calls. It looks like HTML but has a few key differences:

```jsx
// ── JSX rules ──
// 1. className instead of class (class is a JS reserved word)
// 2. htmlFor instead of for (label elements)
// 3. All tags must be self-closing if no children: <img /> <br /> <input />
// 4. Return one root element (or use a Fragment <>...</>)
// 5. Expressions go in curly braces {}

function WelcomeBanner({ name, streak }) {
    const isOnFire = streak >= 7;

    return (
        <div className="banner">
            <h1>Welcome, {name}! 🚀</h1>
            <p>Current streak: <strong>{streak}</strong> days</p>
            {isOnFire && <span className="badge">🔥 On Fire!</span>}
        </div>
    );
}
```

---

## PART 3 — Props: Passing Data Down

Props are read-only parameters passed from parent to child. They make components reusable.

```jsx
// ── Child Component ──
function StudentCard({ name, score, track = "General" }) {
    // Destructured props with a default value for track
    const gradeColor = score >= 90 ? "#4ade80" : score >= 75 ? "#facc15" : "#f87171";

    return (
        <div className="card" style={{ borderLeft: `4px solid ${gradeColor}` }}>
            <h3>{name}</h3>
            <p>Track: {track}</p>
            <p>Score: <strong style={{ color: gradeColor }}>{score}</strong></p>
        </div>
    );
}

// ── Parent Component (using StudentCard) ──
function StudentList() {
    const students = [
        { id: 1, name: "Vibhav", score: 92, track: "Python" },
        { id: 2, name: "Alice",  score: 85, track: "JavaScript" },
        { id: 3, name: "Bob",    score: 78, track: "DSA" },
    ];

    return (
        <div className="student-list">
            {students.map(student => (
                // Always provide a unique 'key' prop when rendering lists!
                <StudentCard
                    key={student.id}
                    name={student.name}
                    score={student.score}
                    track={student.track}
                />
            ))}
        </div>
    );
}
```

---

## PART 4 — `useState`: Reactive Local State

`useState` is a Hook that lets a component remember values between renders.
When state changes → React re-renders the component → UI updates automatically.

```jsx
import { useState } from "react";

function Counter() {
    // [currentValue, setterFunction] = useState(initialValue)
    const [count, setCount] = useState(0);
    const [label, setLabel] = useState("Sprint Counter");

    return (
        <div>
            <h2>{label}: {count}</h2>
            <button onClick={() => setCount(count + 1)}>+1</button>
            <button onClick={() => setCount(count - 1)}>-1</button>
            <button onClick={() => setCount(0)}>Reset</button>
        </div>
    );
}

// ── Object state — use spread to update immutably! ──
function ProfileEditor() {
    const [user, setUser] = useState({ name: "Vibhav", streak: 0, active: true });

    const incrementStreak = () => {
        // NEVER mutate state directly: user.streak++ ❌
        // ALWAYS create a new object:
        setUser(prev => ({ ...prev, streak: prev.streak + 1 }));
    };

    return (
        <div>
            <p>{user.name} — Streak: {user.streak}</p>
            <button onClick={incrementStreak}>Log Today ✅</button>
        </div>
    );
}
```

---

## PART 5 — `useEffect`: Side Effects & Lifecycle

`useEffect` runs **after** the component renders. Use it for:
- Fetching data from an API
- Setting up event listeners or timers
- Syncing with localStorage

```jsx
import { useState, useEffect } from "react";

function GitHubProfile({ username }) {
    const [profile, setProfile] = useState(null);
    const [loading, setLoading] = useState(true);
    const [error, setError] = useState(null);

    // Runs after render when 'username' changes
    useEffect(() => {
        setLoading(true);
        setError(null);

        fetch(`https://api.github.com/users/${username}`)
            .then(res => {
                if (!res.ok) throw new Error(`User "${username}" not found`);
                return res.json();
            })
            .then(data => setProfile(data))
            .catch(err => setError(err.message))
            .finally(() => setLoading(false));

        // Cleanup function (runs before next effect or unmount)
        return () => {
            // Cancel any pending requests (e.g., AbortController)
        };
    }, [username]);  // ← dependency array: re-run when username changes

    if (loading) return <p>Loading...</p>;
    if (error)   return <p>Error: {error}</p>;

    return (
        <div>
            <img src={profile.avatar_url} alt={profile.name} width={80} />
            <h2>{profile.name}</h2>
            <p>{profile.bio}</p>
            <p>📦 Public Repos: {profile.public_repos}</p>
            <p>⭐ Followers: {profile.followers}</p>
        </div>
    );
}
```

### `useEffect` Dependency Array Summary

| Dependency Array | Runs when? |
|---|---|
| `useEffect(() => {...})` | Every render (no array — usually a bug!) |
| `useEffect(() => {...}, [])` | Only once on mount (like `componentDidMount`) |
| `useEffect(() => {...}, [val])` | On mount AND whenever `val` changes |

---

## 💡 Day 06 Challenge: Build a Live Search Component

```jsx
import { useState, useEffect } from "react";

function LiveSearch() {
    const [query, setQuery] = useState("");
    const [results, setResults] = useState([]);

    const items = ["Python", "JavaScript", "TypeScript", "React", "FastAPI", "PostgreSQL", "NumPy", "Pandas"];

    useEffect(() => {
        if (!query.trim()) {
            setResults([]);
            return;
        }
        const filtered = items.filter(item =>
            item.toLowerCase().includes(query.toLowerCase())
        );
        setResults(filtered);
    }, [query]);

    return (
        <div>
            <input
                type="text"
                value={query}
                onChange={e => setQuery(e.target.value)}
                placeholder="Search technologies..."
            />
            <ul>
                {results.map(r => <li key={r}>{r}</li>)}
                {results.length === 0 && query && <li>No results found.</li>}
            </ul>
        </div>
    );
}
```
