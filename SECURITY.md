# Security policy

## Threat model

The Cyber Incident Response Planner is a single-file static HTML application served by GitHub Pages. It has no backend, no authentication, and no shared state between users. The only remote endpoint contacted by the tool is GoatCounter (privacy-friendly anonymous analytics — no cookies, no fingerprinting, GDPR-compliant). No user-entered plan data is ever transmitted; only an anonymous page-view ping.

Each user's data lives in their own browser session (`sessionStorage`) and is wiped on tab close. Saved progress is downloaded as a local JSON file.

The realistic security concerns for this kind of tool are therefore:

1. **Self-XSS via user input** — a user could enter malicious markup into a form field and have it rendered back via `innerHTML`. Since there is no other user to attack, the only victim is the user themselves, but it could still be used to corrupt their plan output or import malicious behaviour from a shared JSON file.
2. **JSON import** — a user might be sent a malicious "saved plan" file that, when imported, performs prototype pollution, type confusion, denial-of-service via huge payloads, or attempts to inject HTML.
3. **Tabnabbing / referrer leakage** — external links to GOV.UK, NCSC, GitHub, etc. could leak the user's session if the destination is compromised.
4. **Clickjacking / framing** — the tool could be embedded in a hostile iframe.
5. **Future remote injection** — if the tool ever changed to fetch or load remote content, that vector should be locked down by policy now.

## Defences in place

| Concern | Mitigation |
|---|---|
| Self-XSS via field rendering | All user-provided strings rendered via `escapeHtml`, which escapes `& < > " ' / ` ` =`. Defence-in-depth beyond the strict-minimum four characters |
| JSON import — prototype pollution | `deepMergeSchema` only iterates schema-defined keys, never keys from the imported object, blocking `__proto__` / `constructor` injection |
| JSON import — unknown fields | Schema-driven merge silently drops keys not present in the expected state shape |
| JSON import — type confusion | Each field type-checked against the schema; mismatches fall back to the default |
| JSON import — DoS via large input | 1MB file size cap, 50KB string field cap, 200-item array cap |
| Remote script / data injection | CSP `default-src 'none'`, `script-src` and `connect-src` allow only `'self'`, inline, and the GoatCounter endpoint &mdash; nothing else |
| Tabnabbing | All `target="_blank"` links use `rel="noopener noreferrer"` |
| Referrer leakage | `<meta name="referrer" content="strict-origin-when-cross-origin">` |
| Clickjacking | CSP `frame-ancestors 'none'` |
| Base tag injection | CSP `base-uri 'none'` |
| MIME-sniffing | `<meta http-equiv="X-Content-Type-Options" content="nosniff">` |
| Browser API abuse | `Permissions-Policy` disables camera, microphone, geolocation, USB, etc. |

## What is not in scope

- **Confidentiality of saved plans on disk.** Once you save a plan to JSON, protecting that file is your responsibility (file permissions, encryption at rest, etc.).
- **Print output redaction.** Printed plans contain everything you typed. Treat the printed copy as a confidential document.
- **Browser compromise.** If your browser is compromised, the tool cannot defend against that.
- **Phishing of the URL.** Always verify you are on `cmaddocks-uk.github.io/cyber-response`. There is no other authoritative URL.

## Reporting a vulnerability

If you find a security issue, please open a [GitHub issue](https://github.com/cmaddocks-uk/cyber-response/issues) marked clearly as a security concern. For sensitive issues that should not be public, contact via the email on the GitHub profile and the issue can be coordinated privately.

## Verifying the deployed file

This is a single-file application. The version number is shown in the header and the footer. Anyone in the school IT community can review the source by viewing the page source in a browser, or by checking the [GitHub repository](https://github.com/cmaddocks-uk/cyber-response).

There are no minifiers or build steps — what is in the repo is what is served.
