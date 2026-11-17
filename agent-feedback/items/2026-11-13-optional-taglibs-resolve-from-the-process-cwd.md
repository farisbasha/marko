---
type: unclear
impact: low
effort: med
site: packages/compiler/src/taglib/index.ts › resolveOptionalTaglibs
---

# Optional translator taglibs resolve from the process working directory

`resolveOptionalTaglibs` resolves a translator's `optionalTaglibs` (eg Marko 5's `@marko/tags-api-preview`) with `markoModules.resolve` from the root found at `process.cwd()`, not from the project being compiled, so the same template's tags depend on where the process started, and neither `meta.dependencies` nor `taglib.getDependencies` can describe that input. Resolving per project (from the package a lookup belongs to) would make it an ordinary package input.

Check: compile a Marko 5 template whose project depends on an optional taglib with `process.cwd()` outside that project: its optional tags are not found, and `taglib.getDependencies` lists no read for them.
