# ⚡ JavaScript — Day 07: React Advanced Patterns
### React Router · Context API · Custom Hooks · Form Handling · Performance

---

> 📌 **Setup:**
> ```bash
> npm install react-router-dom
> ```

---

## PART 1 — React Router v6: Client-Side Navigation

React Router lets you build SPAs (Single Page Applications) where navigation happens without a full page reload.

```jsx
// main.jsx / index.jsx — wrap your app
import { BrowserRouter } from "react-router-dom";
import App from "./App";

ReactDOM.createRoot(document.getElementById("root")).render(
    <BrowserRouter>
        <App />
    </BrowserRouter>
);

// App.jsx — define routes
import { Routes, Route, NavLink, Navigate } from "react-router-dom";
import Home from "./pages/Home";
import Dashboard from "./pages/Dashboard";
import Profile from "./pages/Profile";
import NotFound from "./pages/NotFound";

function App() {
    const isLoggedIn = true; // replace with real auth state

    return (
        <>
            <nav>
                <NavLink to="/"          className={({ isActive }) => isActive ? "active" : ""}>Home</NavLink>
                <NavLink to="/dashboard" className={({ isActive }) => isActive ? "active" : ""}>Dashboard</NavLink>
                <NavLink to="/profile"   className={({ isActive }) => isActive ? "active" : ""}>Profile</NavLink>
            </nav>

            <Routes>
                <Route path="/"           element={<Home />} />
                <Route path="/dashboard"  element={isLoggedIn ? <Dashboard /> : <Navigate to="/" />} />
                <Route path="/profile/:username" element={<Profile />} />  {/* URL params */}
                <Route path="*"           element={<NotFound />} />
            </Routes>
        </>
    );
}
```

```jsx
// pages/Profile.jsx — reading URL params
import { useParams, useNavigate } from "react-router-dom";

function Profile() {
    const { username } = useParams();      // reads :username from URL
    const navigate     = useNavigate();    // programmatic navigation

    return (
        <div>
            <h1>Profile: @{username}</h1>
            <button onClick={() => navigate("/dashboard")}>Go to Dashboard</button>
            <button onClick={() => navigate(-1)}>← Back</button>
        </div>
    );
}
```

---

## PART 2 — Context API: Global State Without Prop Drilling

**Prop drilling** = passing data through many intermediate components that don't need it.
Context API lets any component in the tree read data directly.

```jsx
// contexts/ThemeContext.jsx
import { createContext, useContext, useState } from "react";

const ThemeContext = createContext(null);

// Provider wraps the app and holds the state
export function ThemeProvider({ children }) {
    const [theme, setTheme] = useState("dark");
    const toggleTheme = () => setTheme(t => t === "dark" ? "light" : "dark");

    return (
        <ThemeContext.Provider value={{ theme, toggleTheme }}>
            {children}
        </ThemeContext.Provider>
    );
}

// Custom hook for clean usage
export function useTheme() {
    const ctx = useContext(ThemeContext);
    if (!ctx) throw new Error("useTheme must be used inside ThemeProvider");
    return ctx;
}

// App.jsx
import { ThemeProvider } from "./contexts/ThemeContext";

function App() {
    return (
        <ThemeProvider>
            <Header />     {/* can use useTheme() here */}
            <Main />       {/* can use useTheme() here — no prop drilling! */}
            <Footer />
        </ThemeProvider>
    );
}

// Any deeply nested component
function ToggleButton() {
    const { theme, toggleTheme } = useTheme();
    return (
        <button onClick={toggleTheme}>
            Mode: {theme === "dark" ? "🌙 Dark" : "☀️ Light"}
        </button>
    );
}
```

---

## PART 3 — Custom Hooks: Reusable Logic

Extract reusable stateful logic into custom hooks. A custom hook is just a function whose name starts with `use`.

```jsx
// hooks/useFetch.js
import { useState, useEffect } from "react";

export function useFetch(url) {
    const [data,    setData]    = useState(null);
    const [loading, setLoading] = useState(true);
    const [error,   setError]   = useState(null);

    useEffect(() => {
        const controller = new AbortController();
        setLoading(true);

        fetch(url, { signal: controller.signal })
            .then(res => { if (!res.ok) throw new Error(`HTTP ${res.status}`); return res.json(); })
            .then(setData)
            .catch(err => { if (err.name !== "AbortError") setError(err.message); })
            .finally(() => setLoading(false));

        return () => controller.abort();  // cleanup on unmount or url change
    }, [url]);

    return { data, loading, error };
}

// hooks/useLocalStorage.js
import { useState } from "react";

export function useLocalStorage(key, initialValue) {
    const [value, setValue] = useState(() => {
        try {
            const stored = localStorage.getItem(key);
            return stored ? JSON.parse(stored) : initialValue;
        } catch { return initialValue; }
    });

    const setStoredValue = (newValue) => {
        setValue(newValue);
        localStorage.setItem(key, JSON.stringify(newValue));
    };

    return [value, setStoredValue];
}

// Usage in a component
import { useFetch } from "../hooks/useFetch";
import { useLocalStorage } from "../hooks/useLocalStorage";

function RepoList({ username }) {
    const { data, loading, error } = useFetch(`https://api.github.com/users/${username}/repos`);
    const [savedRepos, setSavedRepos] = useLocalStorage("saved-repos", []);

    if (loading) return <div>Loading repos...</div>;
    if (error)   return <div>Error: {error}</div>;

    return (
        <ul>
            {data?.map(repo => (
                <li key={repo.id}>
                    <strong>{repo.name}</strong> — ⭐ {repo.stargazers_count}
                </li>
            ))}
        </ul>
    );
}
```

---

## PART 4 — Controlled Form Handling

```jsx
import { useState } from "react";

function RegistrationForm() {
    const [formData, setFormData] = useState({
        name: "", email: "", track: "Python", agreeToTerms: false
    });
    const [errors, setErrors] = useState({});

    const handleChange = (e) => {
        const { name, value, type, checked } = e.target;
        setFormData(prev => ({
            ...prev,
            [name]: type === "checkbox" ? checked : value   // computed property name
        }));
    };

    const validate = () => {
        const errs = {};
        if (!formData.name.trim())       errs.name  = "Name is required";
        if (!/\S+@\S+\.\S+/.test(formData.email)) errs.email = "Valid email required";
        if (!formData.agreeToTerms)      errs.terms = "You must accept terms";
        return errs;
    };

    const handleSubmit = (e) => {
        e.preventDefault();
        const errs = validate();
        if (Object.keys(errs).length > 0) { setErrors(errs); return; }
        console.log("Submitting:", formData);
    };

    return (
        <form onSubmit={handleSubmit}>
            <input name="name"  value={formData.name}  onChange={handleChange} placeholder="Full Name" />
            {errors.name  && <span className="error">{errors.name}</span>}

            <input name="email" value={formData.email} onChange={handleChange} placeholder="Email" />
            {errors.email && <span className="error">{errors.email}</span>}

            <select name="track" value={formData.track} onChange={handleChange}>
                <option>Python</option>
                <option>JavaScript</option>
                <option>DSA</option>
            </select>

            <label>
                <input type="checkbox" name="agreeToTerms" checked={formData.agreeToTerms} onChange={handleChange} />
                I agree to the terms
            </label>
            {errors.terms && <span className="error">{errors.terms}</span>}

            <button type="submit">Register</button>
        </form>
    );
}
```
