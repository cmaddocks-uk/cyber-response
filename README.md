# Cyber Incident Response Planner — for UK Schools & Colleges

[![Security Policy](https://img.shields.io/badge/security-policy-green)](SECURITY.md)

**Current version:** 1.1.11 — see [in-app changelog](https://cmaddocks-uk.github.io/cyber-response/#changelog).

A free, single-file, browser-based planning tool for UK schools and colleges. Assesses cyber response readiness and generates a tailored **Cyber Incident Response Plan** mapped to NCSC, DfE Digital Standards 2030, DfE Risk Protection Arrangement (RPA), and the Ofsted Inspection Toolkit (where cyber response intersects with safeguarding and leadership).

🌐 **Live tool:** [cmaddocks-uk.github.io/cyber-response](https://cmaddocks-uk.github.io/cyber-response)

Companion to the [DfE Digital Standards 2030 — Self-Assessment Tool](https://cmaddocks-uk.github.io/digital-standards-2030/).

---

## What it does

1. **Readiness check** — 12 RAG-scored questions on the current state of your incident response capability, mapped to NCSC, DfE and RPA expectations.
2. **Plan builder** — 10 structured sections covering response team, external contacts, severity grading, escalation authority, playbooks, communications, recovery, and post-incident review.
3. **Plan output** — generates a printable, governor-ready Cyber Incident Response Plan, with generic playbooks for ransomware, data breach, account compromise, phishing, denial of service and insider threat already populated.

## Privacy

- Runs entirely in your browser. Plan data never leaves your device.
- Data is held in the browser session only. Closing the tab wipes it.
- Save progress to a local JSON file (and re-import later) when you want to persist.
- Anonymous page-view counts via [GoatCounter](https://www.goatcounter.com/help/gdpr) — privacy-friendly, GDPR-compliant, no cookies, no fingerprinting, no advertising trackers.

## Security

The tool is hardened against common single-page-app risks: a Content Security Policy locks down remote content (only the GoatCounter analytics endpoint is permitted; everything else is blocked), JSON imports are validated against a schema (preventing prototype pollution and type confusion), all user input is HTML-escaped before rendering, and external links use `rel="noopener noreferrer"`.

See [SECURITY.md](SECURITY.md) for the threat model and how to report issues. An automated security test suite (`security-test.js`) covers XSS injection, prototype pollution, schema validation and link hardening — run it with `node security-test.js`.

## Local development

It's a single HTML file. Open `index.html` in a browser, or serve it with any static server:

```bash
python3 -m http.server 8000
# then open http://localhost:8000
```

## Deployment

The repo is configured for GitHub Pages. Push to `main` and the site is served at `cmaddocks-uk.github.io/cyber-response`.

## Frameworks referenced

- [NCSC Incident Management collection](https://www.ncsc.gov.uk/collection/incident-management)
- [DfE Cyber Security core standard](https://www.gov.uk/guidance/meeting-digital-and-technology-standards-in-schools-and-colleges/cyber-security-standards-for-schools-and-colleges) — one of six core Digital and Technology Standards by 2030
- DfE Risk Protection Arrangement (RPA) Cyber Response Plan template

## Disclaimer

This tool is provided as-is, without warranty. It is not legal, regulatory or insurance advice. Always validate your plan with your IT support, DPO, SLT and insurer before relying on it. Not affiliated with the DfE, NCSC, RPA or any government body.

## Licence

MIT — see [LICENSE](LICENSE).

## Author

Christopher Maddocks (former ANME Ambassador). Built as a contribution to the UK education community.
