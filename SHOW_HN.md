# Show HN: Claude Bootstrap – Opinionated guardrails for Claude Code

**Title:** Show HN: Claude Bootstrap – TDD-first guardrails that make AI-generated code reviewable

**Link:** https://github.com/alinaqi/claude-bootstrap

---

## Post Text:

After 100+ AI-assisted projects, I noticed a pattern: the bottleneck moved from code generation to code comprehension. AI can generate infinite code, but humans still need to review, understand, and maintain it.

Claude Bootstrap is a skills package for Claude Code that enforces guardrails to keep AI-generated code simple, secure, and verifiable.

**Core philosophy:**

- **TDD is mandatory** – Tests must fail before implementation. No exceptions.
- **Iterative loops** – Claude runs autonomously until tests pass, not one-shot.
- **Hard complexity limits** – 20 lines/function, 200 lines/file, 3 params max.
- **Code review on every push** – Pre-push hook blocks on Critical/High issues.
- **Commit hygiene** – Warns when changes get too large for reviewable PRs.

**What it actually does:**

```bash
# Install
git clone https://github.com/alinaqi/claude-bootstrap.git ~/.claude-bootstrap
cd ~/.claude-bootstrap && ./install.sh

# In any project
claude
> /initialize-project
```

This sets up:
- Skills files that Claude reads before writing code
- Pre-push hooks that run `/code-review` and block bad code
- Session management for context preservation across conversations
- Atomic todo tracking with test cases

**The key insight:** Engineers struggle with Claude Code not because of the tool, but because of how they instruct it. The delta is in the guardrails.

**39 skills included:** TDD workflows, security patterns, React/Node/Python frameworks, Supabase, Shopify, Reddit Ads API, MS Teams bots, and more.

Would love feedback from others using Claude Code or similar AI coding tools. What guardrails have you found essential?

---

## Alternative Shorter Version:

After 100+ AI-assisted projects: the bottleneck isn't code generation, it's code comprehension.

Claude Bootstrap adds guardrails to Claude Code:

- TDD mandatory (tests fail first)
- Hard limits (20 lines/function, 200/file)
- Pre-push hooks block Critical/High code review issues
- Commits monitored for PR reviewability

```bash
git clone https://github.com/alinaqi/claude-bootstrap.git ~/.claude-bootstrap
./install.sh && claude && /initialize-project
```

39 skills for React, Node, Python, Supabase, security, and more.

The insight: engineers struggle with AI coding tools not because of the tool, but because of missing guardrails.

---

## Tips for HN:

1. **Post timing:** Weekday mornings (9-11am EST) get best visibility
2. **Engage quickly:** Respond to early comments within first hour
3. **Be technical:** HN appreciates implementation details
4. **Acknowledge limitations:** Be honest about what it doesn't do
5. **No marketing speak:** Keep it factual and technical

## Potential Questions to Prepare For:

**Q: How is this different from cursor rules or .cursorrules?**
A: Similar concept, but for Claude Code specifically. Focus is on TDD enforcement and iterative loops, not just style preferences.

**Q: Does this work with other AI coding tools?**
A: Currently Claude Code only. The skills are markdown files, so conceptually portable.

**Q: Why markdown files instead of actual code enforcement?**
A: Claude reads and follows instructions in markdown. The pre-push hooks add actual enforcement for code review.

**Q: Isn't 20 lines per function too restrictive?**
A: It's a forcing function. When you hit the limit, it signals time to extract. Most functions naturally stay under if designed well.

**Q: What about test coverage gaming?**
A: TDD-first means tests must fail before implementation. Harder to game when the test has to prove it catches the bug first.
