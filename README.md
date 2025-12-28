# GitHub Actions Hands-On Learning Guide

**15 Concepts | 15 Workflows | Learn by Doing**

Push this entire repository to GitHub and observe each workflow in action!

---

## 🚀 Quick Start

```bash
# 1. Initialize git (if not done)
git init

# 2. Add all files
git add .

# 3. Commit
git commit -m "Add GitHub Actions learning workflows"

# 4. Add your GitHub repo as remote
git remote add origin https://github.com/YOUR_USERNAME/github-actions-learn.git

# 5. Push to GitHub
git push -u origin main

# 6. Go to GitHub > Actions tab to see workflows running!
```

---

## 📚 CONCEPT 1: Workflow Triggers (push, issue, discussion)

### MCQ CONCEPT COVERED:
`on:`, `push:`, `issues:`, `discussion:`, event types, `github.event_name`

### WORKFLOW FILES:
- `.github/workflows/01-triggers-push.yml`
- `.github/workflows/01b-triggers-issue.yml`
- `.github/workflows/01c-triggers-discussion.yml`

### STEPS TO RUN:

**For Push Trigger:**
1. Push code to your repository
2. Go to **Actions** tab
3. Click on "01 - Push Trigger Demo"
4. See the workflow run automatically

**For Issue Trigger:**
1. Go to **Issues** tab
2. Click **New Issue**
3. Add any title and submit
4. Go to **Actions** tab
5. See "01b - Issue Trigger Demo" running

**For Discussion Trigger:**
1. Go to **Settings** > **General** > Enable **Discussions**
2. Go to **Discussions** tab
3. Create a new discussion
4. Go to **Actions** tab
5. See "01c - Discussion Trigger Demo" running

### WHAT TO OBSERVE IN UI:
- Actions tab shows workflow triggered by event
- Logs show `github.event_name` value
- Different events trigger different workflows

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "Which trigger runs on new issue?" → `issues: types: [opened]`
- MCQs ask "What is `github.event_name`?" → The name of the triggering event
- MCQs ask "Can one workflow have multiple triggers?" → YES, list them under `on:`

### COMMON MCQ TRAPS:
- ❌ `on: issue` → ✅ Correct: `on: issues` (plural!)
- ❌ `push` triggers on PR → ✅ `push` is for direct pushes, use `pull_request` for PRs

---

## 📚 CONCEPT 2: Debug, Warning, Error Logs

### MCQ CONCEPT COVERED:
`echo "::debug::"`, `echo "::warning::"`, `echo "::error::"`, `echo "::notice::"`, `ACTIONS_STEP_DEBUG`

### WORKFLOW FILE:
`.github/workflows/02-debug-logs.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Click on "02 - Debug Warning Error Logs"
4. Click on **show-log-levels** job
5. Expand each step to see logs

**To enable DEBUG logs:**
1. Go to repo **Settings** > **Secrets and variables** > **Actions**
2. Click **Variables** tab > **New repository variable**
3. Name: `ACTIONS_STEP_DEBUG`, Value: `true`
4. Re-run the workflow
5. Now debug messages are visible!

### WHAT TO OBSERVE IN UI:
- Normal logs: White text
- Warning: Yellow highlighted with ⚠️ icon
- Error: Red highlighted with ❌ icon
- Notice: Blue highlighted with ℹ️ icon
- Debug: HIDDEN unless `ACTIONS_STEP_DEBUG=true`

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to output debug message?" → `echo "::debug::message"`
- MCQs ask "How to enable debug logging?" → Set `ACTIONS_STEP_DEBUG` secret/variable to `true`
- MCQs ask "Does `::error::` fail the workflow?" → NO, it's just a log annotation

### COMMON MCQ TRAPS:
- ❌ `echo "::error::"` fails the step → ✅ It only adds annotation, use `exit 1` to fail
- ❌ Debug is always visible → ✅ Debug is hidden by default, needs `ACTIONS_STEP_DEBUG=true`

---

## 📚 CONCEPT 3: Environment Variables (GITHUB_ENV vs secrets)

### MCQ CONCEPT COVERED:
`env:` at workflow/job/step level, `$GITHUB_ENV`, `>> $GITHUB_ENV`, variable scope

### WORKFLOW FILE:
`.github/workflows/03-environment-variables.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Click on "03 - Environment Variables Demo"
4. Expand each step in the logs
5. Observe which variables are available where

### WHAT TO OBSERVE IN UI:
- Step 1 shows all three levels (workflow, job, step vars)
- Step 2 sets a variable via `GITHUB_ENV`
- Step 3 reads the variable from previous step
- Notice: `STEP_VAR` only exists in step 1!

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to pass variable between steps?" → `echo "VAR=value" >> $GITHUB_ENV`
- MCQs ask "Where is env: defined?" → workflow level, job level, or step level
- MCQs ask "Which has narrowest scope?" → Step-level env

### COMMON MCQ TRAPS:
- ❌ `export VAR=value` passes to next step → ✅ Use `$GITHUB_ENV` instead
- ❌ Step-level env is available in next step → ✅ It's only in THAT step
- ❌ `${{ env.VAR }}` and `$VAR` are same → ✅ Context syntax vs shell syntax

---

## 📚 CONCEPT 4: Secrets Usage

### MCQ CONCEPT COVERED:
`secrets` context, `secrets.GITHUB_TOKEN`, masking, why not use CLI for secrets

### WORKFLOW FILE:
`.github/workflows/04-secrets-usage.yml`

### STEPS TO RUN:
1. Go to repo **Settings** > **Secrets and variables** > **Actions**
2. Click **New repository secret**
3. Name: `MY_API_KEY`, Value: `super-secret-123`
4. Push the workflow
5. Go to **Actions** tab and run the workflow
6. Check logs - secret will show as `***`

### WHAT TO OBSERVE IN UI:
- The actual secret value is NEVER shown in logs
- GitHub automatically replaces it with `***`
- `GITHUB_TOKEN` is auto-provided (no need to create it)

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to access secrets?" → `${{ secrets.SECRET_NAME }}`
- MCQs ask "What is GITHUB_TOKEN?" → Auto-provided token for GitHub API access
- MCQs ask "Why not use CLI to print secrets?" → They are masked, debugging is hard

### COMMON MCQ TRAPS:
- ❌ Secrets can be printed in logs → ✅ They are ALWAYS masked with `***`
- ❌ You must create GITHUB_TOKEN → ✅ It's auto-provided by GitHub
- ❌ Secrets are available in forked PRs → ✅ NO! Security measure

---

## 📚 CONCEPT 5: Commit SHA Pinning for Actions

### MCQ CONCEPT COVERED:
SHA pinning, `uses: action@sha`, version tags vs SHA, supply chain security

### WORKFLOW FILE:
`.github/workflows/05-sha-pinning.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Run "05 - SHA Pinning Demo"
4. Both checkout steps work the same way
5. But SHA version is MORE SECURE!

**How to find SHA for any action:**
1. Go to action's GitHub repo (e.g., github.com/actions/checkout)
2. Click **Releases**
3. Find the version you want
4. Click the commit SHA next to it
5. Copy the full 40-character SHA

### WHAT TO OBSERVE IN UI:
- Both methods work identically
- No visible difference in logs
- The difference is SECURITY, not functionality

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "Most secure way to pin action?" → Full commit SHA
- MCQs ask "Why not use tags?" → Tags can be moved/changed by maintainer
- MCQs ask "What is SHA pinning?" → Using immutable commit hash instead of tag

### COMMON MCQ TRAPS:
- ❌ `@v4` is secure enough → ✅ Tags can be reassigned, SHA cannot
- ❌ SHA and tag point to same code forever → ✅ Tags can be moved
- ❌ Short SHA (7 chars) is fine → ✅ Use full 40-character SHA

---

## 📚 CONCEPT 6: action.yml Basics (inputs, outputs, runs)

### MCQ CONCEPT COVERED:
`action.yml`, `inputs`, `outputs`, `runs`, `with:`, `steps.<id>.outputs`

### WORKFLOW FILE:
`.github/workflows/06-action-yml-basics.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Run "06 - Action Inputs and Outputs"
4. Click on the job
5. See "hello" step output and how next step reads it

### WHAT TO OBSERVE IN UI:
- First step uses `with:` to pass input `who-to-greet`
- First step has `id: hello` which is needed for outputs
- Second step reads `${{ steps.hello.outputs.time }}`

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to pass input to action?" → Use `with:` keyword
- MCQs ask "How to read action output?" → `${{ steps.<step-id>.outputs.<output-name> }}`
- MCQs ask "What does action.yml contain?" → inputs, outputs, runs (using, main/image)

### COMMON MCQ TRAPS:
- ❌ Outputs work without step `id` → ✅ You MUST have `id:` to reference outputs
- ❌ `inputs` and `with` are same thing → ✅ `inputs` is in action.yml, `with` is in workflow
- ❌ All actions have outputs → ✅ Outputs are optional, check action docs

---

## 📚 CONCEPT 7: Job Dependencies (needs keyword)

### MCQ CONCEPT COVERED:
`needs:`, job execution order, parallel vs sequential jobs, dependent jobs

### WORKFLOW FILE:
`.github/workflows/07-job-dependencies.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Run "07 - Job Dependencies Demo"
4. Watch the **workflow visualization graph**
5. See jobs run in order: build → test → deploy

### WHAT TO OBSERVE IN UI:
- GitHub shows a visual graph of job dependencies
- `build` job runs first (no needs)
- `test` job waits for `build` (has `needs: build`)
- `deploy` job waits for both (has `needs: [build, test]`)
- Click each job to see timestamps proving order

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to make job B wait for job A?" → `needs: A`
- MCQs ask "Default job behavior?" → Jobs run in PARALLEL by default
- MCQs ask "How to wait for multiple jobs?" → `needs: [job1, job2]`

### COMMON MCQ TRAPS:
- ❌ Jobs run sequentially by default → ✅ They run in PARALLEL, use `needs` for sequence
- ❌ `needs` accepts only one job → ✅ Can be array: `needs: [job1, job2]`
- ❌ Failed job still allows dependent jobs → ✅ Dependent jobs are SKIPPED if needed job fails

---

## 📚 CONCEPT 8: Artifacts Upload + Retention

### MCQ CONCEPT COVERED:
`actions/upload-artifact`, `actions/download-artifact`, `retention-days`, sharing files between jobs

### WORKFLOW FILE:
`.github/workflows/08-artifacts.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Run "08 - Artifacts Demo"
4. Wait for workflow to complete
5. Scroll down to **Artifacts** section
6. Click **my-build-artifact** to download!

### WHAT TO OBSERVE IN UI:
- After workflow completes, scroll to bottom
- **Artifacts** section shows uploaded files
- Click to download the artifact ZIP
- Job 2 logs show it read the file from Job 1

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to share files between jobs?" → Use artifacts (upload in job1, download in job2)
- MCQs ask "Default retention period?" → 90 days (can customize with `retention-days`)
- MCQs ask "Where to find artifacts?" → Actions tab > workflow run > Artifacts section

### COMMON MCQ TRAPS:
- ❌ Files persist between jobs automatically → ✅ Each job has fresh environment, use artifacts
- ❌ Artifacts are kept forever → ✅ Default 90 days, max depends on plan
- ❌ `path` in download must match upload exactly → ✅ Use `name` to match, path is where to save

---

## 📚 CONCEPT 9: Cache Dependencies

### MCQ CONCEPT COVERED:
`actions/cache`, cache `key`, cache `path`, `cache-hit` output, hash functions

### WORKFLOW FILE:
`.github/workflows/09-cache.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Run "09 - Cache Demo" - **FIRST RUN**
4. Check logs - "Cache miss!" message appears
5. Run workflow **AGAIN** (click "Re-run all jobs")
6. Check logs - "Cache hit!" this time, faster!

### WHAT TO OBSERVE IN UI:
- **First run**: "Cache miss" → installs dependencies (slower)
- **Second run**: "Cache hit" → skips installation (faster!)
- Cache step shows "Cache restored from key: my-cache-xxx"
- Check **Caches** in repo sidebar under Actions

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to cache npm/pip packages?" → Use `actions/cache` with appropriate path
- MCQs ask "What is cache key?" → Unique identifier; if file changes, key changes, cache invalidates
- MCQs ask "How to check if cache hit?" → `steps.<id>.outputs.cache-hit`

### COMMON MCQ TRAPS:
- ❌ Cache and artifacts are same → ✅ Cache is for SPEEDING UP, artifacts for SHARING/STORING
- ❌ Cache key should be static → ✅ Use `hashFiles()` so cache invalidates on dependency change
- ❌ Cache persists across repos → ✅ Cache is per-repository

---

## 📚 CONCEPT 10: Executable Scripts (chmod)

### MCQ CONCEPT COVERED:
`chmod +x`, file permissions, running shell scripts, `continue-on-error`

### WORKFLOW FILE:
`.github/workflows/10-executable-scripts.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Run "10 - Executable Scripts Demo"
4. Expand each step in the logs
5. See the "permission denied" error, then success after chmod

### WHAT TO OBSERVE IN UI:
- Step "Try to run without chmod" FAILS with "Permission denied"
- But workflow continues because of `continue-on-error: true`
- Step "Make script executable" runs `chmod +x`
- Step "Run the executable script" NOW WORKS!

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to run a script file?" → First `chmod +x script.sh`, then `./script.sh`
- MCQs ask "What does chmod +x do?" → Adds execute permission
- MCQs ask "Script fails with permission denied?" → Missing chmod +x

### COMMON MCQ TRAPS:
- ❌ Scripts from repo are auto-executable → ✅ Git preserves permissions, but verify with chmod
- ❌ `bash script.sh` needs chmod → ✅ `bash script.sh` works without chmod, `./script.sh` needs it
- ❌ Windows uses chmod → ✅ chmod is Linux/macOS; Windows uses different permissions

---

## 📚 CONCEPT 11: Docker Container Action Basics

### MCQ CONCEPT COVERED:
`uses: docker://image`, Docker-based actions, `action.yml` with `using: docker`, container isolation

### WORKFLOW FILE:
`.github/workflows/11-docker-action.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Run "11 - Docker Action Demo"
4. Watch Docker images being pulled
5. See commands running inside containers

### WHAT TO OBSERVE IN UI:
- "Run Docker-based action" pulls and runs a Docker action
- "Run in Alpine container" shows `docker://alpine:3.18` being used
- "Run Python in container" runs Python without installing it on runner
- Logs show container startup time (slower than native)

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to run action in Docker?" → `uses: docker://image:tag`
- MCQs ask "Docker action.yml has what?" → `runs: using: 'docker'`
- MCQs ask "Docker vs JS action?" → Docker is isolated but slower; JS is faster

### COMMON MCQ TRAPS:
- ❌ Docker actions run on Windows runners → ✅ Docker actions need Linux runners
- ❌ `uses: docker://` requires Dockerfile → ✅ It pulls pre-built image from Docker Hub
- ❌ Docker actions are faster → ✅ JS actions are faster (no container overhead)

---

## 📚 CONCEPT 12: JavaScript Action Basics

### MCQ CONCEPT COVERED:
`using: node20`, `@actions/core`, `@actions/github`, `actions/github-script`, JavaScript actions

### WORKFLOW FILE:
`.github/workflows/12-javascript-action.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Run "12 - JavaScript Action Demo"
4. See checkout action (JS-based) run
5. See github-script run custom JavaScript code

### WHAT TO OBSERVE IN UI:
- `actions/checkout@v4` runs fast (no Docker pull)
- `actions/github-script` runs JavaScript inline
- Logs show repo info fetched via GitHub API
- JS actions start immediately (no container startup)

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How does JS action.yml look?" → `runs: using: 'node20', main: 'index.js'`
- MCQs ask "How to run inline JavaScript?" → Use `actions/github-script`
- MCQs ask "JS vs Docker action?" → JS faster, Docker more isolated

### COMMON MCQ TRAPS:
- ❌ JS actions need Node installed manually → ✅ Runner has Node pre-installed
- ❌ JS actions can only use GitHub API → ✅ They can run any Node.js code
- ❌ `using: node16` is same as `using: node20` → ✅ Different Node versions!

---

## 📚 CONCEPT 13: Environment Approvals (Manual Review)

### MCQ CONCEPT COVERED:
`environment:`, protection rules, required reviewers, deployment gates, `environment.url`

### WORKFLOW FILE:
`.github/workflows/13-environment-approvals.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to repo **Settings** > **Environments**
3. Click **New environment** > Name: `production` > **Configure environment**
4. Check ✅ **Required reviewers**
5. Add yourself as a reviewer
6. Click **Save protection rules**
7. Go to **Actions** tab
8. Run "13 - Environment Approvals Demo"
9. Watch it PAUSE at production job!
10. Click **Review deployments** > Approve > **Approve and deploy**

### WHAT TO OBSERVE IN UI:
- `build` and `deploy-staging` run automatically
- `deploy-production` shows "Waiting for review"
- Yellow banner: "Review pending"
- You must click approve for it to continue
- After approval, job runs and shows green check

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to require approval before deploy?" → Use `environment:` with protection rules
- MCQs ask "Where to configure approvers?" → Settings > Environments > Required reviewers
- MCQs ask "What pauses workflow for approval?" → Environment protection rules

### COMMON MCQ TRAPS:
- ❌ `environment:` alone requires approval → ✅ You must CONFIGURE protection rules in Settings
- ❌ Approval is per-workflow → ✅ Approval is per-ENVIRONMENT
- ❌ Anyone can approve → ✅ Only designated reviewers can approve

---

## 📚 CONCEPT 14: Runner Types (ubuntu, windows, macos)

### MCQ CONCEPT COVERED:
`runs-on:`, `ubuntu-latest`, `windows-latest`, `macos-latest`, runner labels, hosted vs self-hosted

### WORKFLOW FILE:
`.github/workflows/14-runner-types.yml`

### STEPS TO RUN:
1. Push the workflow to GitHub
2. Go to **Actions** tab
3. Run "14 - Runner Types Demo"
4. Watch ALL THREE jobs run IN PARALLEL
5. Click each job to see OS-specific output

### WHAT TO OBSERVE IN UI:
- Three jobs run simultaneously (ubuntu, windows, macos)
- Each shows different OS information
- Ubuntu uses bash, Windows uses PowerShell
- macOS shows macOS version
- Note: Ubuntu finishes fastest, macOS slowest

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "How to specify runner OS?" → `runs-on: ubuntu-latest`
- MCQs ask "Default shell on Windows?" → PowerShell
- MCQs ask "Which runner is cheapest?" → Ubuntu (Linux)

### COMMON MCQ TRAPS:
- ❌ All runners cost the same → ✅ Linux < Windows < macOS in cost
- ❌ `runs-on: linux` → ✅ Correct: `runs-on: ubuntu-latest`
- ❌ macOS runner uses bash by default → ✅ Recent macOS uses zsh, but GitHub defaults to bash

---

## 📚 CONCEPT 15: Workflow Status Visibility

### MCQ CONCEPT COVERED:
Actions tab, commit status checks, PR checks, branch protection, status badges

### WORKFLOW FILE:
`.github/workflows/15-status-visibility.yml`

### STEPS TO RUN:

**See status in Actions tab:**
1. Push workflow to GitHub
2. Go to **Actions** tab
3. See workflow run with all jobs

**See status on commits:**
1. Go to **Code** tab
2. Look at commit list
3. See ✅ or ❌ next to each commit
4. Click the icon for details

**See status on Pull Requests:**
1. Create a new branch: `git checkout -b test-pr`
2. Make any small change, commit, push
3. Open a Pull Request
4. See **Checks** tab on PR page
5. All jobs show as individual checks

**Configure branch protection:**
1. Go to **Settings** > **Branches**
2. Click **Add rule** for `main`
3. Check ✅ **Require status checks to pass**
4. Select which jobs must pass
5. Now PRs cannot merge until checks pass!

### WHAT TO OBSERVE IN UI:
- **Actions tab**: Full workflow run details
- **Commit**: Small icon (✅/❌/🟡) next to commit message
- **PR page**: Checks tab shows each job status
- **Branch protection**: Blocks merge if checks fail

### WHY THIS MATTERS FOR MCQs:
- MCQs ask "Where to see workflow status?" → Actions tab, commit status, PR checks
- MCQs ask "How to require checks before merge?" → Branch protection rules
- MCQs ask "What does yellow dot mean?" → Workflow is still running

### COMMON MCQ TRAPS:
- ❌ All workflows appear as PR checks → ✅ Only workflows triggered by `pull_request` event
- ❌ Status checks are auto-required → ✅ You must configure branch protection
- ❌ Failed workflow blocks all commits → ✅ Only blocks if branch protection is configured

---

## 🎯 MCQ Quick Reference Cheat Sheet

| Concept | Key Syntax | Common MCQ Trap |
|---------|-----------|-----------------|
| Push trigger | `on: push: branches: [main]` | `push` ≠ `pull_request` |
| Debug logs | `echo "::debug::msg"` | Hidden by default |
| Env vars | `>> $GITHUB_ENV` | Not `export` |
| Secrets | `${{ secrets.NAME }}` | Always masked |
| SHA pinning | `uses: action@SHA` | SHA ≠ tag |
| Action outputs | `steps.<id>.outputs.name` | Needs `id:` |
| Job depends | `needs: [job1, job2]` | Default is parallel |
| Artifacts | `upload-artifact` / `download-artifact` | Different from cache |
| Cache | `actions/cache` with key | Key includes hash |
| Scripts | `chmod +x` then `./script.sh` | `bash script.sh` works without chmod |
| Docker action | `uses: docker://image` | Linux runners only |
| JS action | `using: node20` | Faster than Docker |
| Approvals | `environment:` + protection rules | Must configure in Settings |
| Runners | `runs-on: ubuntu-latest` | Linux is cheapest |
| Status | Actions tab, PR checks, commit status | Configure branch protection |

---

## 📁 Files in This Repository

```
.github/
└── workflows/
    ├── 01-triggers-push.yml
    ├── 01b-triggers-issue.yml
    ├── 01c-triggers-discussion.yml
    ├── 02-debug-logs.yml
    ├── 03-environment-variables.yml
    ├── 04-secrets-usage.yml
    ├── 05-sha-pinning.yml
    ├── 06-action-yml-basics.yml
    ├── 07-job-dependencies.yml
    ├── 08-artifacts.yml
    ├── 09-cache.yml
    ├── 10-executable-scripts.yml
    ├── 11-docker-action.yml
    ├── 12-javascript-action.yml
    ├── 13-environment-approvals.yml
    ├── 14-runner-types.yml
    └── 15-status-visibility.yml
```

---

**Happy Learning! 🚀**
