# AppWizzy Projects for Cursor

**Preview, 2026-10-02:** the Cursor OAuth backend is deployed. Desktop OAuth, discovery of all six tools, and read-only project and machine calls are verified. The creation quote and explicit refusal checks passed without creating a project. The Marketplace application has been submitted and is awaiting review. A public Marketplace listing is not yet verified; use the source installation instructions below.

![AppWizzy](assets/logo.svg)

Connect Cursor to [AppWizzy](https://appwizzy.com) to inspect your projects, compare available machines, and create an app environment from GitHub, a ZIP archive, or an explicitly requested empty project.

This package contains a Cursor plugin manifest, remote MCP configuration, and an agent skill. It connects to the hosted AppWizzy service at `https://appwizzy.com/mcp/projects`; it does not run a local server.

## OpenAI package draft

The separate [openai/ package](openai/README.md) uses the portable Agent Plugins format for OpenAI. It is a prepared draft, not an uploaded, submitted, or published OpenAI listing. Package only that directory for OpenAI; the root configuration remains the native Cursor package described below.

## Requirements

- An AppWizzy account and a Cursor version with plugin support.
- An internet connection and permission to use MCP servers in your Cursor organization.
- Available AppWizzy credits and project permissions for paid creation.
- GitHub connected in AppWizzy for any GitHub import, including a public repository.

## Install from source

1. Clone or download this repository.
2. Copy its plugin files into `~/.cursor/plugins/local/appwizzy-projects/`. The destination must contain `.cursor-plugin/plugin.json`, `mcp.json`, `skills/`, and `assets/`.
3. Restart Cursor or run **Developer: Reload Window**.
4. Open **Customize** and confirm that the AppWizzy skill and MCP server are available. Enable the server if needed and complete the AppWizzy sign-in and OAuth consent flow.
5. Ask Cursor to list available AppWizzy machines as a read-only connection check.

Cursor skips a symlink to a plugin directory outside `~/.cursor/plugins/local`. Team administrators may need to allow local plugin imports; an installed marketplace plugin with the same name takes precedence over a local copy. See [Cursor's local plugin instructions](https://cursor.com/docs/plugins#test-plugins-locally).

The OAuth client ID in `mcp.json` is public. You do not need to copy an access token or client secret into this repository. AppWizzy sign-in authorizes access to your own account. The plugin requests `projects:read` and `projects:create`; available tools depend on the scopes granted.

## Try it

- "List my AppWizzy projects. Do not create anything."
- "Show the available AppWizzy machines and current daily prices."
- "Prepare a quote for importing my GitHub repository. Explain all charges and wait for my confirmation."
- "Check the status of my AppWizzy project."

## Tools

| Tool | Purpose |
| --- | --- |
| `list_project_machines` | Read available machine configurations and current tariffs. |
| `list_projects` | Read your projects with pagination. |
| `get_project_status` | Read a project's provisioning status. |
| `check_project_creation` | Check eligibility and obtain a short-lived hosting quote without creating a project. |
| `prepare_project_archive_upload` | Obtain a one-time upload URL for a ZIP import. |
| `create_project` | Create a project and start infrastructure and AI preparation after explicit consent. |

## Costs and confirmation

Creating a project can incur charges. Before creation, the agent must show the selected machine, source or requested empty project, visibility, hosting credits reserved for the first 24 hours, and the ongoing daily hosting tariff. The quote's `estimated_credit_cost` is the daily hosting price, not a monthly estimate or final total.

Any monthly hosting extrapolation must state the assumed number of days, continuous usage, and unchanged daily tariff. It excludes AI usage and is not evidence of the account's actual bill or current charges.

Creation also automatically starts AI preparation or launch, including GitHub imports, ZIP imports, and empty projects. AI usage is charged separately in AppWizzy credits. The hosting quote excludes this usage, and the final AI cost is not known in advance. The agent must obtain explicit consent to both hosting and additional AI charges before calling `create_project`.

Refusing creation must leave the project uncreated. A quote, OAuth authorization, or successful ZIP upload is not consent to paid creation. Changed parameters, changed prices, or an expired quote require a new allowed quote and fresh consent. Retrying the same creation intent after a timeout must reuse the same idempotency key.

## Project sources and status

Use `owner/repository` for GitHub imports. For ZIP imports, upload to the one-time URL returned by `prepare_project_archive_upload`, then use its `archive_upload_id`. Choose one source; GitHub and ZIP cannot be combined. Only create an empty project when the user explicitly requests one.

Count unique returned project IDs, including across repeated pages, and state whether a summary covers all pages or only the pages retrieved. Similar names do not establish duplicate projects or justify a deletion recommendation.

Report returned project links and provisioning status accurately. A `ready` provisioning status alone does not establish that a VM is currently running, charges are accruing, the application's endpoints are healthy, or AI preparation has finished. Verify these separately before making such claims.

## Troubleshooting

If authentication fails, reconnect through Cursor and AppWizzy. If tools are missing, check the granted scopes and your organization's MCP policy. If an import is denied, check the repository's access through the GitHub connection in AppWizzy. Surface tool errors and blocking reasons; do not retry a paid creation with a new idempotency key after an unknown outcome.

See the [AppWizzy MCP documentation](https://appwizzy.com/documentation/appwizzy-mcp) for service behavior and [Cursor's static OAuth documentation](https://cursor.com/docs/mcp#static-oauth-for-remote-servers) for the connection format.

## Support and service policies

- [AppWizzy MCP documentation](https://appwizzy.com/documentation/appwizzy-mcp)
- [Privacy policy](https://appwizzy.com/privacy)
- [Terms of service](https://appwizzy.com/terms)
- [Contact support](mailto:support@appwizzy.com)

## Package format and license

This is a native Cursor plugin: its manifest is `.cursor-plugin/plugin.json`, and its root `mcp.json` uses Cursor's static OAuth configuration. It does not claim conformance to the portable Agent Plugins MCP schema, which has no `auth` field. Other clients may require their own configuration.

The [MIT license](LICENSE) covers this integration package. It does not license the AppWizzy backend or hosted service and does not grant rights to the AppWizzy or Flatlogic names and trademarks. The logo identifies this integration.
