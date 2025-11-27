# Frontend Code Review Guidelines

> **Goal:** Ensure code is readable, maintainable, secure, performant, and follows best practices. Code reviews are collaborative—focus on constructive feedback and continuous improvement.

---

## Table of Contents

1. [General Code Quality](#general-code-quality)
2. [TypeScript & Type Safety](#typescript--type-safety)
3. [React Best Practices](#react-best-practices)
4. [State Management](#state-management)
5. [Performance](#performance)
6. [Imports & Organization](#imports--organization)
7. [CSS & Styling](#css--styling)
8. [Security](#security)
9. [Testing](#testing)
10. [Accessibility](#accessibility)
11. [Git & Version Control](#git--version-control)
12. [Review Etiquette](#review-etiquette)

---

## General Code Quality

### Naming Conventions

- ✅ Use **meaningful and consistent** naming conventions
- ✅ Use **UPPER_SNAKE_CASE** for constants
- ✅ Use **camelCase** for variables and functions
- ✅ Use **PascalCase** for components and classes
- ✅ Use `on` prefix for **props** (e.g., `onClick`, `onSubmit`)
- ✅ Use `handle` prefix for **internal functions** (e.g., `handleClick`, `handleSubmit`)
- ✅ Use `is`/`has`/`should` for boolean variables (e.g., `isLoading`, `hasError`, `shouldRender`)
- ❌ Avoid abbreviations unless universally understood (e.g., `btn` → `button`)

### Code Clarity

- ✅ Fix all **typos** (install [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker))
- ✅ Add **comments** just for:
  - Complex logic or algorithms
  - Non-obvious business rules
- ✅ Keep functions small and focused (Single Responsibility Principle)
- ✅ **Break down large components** into smaller, reusable components
- ✅ Remove all dead code, unused variables, and functions
- ❌ Don't leave commented-out code (use git history instead)

### Variables & Operators

- ✅ Use **strict equality**: `===` instead of `==`, `!==` instead of `!=`
- ✅ Prefer `const` over `let` (immutability by default)
- ❌ Never use `var`
- ✅ Use **optional chaining** (`?.`) for potentially null/undefined values, only in cases you have to.
- ✅ Use **nullish coalescing** (`??`) instead of `||` when checking for `null`/`undefined` specifically

```javascript
// ❌ Bad
const value = obj.prop || "default"; // Fails for 0, false, ''

// ✅ Good
const value = obj.prop ?? "default"; // Only fails for null/undefined
```

### Constants & Magic Numbers

- ✅ Replace hard-coded values with **named constants**
- ✅ Group related constants in separate files or enums

```javascript
// ❌ Bad
if (status === 'pending') { }

// ✅ Good
const ORDER_STATUS = {
  PENDING: 'pending',
  ACTIVE: 'active',
  COMPLETED: 'completed'
} as const;

if (status === ORDER_STATUS.PENDING) { }
```

### Debugging & Logging

- ❌ Remove all `console.log`, `console.warn`, `debugger`, and debugging statements
- ✅ Use proper logging library for production (e.g., Sentry, LogRocket)
- ✅ Log meaningful error messages, not just `console.error(e)`

---

## TypeScript & Type Safety

### Type Annotations

- ✅ **Always specify types** for:
  - Props (use `interface` or `type`)
  - State variables
  - Function arguments and return values
  - API responses
- ✅ Use `interface` for object shapes, `type` for unions/intersections
- ❌ Avoid `any` type (use `unknown` if type is truly unknown, then narrow it)
- ✅ Use **type imports**: `import type { User } from './types'`
- ✅ Use TypeScript utility types: `Partial<T>`, `Pick<T>`, `Omit<T>`, `Record<K, V>`

### Type Guards & Narrowing

- ✅ Use type guards for runtime type checking
- ✅ Validate API responses with libraries like Zod or Yup

---

## React Best Practices

### Component Structure

- ✅ Use **functional components** with hooks (avoid class components)
- ✅ Keep component files under 250 lines (split if larger)
- ✅ Use **self-closing tags** when no children: `<Component />`
- ✅ Use React Fragment shorthand `<></>` (use `<Fragment key={id}>` when key is needed)
- ✅ Extract complex JSX logic into separate components or variables

```jsx
// ❌ Bad
return (
  <div>
    {showModal && user && user.isActive && !user.isBanned && <Modal>...</Modal>}
  </div>
);

// ✅ Good
const shouldShowModal = showModal && user?.isActive && !user?.isBanned;
return <div>{shouldShowModal && <Modal>...</Modal>}</div>;
```

### Conditional Rendering

- ⚠️ Ensure `value` is **boolean** before using `value && <Component />`
  - Prevents displaying `0`, `""`, or `NaN` on screen

### Props & Prop Drilling

- ✅ Destructure props in function signature
- ✅ Avoid prop drilling—use Context API or state management for deeply nested data
- ✅ Use `children` prop for composition
- ❌ Don't spread all props blindly: `{...props}` (be explicit)
- ✅ Define default props using default parameters

### Hooks Best Practices

- ✅ Avoid defining functions or variables **inside `useEffect`** unless necessary
  - Define them outside to prevent unnecessary re-renders
- ✅ Add dependencies to `useEffect` array
- ✅ Use `useCallback` for functions passed as props to memoized components
- ✅ Use `useMemo` for expensive computations
- ✅ Clean up side effects in `useEffect` return function

```javascript
// ❌ Bad - function recreated on every render
useEffect(() => {
  const fetchData = async () => {
    /* ... */
  };
  fetchData();
}, []);

// ✅ Good - function defined outside
const fetchData = useCallback(async () => {
  /* ... */
}, []);

useEffect(() => {
  fetchData();
}, [fetchData]);
```

### Keys in Lists

- ✅ Always use **stable, unique keys** for list items
- ❌ Never use array index as key (unless list never reorders)
- ✅ Use item ID or unique identifier

---

## State Management

### Local State

- ✅ Use `useState` for component-local state
- ✅ Use `useReducer` for complex state logic
- ✅ Keep state as close to where it's used as possible (avoid lifting unnecessarily)
- ✅ Group related state into objects

```javascript
// ❌ Bad - multiple related state variables
const [firstName, setFirstName] = useState("");
const [lastName, setLastName] = useState("");
const [email, setEmail] = useState("");

// ✅ Good - grouped related state
const [user, setUser] = useState({
  firstName: "",
  lastName: "",
  email: "",
});
```

### Defensive Programming

- ✅ **Never assume data will be available** when component renders
- ✅ Always show loading states until data is validated

```javascript
// ❌ Bad - can crash if data is undefined
return <div>{data.user.name}</div>;

// ✅ Good - defensive programming
if (isLoading || isFetching || !data) {
  return <Loader />;
}

return <div>{data.user.name}</div>;
```

---

## Performance

### Optimization

- ✅ Use `React.memo` for expensive components that render often
- ✅ Use `useMemo` for expensive calculations
- ✅ Use `useCallback` for functions passed to memoized children
- ✅ Lazy load components: `const Component = lazy(() => import('./Component'))`
- ✅ Debounce/throttle expensive operations (search inputs, scroll handlers)
- ✅ Use `React.Suspense` for lazy-loaded components
- ✅ Virtualize long lists (react-window, react-virtualized)

### Bundle Size

- ✅ Check bundle size impact of new dependencies
- ✅ Import only what you need: `import { debounce } from 'lodash/debounce'` not `import _ from 'lodash'`
- ✅ Use dynamic imports for large libraries
- ✅ Remove unused dependencies

### Images & Assets

- ✅ Optimize images (use WebP format, compress)
- ✅ Use lazy loading for images: `<img loading="lazy" />`
- ✅ Provide multiple image sizes (responsive images)
- ✅ Use CDN for static assets

---

## Imports & Organization

### Import Order

1. React and third-party libraries
2. Modules (actions, hooks, utilities)
3. Components
4. Types
5. Constants
6. Styles and icons (at the bottom)

### Import Best Practices

- ✅ Use **absolute imports** (configured in `tsconfig.json`)
- ✅ For type imports, always use `import type`
- ✅ Remove unused imports

---

## CSS & Styling

### Best Practices

- ✅ Do not use **units for zero** values: use `0`, not `0rem` or `0px`
- ✅ Prefer `rem` and `%` over `px` for scalability
  - Exception: `1px` or `2px` for borders/shadows can stay as `px`
- ✅ Use `dvh` instead of `vh` (fixes mobile viewport bugs - [read more](https://blog.stackademic.com/exploring-css-units-unleashing-the-power-of-dvh-over-vh-1ff61b9c87ae))
- ✅ Use **logical properties** for internationalization (RTL support):
  - `margin-inline-start` / `margin-inline-end` instead of `margin-left` / `margin-right`
  - `padding-inline-start` / `padding-inline-end` instead of `padding-left` / `padding-right`
- ❌ Avoid `!important` unless absolutely necessary (document why if used)

### CSS Modules / Styled Components

- ✅ Use CSS Modules or styled-components for component-scoped styles
- ✅ Avoid global styles unless necessary

---

## Security

### Data Validation

- ✅ **Always validate API responses** contain only necessary data
- ✅ Ensure sensitive information is **filtered out** before reaching client:
  - Authentication tokens
  - API keys
  - Internal IDs
  - Personal identifiable information (PII)
- ✅ Sanitize user input before rendering (prevent XSS)
- ✅ Use `DOMPurify` for rendering HTML from user input

### Authentication & Authorization

- ✅ Never store sensitive tokens in localStorage (use httpOnly cookies)
- ✅ Implement proper error handling without exposing system details
- ✅ Check user permissions before rendering sensitive UI
- ✅ Validate file uploads (type, size, content)

### Dependencies

- ✅ Keep dependencies up to date
- ✅ Review security advisories (`npm audit`, `yarn audit`)
- ✅ Avoid packages with known vulnerabilities

---

## Testing

### Coverage

- ✅ Write tests for:
  - Critical business logic
  - Utility functions
  - Complex components
  - Edge cases and error states
- ✅ Aim for meaningful coverage, not just high percentages
- ✅ Test user interactions, not implementation details

### Testing Best Practices

- ✅ Use **React Testing Library** (test behavior, not implementation)
- ✅ Write tests that resemble how users interact with your app
- ✅ Mock API calls and external dependencies
- ✅ Test accessibility (screen reader interactions, keyboard navigation)

---

## Accessibility

_For accessibility, refer to the [Accessibility Guidelines](./Guidelines.md) document._

---

## Git & Version Control

### Commit Messages

- ✅ Write clear, descriptive commit messages
- ✅ Follow conventional commits format: `type(scope): description`
  - Types: `feat`, `fix`, `docs`, `style`, `refactor`, `test`, `chore`
- ✅ Reference ticket/issue numbers

### Pull Requests

- ✅ Keep PRs small and focused (easier to review)
- ✅ Provide clear PR description with:
  - What changed and why
  - How to test
  - Screenshots/videos for UI changes
  - Relevant ticket links
- ✅ Self-review before requesting review
- ✅ Resolve merge conflicts before review
- ✅ Ensure CI/CD passes (tests, linting, build)

### Branch Management

- ✅ Use descriptive branch names: `feature/user-auth`, `fix/login-bug`
- ✅ Delete branches after merging
- ✅ Keep branches up to date with main/develop

---

## Review Etiquette

### For Reviewers

- ✅ Be **constructive and respectful** in feedback
- ✅ Explain **why** something should be changed
- ✅ Differentiate between:
  - **Blocking issues** (must fix): bugs, security issues, breaking changes
  - **Suggestions** (nice to have): style preferences, optimizations
- ✅ Praise good code and improvements
- ✅ Use prefixes: `[nit]`, `[question]`, `[suggestion]`, `[blocking]`
- ✅ Review promptly (within 24 hours when possible)
- ❌ Don't bikeshed (argue over trivial matters)

```
// ❌ Bad feedback
"This is wrong."

// ✅ Good feedback
"[suggestion] Consider using `useMemo` here to avoid recalculating on every render.
This function is called frequently and the computation is expensive."
```

### For Authors

- ✅ **Respond to all comments** (even if just "Done" or "Acknowledged") (mostly I prefer add ✅ emoji for the comment!)
- ✅ Ask questions if feedback is unclear
- ✅ Be open to feedback—it's not personal
- ✅ Update PR description as you make changes
- ✅ Mark conversations as resolved after addressing
- ✅ Thank reviewers for their time

---

## Review Checklist

Use this checklist when reviewing PRs:

### Functionality

- [ ] Code does what it's supposed to do
- [ ] Edge cases are handled
- [ ] Error states are handled gracefully
- [ ] Loading states are shown appropriately

### Code Quality

- [ ] Code is readable and maintainable
- [ ] No code duplication (DRY principle)
- [ ] Functions are small and focused
- [ ] Naming is clear and consistent
- [ ] No commented-out code
- [ ] No console logs or debuggers

### TypeScript

- [ ] All props, state, and functions are properly typed
- [ ] No `any` types (use `unknown` if needed)
- [ ] Type imports use `import type`

### React

- [ ] Components are appropriately sized
- [ ] Hooks follow rules and best practices
- [ ] Keys are used correctly in lists
- [ ] Conditional rendering is safe (no `0` or `""` displayed)
- [ ] No unnecessary re-renders

### Performance

- [ ] No obvious performance issues
- [ ] Expensive operations are memoized/optimized
- [ ] Images are optimized
- [ ] Bundle size impact is acceptable

### Styling

- [ ] CSS follows project conventions
- [ ] Uses CSS variables where appropriate
- [ ] Responsive design works on all screen sizes
- [ ] RTL support (if applicable)

### Security

- [ ] No sensitive data exposed
- [ ] User input is validated and sanitized
- [ ] API responses are validated

### Accessibility

- [ ] Keyboard navigation works
- [ ] Screen reader compatible
- [ ] Sufficient color contrast
- [ ] Semantic HTML used

### Testing

- [ ] New functionality has tests
- [ ] Tests are meaningful and pass
- [ ] Edge cases are tested

### Documentation

- [ ] Complex logic is commented
- [ ] README updated if needed
- [ ] API changes are documented

---

## Final Thoughts

> **Remember:** The goal of code review is not just to catch bugs, but to:
>
> - Ensure code quality and maintainability
> - Share knowledge across the team
> - Maintain consistency in the codebase
> - Improve as developers together
>
> **Let's keep learning and improving together! 🚀**
