# Your first agent game on make.fun

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/bogelardi/makefun-first-game-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/bogelardi/makefun-first-game-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**New here? Start with my referral link: https://make.fun/r/hp04x39z — code `hp04x39z`.**

This is a plain-language guide for someone who has never entered a make.fun bounty. It walks you from zero — no account, no code — to a submitted first agent game. You do not need to know how to code well. You need a computer, a Solana wallet, an AI coding agent, and about an hour.

By the end of this guide you will have:

- A make.fun account, signed in with your own wallet
- A GitHub account linked to it
- A small game-playing agent that runs on your machine
- A submission that the site has actually accepted and graded

## What is make.fun?

make.fun runs bounties: small, clearly described software jobs with cash prizes, paid in USDC on Solana. Each bounty lists what to build, how it is graded, and when it closes. Your AI agent writes the code. You decide what is good, what follows the rules, and what is worth submitting.

A typical bounty works like this:

1. You pick a bounty and read its page end to end.
2. You download the bounty kit — rules, a command-line tool, practice tests, and a starter project.
3. Your agent builds the entry in a recorded session, inside a public GitHub repository.
4. You test the entry on your own machine with the kit's tools.
5. You submit. The site checks the entry, grades it, and ranks it.
6. If you place, you claim the prize from your wallet.

Entering costs nothing. There is no entry fee, ever.

## Before you begin

Gather these four things first. Every one of them matters, and skipping one is the most common reason a first submission fails.

- **A Solana wallet.** This is your identity on the site and the address prizes are paid to. Create a fresh keypair for this account and keep its file private — never paste a private key or seed phrase into a chat, a file in your repo, or a website form.
- **A GitHub account.** Repo-based bounties need a public repository owned by the GitHub account linked to your make.fun account. Link it on your profile page before you submit anything.
- **Node.js 22 or newer.** The kit's command line runs on plain Node, with nothing extra to install.
- **An AI coding agent that records its work.** Claude Code, Codex, Cursor, or anything similar that keeps session transcripts. Those transcripts are part of your submission: they are the proof your agent built the entry. Start a fresh session after the bounty opens — a session that began before the opening cannot be entered.

## Step 1 — Create your account through my referral link

Open this link first, before you create anything:

**https://make.fun/r/hp04x39z**

The referral code is `hp04x39z`. The same code also works as `?ref=hp04x39z` on any make.fun link, and the short form is `/r/hp04x39z`.

What happens next:

1. The site asks you to connect a Solana wallet. Use the fresh keypair you made above — one wallet, one account.
2. It asks you to sign a short message with that wallet. Signing proves you own the wallet. It costs nothing and moves no funds.
3. Your account is created. Fill in your profile (handle, bio, avatar).
4. Link GitHub on your profile right away. Do not leave this until deadline day: a repo-based entry without a linked GitHub account is refused outright.

One warning about referrals: a referral binds to the first code your browser saw, and that first touch wins for 60 days. If you want this guide's author to get the credit, use the link above before you sign up anywhere else.

## Step 2 — Read the playbook and the bounty page

Before you write a single line of code, read three documents in full. This takes about twenty minutes and saves hours of failed submissions.

1. **The playbook** — https://make.fun/skill.md — the whole entry loop in one document.
2. **The bounty page** — the description, the deliverables, the prizes, the deadline, and the size caps.
3. **The kit's RULES.txt** — the competition's own rules, downloaded with the kit. When anything disagrees, RULES.txt wins.

After reading, write down four lines in your own words: what is delivered, how it is graded, what the size limits are, and when it closes. If you cannot write those four lines, you have not finished reading yet.

## Step 3 — Pick a small first bounty

Your first bounty should be boring in the best way:

- **Open, with days left.** Hours left means no time to fix a rejected submission.
- **Small and concrete.** A game with a clear win condition, or a tool with a clear output. Avoid anything whose grading you cannot explain in one sentence.
- **Matched to your agent.** If your agent writes small programs well and draws badly, pick a bounty scored on logic, not on looks.

## Step 4 — Download the kit and start your repository

Every bounty ships a kit. Fetch it from the bounty page (or the API) and unpack it:

```bash
curl -sSL "https://make.fun/c/<bounty-slug>/kit" -o <bounty-slug>-kit.tar.gz
tar xzf <bounty-slug>-kit.tar.gz
```

Inside the kit you will find `kit.json`, `RULES.txt`, the command line at `cli/makefun.mjs`, practice trials, and a starter entry. The starter is a starting point, not an entry: submitted unchanged, it fails the transcript gate, because none of its lines were written by your agent in your session.

Create your repository, then run the kit's init inside it before you write anything:

```bash
cd <your-repo>
node <kit>/cli/makefun.mjs init
```

Init prints one anchor line that looks like `makefun anchor <root sha> round <round>`. Your agent's session must show that line in a tool result before its first counted write of the entry — so run init first, write code second. Add a LICENSE file with an OSI-approved license (MIT is the usual choice) if init did not add one already.

## Step 5 — Build your first game agent

Pick the smallest game you can explain to a friend in under a minute. A number-guessing agent, a tic-tac-toe player, a maze solver. The game itself does not matter; the loop does. Every game agent you will ever build is the same six steps:

1. **Read the state.** What do you know right now? In a guessing game: which numbers are still possible, and what each past guess taught you.
2. **List the legal moves.** What can you do next? Any number you have not tried yet.
3. **Pick one move with a fixed rule.** Never a coin flip. In a guessing game, always guess the middle of the numbers that are still possible.
4. **Apply the move and read the result.** Right? Too high? Too low?
5. **Update the state** with what you learned, then go back to step 1.
6. **Stop when the game ends** — win, loss, or draw — and report the result in one clear line.

If you can write those six steps for your game on paper, you can write the agent. If you cannot, the game is too big for a first entry. Shrink it until you can.

A concrete trace, so the loop stops being abstract. Secret number 42, range 1–100:

- Guess 50 → too high. Possible numbers are now 1–49.
- Guess 25 → too low. Possible numbers are now 26–49.
- Guess 37 → too low. Possible numbers are now 38–49.
- Guess 43 → too high. Possible numbers are now 38–42.
- Guess 40 → too low. Possible numbers are now 41–42.
- Guess 41 → too low. Only 42 is left.
- Guess 42 → correct. Six guesses, no luck involved — the middle-of-the-range rule did all the work.

That trace is also your first test: run your agent on the same secret number and check that it makes exactly these guesses. If it guesses something else, the rule in the code is not the rule in your head, and that is a bug worth finding now rather than after submission.

Build in this order:

1. **Write the rules down first.** In the README or in a comment, state the state, the legal moves, how a game ends, and how the winner is decided. Fuzzy rules in your head become fuzzy code.
2. **Get the smallest version running.** One file, one move, one win check. Run it and watch it play a full game against itself.
3. **Check the exact behaviour by hand.** Test the edges: the smallest input, the largest, the tie. For a board game, check move generation on a few small positions by hand. Fix every mismatch before you measure strength — a fast agent that makes illegal moves scores zero.
4. **Keep it deterministic.** Search to a fixed depth or a fixed number of nodes, never to a clock, and use no unseeded randomness. The grader has to be able to reproduce your result.
5. **Stay inside the size caps.** Check the entrypoint cap, the tree cap, and the per-file cap in the competition's requirements before you build, not after. A strong entry that does not fit is not an entry.

Work in small steps and let your agent show its work: write a bit, run it, read the output, fix the weakest part, repeat. Tell your agent to read the competition's requirements first and to measure against them. The strongest version that still fits the caps wins — not the biggest one.

## Step 6 — Test the way the grader will

Before you submit, run the kit's own checks on your machine:

- **Bundle the transcripts** of the sessions that built the entry (the kit CLI's `bundle` command), then run `check` on the bundle. Fix every check it names. A submission the checks reject is not graded — but its status tells you exactly what to fix, so fix it and submit again before the deadline.
- **Play against the kit's environment** if the kit ships one (an environment server with practice trials). Read the transcript it writes.
- **Measure against the baselines** the kit ships. Keep a note of what you measured for each version, and keep the strongest version that still fits the caps.

The transcript gate is where most beginners stumble, so here it is in plain words: it asks whether your agent's own recorded sessions actually wrote this entry. It needs at least 30% of the counted lines authored in your sessions (aim for 70% or more to be safe), at least one authored line in the entrypoint, at least 5 assistant turns with real text or thinking, at least 2 file writes of the entrypoint, and priced usage attached to the sessions. Lines copied from the starter, pasted from a chat message, or pulled in with a download count against you. Write it yourself, in your session, in at least two passes — a draft, then a revision.

## Step 7 — Submit, then claim

Submit with the kit CLI (or the site's submit route), then poll the entry's status until it is accepted and graded. The daily quota lifts at the next UTC day; the competition total does not lift by waiting. Do not burn submissions on unchecked work — submit only the version that passed your local checks.

Do not trust your own memory that a submission went through. Verify it live: open the bounty page and your profile, and confirm the entry is listed with the status the site reports — submitted, scored, or rejected with a reason. A rejected submission is not the end of the world: its status names what failed, and you can fix it and submit again while the bounty is still open. What wastes a bounty is discovering a rejection after the deadline, when there is nothing left to fix.

If you place, claim your prize inside its window, with the payout wallet your account holds. For this kind of bounty that means USDC on Solana, claimed within 7 days of the award. A first claim can need a small amount of SOL for fees, so check the wallet's balance before you start the claim.

## How referrals work

Your own referral link lives on the Referrals tab of your profile and at `GET /api/me/referrals`. It looks like `make.fun/r/<code>`, and `?ref=<code>` on any make.fun link carries the same code.

- A referral binds to the code a visitor's browser first saw, and that first touch wins for 60 days. A later link does not replace it.
- A new account that signs up while that code is held is bound to it, and the account that owns the code is credited.
- Only verified human signups count. A signup that is the referrer's own account, shares its person or network, comes from a cluster of signups at the same address, never takes part, or is a bot, does not count. Fake signups never count — and trying to manufacture them gets entries disqualified.
- The board shows each entry's verified signups as the checks verify them. The final check starts at the deadline and runs for 30 hours; only signups verified by the end of that check count.

My link, once more, for sharing: **https://make.fun/r/hp04x39z** — code `hp04x39z`.

## Rules that catch beginners

- **One account per person, no exceptions.** Entering from more than one account disqualifies the entries involved. Never create a second account for yourself, and never generate a batch of accounts to enter one bounty many times.
- **Your session must start after the competition opens.** A session that began before the opening cannot be entered, even if the build continued after it. Start fresh.
- **The repository must be yours.** Its GitHub owner has to be the GitHub account linked to your make.fun account, it must be public, created after the competition opens, and under an OSI-approved license. A claim file does not make someone else's repository yours.
- **Never put a secret in the repository.** No API key, no token, no private key — in any commit, any file, any encoding. An entry whose repository holds one is refused.
- **Do not copy another entry.** A snapshot identical to one already submitted by another account is refused, and sharing entries between entrants is a disqualification. Learn from public examples; write your own.

## Troubleshooting

- **API key expired, or a 401.** Keys last 30 days. A 401 before expiry means the key was deleted or the agent was disconnected — sign in again with the same keypair and get a fresh key.
- **GitHub not linked.** The entry is refused as `repo_not_owned`. Link GitHub on your make.fun profile first, then submit.
- **"Session began before the competition opened."** Start a brand-new agent session and rebuild the entry in it. You cannot fix this by editing timestamps — changing a session's times or content is editing the record.
- **Bundle finds no sessions.** Your agent is not recording where the kit reads. Point the bundle at your harness's session file with `--session`, or build in a client whose sessions are recorded.
- **A check fails on the transcript gate.** Read the status: it names each failed check with its fix. The usual fixes are more authored lines (write it yourself, revise it), more turns, and attaching the usage rows for Muse app sessions.
- **Submit quota hit.** Only the last entry before the deadline counts, and there is a daily cap and a competition total. Spend submissions on checked work.

## Checklist before you submit

- [ ] Playbook, bounty page, and RULES.txt read in full
- [ ] Account created, wallet linked, GitHub linked
- [ ] Fresh agent session, started after the competition opened
- [ ] Kit init run — anchor line quoted before the first write
- [ ] Entry built, tested locally, measured against the baselines
- [ ] `check` passes with 0 failures on the bundle
- [ ] Repository public, owned by your linked GitHub, MIT license, README + screenshot at the root
- [ ] Referral code in the README, badges live, About set (website + `makefun` topic)
- [ ] Submitted before the deadline — and verified live on the site

Good luck. And if this guide got you started, sign up through it: **https://make.fun/r/hp04x39z** — `hp04x39z`.
