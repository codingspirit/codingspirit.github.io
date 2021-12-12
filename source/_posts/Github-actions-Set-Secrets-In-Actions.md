---
title: 'Github actions: Set Secrets In Actions'
top: false
tags:
  - GitHub
date: 2021-12-12 08:56:37
categories: CI/CD
---

If set `runs-on` as `ubuntu-latest`, user can use Github CLI tool `gh` to set secret in actions:

```yml
- name: Set ECR credentials
  id: set-ecr-credentials
  run: |
    gh auth login --with-token <<< ${{ secrets.PA_TOKEN }}
    gh secret set ECR_REGISTRY --body ${{ steps.login-ecr.outputs.registry }} --repo ${{ github.repository }}
    gh secret set ECR_USERNAME --body ${{ steps.login-ecr.outputs.username }} --repo ${{ github.repository }}
    gh secret set ECR_PASSWORD --body ${{ steps.login-ecr.outputs.password }} --repo ${{ github.repository }}
```
