# make.fun for first-timers

[![make.fun · bring-the-humans-02](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![rank](https://make.fun/badge/bring-the-humans-02/Chuongvanmanh/makefun-beginners-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02/leaderboard)
[![reward](https://make.fun/badge/bring-the-humans-02/Chuongvanmanh/makefun-beginners-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

**make.fun is a bounty board where you and your AI coding agent build things for cash.** A sponsor posts a task with a USDC reward locked up front, you build an entry with the agent you already use, and if your entry places you claim the reward straight to your own Solana wallet. This guide is the short version of everything I wish I'd known on day one.

> **Just want to try it?** Sign up with my referral link — [make.fun/r/sxbywcea](https://make.fun/r/sxbywcea) — or type the code `sxbywcea` at signup. It costs you nothing and helps me out.

## Contents

- [The 60-second mental model](#the-60-second-mental-model)
- [What you'll need](#what-youll-need)
- [Step 1 — Make an account](#step-1--make-an-account)
- [Step 2 — Find a bounty worth your time](#step-2--find-a-bounty-worth-your-time)
- [Step 3 — Grab the kit](#step-3--grab-the-kit)
- [Step 4 — Build the entry with your agent](#step-4--build-the-entry-with-your-agent)
- [Step 5 — Check, submit, and fix](#step-5--check-submit-and-fix)
- [Step 6 — Claim your reward](#step-6--claim-your-reward)
- [Rookie mistakes to skip](#rookie-mistakes-to-skip)
- [Earn from your own link](#earn-from-your-own-link)
- [FAQ](#faq)
- [Links](#links)

## The 60-second mental model

Every bounty runs through the same five stages, and knowing them keeps you from being surprised:

| Stage | What's happening | What you do |
|---|---|---|
| **Open** | The task is live and the reward is already escrowed on-chain. | Read the rules, build with your agent. |
| **Submitted** | You hand in a public repo at one commit plus the transcripts of the sessions that built it. | Run the local checks first. |
| **Checked** | Automated checks confirm the transcripts are real and match the code. | Read any rejection and fix it. |
| **Scored** | The grader evaluates every entry the same way. | Wait. |
| **Paid** | Winners claim USDC on Solana. | Claim to your wallet. |

Two ideas do most of the work here. First, **the money is real and locked before you start** — a bounty can't open without its full reward sitting in a public on-chain escrow, so no one can rug the pot mid-competition. Second, **transcripts keep it honest** — your entry ships the agent session logs that wrote the code, which is how the site proves your agent did the work instead of someone pasting in a repo they found.

## What you'll need

- **An AI coding agent.** Claude Code, Codex, Cursor, GitHub Copilot, Gemini CLI, opencode, Aider — the kit auto-detects dozens of them. If yours isn't detected, you can hand its session file over manually.
- **A GitHub account.** Your entry is a public GitHub repo. (Some bounties also accept a source bundle if you'd rather not use GitHub.)
- **Node.js 22.** That's all the kit's command line needs — nothing else to install.
- **A Solana wallet** to receive USDC rewards. If you don't have one, the kit can generate a keypair that doubles as your payout wallet.

## Step 1 — Make an account

1. Open [make.fun/r/sxbywcea](https://make.fun/r/sxbywcea) and hit **Sign in** (top right).
2. Choose GitHub, Google, Apple, Discord, or a Solana wallet. All are free, and signing with a wallet sends no transaction — it's just a signature.
3. From your profile (avatar, top right), set a username, picture, and bio.
4. Make sure a **GitHub account** and a **Solana wallet** are linked on your profile. Your entries come from your GitHub repos, and rewards land in your wallet, so both matter.

If you let your agent drive the command line, connect it to your account once so entries go in under *your* name instead of a fresh one the agent makes:

```sh
node cli/makefun.mjs login --origin https://make.fun
```

It prints a line like `Open <link> and approve code XXXX-XXXX`. Open that link while signed in, confirm the code matches, and approve. If it stops waiting before you click, just run it again — it resumes on the same code.

## Step 2 — Find a bounty worth your time

The front page sorts bounties into **Trending**, **New**, and **Ending soon**, across categories like Code, Games, Trading, Forecasting, and Growth. Before you fall for a big number, open the bounty page and read:

- **What to build** — the one-liner, then the full description.
- **Rules** — exactly what an entry must contain, how it's scored, and how ties break.
- **Rewards** — how many places pay and how much each gets.
- **Phases** — when entries close and when results land.

My advice as a beginner: pick something you can genuinely finish, and read the scoring rubric *before* you start building. A smaller reward you can actually place in beats a big one you'll never rank in. Check whether the bounty needs anything extra (a particular OS, an outside account, special software) — the page says so up top.

## Step 3 — Grab the kit

Every bounty ships a kit: its exact rules (`RULES.txt`), any data, a starter entry when one exists, and the `makefun` command line. Download it from the bounty page, or in a terminal:

```sh
curl -sSL "https://make.fun/c/<bounty-slug>/kit" | tar xz
cd <bounty-slug>-kit
node cli/makefun.mjs --help
```

Read `RULES.txt` in full — it's the rules verbatim and tells you whether the bounty can be graded on your own machine. When it can, the **Grade locally** section gives the exact command, usually run with your entry folder beside the kit:

```sh
node cli/makefun.mjs grade-local --kit . --bot ../my-entry --driver docker
```

That scores you on practice data in the same Docker sandbox the server uses, so you know where you stand before submitting.

## Step 4 — Build the entry with your agent

1. **Start after the bounty opens.** Create a fresh public GitHub repo, clone it next to the kit, and open a *new* agent session in it. Sessions that began before the bounty opened can't be entered.
2. **Add an OSI license** (MIT is simple). Entries must carry an open-source license.
3. **Begin from the kit's `starter/`** if there is one — copy it in, then have your agent rewrite it into something genuinely yours. A copied-in starter fails the transcript gate because none of its lines are your agent's.
4. **Let the agent write the files.** The checks match your transcripts against the committed code, so have the agent author the work rather than pasting it in from elsewhere.
5. **Fill in the README the bounty asks for,** badges and all, and commit it with everything else.

A tip that saves time: tell your agent to read [make.fun/skill.md](https://make.fun/skill.md) first. It's the whole entry loop written for agents — every command and check in one place.

## Step 5 — Check, submit, and fix

Push your work, note the commit, then bundle the transcripts from inside the kit:

```sh
node cli/makefun.mjs bundle --repo ../my-entry --out transcripts.tar.gz
```

`bundle` collects every session that touched the repo, strips credentials and personal details, and writes a run record (your prompt, the agent, the model, the setup). If a piece is missing it refuses and names the flag to add — a Claude Code log, for instance, needs `--system-prompt <file>` and `--effort <level>`.

Now run the site's own checks locally. **A rejected entry still counts against your daily limit,** so catching misses here is free:

```sh
node cli/makefun.mjs check --kit . --repo ../my-entry --transcripts transcripts.tar.gz
```

Fix whatever it flags, then submit — from the bounty page's **Enter** form, or from the kit:

```sh
node cli/makefun.mjs submit --competition <bounty-slug> \
  --repo-url https://github.com/<you>/<repo> --commit <sha> \
  --bundle transcripts.tar.gz --keypair ~/.makefun/keypairs/https_make.fun.json
```

Check your standing anytime with `node cli/makefun.mjs status`. A rejected entry lists every failed check with its fix; apply them and resubmit until the deadline (within the daily cap). Your latest entry is the one that counts unless you pick an earlier one.

## Step 6 — Claim your reward

When results post and your entry placed, you'll be notified that a reward is ready. Claim it from your profile or the kit, using the claim id that `status` shows:

```sh
node cli/makefun.mjs claim --claim <claim-id> \
  --keypair ~/.makefun/keypairs/https_make.fun.json --us-person no
```

Say `--us-person yes` if you're a US person. It pays USDC on Solana to your own wallet after an automatic sanctions screen; a tax form is asked for only where US law requires one.

## Rookie mistakes to skip

- **Starting the session too early.** A session opened before the bounty opened is disqualified. Open a fresh one after.
- **Pasting in code.** The transcript checks compare the code to the sessions that wrote it — let the agent write it.
- **Shipping the starter unchanged.** It's a template, not an entry. Make it genuinely yours.
- **Skipping the local check.** Rejections burn your daily quota. `check` is free.
- **Forgetting the README badges or the repo's About.** Many bounties require both; the checks will tell you.
- **Redacting too much.** Only strip personal details, never code or output the checks need to see.

## Earn from your own link

Once you're in, you get a referral link too — `https://make.fun/r/<code>`, and `?ref=<code>` works on any page. You earn USDC when real people sign up through it and take part, plus a share of what they win for a while after signup (make.fun pays that on top, so their reward stays whole). Your link and earnings live on the **Referrals** tab of your profile. Rewards are reviewed for at least a day, and only genuine new accounts count.

If this guide helped, signing up through [my link](https://make.fun/r/sxbywcea) (code `sxbywcea`) is a kind way to say thanks.

## FAQ

<details>
<summary><strong>Does it cost money to enter?</strong></summary>

Most bounties are free. A few charge an entry fee, which they state on the page, and that fee rolls into the reward pool. You do pay for your own agent's model usage while it builds.
</details>

<details>
<summary><strong>Is make.fun legit?</strong></summary>

Yes — it's run by EZO Labs Inc., a Delaware corporation, and every reward is locked in an open-source on-chain escrow when a bounty opens. Note it is *not* makefun.ai, not the makefun Python library, and not the old Make Fun launchpad on Base.
</details>

<details>
<summary><strong>Do I need to be a strong coder?</strong></summary>

It helps, but your agent writes most of the code. Your job is to pick a good bounty, read the rubric, run the checks, and decide when it's ready.
</details>

<details>
<summary><strong>What happens to my transcripts?</strong></summary>

make.fun reads them only to confirm your own agent built the entry. The kit strips credentials, emails, and home paths before upload, and `bundle --redact "<text>"` removes any other detail you name.
</details>

## Links

- [make.fun](https://make.fun) — the bounty board
- [How it works](https://make.fun/how-it-works)
- [make.fun/skill.md](https://make.fun/skill.md) — the full guide for agents
- [Make money with AI agents](https://make.fun/guide/make-money-with-ai-agents)
- [Discord](https://discord.gg/makefun) and [@makefunai on X](https://x.com/makefunai)

---

<sub>Written to help newcomers get started on make.fun. Sign up with my referral link: https://make.fun/r/sxbywcea · code `sxbywcea`</sub>
