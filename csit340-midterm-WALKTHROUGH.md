# CSIT340 Midterm Practice: Step-by-Step Answer Walkthrough

> ⚠ This is the **answer guide**. Keep it outside the repo and don't push it.
> Try each step yourself first, then look here if you get stuck.

**Work folder:** `C:\Users\SYOSAK\midterm-sim` (your clone of Sirrr-)
**Terminal:** Command Prompt (Start → type `cmd`). Keep `npm run dev` running in one window, and use a second window for git.

---

## Step 0: Setup (do this once)

```
cd C:\Users\SYOSAK
git clone https://github.com/dragonkhan6674-dotcom/Sirrr-.git midterm-sim
cd midterm-sim
npm install
npm run dev
```
Open **http://localhost:5173** and leave this window running.

In a **second** Command Prompt, add your fork:
```
cd C:\Users\SYOSAK\midterm-sim
git remote add myfork https://MAIN-USERNAME@github.com/MAIN-USERNAME/CSIT340-G6-Midterms-Cenas.git
git remote -v
```
✅ You should see **origin** (Sirrr-) and **myfork** (your fork).

Open the folder in VS Code: `code .`

> **The golden rule for every step:** cut the HTML out of `App.jsx`, paste it **unchanged** into the new component, then put `<ComponentName />` where it used to be. Never change a className or any text.

---

## Step 1: Replace YOUR NAME

**File:** `src/App.jsx`, in the `<footer>` near the bottom.

Before:
```jsx
© 2026 Cloudline · Page by YOUR NAME
```
After:
```jsx
© 2026 Cloudline · Page by Cassius L. Cenas
```
✅ **Check:** the page footer shows your name.
```
git add .
git commit -m "chore: replace YOUR NAME with Cassius L. Cenas"
```

---

## Step 2: Navbar (the easiest one, no props)

**2a. Create** `src/components/Navbar.jsx` by cutting the `<nav>...</nav>` block from App.jsx:
```jsx
function Navbar() {
  return (
    <nav className="flex items-center justify-between px-20 py-6">
      <span className="text-3xl font-bold">Cloudline</span>
      <span className="text-xl">Cloud storage for growing teams</span>
    </nav>
  );
}

export default Navbar;
```
**2b. In `App.jsx`:** add the import at the top, under the `useState` import:
```jsx
import Navbar from "./components/Navbar.jsx";
```
Then put `<Navbar />` where the `<nav>` block used to be:
```jsx
    <div className="min-h-screen bg-[#EEEEEE] font-sans text-[#000000]">
      <Navbar />
```
✅ **Check:** the page looks the same and the browser console (F12) has no errors.
```
git add .
git commit -m "refactor: extract Navbar component"
```

---

## Step 3: Footer (no props)

**3a. Create** `src/components/Footer.jsx`:
```jsx
function Footer() {
  return (
    <footer className="px-20 py-8 text-center text-lg">
      © 2026 Cloudline · Page by Cassius L. Cenas
    </footer>
  );
}

export default Footer;
```
**3b. In `App.jsx`:** add `import Footer from "./components/Footer.jsx";` at the top, and replace the `<footer>...</footer>` block with:
```jsx
      <Footer />
```
✅ **Check:** the footer still shows your name.
```
git add .
git commit -m "refactor: extract Footer component"
```

---

## Step 4: Feature (your first prop: `text`)

A Feature is **one `<li>`**. The text inside it is different each time, so it becomes a prop.

**4a. Create** `src/components/Feature.jsx`:
```jsx
function Feature({ text }) {
  return <li>{text}</li>;
}

export default Feature;
```
`{ text }` takes the `text` prop out of the props object (this is called destructuring).

**4b. In `App.jsx`:** add `import Feature from "./components/Feature.jsx";` and replace **all 6** `<li>` lines:

Before:
```jsx
<li>50 GB storage</li>
<li>Email support</li>
```
After:
```jsx
<Feature text="50 GB storage" />
<Feature text="Email support" />
```
Do the same for Pro (`1 TB storage`, `Chat support`) and Business (`10 TB storage`, `24/7 phone support`).

✅ **Check:** all 6 bullet points still show.
```
git add .
git commit -m "refactor: extract Feature component"
```

---

## Step 5: PlanCard (6 props, including the trap)

Look at the 3 cards. They have the **same structure**, and only these parts change:

| Changes per card | Starter | Pro | Business |
|---|---|---|---|
| name | "Starter" | "Pro" | "Business" |
| price (**number**) | 150 | 400 | 900 |
| storage | "50 GB" | "1 TB" | "10 TB" |
| support | "Email" | "Chat" | "24/7 phone" |

PlanCard also needs **seats** and **months**, because the total is `price × seats × months`.

**5a. Create** `src/components/PlanCard.jsx`:
```jsx
import Feature from "./Feature.jsx";

function PlanCard({ name, price, storage, support, seats, months }) {
  return (
    <div className="rounded-xl border-2 border-[#000000] bg-[#FFFFFF] p-6">
      <h2 className="text-2xl font-bold">{name}</h2>
      <p className="mt-2 text-xl">₱{price} per seat / month</p>
      <p className="mt-1 text-xl font-bold">Total: ₱{price * seats * months}</p>
      <ul className="mt-4 list-disc pl-6">
        <Feature text={`${storage} storage`} />
        <Feature text={`${support} support`} />
      </ul>
    </div>
  );
}

export default PlanCard;
```
🔑 **The trap:** the prop is `storage="50 GB"`, but the bullet must still say **"50 GB storage"**. The template string `` `${storage} storage` `` adds the word back on.
🔑 Feature is now used **inside PlanCard**, so the import moves here, and the path is `./Feature.jsx` (same folder).

**5b. In `App.jsx`:** add `import PlanCard from "./components/PlanCard.jsx";`, **delete** the Feature import (App doesn't use Feature anymore), and replace each card `<div>...</div>`:
```jsx
      <main className="grid grid-cols-3 gap-8 px-20 py-12">
        <PlanCard
          name="Starter"
          price={150}
          storage="50 GB"
          support="Email"
          seats={seats}
          months={months}
        />
        <PlanCard
          name="Pro"
          price={400}
          storage="1 TB"
          support="Chat"
          seats={seats}
          months={months}
        />
        <PlanCard
          name="Business"
          price={900}
          storage="10 TB"
          support="24/7 phone"
          seats={seats}
          months={months}
        />
      </main>
```
🔑 **Rule 2:** numbers use braces, so `price={150}` is right and `price="150"` is wrong (that would be a string).

✅ **Check:** totals are ₱750 / ₱2000 / ₱4500. Click **Add seat** and they should change to ₱900 / ₱2400 / ₱5400.
```
git add .
git commit -m "refactor: extract PlanCard component"
```

---

## Step 6: Summary (the `<header>`, props seats, months, customer)

**6a. Create** `src/components/Summary.jsx`:
```jsx
function Summary({ seats, months, customer }) {
  return (
    <header className="mb-8">
      <h1 className="text-5xl font-bold">Quote for {customer}</h1>
      <p className="mt-2 text-2xl">
        {seats} seats for {months} months
      </p>
    </header>
  );
}

export default Summary;
```
"Acme Corp" became the `customer` prop, which is a **string**.

**6b. In `App.jsx`:** add `import Summary from "./components/Summary.jsx";` and replace the `<header>...</header>` block with:
```jsx
        <Summary seats={seats} months={months} customer="Acme Corp" />
```
✅ **Check:** the header still says "Quote for Acme Corp / 5 seats for 1 months" and updates when you click the buttons.
```
git add .
git commit -m "refactor: extract Summary component"
```

---

## Step 7: PlanPicker (the hardest one: Fragment and moving state)

PlanPicker = **the black `<div>` + the `<main>` after it**. Two things make this step hard:

1. **They are 2 sibling elements.** A component can only return **one** thing, and you're **not allowed to add a wrapper `<div>`** (rule: no new tags). The fix is a **Fragment** `<>...</>`, which groups them without adding any HTML.
2. **The state must move.** App may only render Navbar, PlanPicker, Waitlist and Footer (rule 3), but the buttons and cards need `seats` and `months`. So `useState` **moves from App into PlanPicker**, and PlanPicker receives **no props**.

**7a. Create** `src/components/PlanPicker.jsx`:
```jsx
import { useState } from "react";
import Summary from "./Summary.jsx";
import PlanCard from "./PlanCard.jsx";

function PlanPicker() {
  const [seats, setSeats] = useState(5);
  const [months, setMonths] = useState(1);

  return (
    <>
      <div className="bg-[#000000] px-20 py-12 text-[#EEEEEE]">
        <Summary seats={seats} months={months} customer="Acme Corp" />
        <button
          className="mr-4 cursor-pointer rounded-lg bg-[#C82957] px-8 py-3 text-xl font-bold"
          onClick={() => setSeats(seats + 1)}
        >
          Add seat
        </button>
        <button
          className="mr-4 cursor-pointer rounded-lg border-2 border-[#EEEEEE] px-8 py-3 text-xl font-bold"
          onClick={() => setSeats(seats > 1 ? seats - 1 : 1)}
        >
          Remove seat
        </button>
        <button
          className="cursor-pointer rounded-lg border-2 border-[#EEEEEE] px-8 py-3 text-xl font-bold"
          onClick={() => setMonths(months === 1 ? 12 : 1)}
        >
          Switch billing
        </button>
      </div>

      <main className="grid grid-cols-3 gap-8 px-20 py-12">
        <PlanCard name="Starter" price={150} storage="50 GB" support="Email" seats={seats} months={months} />
        <PlanCard name="Pro" price={400} storage="1 TB" support="Chat" seats={seats} months={months} />
        <PlanCard name="Business" price={900} storage="10 TB" support="24/7 phone" seats={seats} months={months} />
      </main>
    </>
  );
}

export default PlanPicker;
```

**7b. Replace ALL of `App.jsx`** with this. The `useState` lines and the Summary/PlanCard imports are gone from App:
```jsx
import Navbar from "./components/Navbar.jsx";
import PlanPicker from "./components/PlanPicker.jsx";
import Footer from "./components/Footer.jsx";

function App() {
  return (
    <div className="min-h-screen bg-[#EEEEEE] font-sans text-[#000000]">
      <Navbar />
      <PlanPicker />
      <Footer />
    </div>
  );
}

export default App;
```
✅ **Check:** the page looks exactly the same, and all 3 buttons still change the header and the totals.
```
git add .
git commit -m "refactor: extract PlanPicker component"
```

---

## Step 8: Waitlist (Part B: converting jsx() calls to JSX)

**How to translate:**

| jsx() call | JSX |
|---|---|
| `jsx("h3", { className: "x", children: "Hi" })` | `<h3 className="x">Hi</h3>` |
| `children: [teams, " teams joined the waitlist."]` | `{teams} teams joined the waitlist.` |
| `onClick: handleJoin` | `onClick={handleJoin}` (no `()`: you pass the function, not call it) |
| `jsxs(...)` | same as `jsx`; it just means there are several children |

**8a. Create** `src/components/Waitlist.jsx`:
```jsx
import { useState } from "react";

function Waitlist() {
  const [teams, setTeams] = useState(0);

  function handleJoin() {
    setTeams(teams + 1);
  }

  function handleClear() {
    setTeams(0);
  }

  return (
    <div className="mx-20 mt-8 flex items-center justify-between rounded-xl border-2 border-[#000000] bg-[#EEEEEE] p-6">
      <div>
        <h3 className="text-2xl font-bold">Cloudline Enterprise is coming soon.</h3>
        <p className="mt-1 text-xl">{teams} teams joined the waitlist.</p>
      </div>
      <div className="flex gap-4">
        <button
          className="cursor-pointer rounded-lg bg-[#C82957] px-8 py-3 text-xl font-bold text-[#EEEEEE]"
          onClick={handleJoin}
        >
          Join
        </button>
        <button
          className="cursor-pointer rounded-lg border-2 border-[#000000] px-8 py-3 text-xl font-bold"
          onClick={handleClear}
        >
          Clear
        </button>
      </div>
    </div>
  );
}

export default Waitlist;
```
The `jsx()` code uses `handleJoin` and `handleClear`, so you must **create those two functions** with exactly those names.

**8b. In `App.jsx`:** add the import, and put `<Waitlist />` **between PlanPicker and Footer** (rule 3 order):
```jsx
import Waitlist from "./components/Waitlist.jsx";
...
      <Navbar />
      <PlanPicker />
      <Waitlist />
      <Footer />
```
✅ **Check:** the box shows "0 teams joined the waitlist.", Join adds 1, and Clear resets it to 0.
```
git add .
git commit -m "feat: add Waitlist component"
```

**Push the main part:**
```
git push myfork main
```
(Use the **main** account when the login window asks.)

---

## Step 9 (BONUS): Controls

"Move the three buttons into Controls. **The state stays where it is.**" That means `useState` stays in **PlanPicker**, and PlanPicker passes the **click functions** down as props.

The 3 buttons are siblings, so Controls also needs a **Fragment** (no wrapper div allowed).

**9a. Create** `src/components/Controls.jsx`:
```jsx
function Controls({ onAddSeat, onRemoveSeat, onSwitchBilling }) {
  return (
    <>
      <button
        className="mr-4 cursor-pointer rounded-lg bg-[#C82957] px-8 py-3 text-xl font-bold"
        onClick={onAddSeat}
      >
        Add seat
      </button>
      <button
        className="mr-4 cursor-pointer rounded-lg border-2 border-[#EEEEEE] px-8 py-3 text-xl font-bold"
        onClick={onRemoveSeat}
      >
        Remove seat
      </button>
      <button
        className="cursor-pointer rounded-lg border-2 border-[#EEEEEE] px-8 py-3 text-xl font-bold"
        onClick={onSwitchBilling}
      >
        Switch billing
      </button>
    </>
  );
}

export default Controls;
```
**9b. In `PlanPicker.jsx`:** add `import Controls from "./Controls.jsx";` and replace the 3 `<button>` blocks with:
```jsx
        <Controls
          onAddSeat={() => setSeats(seats + 1)}
          onRemoveSeat={() => setSeats(seats > 1 ? seats - 1 : 1)}
          onSwitchBilling={() => setMonths(months === 1 ? 12 : 1)}
        />
```
✅ **Check:** the buttons still work.
```
git add .
git commit -m "refactor: extract Controls component"
```

---

## Step 10 (BONUS): Render PlanCards with map()

**10a. In `PlanPicker.jsx`:** add an array **above** `function PlanPicker()`:
```jsx
const plans = [
  { name: "Starter", price: 150, storage: "50 GB", support: "Email" },
  { name: "Pro", price: 400, storage: "1 TB", support: "Chat" },
  { name: "Business", price: 900, storage: "10 TB", support: "24/7 phone" },
];
```
**10b.** Replace the 3 `<PlanCard ... />` lines inside `<main>` with:
```jsx
        {plans.map((plan) => (
          <PlanCard
            key={plan.name}
            name={plan.name}
            price={plan.price}
            storage={plan.storage}
            support={plan.support}
            seats={seats}
            months={months}
          />
        ))}
```
🔑 `key` is required whenever you use map. It must be unique, and `plan.name` works. `key` is not a prop that PlanCard receives, so it doesn't break rule 1.
🔑 Use `(plan) => ( ... )` with **round brackets** so it returns the JSX. With `{ }` you'd need to write `return`.

✅ **Check:** there are still 3 cards with the same totals.
```
git add .
git commit -m "refactor: render PlanCards with map"
git push myfork main
```

---

## Final checklist
- [ ] `src/components/` has: Navbar, PlanPicker, Summary, PlanCard, Feature, Footer, Waitlist (+ Controls).
- [ ] Every file ends with `export default ComponentName;`
- [ ] `App.jsx` = outer `<div>` + `<Navbar /> <PlanPicker /> <Waitlist /> <Footer />`, and nothing else.
- [ ] price / seats / months are passed with `{ }` (numbers).
- [ ] No className or text was changed, and no extra `<div>` was added (Fragments only).
- [ ] `git log --oneline` shows each commit message **exactly** as given.
- [ ] Your fork on GitHub shows the commits.

## Common errors and fixes
| Error / symptom | Cause | Fix |
|---|---|---|
| `Failed to resolve import "./components/Navbar"` | Wrong path or file name | The file must be `src/components/Navbar.jsx` and the import `./components/Navbar.jsx` (from App) or `./Navbar.jsx` (from another component) |
| `X is not defined` | You forgot the import | Add `import X from "..."` at the top of the file that uses it |
| `Adjacent JSX elements must be wrapped` | You returned 2 elements | Wrap them in `<>...</>` |
| `seats is not defined` (inside PlanCard) | You forgot to pass/receive the prop | Pass it: `seats={seats}`, and receive it: `{ ..., seats }` |
| Totals show `NaN` | A prop is missing or misspelled | Check that every prop name matches exactly (`months`, not `month`) |
| Page looks fine but you lose points on rule 2 | price passed as a string (`price="150"`); `*` still turns it into a number, so the total looks correct and the mistake is easy to miss | Use `price={150}`, not `price="150"` |
| Buttons do nothing | `useState` is still in App, or a setter isn't passed down | The state lives in PlanPicker; Controls gets the functions as props |
| Warning: "Each child in a list should have a unique key" | map without `key` | Add `key={plan.name}` |
| Committed with a typo in the message | | Make the fix and commit it as `fix: <what you fixed>` |

**Compare with the official solution:**
in `C:\Users\SYOSAK\csit340-midterm-practice`, run `git checkout solution` and look at `src/`. Run `git checkout main` to go back.
