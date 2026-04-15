# The Difference Between a Library and a Framework ☕

> **3 min read** · Concepts

"Is React a library or a framework?"

This question comes up in almost every junior developer interview. The answer is *library* — but more importantly, do you know why? And does the distinction actually matter?

---

## The One-Sentence Answer

> **A library is code you call. A framework is code that calls you.**

This is sometimes called the **Hollywood Principle**: *"Don't call us, we'll call you."*

---

## What a Library Looks Like

When you use a library, **you're in control**. You call it when you need it, then continue doing your own thing.

```javascript
// You decide when to call it
import { format } from 'date-fns';

const today = new Date();
const nicely = format(today, 'dd MMM yyyy');
// "15 Jan 2024"
```

Other examples: Lodash, Axios, Moment.js, Chart.js.

You import it, use specific functions, carry on. The library doesn't care about the structure of your project. You could use it once or a hundred times.

---

## What a Framework Looks Like

With a framework, **the framework is in control**. It dictates the structure of your project, calls your code at the right moments, and expects you to follow its rules.

```python
# Django decides when to call your view
# You write the function — Django calls it when a request arrives

def my_view(request):
    return HttpResponse("Hello")
```

```
Django is running → request comes in → Django looks up URL → 
Django calls YOUR function → Django returns the response
```

You didn't call `my_view`. Django did. You just wrote the function and put it in the right place.

Other examples: Django, Rails, Angular, Spring Boot, Next.js.

---

## React Is an Interesting Case

React is officially described as **a library for building UIs**. But developers often debate this because React starts to feel framework-like once you add the surrounding ecosystem (React Router, Redux, etc.).

The technical answer: React itself is a library. You call `React.createElement`, you control when components render, you structure your files however you want.

But when people say "React app" they often mean the whole ecosystem — which blurs the line.

---

## Why the Distinction Matters

**Libraries are easier to add and remove.** You can drop Axios from a project and replace it with Fetch with minimal disruption.

**Frameworks are harder to replace.** Migrating from Django to Flask is a major undertaking because Django's assumptions are baked throughout your codebase.

The trade-off:
| | Library | Framework |
|--|---------|-----------|
| **Control** | You | The framework |
| **Flexibility** | High | Lower |
| **Decision fatigue** | High (you choose everything) | Low (decisions made for you) |
| **Switching cost** | Low | High |
| **Getting started** | Slower (you build structure) | Faster (structure provided) |

---

## The Takeaway

> Libraries are tools you use. Frameworks are structures you work within. The key difference is control flow — who calls whom. React is a library. Django and Angular are frameworks. Next.js is a framework built on top of the React library.

---

## Want to Go Deeper?

- 📄 [React Guide](../frontend/react.md) — components, hooks, state management
- 📄 [Django Guide](../backend/django.md) — MVT architecture, ORM, views

---

*← [Back to Coffee Reads](./README.md)*
