# Governance

How this project is organised and how decisions get made. Kept short on purpose.

## Roles

### Maintainers

Merge rights on `main`, ownership of branch-protection settings, and the final call
when a discussion does not resolve on its own. There are always at least two, so no
single person is a bottleneck or a single point of failure.

| Name | GitHub handle | Notes |
| --- | --- | --- |
| Linda Angulo Lopez | `@lindangulopez` | Project lead |
| _to be named_ | | Backup maintainer |

### Module owners

Each module repository has an owner who reviews changes in it. Owners are listed in
that repository's own `CODEOWNERS`.

| Module | Repository | Owner |
| --- | --- | --- |
| Data management | `coa-connectivity-lab/data-management` | _to be named_ |
| Database | `coa-connectivity-lab/db-schema` | _to be named_ |
| ML models | `coa-connectivity-lab/ml-models` | _to be named_ |
| QGIS projects | `coa-connectivity-lab/qgis-projects` | _to be named_ |
| Reproducibility logs | `coa-connectivity-lab/reproducibility-logs` | _to be named_ |

### Data steward

One named person is responsible for the sensitive-species generalisation rule (see
[`CONTRIBUTING.md`](CONTRIBUTING.md) section 9) and for checking that every data
source is properly licensed and attributed.

- Data steward: _to be named_

### Contributors

Anyone with an accepted pull request. After a first merged pull request a
contributor is added to the `contributors` team, which carries Write access (push
branches, open pull requests) across the stack.

| Name | GitHub handle | Role | Module(s) | Affiliation |
| --- | --- | --- | --- | --- |
| _add collaborators here_ | | | | |

## Decision-making

- **Lazy consensus.** A proposal on a pull request or issue goes ahead unless
  someone raises a substantive objection within a stated window (two working days
  for routine changes, longer if asked).
- **Escalation.** If consensus stalls, the maintainers decide, and record the
  reasoning on the issue.
- **Bigger changes** (a new dependency, a method change, a schema change) get an
  issue first, so the discussion is not buried in a pull request diff.

## Adding a collaborator

1. Any maintainer proposes the person on an issue.
2. They are added to a GitHub **team**, not to individual repositories, so access
   is managed in one place.
3. Everyone starts in `contributors` (Write). Promotion to Maintain or Admin is
   done by an existing maintainer only, and only when there is a reason for it.

## Repositories

`eco-connectivity-workflow` is the umbrella: shared documentation, the demo, and
these rules. The five module repositories in the table above hold the actual
pipeline. All are public under the MIT licence.
