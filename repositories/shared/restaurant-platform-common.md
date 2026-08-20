# restaurant-platform-common

## Purpose

Shared Java library for reusable backend components.

## Reference

- Source of truth: [.github/input/git-repository-design.md](../../.github/input/git-repository-design.md)
- Main repo docs: [README.md](../../README.md)

## Scope

- DTO
- Common response
- Exception handling
- Error codes
- Utilities
- Logging helpers
- Security components

## Design rules

- Keep the library generic.
- Do not place service-specific business logic here.
- Expose stable public contracts only.
- Keep backward compatibility as a priority.
