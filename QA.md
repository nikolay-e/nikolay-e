# QA — nikolay-e

GitHub profile README repo. Not deployed — QA = lint + dead-link check.

Generic patterns live in the `/qa` skill — do not duplicate here.

## Applicability matrix

| Check         | Applies | Why                                                |
| ------------- | ------- | -------------------------------------------------- |
| CI            | ❌      | No workflows — nothing to build                    |
| CD / ArgoCD   | ❌      | Not deployed                                       |
| K8s logs      | ❌      | No cluster surface                                 |
| Browser QA    | ❌      | No app — GitHub renders the README                 |
| Backend smoke | ❌      | No service                                         |
| Tests         | ❌      | No code                                            |
| Diff-context  | ❌      | Single README; diff is trivially reviewable        |
| autoqa        | ❌      | No public URL / HTTP surface of our own            |
| Schemathesis  | ❌      | No OpenAPI                                          |
| ZAP           | ❌      | No app surface                                     |
| SonarCloud    | ❌      | No code to analyze                                 |
| Chat-review   | ❌      | No model/bot-authored communication               |
| Walkthrough   | ❌      | No UI                                              |
| Markdown lint | ✅      | README correctness                                 |
| Dead-link     | ✅      | Outbound links (LinkedIn, personal site) must 200  |
| Prettier      | ✅      | Markdown formatting                                |

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
```

Generic QA patterns live in the `/qa` skill — do not duplicate here.
