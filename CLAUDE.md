# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this is

A scaffold Terraform playground. `main.tf` currently contains only a stub, unconfigured `provider` block — there is no real infrastructure defined yet.

## Commands

- `terraform init` — initialize providers/modules (run first, and after adding a provider)
- `terraform fmt` — format `.tf` files
- `terraform validate` — check config syntax/internal consistency
- `terraform plan` — preview changes
- `terraform apply` — apply changes
- `terraform plan -out=tfplan` then `terraform apply tfplan` — for a saved plan

There is no build, lint, or test tooling configured beyond the above.

## CI/CD

`.github/workflows/terraform-deploy.yml` runs `terraform fmt/init/validate/plan/apply` against Azure on every push to `main` (direct pushes and PR merges both trigger it, since merging produces a push to `main`). Auth is via Azure OIDC federated credentials (`azure/login`, no client secret), using repo secrets:
- `AZURE_APP_ID` → client/app ID
- `AZURE_DIR_ID` → tenant ID
- `AZURE_SUB_ID` → subscription ID
- `AZURE_OBJ_ID` exists as a secret but is unused by this workflow.

There is no plan-on-PR gate yet — `main.tf` is still a stub, so nothing actually applies until the provider block is filled in.

## Notes

- `*.tfvars`, `*.tfstate`, `.terraform/`, and override files are gitignored (see `.gitignore`) since they carry sensitive/environment-specific or local state.
