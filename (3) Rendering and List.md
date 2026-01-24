
```markdown
# React Rendering, Conditional Rendering, Lists & Keys  

<p align="center">
  <img src="https://img.shields.io/badge/React-2025-blue?logo=react&logoColor=white&style=for-the-badge" alt="React 2025" />
  <img src="https://img.shields.io/badge/Rendering-Lists-Keys-orange?style=for-the-badge" alt="Rendering • Lists • Keys" />
  <img src="https://img.shields.io/badge/Interview-Ready-success?style=for-the-badge" alt="Interview Ready" />
</p>

Rendering in React is the process of turning your component tree (JSX + state/props) into real DOM nodes.  
React uses a **declarative** approach: you describe *what* the UI should look like, and React efficiently figures out *how* to update the DOM.

## ✨ Types of Rendering

| Type               | When it happens                          | What happens                                                                |
|--------------------|------------------------------------------|-----------------------------------------------------------------------------|
| **Initial Render** | Component mounts for the first time      | Full DOM tree is created                                                    |
| **Re-render**      | State or props change                    | Virtual DOM diff → only necessary parts of the real DOM are updated         |

## 🔹 Conditional Rendering

```tsx
function UserPanel({ user, isLoading, error }) {
  // Early returns – cleanest and most maintainable approach
  if (isLoading) return <LoadingSpinner size="large" />;
  if (error)    return <ErrorAlert message={error} />;
  if (!user)    return <LoginPage />;

  // Ternary operator
  const greeting = user.isPro ? "Pro Member" : "Regular";

  // Logical AND – render only if condition is true
  {user.hasNotifications && <NotificationBadge count={user.unread} />}

  // Object lookup – very clean for multiple discrete states
  const dashboard = {
    admin:      <AdminDashboard />,
    moderator:  <ModeratorDashboard />,
    user:       <UserDashboard />,
    guest:      <GuestMessage />,
  };

  return (
    <div className="panel">
      <h1>{greeting}</h1>
      {dashboard[user.role] || <AccessDenied />}
    </div>
  );
}
```

### Conditional Rendering

| Pattern               | Best for                                  |   When to prefer                   |
|-----------------------|-------------------------------------------|------------------------------------|
| Early return          | Loading, error, auth states               | Almost always (top of component)   |
| Ternary               | Small true/false branches                 | Inline short conditions            |
| && operator           | "Show only if true"                       | Badges, alerts, optional sections  |
| Object / Map lookup   | Many discrete states                      | Role-based UI, tabs, wizards       |
| Component variable    | Complex logic → clean JSX                 | Medium-large conditional blocks    |

## 🔹 Rendering Lists & Keys – The Most Important (and Bug-Prone) Part

```tsx
// ❌ Wrong: using index as key
{items.map((item, index) => (
  <TodoItem key={index} {...item} />
))}

// ✅ Correct (almost every real-world production case)
{items.map(item => (
  <TodoItem key={item.id} {...item} />
))}
```

### Why Keys Matter

- Keys tell React which items are the same between renders
- Stable keys → React reuses DOM nodes → better performance + preserves state
- Unstable keys (or index) → unnecessary remounts → lost focus, broken animations, stale state

### Key Selection Guide

```
Does the item have a stable, unique ID? (db id, _id, uuid)
├── Yes → Use it: key={item.id} / key={item._id}
└── No
    ├── Client-generated items → Use UUID / nanoid
    └── No possible stable identifier → index (add comment explaining why)
```

### When is index as key acceptable? (Very few cases)

- Completely static list (never reordered, filtered, or items removed/added)
- Purely presentational items (no local state, inputs, or animations)

### Powerful Pattern: Force Remount / Reset State

```tsx
// Form fully resets when user changes
<Form key={selectedUser.id} initialValues={selectedUser} />
```

## Real-World Example – E-commerce Orders List

```tsx
function Orders({ orders }) {
  return (
    <div className="orders-grid">
      {orders.map(order => (
        <div key={order._id} className="order-card">
          <OrderHeader order={order} />

          {order.status === 'cancelled' ? (
            <CancelledOrderView order={order} />
          ) : order.status === 'delivered' ? (
            <DeliveredOrderView order={order} />
          ) : (
            <ActiveOrderActions order={order} />
          )}
        </div>
      ))}
    </div>
  );
}
```

## Quick Summary 

| Topic                     | Best Practice 2025–2026                             |
|---------------------------|-----------------------------------------------------|
| Conditional Rendering     | Early return > Ternary > && > Object lookup         |
| List Rendering            | `.map()` + stable `key`                             |
| Best Key                  | `id` / `_id` > UUID > nanoid > composite            |
| Index as Key              | Only for static & stateless lists                   |
| Force Remount / Reset     | `key={changingValue}`                               |
| List Performance          | Virtualization + `useMemo` for expensive operations |

---
Let me know if you'd like to add sections (e.g. Virtualization, Concurrent Features, Suspense examples) or make it shorter/more minimal.
