## What and why

<!-- What this changes, and the reason it is needed. Link the issue if there is one. -->

## How this was tested

<!-- Commands run, outputs compared against a known-good result, CRS / grid
alignment checks, and anything a reviewer should re-run. -->

## AI assistance

<!-- See CONTRIBUTING.md section 6. Routine editor autocompletion does not need
disclosing; AI use that shaped a method, a parameter, a data-cleaning decision, or
a non-trivial block of code does. -->

- [ ] No AI assistance beyond routine editor completion
- [ ] AI-assisted (fill in below)

**Files / functions:**

**Prompt or approach:**

**What I verified, and how:**

## Checklist

- [ ] Branched off `main` and rebased on the latest `main`
- [ ] Commits signed off (`git commit -s`)
- [ ] No data files added; new data has an `acquire_*` script and a cited, licensed source
- [ ] Sensitive-species locations generalised to 5 km before any output
- [ ] Dependencies pinned in `environment.yml`; any new package justified above
- [ ] Analysis change runs end to end from the entry script
- [ ] Limitations stated in docstrings / notebook prose
- [ ] `reproducibility-logs` updated if outputs changed
