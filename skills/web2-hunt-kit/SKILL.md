---
name: web2-hunt-kit
description: >-
  Hunt web and app bugs (IDOR, SSRF, auth bypass, business logic, and more)
  with a recon chain, a 26-class vuln index, and a 7-Question validation gate.
  Complements bug-bounty-specialist for web and API scope. Use for any program
  whose scope is a web app or API, and use the 7-Question Gate before writing
  any draft, web or smart-contract alike.
---

# Web2 Hunt Kit

Adapted from [Agentic-Bug-Hunter](https://github.com/awarexone/Agentic-Bug-Hunter) by AwareXone (MIT). Web and app attack-surface hunting plus finding validation, complementing bug-bounty-specialist. The source repo is a read-only reference; nothing in it is installed or run.

## When to use

- Use this skill for web and app targets, and for any program whose scope is a web app or API.

- bug-bounty-specialist is primary for smart-contract hunts. This skill complements it with web attack surface, recon, and validation.

- Use the 7-Question Gate before writing any draft, for web and smart-contract findings alike.

- Do not use this skill for credential attacks, meme-coin audits, or report templates.

## Guardrails

Non-negotiable. They override the source repo defaults.

- Read the program full official rules and scope first. Test only what it authorizes.

- Keep web testing non-destructive and in scope only. No destructive tests, no live exploitation beyond the minimum proof, no contacting developers, no joining programs.

- Run smart-contract proofs only on a local Foundry fork, never mainnet or public testnet.

- Draft only. Never submit anything. Submission is always the user's own action.

- Never run an end-to-end autopilot loop. No credential spraying.

- Do not use HackerOne, Bugcrowd, Intigriti, or Immunefi templates.

- Treat every scanner hit or AI suggestion as a hypothesis until one reproducible request proves it.

## Workflow

- Start every session by defining the goal in one sentence: the feature or domain plus the impact you want.

- Pick one or two vuln classes per session and focus only on those techniques.

- Work the five phases in any order: Recon, Map, Find, Prove, then Validate and draft. Move back a phase whenever stuck or finding a new endpoint.

- 5-minute rule: no interesting surface after 5 minutes on a host means move to the next host.

- Apply the 20-minute rotation: no progress after 20 minutes means rotate endpoint, subdomain, or vuln class. Stop a rabbit hole after 45 minutes on one parameter.

- After one confirmed finding, spend 20 minutes hunting siblings.

- Keep a lead list in the hunt notes file with each lead, its status, and what killed it.

- Think like an attacker with a specific goal, not like a scanner. Question trust boundaries and run What-If experiments such as skipping a checkout step or swapping an id for a sibling id.

## Recon chain

- Score the target before spending time and skip below 4: max bounty at least $5K is 2, large user base or handling money is 2, program launched under 60 days ago is 2, complex features is 1, recent code changes is 1, private program is 1, a stack you know is 1, source code available is 1, prior disclosed reports is 1.

- Hard skip signals: max bounty under $500, all recent reports N/A or duplicate, scope only a static marketing page, company under 5 employees, planned bug class excluded.

- 30-minute recon protocol: 0 to 5 read the program page and note in-scope and out-of-scope assets, 5 to 15 asset discovery, 15 to 25 classify URLs, 25 to 30 explore the app in the live browser.

- Discover subdomains from crt.sh with curl and jq, deduplicate, strip wildcards. Probe live hosts with curl for status, title, headers.

- Crawl URLs from live hosts and JS bundles. Pull historical URLs from web archives when reachable. Classify URLs with grep into parameters, API endpoints, upload paths, admin paths, auth paths.

- Analyze JS bundles for hidden routes. Grep downloaded JS for api keys, tokens, secrets, then verify any key works before treating it as a finding.

- Probe the highest-signal disclosure paths on every live host: .git/HEAD, .git/config, .svn/wc.db, .DS_Store, .env, web.config, WEB-INF/web.xml, application.properties, backup archives. A 200 alone is Info. It becomes real only when recovered source yields a working secret or a confirmed exploitable sink.

- Map the stack to first bug classes: Rails and Laravel mean mass assignment then IDOR, Django means IDOR then SSTI, Flask means SSTI then SSRF, Express means prototype pollution then path traversal, Spring Boot means actuator endpoints then SSTI, ASP.NET means ViewState deserialization then open redirect, Next.js means SSRF via Server Actions then open redirect, GraphQL means introspection then auth bypass, WordPress means plugin SQLi then REST API auth bypass, SPA frameworks mean DOM XSS sinks then client-side route auth bypass.

- Prioritize after recon: API endpoints with ID parameters, file upload, OAuth and SSO, search inputs, admin and debug endpoints.

## Vuln class index

Detail lives in the upstream repo's `web2-vuln-classes` skill, payloads in its `security-arsenal` skill (see Source references).

- Auth and access: 2 Broken auth, 8 OAuth and OIDC, 13 Account takeover, 19 MFA and 2FA bypass, 20 SAML and SSO. Compare sibling functions.

- Injection: 3 XSS, 7 SQL injection, 14 SSTI, 23 LFI and file inclusion, 24 Insecure deserialization, 26 Padding oracle and crypto misuse. Prove impact, not possibility.

- Server side: 4 SSRF, 16 Cloud and infra misconfigs, 17 HTTP request smuggling, 18 Cache poisoning. Escalate DNS-only callbacks to data or RCE.

- Logic and state: 1 IDOR, 5 Business logic, 6 Race conditions. Cross-user data, fast paths skipping state updates, parallel requests.

- Client and content: 9 File upload, 10 GraphQL, 11 LLM and AI features, 21 Error disclosure and debug endpoints, 22 CSS injection. Introspection alone is Info until chained.

- Infrastructure: 15 Subdomain takeover, 25 Dependency confusion. Dangling CNAME and private package names claimed in public registries.

- Payloads: the upstream `security-arsenal` skill holds XSS, SSRF, SQLi, XXE, path traversal, IDOR and auth bypass, JWT and OAuth, NoSQLi, command injection, SSTI, HTTP smuggling, WAF bypass, WebSocket, MFA bypass, and SAML payloads, plus gf pattern names and framework 1-day quick-wins.

## Writeup reference

- Real chaining examples live in [alexbieber/Bug_Bounty_writeups](https://github.com/alexbieber/Bug_Bounty_writeups) (README link list by OWASP Top 10: XSS, CSRF, Clickjacking, LFI, Subdomain Takeover, DoS, Auth Bypass, SQLi, IDOR, 2FA, CORS, SSRF, Race Condition, RCE, Buffer Overflow, Android Pentesting).
- Use it when a vuln class needs a real chaining idea: self-XSS to stored XSS to account takeover, XSS to RCE, cache poisoning to stored XSS, SSRF escalation.
- Rule: learn the technique and the chain, NEVER copy someone else's PoC. Every finding still needs its own reproducible proof on the target and must pass the 7-Question Gate.
- Links are external (Medium, blogs, HackerOne); some may be dead or paywalled — treat the README as an index, not as the content itself.

## The 7-Question Gate

Ask in order. One wrong answer means stop and kill it.

- Q1 Can an attacker use this right now, step by step? Write the setup, the exact copy-paste ready request, the data the response shows, the consequence, and the time and capital cost. If you cannot write the request, kill it.

- Q2 Is the impact on the program accepted impact list? A listed exclusion kills it.

- Q3 Is the root cause in an in-scope asset? Confirm the domain is listed and production, not a third-party service the company only uses.

- Q4 Does it need privileged access an attacker cannot realistically get? Admin can do X is not a bug. Non-admin doing what only admin should do is valid.

- Q5 Is this already known or accepted behavior? Search disclosed reports, issues, changelogs, docs. Documented behavior kills it.

- Q6 Can you prove impact beyond technically possible? XSS must show cookie or session theft, SSRF must return internal data, SQLi must show real table data, IDOR must show another user data. Possibility alone means downgrade.

- Q7 Is this a known-invalid bug class? A listed class without a chain kills it.

- Q8 Identity check for authenticated findings. Record the session and identity used, whether the request works with no auth header, under a second session, and logged out or expired. IDOR must read session B data with session A. Privilege escalation must read high-privilege data with a low-privilege session. Auth bypass must work without a valid session. Unanswered identity questions mean the finding is unproven.

### Four pre-submission gates

All four must pass before any draft.

- Gate 0 Reality check in 30 seconds. The bug is real and confirmed with actual requests, in scope, reproducible from a fresh session, evidence ready.

- Gate 1 Impact validation in 2 minutes. State what the attacker can do that they could not before, name a real victim, and do not rely on unlikely victim behavior.

- Gate 2 Deduplication in 5 minutes. Search disclosed reports, repo issues, changelogs, and the web for target plus endpoint plus bug class.

- Gate 3 Draft quality in 10 minutes. The title states bug class, endpoint, actor, and impact. Steps are copy-pasteable requests. Evidence shows actual impact. Severity matches CVSS and the program definitions. Remediation is one or two concrete sentences. Never write could potentially.

### Never submit list

Submitting these destroys the validity ratio: missing security headers, missing SPF DKIM DMARC, GraphQL introspection alone, banner disclosure without a working CVE exploit, clickjacking on non-sensitive pages, tabnabbing, CSV injection without code execution, CORS wildcard without credential exfil proof, logout CSRF, self-XSS, open redirect alone, SSRF DNS callback only, host header injection alone, rate limit on non-critical forms, session issues. Full list lives in the upstream `triage-validation` skill.

### Conditionally valid only with a chain

Prove the chain end to end before drafting. Open redirect plus OAuth redirect_uri theft becomes account takeover. CORS wildcard plus credentialed PII exfil becomes High. CSRF plus a sensitive action becomes High. Rate limit bypass plus OTP brute force becomes Medium or High. SSRF DNS-only plus internal data becomes Medium. Host header injection plus password reset poisoning becomes High. Self-XSS plus CSRF trigger becomes Medium. Subdomain takeover plus OAuth redirect_uri becomes Critical. Full table lives in the upstream `triage-validation` skill.

### Kill fast rules

- Cannot fill in the Q1 template in 5 minutes means move on.

- More than 2 preconditions at once means kill it.

- Nothing tangible for the attacker means kill it.

- Admin can do X is never a bug.

- Documented behavior kills it immediately.

- 30 minutes on Q6 with no proof means kill it.

## Web3 cross-check

Cross-check only for smart-contract hunts. bug-bounty-specialist stays primary.

- 1 Accounting desync. One path updates one of two synced state variables, forgetting the other.

- 2 Access control. Missing modifier on a sibling, existence check instead of ownership, silent if modifier, uninitialized proxy.

- 3 Incomplete code path. Function B lacks the reverse of what function A does, such as a refund.

- 4 Off-by-one. For every comparison ask what happens when A equals B.

- 5 Oracle manipulation. Missing staleness check, missing Pyth confidence interval, short TWAP, single source.

- 6 ERC4626 vaults. First-depositor share inflation, share transfer without moving lock records.

- 7 Reentrancy. Single-function, cross-function, cross-contract, read-only. Effects before interactions.

- 8 Flash loan attacks. Spot price from reserves or slot0 read as an oracle.

- 9 Signature replay. Missing nonce, chain id, or contract address in the signed hash.

- 10 Proxy and upgrade. Storage collision, uninitialized implementation, delegatecall to a user-controlled address.

- The one rule. Read all sibling functions. If vote() has a modifier, check poke(), reset(), and harvest().

- Foundry PoC template. A forge-std Test with setUp forking mainnet at a pinned block, attacker and victim from makeAddr, deal for funding, vm.startPrank, exploit steps, and an assertGt on the attacker balance. Run with forge test --match-test test_exploit -vvvv on a local fork. Detail lives in the upstream `web3-audit` skill.

## Tool availability

- If your environment lacks the usual recon tools (subfinder, httpx, nuclei, katana, ffuf, nmap, dalfox, dnsx, naabu, gau, arjun), do not install them blindly. Adapt the recon chain to curl, jq, grep, the live browser, and crt.sh.

- If a tool is ever installed later, re-verify it with command -v before writing it into a plan.

## Source references

- Upstream repo, read-only reference: https://github.com/awarexone/Agentic-Bug-Hunter

- Methodology: https://github.com/awarexone/Agentic-Bug-Hunter/tree/main/skills/bb-methodology

- Recon chain: https://github.com/awarexone/Agentic-Bug-Hunter/tree/main/skills/web2-recon

- Vuln classes and bypass tables: https://github.com/awarexone/Agentic-Bug-Hunter/tree/main/skills/web2-vuln-classes

- Payloads: https://github.com/awarexone/Agentic-Bug-Hunter/tree/main/skills/security-arsenal

- Validation gate: https://github.com/awarexone/Agentic-Bug-Hunter/tree/main/skills/triage-validation

- Smart-contract classes and PoC: https://github.com/awarexone/Agentic-Bug-Hunter/tree/main/skills/web3-audit

- Not used: the upstream `credential-attack`, `meme-coin-audit`, and `report-writing` skills, the bughunter autopilot loop, and the MCP server.
