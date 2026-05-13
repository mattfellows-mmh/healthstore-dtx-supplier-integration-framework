# Health Store Supplier Integration Framework

Contract repository for DTx-supplier integration with the Health Store:
specs, schemas, examples and tests.

A starter for ten. Expected to evolve.

## API shape

Inbound REST API hosted by the Health Store, FHIR-aligned, defined by an OpenAPI spec.

## Scope

In scope:

- **Schemas**: the OpenAPI specification and FHIR-aligned resource definitions.
- **Docs**: cross-cutting semantics held as standalone prose, referenced from the spec.
- **Examples**: concrete payloads per operation, plus narrative end-to-end flows.
- **Tests**: validators and tooling that exercise the artefacts in this repo.

Out of scope:

- Runtime code, deployment.
- Finalised change-control and CI/validation wiring (added in later passes).

## Conventions we expect to adopt

- **Contract as source of truth.** Runtime code conforms to the OpenAPI spec and schemas here.
- **Examples discipline.** Every operation has at least one concrete payload example, validated against the schemas.

## Top-level layout

```
schemas/   # OpenAPI + FHIR resource schemas (contract source of truth)
docs/      # cross-cutting semantics, decision records
examples/  # payload examples + narrative flows
tests/     # validators and contract-test collections
```

## How to contribute

- Raise an issue or open a PR; see `.github/PULL_REQUEST_TEMPLATE.md`.
- Review ownership: `.github/CODEOWNERS`.
- Security disclosure: `.github/SECURITY.md`.

## Licence

MIT; see `LICENCE.md`. HTML/Markdown documentation is © Crown Copyright
and available under the Open Government Licence v3.0.
