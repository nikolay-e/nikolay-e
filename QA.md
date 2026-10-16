# QA — nikolay-e

GitHub profile README repo. No runtime surface — QA = lint + dead-link check.
The published surface is the GitHub profile render, and publish = a push
reaching the `github` remote. A local `main` ahead of `github/main` is an
unpublished change — report it, don't wave it off.

Generic patterns live in the `/qa` skill — do not duplicate here.

## Applicability matrix

| Check         | Applies | Why                                         |
| ------------- | ------- | ------------------------------------------- |
| CI            | ❌      | No workflows — nothing to build             |
| CD / ArgoCD   | ❌      | No cluster deploy — publish = `github` push |
| K8s logs      | ❌      | No cluster surface                          |
| Browser QA    | ❌      | No app — GitHub renders the README          |
| Backend smoke | ❌      | No service                                  |
| Tests         | ❌      | No code                                     |
| Diff-context  | ❌      | Single README; diff is trivially reviewable |
| autoqa        | ❌      | No public URL / HTTP surface of our own     |
| Schemathesis  | ❌      | No OpenAPI                                  |
| ZAP           | ❌      | No app surface                              |
| SonarCloud    | ❌      | No code to analyze                          |
| Chat-review   | ❌      | No model/bot-authored communication         |
| Walkthrough   | ❌      | No UI                                       |
| Markdown lint | ✅      | README correctness                          |
| Dead-link     | ✅      | Every outbound link must 200 (derived live) |
| Prettier      | ✅      | Markdown formatting                         |

The ❌ rows hold only while the repo is Markdown-only — re-derive the shape
each pass (`git ls-files`; a workflow file, code, or a deploy manifest flips
its row) rather than trusting this table.

## Coordinates

- Public URL: none (rendered as the profile README on the `nikolay-e` GitHub profile)
- Namespace: none
- ArgoCD app: none
- OpenAPI path: none
- Sonar key: not registered
- Keychain token entry: none required

## The applicable QA run

```bash
cd ~/nikolay-e
npx prettier --check README.md
npx markdownlint-cli README.md
# dead-link check on the outbound links
grep -oE 'https?://[^ )]+' README.md | while read u; do
  curl -sS -o /dev/null -w "%{http_code} $u\n" -L --max-time 10 "$u"
done
# published-state check — the GitHub render is the publish; local main
# ahead of github/main = unpublished changes.
git fetch github
git rev-parse main github/main
```
