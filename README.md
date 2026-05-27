# Code in React 19

A hands-on React 19 project exploring **controlled vs uncontrolled forms** and **state vs refs**.

## Tech Stack

- **React 19** — latest stable version
- **Vite** — fast dev server and build tool
- **TailwindCSS 4.x** — utility-first styling

## Project Structure

```text
src/
├── App.jsx                                  # Root component
├── main.jsx                                 # Entry point
│
├── state-ref/
│   └── components/
│       ├── Counter.jsx                      # useState counter
│       ├── CounterWithRef.jsx               # useRef vs useState comparison
│       └── AutoFocusInput.jsx               # useRef for DOM access
│
├── controlled/
│   └── ControlledFeedbackForm.jsx           # Pure controlled form
│
├── uncontrolled/
│   ├── UncontrolledFeedbackForm.jsx          # Uncontrolled with useRef
│   └── UncontrolledFormNoRef.jsx             # Uncontrolled with FormData API
│
└── messy-way/
    └── FeedbackForm.jsx                      # Mixed controlled/uncontrolled (anti-pattern)
```

---

## Form Design: Controlled vs Uncontrolled

### Controlled Components

The component **owns the form state** via `useState`. Every keystroke updates state, and the input's `value` is driven by that state.

```jsx
const [form, setForm] = useState({ name: '', email: '', message: '' });

<input
  name="name"
  value={form.name}            // value comes from state
  onChange={(e) => setForm({ ...form, name: e.target.value })}
/>
```

**When to use:**

- Real-time validation on every keystroke
- Conditional rendering based on input values
- Form values needed in sibling components
- Disabling/enabling submit button dynamically

**Trade-offs:** Re-renders on every keystroke, more boilerplate for many fields

> See: [ControlledFeedbackForm.jsx](src/controlled/ControlledFeedbackForm.jsx)

---

### Uncontrolled Components (with useRef)

The **DOM owns the state**. Values are read from refs only when needed (e.g., on submit). No `value` or `onChange` props.

```jsx
const nameRef = useRef();

<input ref={nameRef} placeholder="Name" />

// Read value on submit
const handleSubmit = (e) => {
  e.preventDefault();
  const name = nameRef.current.value;
};
```

**When to use:** Simple forms without real-time validation, file inputs, integrating non-React libraries

**Trade-offs:** No real-time validation or conditional UI, imperative DOM access

> See: [UncontrolledFeedbackForm.jsx](src/uncontrolled/UncontrolledFeedbackForm.jsx)

---

### Uncontrolled with FormData API (No Refs)

Uses the browser's native `FormData` API — no `useRef` or `useState` at all. The simplest approach for submit-only forms.

```jsx
const handleSubmit = (e) => {
  e.preventDefault();
  const formData = new FormData(e.target);
  const data = Object.fromEntries(formData.entries());
  // data = { username: "...", email: "..." }
};

<input name="username" placeholder="Username" />
```

**When to use:** Forms that only need values on submit, quick prototypes

**Trade-offs:** No access to values before submit, no validation until submit time

> See: [UncontrolledFormNoRef.jsx](src/uncontrolled/UncontrolledFormNoRef.jsx)

---

### The Messy Way (Anti-Pattern)

Mixing controlled and uncontrolled inputs in the same form. Some fields use `useState`, others use `useRef`. This creates inconsistency and makes the form harder to reason about.

> See: [FeedbackForm.jsx](src/messy-way/FeedbackForm.jsx) — for learning purposes only.

### Quick Comparison

| Aspect | Controlled | Uncontrolled (useRef) | Uncontrolled (FormData) |
| --- | --- | --- | --- |
| State owner | React (`useState`) | DOM (via `useRef`) | DOM (native API) |
| Real-time validation | Yes | No | No |
| Re-renders on input | Yes | No | No |
| Value on submit | From state | From `ref.current.value` | From `FormData` |
| Boilerplate | High | Medium | Low |
| React recommended | Yes | For specific cases | For simple forms |

---

## State & Ref Fundamentals

The `state-ref/` folder covers foundational React concepts:

- **Counter.jsx** — `useState` for reactive UI updates
- **CounterWithRef.jsx** — `useRef` values persist across renders without triggering re-renders
- **AutoFocusInput.jsx** — `useRef` to access DOM nodes directly (e.g., `focus()`)

---

## Run it Locally

```bash
pnpm install
pnpm dev
```

The app will be available on `http://localhost:5173` by default.

## Support

Liked it? You can show your support with a STAR(⭐).
