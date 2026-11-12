# spec-kit refine

Takes fixes and additions for the **latest feature** and runs the whole pipeline in one command:

1. amend `spec.md` in place (with a dated `Refinements` entry)
2. plan (updating existing artifacts, not regenerating)
3. tasks (completed tasks stay, new ones continue the numbering)
4. analyze, then fix the findings (up to 3 rounds)
5. implement

It does not create a new feature or branch and does not commit.

## Install

```bash
specify extension add --dev /path/to/speckit-extensions/refine
```
or
```bash
specify extension add refine --from https://github.com/Zdertis420/spec-kit-refine/archive/refs/tags/v1.0.0.zip
```

## Usage

```
/speckit-refine-refine the dock should hide when a window is fullscreen; also fix the clock showing UTC
```

An argument is required. The feature is taken from `.specify/feature.json`, falling back to the highest-numbered directory in `specs/`.
