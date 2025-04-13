## Path type `Prefix`

When you use:

```yaml
path: /web
pathType: Prefix
```

It will match only those paths that start with `/web` **as a segment**, meaning the slash matters.

### It **matches**:

- `/web`
- `/web/`
- `/web/about`
- `/web/dashboard`

### It **does NOT match**:

- `/weblab`
- `/website`
- `/webify`

Because in `/weblab`, the next character after `/web` is **not a `/`**, which makes it a **different prefix** altogether.

---

This behavior is aligned with the Kubernetes spec:

> For Prefix pathType, matching is done on a path element by element basis. A request is a match for path /web if every element of the request path matches the corresponding element in the path prefix.
> 

So `/web/lab` matches, but `/weblab` does not, because `weblab` is a single path element and does not match the `/web` segment.

### But when you use ingress-nginx, path segment is not considered only the prefix is checked.

## Why?

---

### Kubernetes Spec vs NGINX Reality

According to the **Kubernetes API spec**, `pathType: Prefix` matches **by path segments**. So technically:

> /web should only match /web, /web/xyz, not /website, /weblab.
> 

**However**, NGINX Ingress Controller **does not strictly follow this segment-based matching**. Instead, it interprets `Prefix` as a **simple string prefix**, meaning:

- `/web` matches **any path that starts with `/web`**.

Hence:

- `/weblab` ✔
- `/website` ✔
- `/webzzz` ✔

All work.

---

### TL;DR

| Behavior | Explanation |
| --- | --- |
| `/web`, `/web/abc` matched | ✅ by both spec and NGINX |
| `/weblab`, `/website` matched | ✅ by NGINX's string-prefix logic |
| Spec says they shouldn't? | ✅ Yes, but it's controller-specific behavior |