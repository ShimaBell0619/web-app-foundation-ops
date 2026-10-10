# Web App Foundation operations

Owner: `ShimaBell0619`. Public management repository for owner-approved private repository creation. The Owner explicitly approved public visibility on 2026-10-10 to allow enforced main protection on the existing free plan. Issue names, management code and Actions logs are public; created repositories remain private.

## Activation gate

Before issuing or saving `REPO_CREATION_TOKEN`, merging the creation workflow or executing real creation, verify enforced main protection, final-head CI and independent review. Use active Ruleset `main-pr-ci`: PR required, 0 mandatory approvals, `repository-creation-tests` required from GitHub Actions with latest-base checks, deletion/force-push blocked and no bypass actors. Visibility changes and paid-plan changes require separate Owner approval; this installation authorizes neither future changes nor public repository creation.

## Installation

The Foundation repository-create kit is copied to `.github/workflows/create-private-repo.yml` and `scripts/create-private-repo.mjs`. `.node-version` follows the Foundation baseline. The `repository-creation` Environment must allow only the `main` branch and no tags. Save the creation token only as its Environment secret after main protection is effective.

`ci.yml` runs a syntax check and API-mocked safety tests without secrets. The required check is `repository-creation-tests`; its live PR run is verified before activation. The creation workflow reads only trusted default-branch code and revalidates the live Issue/comment with read-only `GITHUB_TOKEN` permissions (`contents: read`, `issues: read`). This repository has no application or compilation output: typecheck and build are excluded because its executable is plain JavaScript, checked directly and tested with Node.

## Chat operation

Only after the activation gate is satisfied: on an explicit Owner request, Chat creates an open Issue titled `repo-create: <safe-name>` and posts exactly `/create-private-repo` as the Owner. Names contain 3–40 lowercase letters/digits/hyphens, starting with a letter. The script accepts only Owner-created Issues and Owner comments, creates only private repositories, rejects existing names, and verifies owner/private/default_branch with a separate GET.

Chat must inspect the Actions run, created repository and initial branch before claiming success. Never retry an uncertain POST without checking GitHub state. Public creation, deletion, existing repository changes, and visibility changes are excluded. Test repository names must be presented to the Owner before a real run; deletion requires separate approval.

## Credentials

Use an expiring fine-grained PAT owned by ShimaBell0619, with `Repository creation: write` and automatic `Metadata: read`. `All repositories` permits metadata verification of existing and future private repositories; do not grant Administration, Contents or Issues to this PAT. The token can also create public repositories at the provider level; private-only behavior is enforced by protected code. Creation and Secret entry are performed by the Owner directly in GitHub. No token value is stored in source, tests, Issues, PRs, logs or screenshots.

## Source and validation

Baseline: [Foundation v0.12.1 main](https://github.com/ShimaBell0619/web-app-foundation/tree/84477e463d60d460fe8683921b7402e336149798). The kit and tests adopt the fixes merged in [Foundation PR #117](https://github.com/ShimaBell0619/web-app-foundation/pull/117), source commit `b97a0d93cb9f7cedf2c534f1478de95729d6090f`; the released v0.12.1 tag remains unchanged. Copied mock tests verify accepted/rejected requests, fresh authorization, identity, private-only POST, duplicate names, errors, post-verification, Environment binding and token separation.

## Verified installation — 2026-10-10

[Operations PR #1](https://github.com/ShimaBell0619/web-app-foundation-ops/pull/1) passed final-head CI and the separately Owner-authorized independent review on `33086dd63ffb5a27c07c86c3cd92d00aed34d92c`, then merged through the enforced Ruleset. [Post-merge CI](https://github.com/ShimaBell0619/web-app-foundation-ops/actions/runs/38034136373) succeeded. Automatic Codex review is off.

| Setting | Verified value |
| --- | --- |
| Main protection | Active [main-pr-ci Ruleset](https://github.com/ShimaBell0619/web-app-foundation-ops/rules/24832420), no bypass actors |
| Environment | `repository-creation`, only branch `main`, no tags, administrator bypass disabled |
| Secret | Environment-scoped `REPO_CREATION_TOKEN`; saved directly by the Owner |
| Token | Fine-grained PAT `foundation-private-repo-creation`, owner `ShimaBell0619`, expires **2026-11-09** |
| PAT permissions | Repository creation read/write; automatic Metadata read-only; all Owner repositories; no user permissions |

GitHub's [create-repository API](https://docs.github.com/en/rest/repos/repos?apiVersion=2026-03-10#create-a-repository-for-the-authenticated-user) supports `Repository creation: write`; Administration is not needed. The Owner must renew the expiring PAT and update the same Environment secret before expiry to continue creation. Token values must never be placed in requests or verification evidence.

Live tests used protected main `3fb36b5bd50ffcc6c19287e8af3f1274ed63e953` and Owner-authored [Issue #2](https://github.com/ShimaBell0619/web-app-foundation-ops/issues/2):

- [Creation run](https://github.com/ShimaBell0619/web-app-foundation-ops/actions/runs/38034799542) succeeded and independently fetched the created repository to verify owner, private visibility and default branch. The existing ChatGPT GitHub connection created the Issue/comment as the Owner and could subsequently read the new private repository.
- [Duplicate run](https://github.com/ShimaBell0619/web-app-foundation-ops/actions/runs/38034841269) deliberately failed with `Target repository already exists; no changes made`; repository identity and visibility remained unchanged.
- [Invalid-comment run](https://github.com/ShimaBell0619/web-app-foundation-ops/actions/runs/38034863310) skipped the creation job for `/create-private-repo please`.
- [Test repository](https://github.com/ShimaBell0619/foundation-repo-create-smoke-20261010): owner `ShimaBell0619` (ID `112917105`), private, repository ID `1412906145`, default branch `main`, initial commit `006419d8aed12a014122efccc60aa7537d105823`. It is retained; deletion requires separate Owner approval.

Non-Owner requests, malformed titles, edited/deleted/closed requests, identity mismatches, public or mismatched responses, and uncertain POST responses are covered by API-mocked CI tests, not claimed as live tests. The current Work/Codex GitHub connection exposes no direct repository-creation tool; repository creation is implemented through Actions.

The standard ChatGPT GitHub App is [documented as read-only](https://help.openai.com/en/articles/11145903-connecting-github-to-chatgpt). GitHub permissions and this workflow cannot add Issue/comment write tools to that Chat surface. This installation verifies Owner-authored Issue/comment-to-Actions execution through the existing Work/Codex connection; standard Chat-only autonomous creation is not verified or enabled. When Chat has no Issue/comment write action, the Owner can post the request manually, or explicitly request Work execution. Keep planning and instructions Chat-first and use Work only on an explicit request; do not silently switch surfaces or add another app/service.
