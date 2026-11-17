---
type: perf
impact: med
effort: med
site: packages/compiler/src/taglib/dependencies.ts › LookupDependencies.drop
---

# Narrow taglib invalidation to the templates that use a changed tag

A lookup reads its tags dirs for every template and directory, so a tag added to or removed from one drops that lookup and every lookup beneath it, and `LookupDependencies.drop` marks stale every template compiled with any of them (all 1,524 in one project's `src/tags`). Only templates that looked up that tag name (as a custom tag, or as a native element it now replaces) can compile differently; recording the names each compile looked up would let `State.invalidate` mark only those.

Check: compile `src/a.marko` and `src/b.marko` (neither names `<new-tag>`) beside a `src/tags` dir, add `src/tags/new-tag.marko`, and call `taglib.invalidate([that path], cache)`: it returns both templates.
