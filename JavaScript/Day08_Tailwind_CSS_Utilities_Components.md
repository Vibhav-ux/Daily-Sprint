# ⚡ JavaScript — Day 08: Tailwind CSS — Utility-First Styling
### Setup · Core Utilities · Responsive Design · Dark Mode · Animations · Component Patterns

---

> 📌 **Setup with Vite + React:**
> ```bash
> npm create vite@latest my-app -- --template react
> cd my-app
> npm install
> npm install -D tailwindcss @tailwindcss/vite
> ```
> Add to `vite.config.js`:
> ```js
> import tailwindcss from "@tailwindcss/vite";
> export default { plugins: [tailwindcss()] };
> ```
> Add to `index.css`: `@import "tailwindcss";`

---

## PART 1 — The Utility-First Mental Model

**Traditional CSS approach:**
```css
.card { display: flex; flex-direction: column; padding: 1rem; border-radius: 0.5rem; }
```

**Tailwind approach:**
```jsx
<div className="flex flex-col p-4 rounded-lg">...</div>
```

You compose small utility classes directly in your HTML/JSX. No naming conventions to think about, no context-switching between files.

---

## PART 2 — Core Utility Categories

```jsx
// ── Layout ──
<div className="flex items-center justify-between gap-4">  {/* flexbox */}
<div className="grid grid-cols-3 gap-6">                   {/* CSS grid */}
<div className="container mx-auto px-4">                   {/* centered container */}

// ── Spacing (p = padding, m = margin) ──
// Scale: 0(0), 1(4px), 2(8px), 4(16px), 6(24px), 8(32px), 12(48px), 16(64px)
<div className="p-4 px-6 py-3 mt-2 mb-8 mx-auto">

// ── Sizing ──
<div className="w-full max-w-2xl h-screen min-h-0">
<div className="w-1/2 w-[340px]">  {/* arbitrary value with brackets */}

// ── Typography ──
<h1 className="text-4xl font-bold tracking-tight leading-tight">
<p  className="text-base text-gray-400 font-medium">

// ── Colors ──
<div className="bg-slate-900 text-white border border-slate-700">
<button className="bg-indigo-600 hover:bg-indigo-700 text-white">

// ── Border & Radius ──
<div className="border border-slate-700 rounded-2xl">
<img  className="rounded-full border-2 border-indigo-500">

// ── Shadow ──
<div className="shadow-lg shadow-indigo-500/20">
```

---

## PART 3 — Responsive Design (Mobile-First)

Tailwind uses breakpoint prefixes. No prefix = mobile. `sm:` = ≥640px. `md:` = ≥768px. `lg:` = ≥1024px.

```jsx
function CourseCard({ title, students, level }) {
    return (
        // Mobile: full width, stacked. Desktop: side by side
        <div className="
            flex flex-col          {/* mobile: vertical stack */}
            md:flex-row            {/* tablet+: horizontal */}
            gap-4 p-4
            md:p-6
            bg-slate-800
            rounded-2xl
        ">
            <div className="
                w-full md:w-48     {/* full width mobile, fixed width on md+ */}
                text-center
            ">
                <span className="text-5xl md:text-7xl">📚</span>
            </div>

            <div className="flex-1">
                <h3 className="text-lg md:text-2xl font-bold text-white">{title}</h3>
                <p  className="text-sm md:text-base text-slate-400">{students} students</p>
                <span className="
                    inline-block mt-2 px-3 py-1
                    text-xs font-semibold rounded-full
                    bg-indigo-500/20 text-indigo-300
                ">{level}</span>
            </div>
        </div>
    );
}
```

---

## PART 4 — Dark Mode

```jsx
// tailwind.config.js — enable class-based dark mode
// No separate config file needed with v4 — it's automatic!

// Usage: prefix utilities with 'dark:'
<div className="bg-white dark:bg-slate-900 text-gray-900 dark:text-white">
    <h1 className="text-indigo-600 dark:text-indigo-400">Daily Sprint</h1>
</div>

// Toggle dark mode with JS — add/remove 'dark' class on <html>
function DarkModeToggle() {
    const [dark, setDark] = useState(
        () => window.matchMedia("(prefers-color-scheme: dark)").matches
    );

    useEffect(() => {
        document.documentElement.classList.toggle("dark", dark);
    }, [dark]);

    return (
        <button
            onClick={() => setDark(d => !d)}
            className="p-2 rounded-lg bg-slate-200 dark:bg-slate-700 transition-colors"
        >
            {dark ? "☀️" : "🌙"}
        </button>
    );
}
```

---

## PART 5 — Animations & Transitions

```jsx
// ── Built-in transitions ──
<button className="
    px-6 py-3 rounded-xl
    bg-indigo-600
    hover:bg-indigo-700
    hover:-translate-y-1
    hover:shadow-lg hover:shadow-indigo-500/30
    active:scale-95
    transition-all duration-200 ease-out
    text-white font-semibold
">
    Start Learning
</button>

// ── Animated spinner ──
<div className="animate-spin h-8 w-8 rounded-full border-4 border-slate-700 border-t-indigo-500" />

// ── Pulse skeleton loader ──
<div className="animate-pulse flex gap-4">
    <div className="rounded-full bg-slate-700 h-12 w-12" />
    <div className="flex-1 space-y-2 py-1">
        <div className="h-4 bg-slate-700 rounded w-3/4" />
        <div className="h-4 bg-slate-700 rounded w-1/2" />
    </div>
</div>
```

---

## PART 6 — Complete Component: Dashboard Card

```jsx
function StatCard({ label, value, change, icon, color }) {
    const isPositive = change >= 0;
    return (
        <div className="
            relative overflow-hidden
            bg-slate-900 border border-slate-800
            rounded-2xl p-6
            hover:border-slate-600
            transition-all duration-300
            group
        ">
            {/* Glowing background blob */}
            <div className={`
                absolute -top-8 -right-8 w-24 h-24 rounded-full
                opacity-20 group-hover:opacity-30
                transition-opacity duration-300
                ${color}
            `} />

            <div className="relative flex items-start justify-between">
                <div>
                    <p className="text-sm text-slate-400 font-medium">{label}</p>
                    <p className="text-3xl font-bold text-white mt-1">{value}</p>
                    <div className={`flex items-center gap-1 mt-2 text-sm font-medium ${isPositive ? "text-emerald-400" : "text-red-400"}`}>
                        <span>{isPositive ? "↑" : "↓"}</span>
                        <span>{Math.abs(change)}% vs last week</span>
                    </div>
                </div>
                <div className={`p-3 rounded-xl ${color} bg-opacity-20`}>
                    <span className="text-2xl">{icon}</span>
                </div>
            </div>
        </div>
    );
}

// Usage
<div className="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4">
    <StatCard label="Study Streak"    value="14 days" change={40}  icon="🔥" color="bg-orange-500" />
    <StatCard label="Problems Solved" value="48"       change={12}  icon="✅" color="bg-indigo-500" />
    <StatCard label="Average Score"   value="91.2"     change={3.5} icon="📈" color="bg-emerald-500" />
    <StatCard label="Days Remaining"  value="154"      change={-2}  icon="📅" color="bg-sky-500" />
</div>
```

---

## 💡 Day 08 Challenge

Build a fully responsive **Learning Progress Card** in React + Tailwind that shows:
- Student name, avatar placeholder, and current streak
- 3 progress bars for Python, DSA, and JavaScript (use Tailwind `w-[65%]` style widths)
- Dark mode toggle button
- Hover animation on the card
