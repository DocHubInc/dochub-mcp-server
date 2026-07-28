<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/dochub_logo_title_dark.png">
    <img src="assets/dochub_logo_title.png" alt="DocHub" width="320">
  </picture>
</p>

# DocHub MCP Server

Official remote MCP server for [DocHub](https://dochub.com). Securely connect your DocHub account to Claude and other AI tools to create, edit, fill, and e-sign PDF documents — using OAuth 2.1, with nothing to install.

> **Endpoint:** `https://dochub.com/mcp` (streamable HTTP)

## Contents

- [Get started](#get-started)
- [Supported clients](#supported-clients)
- [Available tools](#available-tools)
- [How it works](#how-it-works)
- [Data and security](#data-and-security)
- [Example workflows](#example-workflows)
- [Troubleshooting](#troubleshooting)
- [Support and feedback](#support-and-feedback)
- [Disclaimer](#disclaimer)

## Get started

You need a [DocHub account](https://dochub.com). On first use, your client opens a browser window where you sign in to DocHub and authorize access — no API keys or tokens to manage.

### Claude (web and desktop)

1. Go to **Settings → Connectors → Add custom connector**.
2. Enter the URL `https://dochub.com/mcp` and confirm.
3. When prompted, sign in to DocHub and approve access.

### ChatGPT

1. Go to **Settings → Connectors → Advanced** and enable **Developer mode** (available on paid ChatGPT plans; on Business/Enterprise an admin may need to allow custom connectors).
2. Back in **Connectors**, click **Add custom connector** and enter the URL `https://dochub.com/mcp`.
3. When prompted, sign in to DocHub and approve access.
4. Enable the DocHub connector in each chat where you want to use it.

## Supported clients

- **Claude** — claude.ai and the desktop app
- **ChatGPT** — as a custom connector (Developer mode, paid plans)

The server is built on the open Model Context Protocol standard; support for more clients may be added over time.

## Available tools

| Tool | What it does | Access |
| --- | --- | --- |
| Get Current Authenticated DocHub User | Verify which DocHub account is connected | Read |
| List Documents in DocHub | Browse the documents in your account (paginated) | Read |
| Get Document from DocHub | Get details and a link for a specific document | Read |
| Generate Document PDF | Create a polished, ready-to-use PDF (contracts, letters, invoices, proposals, and more) from a plain-language request | Write |
| Modify a Previously Generated Document | Refine a generated document with follow-up instructions | Write |
| List Form Fields on DocHub Document | Inspect the fillable fields on a document | Read |
| Fill Form Field on DocHub Document | Fill in form fields without opening the editor | Write |
| Assign Form Field to Document Role | Choose which signer completes a given field — including signature fields — on a sign request draft | Write |
| Grant or Revoke Document Annotation for a Role | Control whether a signer may write, draw, or sign anywhere on the document as free content (separate from assigned fields) | Write |
| List Sign Requests in DocHub | Track your signature workflows | Read |
| Get Sign Request Details from DocHub | Check signer status for a specific sign request | Read |
| Create Sign Request Draft from Template | Prepare a sign request draft from one of your templates | Write |
| Create Sign Request from Document | Prepare a sign request on one of your documents, with the signers you specify — created as a draft for review | Write |
| Update Sign Request Draft | Change a draft's title or invitation email wording before it is sent | Write |
| Send Sign Request | Validate a prepared sign request, preview exactly what will go out, and send it after your explicit approval | Write |

No tool deletes documents. Sign requests are prepared as drafts for you to review first — until you send, everything stays adjustable: the title and invitation wording, which fields each signer completes, and whether they may write anywhere on the document. The only tool that contacts anyone is Send Sign Request — and it never sends blindly. Before anything goes out it validates the draft (unsendable drafts are reported with the exact reasons instead of a failed send), then shows you a preview — the document, each recipient with what they will be able to do (how many fields to complete, whether they may annotate the document), and the invitation email subject and body — and asks for your explicit approval. If the draft changes between the preview and your approval, the approval is invalidated and you are shown a fresh preview.

## How it works

The DocHub MCP server is a remote service hosted by DocHub — there is nothing to install or run locally. Your AI client connects to `https://dochub.com/mcp` and authenticates with OAuth 2.1 (Authorization Code flow with PKCE), with DocHub acting as the authorization server. The server then calls the DocHub API on your behalf, with exactly the permissions of your own account.

- Every request is authorized with your OAuth access token; tokens are validated on every call and are bound to this MCP server (RFC 8707 resource indicators), so a token issued for the MCP server cannot be replayed elsewhere.
- The server discovers authorization metadata via the standard `/.well-known/oauth-protected-resource` endpoint (RFC 9728).
- The connector can only see and change what your DocHub account can see and change.

## Data and security

- **Authentication:** OAuth 2.1 with PKCE. No passwords or API keys are shared with the AI client.
- **What is accessed:** documents, form fields, templates, and sign requests in your DocHub account — only when a tool is invoked in your AI client.
- **AI-assisted document generation:** the document generation and modification tools use a third-party AI provider (OpenAI) to draft document content. The instructions and content you provide for generation are processed by that provider as a service provider acting on DocHub's behalf. See the [DocHub Privacy Notice](https://legal.dochub.com/privacy-notice) for details.
- **Revoking access:** disconnect the connector in your AI client's settings at any time — the client discards its credentials and the connector can no longer act on your account. Access tokens expire automatically.
- **Reporting vulnerabilities:** see [SECURITY.md](SECURITY.md).

## Example workflows

- *"Generate an NDA for Acme Corp with a 2-year term and save it to my DocHub account."*
- *"List my most recent documents and open the latest invoice."*
- *"What form fields does my intake form have? Fill in the client details below."*
- *"Who hasn't signed the vendor agreement yet?"*
- *"Prepare a sign request draft from my 'Contractor Agreement' template."*
- *"Assign the signature field on the contract draft to Maria."*
- *"Let Maria sign anywhere on the document instead of adding fields."*
- *"Send the NDA to maria@acme.com for signature."*

## Troubleshooting

- **401 / authorization errors:** your session may have expired — disconnect and reconnect the connector to re-authorize.
- **"Tool call failed" on document actions:** confirm your DocHub account has access to the document (shared documents follow your DocHub permissions).
- **The connector doesn't appear after adding it:** make sure you completed the browser authorization step.
- **ChatGPT doesn't call DocHub tools:** custom connectors must be enabled per chat — check that DocHub is turned on in the current conversation.

## Support and feedback

- Help center: [help.dochub.com](https://help.dochub.com)
- Bug reports and feature requests for the MCP server: [GitHub Issues](https://github.com/DocHubInc/dochub-mcp-server/issues)

## Disclaimer

Generated documents are drafts produced with AI assistance. Review them before use — they are not legal advice, and DocHub does not guarantee their fitness for any particular purpose.
