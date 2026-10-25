# GitHub Copilot instructions for RevPilot

Read `/AGENTS.md` and the following documents before making changes:
- `docs/MASTER_SPEC.md`
- `docs/DOMAIN_MODEL.md`
- `docs/DECISIONS_TO_CONFIRM.md`
- the relevant source specification in `docs/source-specifications/`

Preserve the existing stack and behavior. Financial calculations must live in testable domain functions, not be duplicated in UI components. Keep billed, paid, recognized, remaining-to-recognize, forecast-secured, forecast-unsecured, and at-risk amounts separate. Do not invent missing data or business rules. NEPI price effect requires explicit classification and validation. Keep forecasts versioned and simulations separate from the official forecast. Add tests and update documentation for each feature. Inspect the repository and propose a plan before broad refactors.
