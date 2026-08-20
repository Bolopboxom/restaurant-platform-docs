# restaurant-platform-api

## Purpose

Core backend service for business operations.

## Reference

- Source of truth: [.github/input/git-repository-design.md](../../.github/input/git-repository-design.md)
- Main repo docs: [README.md](../../README.md)
- Shared library: [restaurant-platform-common](../shared/restaurant-platform-common.md)

## Scope

- User management
- Menu management
- Order management
- Payment management
- Reporting

## Recommended structure

- api
- application
- domain
- infrastructure
- dto
- config
- shared integration clients

## Design rules

- Start as a modular monolith.
- Keep each domain isolated by package or module boundary.
- Avoid shared entity coupling across domains.
- Design for later service extraction.
