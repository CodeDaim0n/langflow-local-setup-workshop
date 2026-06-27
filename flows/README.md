# Langflow flows

Export your Langflow flow JSON files here, for example:

```text
01-first-agent-test.json
business-loan-document-analysis.json
```

## Notes

- Export a flow from Langflow before making major changes, so you have a restore point.
- **Do not commit API keys** inside flow JSON. Use Langflow settings, global variables, or Camunda Connector Secrets instead.
- Review every export for sensitive data before committing.

By default, `flows/*.json` is git-ignored (see [`.gitignore`](../.gitignore)). Commit a flow only after confirming it contains no secrets.

## Related repository

- [Business Loan Onboarding & Verification workshop](https://github.com/CodeDaim0n/business-loan-onboarding-workshop) — Camunda process, integrations, and the flow walkthrough
