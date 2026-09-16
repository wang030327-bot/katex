---
name: katex
description: Integrate, configure, troubleshoot, or optimize KaTeX math rendering in browser, Node.js, SSR, static-site, and component-framework projects. Use for KaTeX APIs, auto-render delimiters, CSS/font loading, macros, accessibility, security options, rendering errors, hydration, or migration from another math renderer. Do not use for solving the mathematics itself.
---

# KaTeX

Implement math rendering that fits the project's existing stack and content pipeline. Preserve the user's chosen framework, package manager, hosting model, and styling conventions.

## Establish the rendering path

Inspect the project before editing. Determine:

- whether rendering happens in the browser, during SSR/static generation, or both;
- whether expressions are already isolated or embedded in prose;
- how CSS and font assets are served;
- whether TeX comes from trusted authors or untrusted users;
- the installed KaTeX version and any existing wrapper or Markdown plugin.

Choose the smallest suitable API:

- Use `katex.render(expression, element, options)` for a known DOM target.
- Use `katex.renderToString(expression, options)` for SSR, static generation, or an HTML string.
- Use the auto-render extension only when math delimiters must be discovered inside existing text nodes.
- Keep an existing framework integration when it is maintained and already part of the project; avoid adding a wrapper solely for a single render call.

For version-sensitive APIs, package exports, CDN URLs, integrity hashes, supported commands, or migration behavior, verify the installed package/types and current official KaTeX documentation or the `KaTeX/KaTeX` repository. Do not copy a version or integrity hash from memory.

## Integrate completely

Import or serve the KaTeX stylesheet as well as the JavaScript API. Ensure its `fonts/` directory remains reachable at the relative paths referenced by the CSS. Use an HTML5 doctype in standalone pages.

For SSR output, ship the CSS and fonts to the client even when no client-side KaTeX JavaScript is needed. Keep server and client versions aligned to avoid markup or hydration differences.

Render after the target DOM exists. In reactive applications, scope rendering to the owning component, avoid scanning the full document after every update, and prevent duplicate rendering. Clean up observers or asynchronous work when a component unmounts.

## Configure deliberately

Start from explicit options rather than relying on incidental defaults:

```js
const options = {
  displayMode: false,
  output: "htmlAndMathml",
  throwOnError: false,
  trust: false,
};
```

- Set `displayMode` from the content model, not by inspecting rendered markup.
- Keep `output: "htmlAndMathml"` unless the user has a concrete reason to trade away visual HTML or accessible MathML.
- Use `throwOnError: true` when invalid TeX should fail a build or test. Use `false` in interactive content where showing the source is preferable to breaking the page.
- Treat `strict` separately from parse errors; select `warn`, `error`, or a handler according to the project's compatibility policy.
- Use finite `maxSize` and `maxExpand` limits for untrusted or adversarial input.

If a JavaScript string contains TeX, preserve backslashes correctly. Prefer `String.raw` for readable literals when interpolation and backticks do not create their own escaping problem.

## Handle auto-render carefully

Load the auto-render extension separately from core KaTeX. Pass the narrowest container that owns the content.

When enabling `$...$`, place the `$$...$$` delimiter before `$...$`. Consider currency and ordinary dollar signs before enabling the single-dollar delimiter. Preserve or extend the default ignored regions so code, preformatted text, scripts, styles, textareas, and form options are not rendered as math.

Do not use auto-render when a Markdown/MDX parser or CMS already identifies math nodes; render those nodes directly to avoid delimiter ambiguity and repeated DOM scanning.

## Protect untrusted content

Keep `trust: false` by default. If a feature requires `\\href`, `\\url`, `\\includegraphics`, or HTML-extension commands, use a narrow `trust(context)` allowlist for the required commands and URL protocols instead of enabling all trusted commands.

Do not share one mutable `macros` object across unrelated users, tenants, posts, or trust boundaries. Persistent definitions such as `\\gdef` mutate shared macro state and can redefine later rendering behavior. Scope persistent macros to content with a common author and trust level.

KaTeX-generated markup is intended to resist script injection, but preserve the application's sanitization and Content Security Policy requirements. Be careful when displaying thrown error messages because they can contain the original unescaped TeX source.

## Diagnose by symptom

- Missing glyphs or incorrect layout: verify the CSS loaded, font requests succeed, paths are correct, and CSS/version pairs match.
- Literal TeX remains visible: verify lifecycle timing, delimiters, ignored tags/classes, and that the relevant extension or parser ran.
- Duplicate or nested output: make rendering idempotent and stop rescanning already-rendered nodes.
- SSR hydration warnings: render deterministically with matching versions/options or render on only one side of the boundary.
- Unsupported command: check the official support table, required contrib extension, spelling, and configured macros before replacing KaTeX.
- Build/import failure: inspect the installed package's exports and the project's module format instead of guessing an import path.

## Verify the result

Test at least one inline expression, one display expression, invalid TeX, and the project's relevant untrusted-input case. For auto-render, also test escaped delimiters, ignored code blocks, and currency text when `$...$` is enabled.

Confirm in the rendered page that CSS and fonts load without errors, visual output is not duplicated, keyboard/screen-reader semantics remain available, and server-rendered markup does not produce hydration warnings. Run the project's existing tests, type checks, and build after edits.

## Official sources

Prefer these maintained sources when verification is needed:

- `https://katex.org/docs/api`
- `https://katex.org/docs/options`
- `https://katex.org/docs/autorender`
- `https://katex.org/docs/security`
- `https://github.com/KaTeX/KaTeX`
