# RFC: Enforcing TypeScript Strictness

## 📍 Current State of the Codebase

Over the last two years, our codebase has seen a **dramatic increase in TypeScript (TSC) errors**, jumping from **~1,500 to over 3,100**. This is more than a signal — it's an alarm bell. These errors pose serious threats: runtime crashes, inconsistent behavior, and reduced developer confidence.

### 📈 TSC Error Growth

Between **April 2023 and April 2025**, TSC error count rose from **1,542 to 3,154**. This isn't just technical debt — it’s growing technical liability.

### 📌 Current TSC Error Breakdown

| Error Code | Count | Severity |
|------------|-------|----------|
| `TS2322`   | 723   | 🔴 Type 'x' is not assignable to type 'y' |
| `TS18048`  | 556   | 🔴 Possibly 'null' or 'undefined' |
| `TS2345`   | 518   | 🔴 Argument type mismatch |
| `TS7006`   | 410   | 🟠 Implicit `any` on parameters |
| `TS7053`   | 158   | 🟠 Implicit `any` on indexing |
| `TS2339`   | 133   | 🟠 Property does not exist on type |
| _(and 30+ other codes)_ | ... | ... |

These errors range from unsafe types, missing type declarations, unsafe indexing, and null/undefined runtime risks.
Full report available here [https://jsfiddle.net/programever/9qhc3msg/1]

### 🧱 Beyond Just `strict` and `strictNullChecks`

By "strict mode" we specifically refer to the Typescript compiler options:
-   `strict: true`
-   `strictNullChecks: true`

However, TypeScript offers additional flags that dramatically increase safety and prevent bugs:
```json
{
  "compilerOptions": {
    "noImplicitAny": true,
    "noImplicitReturns": true,
    "noUncheckedIndexedAccess": true,
    "exactOptionalPropertyTypes": true,
    "useUnknownInCatchVariables": true,
    "noPropertyAccessFromIndexSignature": true,
    "noImplicitOverride": true,
    "noUnusedLocals": true,
    "noUnusedParameters": true,
    "noFallthroughCasesInSwitch": true
  }
}
```
These are low-hanging fruit that catch unsafe behaviors **before they hit production**, but for now, we only propose to enforce `strict: true` and `strictNullChecks: true` to improve safety in a minimal and focused way.

## ⚠️ Pain Points

### ⚠️ Runtime Errors in Legacy Code

These are errors from older parts of the system — and since they’ve been around, nobody really owns them or feels responsible for fixing them.
```ts
export const getUsersFollowedByCurrentlyAuthenticatedUser = async (): Promise<User['id'][]> => {
  const axiosConfig = getAxiosConfig();
  const {
    data: { data },
  } = await axios.get<ApiResponse<User['id'][]>>(`${BASE_URL}/member-follows`, axiosConfig);
  return data; // 💥 'data' can be undefined, promise can throw
};
```

### ⚠️ Runtime Errors in New Code

Even new code is not safe without strict checks.
```ts
const NewComponent: React.FC<{ s: string }> = ({ s }) => {
  return <div>{s.toUpperCase()}</div>; // Assumes `s` is a string
};

const Caller: React.FC = () => {
  return <NewComponent s={undefined} />; // 💥 Runtime crash!
};
```

### 📉 The Trend Is Unsustainable

Each commit potentially introduces:
-   More unsafe assumptions
-   More runtime errors
-   More debt to fix later
-   Without automated checks, we are relying on human discipline — and that doesn’t scale.

## 🔐 Proposal 1: Pre-Commit TSC Enforcement

We propose a **pre-commit hook** that:
-   Enforces `strict: true` and `strictNullChecks: true` on changed files only
-   Runs `tsc --noEmit` **just on committing files**
-   **Blocks the commit** if any errors are found — even if the error was not newly introduced. If a file contains any TSC errors, the commit will be blocked.

This way:
-   Does **not** touch untouched legacy code
-   Prevents **new** unsafe code
-   Encourages gradual, organic cleanup — every developer has a chance to improve the codebase, per file that they touch.

We support a flag to skip this Pre-Commit, because there are rare cases where a developer **needs to commit code with existing TSC errors** — for example:

-   Hotfixes or urgent production patches
-   The tsc error are too deep to clean up

## 🧠 Proposal 2: Strategic Approaches For All Errors

### Hybrid Cleanup Strategy (Error Class ➜ then Module Ownership)

Adopt a two-phase cleanup model that begins with global error classes and transitions into feature/module ownership for deeper, safer refactoring.

### 🏗️ Phase 1: Cleanup by Error Type (Centralized Squad)
- A rotating squad focuses on fixing the most common and easy-to-resolve errors (`TS7006`, `TS7031`, `TS7053`, etc.) across the codebase.
- These fixes are typically scriptable or low-risk and help reduce overall noise for developers.
- **Limitations:** This squad may lack deep knowledge of the affected features and won’t have access to dedicated QA. Therefore, they should only handle straightforward, isolated fixes. Any changes that touch complex logic or feature behavior should be deferred to the next phase.
- **Incident Handling:** Not confident? Don’t touch it. Just leave a comment and move on — the squad will review it later.
- **Completion Time:** Remaining errors require contextual knowledge or carry risk — no longer safe for centralized, scriptable fixes.

### 🗂️ Phase 2: Cleanup by Module or Feature (Decentralized Ownership)
- Once the low-hanging errors are cleaned up, remaining issues are analyzed and grouped by feature or module.
- Each team or responsible individual takes ownership of their area, allowing them to refactor with confidence and coordinate QA testing if needed.
- This approach ensures deeper fixes are made by people who understand the business logic and can safely push toward full `strict` compliance.

## 🛡️ Proposal 3: Additional Guardrails for Runtime Safety

### 🌐 Parse at the boundaries

TypeScript only checks **static types**. But when working with data from APIs, we need **runtime validation**. A simple `decoders` or `zod` schema ensures the data truly conforms:
```ts
import * as JD from 'decoders';

const UserDecoder = JD.object({ id: JD.string, name: JD.string });
const parsedResult = UserDecoder.decode(apiResponse);
```

### 🧪 Avoid `as` and use parser

Type assertions using `as` can be dangerous — they tell the compiler to trust you, even when the data may not match the expected shape. Instead, use decoders or parsers to validate the data explicitly at runtime.
```ts
// 🚫 Unsafe - bypasses type checks
const user = apiResponse as { id: string; name: string };

// ✅ Safe - runtime validated
const userResult = UserDecoder.decode(apiResponse);
```

## 🧠 Summary

- TSC errors are not just noise, they lead to real-world bugs, crashes, and silent failures.
- Our current config does nothing to protect new code from slipping into unsafe patterns.
- Every commit is an opportunity to move the codebase toward full type safety — one file at a time.
- Through a coordinated, two-phase cleanup, we can eliminate existing debt without disruption.
- `decoders` or `zod` catch what TypeScript can’t — the boundary of trust. No more `as`, no more `I think` — just `I prove`.
- This is a chance for developer to level up, not just clean up — strict typing encourages better habits, deeper understanding, and long-term confidence.
- We’re laying the foundation for trust — with enough small, steady improvements, full confidence in our system becomes a reality.

## 🧹 Error Code Breakdown For References 

A complete breakdown of all current TypeScript errors in the codebase, grouped by type of issue and sorted by error frequency.

### 🔴 Type Incompatibility Issues

- **`TS2322`** – 723: Type 'x' is not assignable to type 'y'.  
- **`TS2345`** – 518: Argument of type 'x' is not assignable to parameter of type 'y'.  
- **`TS2352`** – 19: Type 'A' cannot be converted to type 'B'.  
- **`TS2739`** – 1: Type 'x' cannot be assigned to type 'y'.

These errors arise from assigning or passing values between incompatible types, often due to incorrect typings, implicit conversions, or faulty assumptions.

---

### 🔴 Null / Undefined Safety

- **`TS18048`** – 556: 'x' is possibly 'null'/'undefined'.  
- **`TS18047`** – 89: 'x' is possibly 'null'/'undefined'.  
- **`TS2532`** – 34: Object is possibly 'null' or 'undefined'.  
- **`TS18049`** – 9: 'x' is possibly 'null'/'undefined'.

These errors highlight unsafe operations on potentially null or undefined values.

---

### 🔴 Unsafe Function or Method Calls

- **`TS2722`** – 51: Cannot invoke an object which is possibly 'undefined'.  
- **`TS2769`** – 43: No overload matches this call.  
- **`TS2366`** – 3: Wrong call signature.  
- **`TS2783`** – 2: Assertion functions have to return data.

This category includes issues where function calls may fail at runtime due to missing, ambiguous, or incorrectly defined signatures.

---

### 🟠 Implicit Any / Missing Types

- **`TS7006`** – 410: Parameter 'x' implicitly has an 'any' type.  
- **`TS7031`** – 160: Missing type declaration.  
- **`TS7034`** – 23: Missing function implementation for 'x'.  
- **`TS7005`** – 84: Variable 'x' implicitly has an 'any[]' type.  
- **`TS7023`** – 2: Synthetic ts-expect-error match ignored.

Caused by undeclared or default `any` types — can be fixed by adding explicit type annotations.

---

### 🟠 Unsafe Indexing

- **`TS7053`** – 158: Element implicitly has an 'any' type because expression of type 'x' cannot be used to index type 'y'.  
- **`TS2538`** – 47: Type 'undefined' cannot be used as an index type.  
- **`TS18046`** – 17: Element implicitly has an 'any' type because of absence of index signatures.  
- **`TS7051`** – 2: Property mismatch per rule violations.

Errors due to accessing object properties with unsafe or untyped keys.

---

### 🟠 Property / Scope / Name Errors

- **`TS2339`** – 133: Property 'x' does not exist on type 'y'.  
- **`TS2531`** – 23: Cannot find name 'x' in module 'y'.  
- **`TS2488`** – 4: The name 'x' does not exist in the current scope.  
- **`TS2551`** – 1: Cannot find name 'x'.  
- **`TS2349`** – 1: Cannot find module 'x'.

These errors occur when accessing non-existent properties or using undeclared identifiers.

---

### 🟠 Syntax or Version Compatibility

- **`TS7015`** – 18: This syntax is only available in TypeScript 3.7 and higher.  
- **`TS7017`** – 1: Configured type aligns wrongly.  
- **`TS7022`** – 1: Label is not valid at this location.

Relate to TypeScript language feature usage or syntax placement.

---

### 🟠 Uninitialized / Unused Variables

- **`TS2454`** – 18: Variable is used before being assigned.  
- **`TS2464`** – 1: Variable is scope-inaccessible.

Generally solvable by reordering code or using default initializations.

---

### 🟢 Low Impact or Rare Issues

- **`TS2790`** – 3: The operand of a 'delete' operator must be optional.  
- **`TS7019`** – 1: Spread assignments are non-spreadable.  
- **`TS2604`** – 1: Component missing attribute 'x'.  
- **`TS2786`** – 1: Variance-monitoring type check failed.  
- **`TS2677`** – 2: Cannot extend type 'x'.

These are edge-case errors with minor or isolated impact.
