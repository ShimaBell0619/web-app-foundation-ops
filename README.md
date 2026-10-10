# Web App Foundation operations

Owner: `ShimaBell0619`. Private repository for owner-approved private repository creation.

## Activation gate

This repository currently cannot enforce main protection on the existing GitHub plan. Do not issue or save `REPO_CREATION_TOKEN`, merge the creation workflow, or execute real creation until main protection is enforced and the Owner has accepted the deployment choice. No public visibility or paid-plan change is authorized by this setup.

## Installation

The Foundation repository-create kit is copied to `.github/workflows/create-private-repo.yml` and `scripts/create-private-repo.mjs`. `.node-version` follows the Foundation baseline. The `repository-creation` Environment must allow only the `main` branch and no tags. Save the creation token only as its Environment secret after main protection is effective.

`ci.yml` runs a syntax check and API-mocked safety tests without secrets. The check to require after protection becomes available is `repository-creation-tests`; its live PR run must be verified before activation. The creation workflow reads only trusted default-branch code and revalidates the live Issue/comment with read-only `GITHUB_TOKEN` permissions. This repository has no application or compilation output: typecheck and build are excluded because its executable is plain JavaScript, checked directly and tested with Node.

## Chat operation

Only after the activation gate is satisfied: on an explicit Owner request, Chat creates an open Issue titled `repo-create: <safe-name>` and posts exactly `/create-private-repo` as the Owner. Names contain 3–40 lowercase letters/digits/hyphens, starting with a letter. The script accepts only Owner-created Issues and Owner comments, creates only private repositories, rejects existing names, and verifies owner/private/default_branch with a separate GET.

Chat must inspect the Actions run, created repository and initial branch before claiming success. Never retry an uncertain POST without checking GitHub state. Public creation, deletion, existing repository changes, and visibility changes are excluded. Test repository names must be presented to the Owner before a real run; deletion requires separate approval.

## Credentials

Use an expiring fine-grained PAT owned by ShimaBell0619, with `Repository creation: write` and automatic `Metadata: read`. `All repositories` permits metadata verification of existing and future private repositories; do not grant Administration, Contents or Issues to this PAT. The token can also create public repositories at the provider level; private-only behavior is enforced by protected code. Creation and Secret entry are performed by the Owner directly in GitHub. No token value is stored in source, tests, Issues, PRs, logs or screenshots.

## Source and validation

Baseline: [Foundation v0.12.1 main](https://github.com/ShimaBell0619/web-app-foundation/tree/84477e463d60d460fe8683921b7402e336149798). The kit and tests adopt the fixes merged in [Foundation PR #117](https://github.com/ShimaBell0619/web-app-foundation/pull/117), source commit `b97a0d93cb9f7cedf2c534f1478de95729d6090f`; the released v0.12.1 tag remains unchanged. Copied mock tests verify accepted/rejected requests, fresh authorization, identity, private-only POST, duplicate names, errors, post-verification, Environment binding and token separation. Live creation has not yet been verified.
