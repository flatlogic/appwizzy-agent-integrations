---
name: appwizzy-projects
description: Inspect AppWizzy projects and machines, prepare hosting quotes, import GitHub repositories or ZIP archives, create explicitly requested empty projects, and check provisioning status. Use when the user asks to create, import, host, or monitor an AppWizzy app environment through its MCP tools.
---

# AppWizzy Projects

Use the connected AppWizzy MCP tools. Read their current schemas and server instructions before acting. Authenticate through the client's OAuth flow; never request an access token or client secret in chat.

## Inspect

- Use `list_projects` to find projects belonging to the user. Continue with `offset=next_offset` while `has_more=true`.
- Use `list_project_machines` for available machines and current daily tariffs. Do not invent machine slugs, prices, project IDs, or links.
- Use `get_project_status` for status checks. These tools require `projects:read`; explain a missing scope instead of assuming access.

## Prepare creation

1. Establish the project name, description, machine, visibility, and source. For GitHub, use `owner/repository`; both public and private imports require GitHub connected in AppWizzy. For ZIP, use the archive upload flow. Never combine GitHub and ZIP. Only choose an empty project when explicitly requested.
2. Call `check_project_creation` with the required project fields. Require `allowed=true`, a valid `quote_token`, and an unexpired quote. Explain any blocking reasons and stop before creation when it is not allowed.
3. For ZIP, call `prepare_project_archive_upload`, PUT the actual file to its one-time URL using the returned upload instructions, and retain `archive_upload_id` only after a successful upload. Keep upload URLs and tokens out of logs and chat. Uploading does not authorize paid creation.

## Disclose costs and obtain consent

Before `create_project`, show the effective machine, GitHub/ZIP source or requested empty project, visibility, `estimated_credit_cost` reserved for the first 24 hours, and the ongoing credits-per-day hosting tariff. The estimate is a daily hosting price, not a monthly estimate or final total.

Explain that creation automatically starts AI preparation or launch, including empty projects and GitHub/ZIP imports. The hosting quote excludes AI. AI is charged separately in AppWizzy credits based on actual usage, and its final cost is unknown in advance.

Obtain explicit user consent to the shown project parameters and both hosting and additional AI charges. A general creation request, OAuth authorization, quote, or upload is insufficient. On refusal, do not call `create_project`. If parameters or price change, or the quote expires, obtain a new allowed quote, disclose the updated terms, and obtain fresh consent.

## Create and follow up

1. Generate one lowercase UUID idempotency key for this project intent. Call `create_project` only after consent, using the quoted parameters and one optional source.
2. Retain the same key after every timeout or retryable error. Check returned status when possible. Do not use a new key to resolve an unknown outcome. After terminal failure, begin a new intent with a new key and repeat the quote and consent steps.
3. Report the returned operation ID, project URL, and status. Use `get_project_status` when the read scope is available. Surface failures and blocking reasons without inventing recovery actions.
4. Describe `ready` as the reported provisioning status. Do not claim the application is healthy or that AI preparation is complete without separate evidence.
