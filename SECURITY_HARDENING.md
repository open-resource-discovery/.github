# Repository security hardening for maintainers

Use this checklist for maintained repositories and revisit it when adding automation or publishing access.
Repository files and live GitHub settings are separate controls.

| Area | Required review |
| --- | --- |
| Ownership | Maintain an inventory of owned repositories and related publishing assets, with an accountable maintainer for each. Archive unused repositories and disable unnecessary Actions. |
| Protected changes | Protect default/release branches and release tags, require independent review of CI/install configuration, dismiss stale approvals, and avoid administrator bypass. Align required check names with actual jobs. |
| Code owners | Assign CI, Action manifests, package manifests, lockfiles, and install-policy files to maintainers. Require code-owner review in branch protection or effective rulesets. |
| Workflow permissions | Prefer repository read defaults and restrictive workflow permissions. Grant specific write and OIDC permissions only to the jobs that need them. Review intentional bot-approval workflows separately. |
| External contributions | Require approval for all external contributors. Keep PR validation free of publishing credentials, privileged tokens, and persistent/shared runner access. |
| Action dependencies | Review and pin remote Actions and reusable workflows to full SHAs, including nested references. Configure a reviewed allowlist and SHA enforcement where supported. |
| Publishing | Restrict protected environments to approved branches/tags and reviewers. Match OIDC trust to the exact repository and protected branch/environment, and verify provenance during a real authorized release. |
| Secrets | Enable secret scanning and push protection where supported. Inventory repository, environment, organization, and provider credentials. Verify issuer revocation/rotation for genuinely exposed credentials and remove unnecessary secrets. |
| Dependency installation | Commit lockfiles and use frozen installs. Apply a two-day age gate with a supported package manager, deny unreviewed dependency lifecycle scripts, and record narrow package/version decisions. |
| Tool downloads | Declare and lock tools used by builds and packaging. Avoid implicit npx downloads; use local executables and prevent fallback installation. |
| Vulnerability handling | Review current alerts, including build/test dependencies. Prefer compatible fixes, assess exceptional overrides, verify resulting lockfiles and behavior, and confirm alerts close after merging. |
| Runner isolation | Prefer disposable GitHub-hosted runners for untrusted PRs. Verify organization runner-group access, isolation, egress, and credential access separately from workflow YAML. |

For npm, use a supported development client with min-release-age=2 and an explicit strict allowScripts policy, or disable dependency scripts when the build permits it.
For pnpm, the equivalent age threshold is minimumReleaseAge=2880 minutes.
A configuration file alone does not establish enforcement by an unsupported client.
Avoid broad package-scope exceptions and review the exact script and registry identity before granting an exception.
Classify scanner matches before taking containment action; identifiers and documentation placeholders are not automatically credentials.

References:

- [GitHub Actions secure use](https://docs.github.com/en/actions/reference/security/secure-use).
- [Workflow permissions](https://docs.github.com/en/actions/reference/workflows-and-actions/workflow-syntax#permissions).
- [Code owners and required reviews](https://docs.github.com/en/repositories/managing-your-repositorys-settings-and-features/customizing-your-repository/about-code-owners).
- [Secret scanning](https://docs.github.com/en/code-security/secret-scanning/introduction/about-secret-scanning).
- [npm configuration](https://docs.npmjs.com/cli/v11/using-npm/config).
