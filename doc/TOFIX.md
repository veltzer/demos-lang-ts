# TOFIX

Findings from a code scan on 2026-10-04.

## Medium

- `rsconstruct.toml:52` - the only processor that touches `hello.ts` is oxlint; nothing in the build compiles or type-checks it. `build.sh:3` (`tsc hello.ts`) is only shellchecked (`rsconstruct.toml:43`), never run, and there is no `[processor.npm]` so the `typescript` dependency in `package.json:3` is never installed by CI. A TypeScript demo repo whose CI never runs `tsc` will not notice a type error; wire `npm` + a `tsc --noEmit` step (script/explicit processor) into the build.

## Low

- `hello.js:1` - committed compiled output of `hello.ts` (produced by `build.sh:3`); it can silently drift from its source. Generate it in the build and stop tracking it (or ignore it), rather than keeping a hand-committed copy.
