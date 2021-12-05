---
title: 'GitHub actions: Delete All Workflow Runs'
top: false
tags:
  - GitHub
date: 2021-12-05 13:31:51
categories: CI/CD
---

Using following command to delete all workflow runs on GitHub actions of a specific branch:

```bash
user=GH_USERNAME repo=REPO_NAME; gh api repos/$user/$repo/actions/runs \--paginate -q '.workflow_runs[] | select(.head_branch == "master") | "\(.id)"' | \xargs -n1 -I % gh api repos/$user/$repo/actions/runs/% -X DELETE
```

Replace `GH_USERNAME` and `REPO_NAME` with your parameters.

> You need to install GitHub CLI tool `gh` first.
