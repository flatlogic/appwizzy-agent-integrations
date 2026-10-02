# AppWizzy Projects for OpenAI

**Prepared draft, 2026-10-02.** This package has not been submitted or published in the OpenAI directory. OpenAI domain verification remains paused at the owner's request. No portal, domain challenge, or OAuth configuration was changed while preparing it.

Connect an existing AppWizzy account to inspect projects and machines, prepare hosting quotes, import GitHub or ZIP sources, and check provisioning status. The package uses the remote endpoint `https://appwizzy.com/mcp/projects`.

## Package contents

- [plugin.json](plugin.json): portable identity, OpenAI listing fields, review case definitions, and release notes.
- [mcp.json](mcp.json): one Streamable HTTP server using the Agent Plugins schema.
- [Project skill](skills/appwizzy-projects/SKILL.md): account access, costs, consent, refusal, retries, and accurate status reporting.
- [Logo](assets/logo.svg): the existing 60 by 60 SVG, used for both the listing and composer icon.
- [LICENSE](LICENSE): MIT for this integration package; it does not license the AppWizzy backend, hosted service, names, or trademarks.

This folder is the package root. A later submission ZIP must contain `plugin.json`, `mcp.json`, `skills/`, `assets/`, README, and LICENSE directly at its root, without the enclosing `openai/` folder or the repository's Cursor configuration. Nothing in this draft installs or enables the plugin automatically.

Authentication belongs to the remote MCP integration and the host client's normal OAuth flow, including Client ID Metadata Documents where supported. The package contains no client ID override, client secret, access token, static authorization header, or assertion that OpenAI OAuth has been tested for this package.

## Account requirements and charges

Creation requires the account's project permissions and available AppWizzy credits. GitHub imports require GitHub connected in AppWizzy, including for public repositories. ZIP imports require a valid supplied archive and a client capable of uploading it to the returned one-time URL.

The agent must obtain an allowed, unexpired quote and show the effective machine, source or explicitly requested empty project, visibility, first-24-hour credit reservation, and ongoing daily hosting tariff. `estimated_credit_cost` is the daily hosting price, not a monthly or final total. A monthly extrapolation must state the assumed days, continuous usage, and unchanged tariff.

Creation automatically starts AI preparation or launch, including imports and empty projects. AI usage is charged separately in AppWizzy credits; the hosting quote excludes AI, and the final AI cost is unknown in advance. Obtain explicit consent to the project parameters and both hosting and additional AI charges before creation. OAuth authorization, a quote, or an upload is not consent. On refusal, do not create. Changed terms or an expired quote require a fresh quote and consent. Reuse the same idempotency key after a timeout or retryable error for the same intent.

Count unique project IDs and disclose pagination scope. Names and provisioning status do not establish duplicate projects, current VM runtime, charges, completed AI preparation, or application health. The MCP tools do not purchase credits, process payments, or delete projects.

## Review case status

**All eight definitions in `plugin.json` are NOT YET EXECUTED for this OpenAI package.** Expected behavior describes acceptance criteria, not observed results. Checks performed in another client do not establish OpenAI acceptance.

| Case | Definition | Execution requirement |
| --- | --- | --- |
| P1 | Read machines and current daily tariffs | Connected test account with read access. |
| P2 | List unique projects across pages and inspect status | Controlled project fixtures and read access. |
| P3 | Obtain a quote and disclose hosting and AI charges | Test account eligible for creation; stop before creation. |
| P4 | Import GitHub after explicit consent | Approved repository fixture, connected GitHub, and a separately authorized paid test budget. |
| P5 | Import ZIP after upload and explicit consent | Approved ZIP fixture, upload capability, and a separately authorized paid test budget. |
| N1 | Refuse paid creation after a quote | A quote conversation without creation consent. |
| N2 | Reject access outside the connected account | No access to another account is granted or attempted. |
| N3 | Decline unsupported deletion based on names | No deletion or replacement creation is authorized. |

The manifest contains exactly five positive and three negative cases for its single MCP server. No paid test was run during preparation. Complete the controlled fixtures and record actual results before claiming these cases passed.

## Remaining submission work

- Resume domain verification only when the owner explicitly requests it; the current pause remains in force.
- After that authorization, verify OpenAI OAuth and tool access, execute the review cases within the agreed budget, and confirm imported metadata in the portal.
- Prepare an accessible video walkthrough and a dedicated test account with controlled sample data. Enter private access details only through the secure dashboard; no reviewer credentials or private sign-in instructions belong in this package.
- Resolve the commerce declaration, geographic availability, verified publishing identity, and required owner attestations in the actual submission workflow. These values are intentionally omitted here.
- Record submission, approval, publication, and connection verification as separate outcomes. This draft does not establish any of them.

## Public links and format references

The four listing destinations were checked over public HTTPS on 2026-10-02: [website](https://appwizzy.com), [support contact page](https://appwizzy.com/contact), [privacy policy](https://appwizzy.com/privacy), and [terms of service](https://appwizzy.com/terms). The contact page identifies the support contact method separately from its general inquiry form.

See [AppWizzy MCP documentation](https://appwizzy.com/documentation/appwizzy-mcp), [OpenAI packaging](https://developers.openai.com/plugins/build/plugins), [OpenAI submission fields](https://developers.openai.com/plugins/deploy/submission#automatically-provide-submission-and-review-information), the [Agent Plugins manifest schema](https://agent-plugins.org/schemas/1.0.0/plugin.schema.json), and the [MCP schema](https://agent-plugins.org/schemas/1.0.0/mcp.schema.json).
