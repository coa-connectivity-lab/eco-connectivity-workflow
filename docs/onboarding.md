# Onboarding

Two parts: a one-time setup the org owner runs to grant access and protect the
repositories, and a walk-through for a new collaborator making a first change.

---

## Part 1 — Access and branch protection (org owner, one time)

Run these from a shell with the [GitHub CLI](https://cli.github.com/) authenticated
as an owner of the `coa-connectivity-lab` organisation. Read the notes under the
runbook before the write calls.

### 0. See what already exists

```bash
gh repo list coa-connectivity-lab --limit 100
gh api orgs/coa-connectivity-lab/teams --jq '.[].slug'
```

### 1. Create any missing module repositories

```bash
for r in db-schema ml-models qgis-projects reproducibility-logs; do
  gh repo create coa-connectivity-lab/$r --public \
    --description "Côa connectivity pipeline module: $r" || true
done
```

### 2. Make the umbrella and module repositories public

```bash
for r in eco-connectivity-workflow data-management db-schema ml-models \
         qgis-projects reproducibility-logs; do
  gh repo edit coa-connectivity-lab/$r --visibility public \
    --accept-visibility-change-consequences
done
```

### 3. Create the teams

GitHub permission levels, plainly:

| Level | What it allows |
| --- | --- |
| Read | Clone, open issues, comment |
| Triage | Manage issues and pull requests, no code rights |
| Write | Push branches, open pull requests. The default for collaborators |
| Maintain | Manage most repo settings, not destructive ones |
| Admin | Everything, including deletion. Keep this to two people |

```bash
gh api -X POST orgs/coa-connectivity-lab/teams -f name='maintainers'  -f privacy='closed'
gh api -X POST orgs/coa-connectivity-lab/teams -f name='contributors' -f privacy='closed'
gh api -X POST orgs/coa-connectivity-lab/teams -f name='reviewers'    -f privacy='closed'
```

### 4. Give each team its role on each repository

```bash
for r in eco-connectivity-workflow data-management db-schema ml-models \
         qgis-projects reproducibility-logs; do
  gh api -X PUT orgs/coa-connectivity-lab/teams/maintainers/repos/coa-connectivity-lab/$r  -f permission=maintain
  gh api -X PUT orgs/coa-connectivity-lab/teams/contributors/repos/coa-connectivity-lab/$r -f permission=push
  gh api -X PUT orgs/coa-connectivity-lab/teams/reviewers/repos/coa-connectivity-lab/$r    -f permission=triage
done
```

### 5. Add people to a team

Add each collaborator to `contributors` to start. Promote to `maintainers` only
with a reason.

```bash
gh api -X PUT orgs/coa-connectivity-lab/teams/contributors/memberships/GITHUB_HANDLE -f role=member
```

Someone who is not an org member and only needs one repository can instead be added
as an outside collaborator at Write:

```bash
gh api -X PUT repos/coa-connectivity-lab/REPO/collaborators/GITHUB_HANDLE -f permission=push
```

### 6. Protect `main` on every repository

Require a reviewed pull request, one approval, a CODEOWNERS review, a linear
history, resolved conversations, and no direct or force pushes.

```bash
for r in eco-connectivity-workflow data-management db-schema ml-models \
         qgis-projects reproducibility-logs; do
  gh api -X PUT repos/coa-connectivity-lab/$r/branches/main/protection \
    -H "Accept: application/vnd.github+json" \
    -f "required_pull_request_reviews[required_approving_review_count]=1" \
    -F "required_pull_request_reviews[require_code_owner_reviews]=true" \
    -F "enforce_admins=true" \
    -F "required_linear_history=true" \
    -F "allow_force_pushes=false" \
    -F "allow_deletions=false" \
    -F "required_conversation_resolution=true" \
    -F "restrictions=" -F "required_status_checks="
done
```

Once `ci.yml` has run once on a pull request, add its check as required, in the web
UI (Settings, then Branches) or by adding
`-F "required_status_checks[contexts][]=ci"` to the call above.

### Notes

- Prefer adding people to a **team** over per-repo invites, so access lives in one
  place.
- `enforce_admins=true` means the rules apply to maintainers too, which is the
  point of "nobody commits to `main`".
- The exact JSON shape of the branch-protection endpoint changes from time to time.
  If the one-liner errors, set the rules once in the web UI for one repository and
  copy them to the others there.

---

## Part 2 — First change (new collaborator)

### Set up locally

```bash
git clone git@github.com:coa-connectivity-lab/eco-connectivity-workflow.git
cd eco-connectivity-workflow
conda env create -f environment.yml
conda activate coa-connectivity
```

A local PostGIS instance is enough for development. The quickest route is Docker:

```bash
docker run --name coa-postgis -e POSTGRES_PASSWORD=coa \
  -p 5432:5432 -d postgis/postgis:16-3.4
```

### Make the change

```bash
git switch -c data/add-elevation-source origin/main   # never work on main

# ... edit files ...

git add -p
git commit -s -m "Add SRTM elevation acquisition step"
```

### Rebase and push

```bash
git switch main && git pull --ff-only
git switch data/add-elevation-source && git rebase main
git push -u origin data/add-elevation-source
```

### Open the pull request

```bash
gh pr create --fill
```

Then complete the template:

- **What and why**, and **how this was tested**.
- The **AI assistance** block. If you used Claude for any part that shaped the
  result, list the files, say what the prompt or approach was, and say what you
  checked and how. See [`CONTRIBUTING.md`](../CONTRIBUTING.md) section 6.
- Request review from the module owner (`CODEOWNERS` does this automatically) and
  from a maintainer.

### What to expect

- A direct push to `main` is rejected by design.
- The pull request merges with "Rebase and merge" once it has an approval and the
  checks pass. This keeps your signed commits intact.
