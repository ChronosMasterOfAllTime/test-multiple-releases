# test-multiple-releases
Test Multiple Release Drafter configurations

## Overview

This repository demonstrates how to use [release-drafter](https://github.com/release-drafter/release-drafter) with multiple release configurations for different branches.

## Setup

The repository is configured to manage separate releases for `v2` and `v3` branches:

- **v2 branch**: Uses `.github/release-drafter-v2.yml` configuration
  - Tags: `v2.x.x`
  - Release name template: `v2.$RESOLVED_VERSION 🌈`
  
- **v3 branch**: Uses `.github/release-drafter-v3.yml` configuration
  - Tags: `v3.x.x`
  - Release name template: `v3.$RESOLVED_VERSION 🚀`

## How it works

1. The GitHub Actions workflow (`.github/workflows/release-drafter.yml`) triggers on:
   - Pushes to `v2` or `v3` branches
   - Pull requests targeting these branches

2. The workflow automatically selects the appropriate configuration based on the target branch

3. Release drafts are created/updated automatically with:
   - Categorized changes (Features, Bug Fixes, Maintenance)
   - Version bumping based on PR labels
   - Contributor attribution

## Usage

### Creating releases for v2

1. Create a `v2` branch if it doesn't exist
2. Push changes or merge PRs to the `v2` branch
3. Release Drafter will create/update a draft release for v2

### Creating releases for v3

1. Create a `v3` branch if it doesn't exist
2. Push changes or merge PRs to the `v3` branch
3. Release Drafter will create/update a draft release for v3

## PR Labels

Use these labels on your PRs to control versioning:
- `major`: Bump major version (x.0.0)
- `minor`: Bump minor version (0.x.0)
- `patch`: Bump patch version (0.0.x) - default

Use these labels to categorize changes:
- `feature`, `enhancement`: Listed under Features
- `fix`, `bugfix`, `bug`: Listed under Bug Fixes
- `chore`, `maintenance`: Listed under Maintenance
