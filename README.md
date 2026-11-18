# Bug Bounty Skills

Two agent skills for hunting bugs in bounty programs, distilled from daily live hunting across email and platform programs. They are written for an AI coding agent (Claude Code, Cursor, Codex, opencode, and similar) that does the hunting, and they assume the agent never submits anything on its own.

## Skills

| Skill | What it does |
| --- | --- |
| `bug-bounty-specialist` | Smart-contract hunts end to end: map the attack surface, build a test matrix, run a five-check validation gate, prove reachability from attacker-only state on a local fork, and write a paste-ready draft with a quantified PoC. |
| `web2-hunt-kit` | Web and app hunts: a recon chain, a 26-class vuln index, and a 7-Question validation gate. Complements the specialist for web and API scope. |

## Install

```bash
npx skills add Onefine8/bug-bounty-skills
```

Or copy a skill folder by hand into your agent's skills directory:

| Agent | Project path | Global path |
| --- | --- | --- |
| Claude Code | `.claude/skills/<name>/` | `~/.claude/skills/<name>/` |
| Codex CLI | `.codex/skills/<name>/` | `~/.codex/skills/<name>/` |
| Cursor | `.cursor/skills/<name>/` | `~/.cursor/skills/<name>/` |
| opencode | `.opencode/skills/<name>/` | `~/.config/opencode/skills/<name>/` |
| Shared fallback | `.agents/skills/<name>/` | `~/.agents/skills/<name>/` |

## How it works

1. Read the program's official rules and scope first. Test only what it authorizes.
2. Map the attack surface, then hunt one or two vuln classes at a time.
3. Every candidate finding goes through the validation gate (five checks for smart contracts, seven questions for web). One failed check kills it.
4. Prove it on a local fork or with reproducible requests. No proof, no draft.
5. Prove reachability from attacker-only state: no injected balances, no victim signatures, no cheatcodes that set up the victim.
6. Write the draft to a file with a quantified impact, CVSS where the program uses it, and a concrete fix. The agent drafts only; submission is always the human's own action.

## Rules the skills never break

- In-scope only, non-destructive testing. No mainnet, no public testnet, no live exploitation beyond the minimum proof.
- No contacting developers or auditors.
- No scanner noise or theoretical guesses reported as confirmed bugs.
- A pass with no proven finding is incomplete work, not a finished hunt — re-hunt with a new angle.

## Attribution

- `web2-hunt-kit` is adapted from [Agentic-Bug-Hunter](https://github.com/awarexone/Agentic-Bug-Hunter) by AwareXone (MIT). Its vuln-class detail, payloads, and validation tables live in that repo's `web2-vuln-classes`, `security-arsenal`, and `triage-validation` skills.
- Chaining examples referenced by the kit come from [alexbieber/Bug_Bounty_writeups](https://github.com/alexbieber/Bug_Bounty_writeups) — learn the technique, never copy a PoC.

## License

MIT. See [LICENSE](LICENSE).
