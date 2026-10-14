# Attest by DataHubz: the Attest connector

Attest by DataHubz is document signing and verifiable records. The Attest connector lets your assistant list the
agreements you sent or must sign, remind signers, prepare new agreements, verify signed documents and records anchored
on the Horizen blockchain, and create Share proofs, zero-knowledge proofs verified on zkVerify. It never reads a
document and never signs: documents stay end-to-end encrypted, and each signer signs in Attest with their own passkey.

This repository holds the configuration that installs the connector in Gemini CLI and Cursor. The connector itself is
a hosted MCP server at `https://attest.datahubz.com/mcp`; nothing here runs on your machine.

Documentation: https://attest.datahubz.com/docs/connector

## What you can do

- See your agreements: the ones you sent and the ones waiting for your signature, with their status, signers, whose
  turn it is and the audit trail up to completion.
- Remind signers of an agreement you sent: the signing link goes again to the signers whose turn it is, at most once a
  day each.
- Prepare a new agreement: give the title, signers, signing order, a note and an expiry date, and get a link to
  Attest's New agreement form, filled in. You attach the PDF and send it in Attest, where it is encrypted in your
  browser.
- Verify a signed document: give the code on its signature stamp (ATT-XXXX-XXXX), an agreement, receipt or proof ID,
  or the file's SHA-256 fingerprint. Attest reports who signed, whether every passkey signature checks out, and the
  record's anchor on the Horizen blockchain. Anyone can run this check, without an account.
- Prove you signed an agreement without revealing it: a Share proof shows your email address (and a co-signer's, if
  you name one) and the day the agreement was completed, never the document, its terms or its title. Each proof is
  submitted to zkVerify for independent verification.
- Record a receipt: anchor a SHA-256 fingerprint on Horizen as proof that the content existed, unchanged, at that
  time, and list your receipts.

Try: "Which agreements are waiting for my signature?", "Who still needs to sign the agreements I sent?", "Prepare a
mutual NDA for Mary Ruiz to sign."

## Install

### Gemini CLI

```bash
gemini extensions install https://github.com/DataHubz/attest-mcp
```

Or add only the server: `gemini mcp add --transport http attest https://attest.datahubz.com/mcp`. Gemini CLI finds
Attest's sign-in on its own; run `/mcp auth attest` to sign in, and again if it asks you to later.

### Cursor and VS Code

The connector's documentation has one-click install links for both: https://attest.datahubz.com/docs/connector#add.
Or add it to Cursor's `~/.cursor/mcp.json` (or `.cursor/mcp.json` in a project), or install the Attest plugin from the
Cursor Marketplace once it is listed there:

```json
{
  "mcpServers": {
    "attest": { "url": "https://attest.datahubz.com/mcp" }
  }
}
```

Cursor finds Attest's sign-in on its own and asks you to sign in the first time it connects. In VS Code, run MCP: Add
Server from the Command Palette with the same URL; the entry in `.mcp.json` is
`{"mcpServers": {"attest": {"type": "http", "url": "https://attest.datahubz.com/mcp"}}}`.

### Claude, Claude Code, ChatGPT, Copilot Studio and other MCP apps

- Claude: Customize, then Connectors, + Add, then Add custom connector. Name it Attest, enter
  `https://attest.datahubz.com/mcp`, choose Sign in now, then Use Claude's published identity (Register automatically
  works too), add it and choose Connect.
- Claude Code: `claude mcp add --transport http attest https://attest.datahubz.com/mcp`, then `/mcp` to sign in.
- ChatGPT: open chatgpt.com/plugins, choose +, then Add custom MCP server. Name it Attest, enter the same URL, choose
  OAuth, confirm the warning and choose Create as a plugin.
- Microsoft Copilot Studio: on the agent's Tools page, Add a tool, New tool, Model Context Protocol; the same URL, with
  OAuth 2.0 and Dynamic discovery.
- Any app that connects to an MCP server by URL over Streamable HTTP can use `https://attest.datahubz.com/mcp`.

### Verify without an account

`https://attest.datahubz.com/mcp/verify` offers verify_record only, with no sign-in. Use it to check a signed document,
a receipt or a proof you received.

## Sign-in and permissions

When an app connects, it sends you to Attest. You sign in, and a consent page shows who is asking, the address it
returns to, and what it will be able to do. An app that identifies itself with a registration it publishes, as ChatGPT
and Claude can, is shown with the site that publishes it (for example, identified by chatgpt.com); an app that
registered itself is shown by the name it gives itself, marked as not verified by Attest. You can untick a permission,
and the app gets nothing until you choose Allow.

| Permission | What it allows |
|---|---|
| Check records | Public checks of agreements, receipts, proofs and fingerprints |
| Read your records | Your agreements, receipts, Share proofs and plan usage |
| Act on your records | Email your signers reminders, record public receipts and create public Share proof links (none of these can be undone or withdrawn), and prepare agreements for you to finish in Attest |

The Security page in Attest lists each connected app with its permissions and last use; Disconnect ends its access at
once.

## What it can't do, by design

- **It can't read your documents.** Every document is end-to-end encrypted in the sender's browser, and Attest stores
  it only as ciphertext it can't read, so no tool reads, summarizes or returns a document.
- **It can't sign.** Each signer signs in Attest with their own passkey (Face ID, Touch ID, Windows Hello or the device
  PIN).
- **It doesn't send documents.** You send them from Attest.
- **It never takes a file.** To check a file, your assistant computes its SHA-256 on your device
  (`shasum -a 256 file.pdf`) or you drop it on https://attest.datahubz.com/verify, where it is fingerprinted in your
  browser and never uploaded.

## Tools

| Tool | What it does |
|---|---|
| verify_record | Checks an agreement ID, the ATT-XXXX-XXXX code on a signature stamp, a receipt ID, a proof ID or a SHA-256 fingerprint: the same public check anyone can run on the verify page |
| list_agreements | Your agreements, sent and received: status, signers, dates, whose turn it is, whether it's anchored, and the certificate link, in pages |
| get_agreement | One agreement, with the note to signers, the document's fingerprint and the audit trail up to completion |
| remind_signers | Emails the signing link again to the signers whose turn it is, at most once a day per signer |
| prepare_agreement | Fills in the New agreement form and gives you the link; you attach the PDF and send it |
| record_receipt | Records a receipt for a SHA-256 fingerprint, or for a short text it fingerprints; only the fingerprint is stored, and it is public |
| list_receipts | Your receipts, with their fingerprints, claims and anchors, in pages |
| share_proof | Creates a Share proof that you signed a completed agreement, alone or with a named co-signer, and gives you its link |
| get_account | What your account has left to use, and whether your signing key is set up |

## Privacy, terms and support

- Privacy: https://www.datahubz.com/privacy-policy#attest-connector
- Terms: https://www.datahubz.com/terms-of-service
- Support: https://www.datahubz.com/contact

## License

MIT, for the files in this repository (the same license as DataHubz's open-source verifier,
https://github.com/DataHubz/attest-verifier). Attest itself is a hosted service at attest.datahubz.com.
