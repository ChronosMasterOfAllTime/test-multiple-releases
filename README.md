# test-multiple-releases
Test Multiple Release Drafter configurations

## Overview

This repository demonstrates how to use [release-drafter](https://github.com/release-drafter/release-drafter) with multiple release configurations for different branches.

## Setup

The repository is configured to manage separate releases for `v2` and `v3` branches:

- **v2 branch**: Uses `.github/release-drafter-v2.yml` configuration
  - Tags: `v2.x.x`
  - Release name template: `v2.$RESOLVED_VERSION 🌈`
  - `commitish: v2` targets the v2 branch for releases
  - `filter-by-commitish: true` ensures v2 releases don't interfere with v3
  
- **v3 branch**: Uses `.github/release-drafter-v3.yml` configuration
  - Tags: `v3.x.x`
  - Release name template: `v3.$RESOLVED_VERSION 🚀`
  - `commitish: v3` targets the v3 branch for releases
  - `filter-by-commitish: true` ensures v3 releases don't interfere with v2

## How it works

1. The GitHub Actions workflow (`.github/workflows/release-drafter.yml`) triggers on:
   - Pushes to `v2` or `v3` branches
   - Pull requests targeting these branches
   - Manual trigger via workflow_dispatch (from GitHub Actions UI)

2. The workflow automatically selects the appropriate configuration based on the target branch

3. Release drafts are created/updated automatically with:
   - Categorized changes (Features, Bug Fixes, Maintenance)
   - Version bumping based on PR labels
   - Contributor attribution

**Important**: The `filter-by-commitish: true` and `commitish` parameters in both v2 and v3 configurations work together to ensure proper branch isolation:
- `commitish: v2` / `commitish: v3` specifies which branch each release targets
- `filter-by-commitish: true` filters releases to only those from the same branch

This prevents the v2 and v3 draft releases from overwriting each other when workflows are triggered on different branches.

## Usage

### Manual Trigger

You can manually trigger the Release Drafter workflow from the GitHub Actions UI:

1. Go to the "Actions" tab in the GitHub repository
2. Select "Release Drafter" from the workflows list
3. Click "Run workflow" button
4. Select the branch you want to run it on (v2, v3, or any other branch)
5. Click "Run workflow" to execute

The workflow will use the appropriate configuration based on the selected branch.

### Creating releases for v2

1. Create a `v2` branch if it doesn't exist
2. Push changes or merge PRs to the `v2` branch
3. Release Drafter will create/update a draft release for v2

### Creating releases for v3

1. Create a `v3` branch if it doesn't exist
2. Push changes or merge PRs to the `v3` branch
3. Release Drafter will create/update a draft release for v3

## PR Labels

### For v2 and v3 branches

Use these labels on your PRs to control versioning:
- `minor`: Bump minor version (v2/v3.x.0)
- `patch`: Bump patch version (v2/v3.0.x) - default

Note: Major version bumping is not available for v2/v3 branches as the major version is fixed by the branch name.

### For other branches

Use these labels on your PRs to control versioning:
- `major`: Bump major version (x.0.0)
- `minor`: Bump minor version (0.x.0)
- `patch`: Bump patch version (0.0.x) - default

### For all branches

Use these labels to categorize changes:
- `feature`, `enhancement`: Listed under Features
- `fix`, `bugfix`, `bug`: Listed under Bug Fixes
- `chore`, `maintenance`: Listed under Maintenance
