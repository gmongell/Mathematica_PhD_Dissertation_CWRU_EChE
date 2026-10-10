# Contributing and reproducibility policy

This repository contains historical scientific research materials. Contributions must preserve original source provenance and distinguish documentation, proposed corrections, and independently verified numerical results.

## Change workflow

1. Work on a topic branch and submit a pull request; do not rewrite historical commits.
2. Describe each changed file, the motivation, and whether the change affects scientific outputs.
3. Record software versions, operating system, input files, units, parameters, random seeds (where applicable), and commands needed to reproduce results.
4. Run relevant syntax checks and tests, and report the exact commands and outcomes. If proprietary software, datasets, or compute resources are unavailable, explicitly mark tests **not run**.
5. For numerical changes, provide an independent analytical limit, reference implementation, conservation check, or benchmark where feasible. Include tolerances and convergence evidence.
6. Never replace a damaged or missing historical artifact with a guessed reconstruction. Add verified recovered material separately with provenance and hashes.
7. Review copyright, third-party licensing, private data, credentials, and dataset redistribution rights before committing.
8. Keep machine-specific absolute paths, credentials, and large generated simulation outputs out of tracked examples.

## Scientific review checklist

- [ ] Inputs and outputs are described with physical units.
- [ ] Dependencies and version constraints are documented.
- [ ] Assumptions, boundary/initial conditions, and normalization conventions are stated.
- [ ] Numerical convergence or appropriate limiting-case tests are supplied, or the absence of validation is clearly stated.
- [ ] Third-party code, notebooks, figures, and datasets retain attribution.
- [ ] Documentation does not claim that unexecuted examples or simulations have been validated.

## Scope

A successful syntax check does not establish physical correctness. A literature citation provides context but does not certify an implementation. Pull requests should be reviewed before merging; historical artifacts should remain traceable.
