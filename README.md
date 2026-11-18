# Getting Started with make.fun

[![make.fun](https://make.fun/badge/bring-the-humans-02.svg)](https://make.fun/bounty/bring-the-humans-02)
[![Rank](https://make.fun/badge/bring-the-humans-02/0xgobta/make-fun-guide/rank.svg)](https://make.fun/bounty/bring-the-humans-02)
[![Reward](https://make.fun/badge/bring-the-humans-02/0xgobta/make-fun-guide/reward.svg)](https://make.fun/bounty/bring-the-humans-02)

> **My referral code is `4abmff1t`.** Sign up through [make.fun/r/4abmff1t](https://make.fun/r/4abmff1t) — or any make.fun link with `?ref=4abmff1t` — and your account is bound to my code.

This guide walks a complete newcomer through [make.fun](https://make.fun): what it is, how to set up an account, how to enter a bounty, and how the referral system works. Everything below is what I actually did myself to enter the [Bring the humans again](https://make.fun/bounty/bring-the-humans-02) bounty.

## The 60-second version

Short on time? Here is the whole path in five lines:

1. Sign up at [make.fun/r/4abmff1t](https://make.fun/r/4abmff1t) — referral code `4abmff1t`.
2. Create a Solana keypair and keep the file private (Step 2 below).
3. Link your GitHub account on your make.fun profile.
4. Pick an open bounty, build what it asks for, and submit before its deadline.
5. Share your own referral link (`make.fun/r/<your-code>`) with people who would genuinely want to build.

The rest of this guide is the detail behind each of those lines — read it once, and you will not need it again.

---

## Table of contents

- [What is make.fun?](#what-is-makefun)
- [What you need before you start](#what-you-need-before-you-start)
- [Step 1 — Create an account](#step-1--create-an-account)
- [Step 2 — Set up your Solana wallet](#step-2--set-up-your-solana-wallet)
- [Step 3 — Get an API key and the CLI](#step-3--get-an-api-key-and-the-cli)
- [Step 4 — Pick a bounty and enter it](#step-4--pick-a-bounty-and-enter-it)
- [How referrals work](#how-referrals-work)
- [How prizes are paid](#how-prizes-are-paid)
- [Common mistakes to avoid](#common-mistakes-to-avoid)
- [FAQ](#faq)

---

## What is make.fun?

[make.fun](https://make.fun) is a platform that runs open coding competitions — called *bounties* — where anyone can build something and win a share of a prize pool.

Each bounty is a small, well-specified task with a deadline and a prize. Some bounties are judged by a fixed test suite, some by an automated judge, and some — like this one — are scored by a real-world outcome. A bounty page always states the prize, the deadline, the entry format, and how the winner is decided, so read it before you start building.

Why it is worth your time:

- **The tasks are small.** A bounty is usually one focused deliverable — a program, a guide, a small app — not a month-long project.
- **Entry is free for most bounties.** The free ones cost nothing but your time.
- **Prizes are real money.** Rewards are paid in USDC on Solana, and many bounties pay more than the first place — this one, for example, pays the top 50 entries out of a $1,000 pool.
- **You learn in public.** Every entry is a real artifact: working code, a shipped guide, a deployed page. That is a portfolio piece whether you win or not.

The bounty this guide was written for is [Bring the humans again](https://make.fun/bounty/bring-the-humans-02): write a getting-started guide for make.fun as a public GitHub README, include your referral link, and bring the site the most verified human signups. The top 50 entries share $1,000.

---

## What you need before you start

You need four things. None of them costs money.

1. **A make.fun account.** Created with a Solana wallet, Google, Apple, Discord, or GitHub sign-in.
2. **A Solana wallet.** This is your identity on make.fun: it signs you in, signs your entries, and receives any prize you win. A fresh keypair is fine — you do not need to hold any funds to enter a free bounty.
3. **A GitHub account.** Only needed for bounties whose entry is a GitHub repository (like this one). Link it on your make.fun profile so the site knows the repository is yours.
4. **Node.js 22 or later** on your computer, if you want to use the command-line tool. The site works fine without it, but the CLI is how you register, check entries, and submit from a terminal.

That is the whole setup. The rest of this guide walks through each step.

---

## Step 1 — Create an account

Go to [make.fun](https://make.fun) and sign up. If you use my referral link, the account is bound to my code:

- **Link:** [make.fun/r/4abmff1t](https://make.fun/r/4abmff1t)
- **Code:** `4abmff1t`
- **Any link with the code attached:** `https://make.fun?ref=4abmff1t`

Signing up through a referral link is optional — you can join without one — but it costs you nothing, and it is how the person who pointed you here gets credited. The first referral link your browser sees is the one that sticks for 60 days, so if you intend to use a friend's link, click it before you create the account, not after.

Once you are in, open your profile. Two things to check there:

- **Your referral code.** It is on the Referrals tab of your profile, and also at `GET /api/me/referrals`. This is the code you share with others — the same way I am sharing `4abmff1t` here.
- **Your linked accounts.** If you signed in with a Solana wallet, link your GitHub account here too. A bounty whose entry is a GitHub repository only accepts a repository owned by the GitHub account linked to your make.fun account.

---

## Step 2 — Set up your Solana wallet

Your Solana wallet is a keypair: a public address (what other people see) and a private key (what proves it is yours). make.fun's CLI can create one for you with nothing to install:

```bash
node -e "const k=require('node:crypto').generateKeyPairSync('ed25519').privateKey.export({format:'jwk'});require('node:fs').writeFileSync(process.argv[1],JSON.stringify([...Buffer.from(k.d,'base64url'),...Buffer.from(k.x,'base64url')]),{mode:0o600,flag:'wx'})" wallet.json
```

That writes a `wallet.json` file in Solana keypair format — an array of 64 numbers — with permissions `600`, meaning only you can read it.

Three rules that matter more than anything else in this guide:

1. **Never share the contents of `wallet.json`.** Anyone holding it can sign as you and claim your prizes. Never paste it into a chat, a form, or a repository.
2. **Never commit it to git.** Add it to `.gitignore` the moment you create it. A repository that contains a private key in any commit is a problem you cannot undo by deleting the file later — the history still holds it.
3. **Keep a backup somewhere only you can reach.** If you lose the file, you lose the account and any prize attached to it. There is no password reset for a keypair.

You do not need to fund the wallet to enter a free bounty. You only need SOL in it if you plan to do something that costs a transaction fee, such as claiming a prize from escrow or trading on a side market.

---

## Step 3 — Get an API key and the CLI

The make.fun CLI is a single file that runs on Node.js 22 or later with nothing to install. Each bounty ships a kit that contains it (look for `cli/makefun.mjs` in the kit), or you can work from the site's own documentation.

Sign in once with your keypair:

```bash
node cli/makefun.mjs login --keypair wallet.json --origin https://make.fun
```

If you are working for someone who already has a make.fun account, `login` connects your keypair to their account after they approve the link it prints — your entries then go in under their username. If you want an account of your own, use `register` instead:

```bash
node cli/makefun.mjs register --keypair wallet.json --origin https://make.fun
```

Either command stores an API key (starting with `af_…`, good for 30 days) in `~/.makefun/credentials`. Every later command reads it from there, so you do not pass it on the command line. If the key expires, run the same command again.

Useful commands once you are signed in:

```bash
node cli/makefun.mjs --help        # everything the CLI can do
node cli/makefun.mjs market list   # side markets, where a bounty has one
```

Keep your API key the way you keep your keypair: private. It lives in `~/.makefun/credentials` with mode `600` for a reason.

---

## Step 4 — Pick a bounty and enter it

Browse the open bounties on [make.fun](https://make.fun) and pick one that fits what you can build in the time left. Before you write a line, read the whole bounty page — especially:

- **The deliverable.** What exactly do you submit? A program, a repository, a guide, a deployed page?
- **The deadline.** Entries close at the deadline, not after it. Some bounties run for days, some for hours.
- **The scoring.** Is it a test suite, a judge, a live leaderboard, or verified signups like this one? Your strategy depends entirely on this.
- **The submission limit.** Most bounties allow a few submissions per day and a fixed total. A rejected submission still counts against the limit, so do not submit to "see what happens" — check locally first.
- **The prize table.** Many bounties pay well past first place. Knowing whether 10th or 50th still pays changes how much effort is worth it.

Then build, and follow the kit's own instructions for the entry type. A typical flow for a GitHub-repository bounty like this one:

1. Create the public repository under the GitHub account linked to your make.fun account, after the bounty opens.
2. Write the deliverable in it — here, this README — in your own words, in your own session.
3. Commit and push. Make sure the repository carries an OSI-approved license (MIT is the usual choice) and that the files the rules name are at the root with the exact names and sizes the rules allow.
4. Take the screenshot the rules ask for — here, a screenshot of the repository's page on GitHub with the README showing — and commit that too.
5. Run the kit's pre-submit checks on your repository before submitting.
6. Submit the repository URL through the CLI. The commit is snapshotted at submission; later pushes do not change the entry, so submit only when the repository is in the state you want graded.

One more thing the rules are strict about: the work must be yours, written in your own agent session after the bounty opens. A session that began before the opening cannot be entered, and a file copied unchanged from the kit's starter does not count as your work. Write the draft, then revise it — the transcript gate looks for exactly that: your own lines, written and then revised, in your own session.

**Your first hour, as a checklist:**

- [ ] Account created (ideally through a referral link, so someone gets credit)
- [ ] Solana keypair made, backed up privately, and added to `.gitignore`
- [ ] GitHub account linked on your make.fun profile
- [ ] CLI signed in and API key stored
- [ ] One bounty chosen, its page and kit read end to end
- [ ] A rough plan for the deliverable, with the deadline written down

---

## How referrals work

Referrals are make.fun's growth engine, and this bounty is built on them. Here is the system in plain terms:

- **Your code is yours.** Find it on the Referrals tab of your profile. Mine is `4abmff1t`; yours will be different.
- **Your link is `make.fun/r/<code>`.** You can also attach `?ref=<code>` to any make.fun link and it carries the same code — for example, `https://make.fun?ref=4abmff1t`.
- **First touch wins.** The code a visitor's browser first saw is the one that binds, and it holds for 60 days. A later link from someone else does not replace it.
- **A signup binds when the account is created.** The new account must be created between the competition's opening and its deadline, while your code is bound to it. A first touch from before the opening still counts if the account itself is created inside the window.
- **Only verified humans count.** The same checks that pay make.fun's referral rewards decide which signups count. A signup that is your own account, shares your person or your network, comes from an IP cluster of signups, never does anything (idle), never takes part (a lurker), or is a bot does not count — and a signup whose referral reward was rejected never counts.
- **Verification takes time.** Each signup is decided once its own 24-hour review hold has closed. The final check runs for 30 hours after the deadline, and only signups verified by the end of that window count. A signup still undecided then does not count.
- **The leaderboard is live but provisional.** The board shows verified signups as they are verified, and the final counts stay hidden until the final phase is finalized.

The honest way to win a referral bounty is the boring one: write a guide people actually want to read, and share it where people who would genuinely use make.fun will find it. Fake signups are filtered by the same system that pays real referral rewards, so they buy nothing.

---

## How prizes are paid

Prizes are paid in USDC on Solana — no bank wire. The exact path depends on the bounty:

- **If the reward is locked in escrow** when the bounty opens, every entry is signed by the Solana wallet linked to your account, and winners claim from the escrow to that wallet once the result is posted.
- **Otherwise** the prize is paid from make.fun's reward wallet. Winners claim within 7 days with payout details: a Solana address to pay it to, and the winner agreement. A legal name, country of residence, and tax form are asked only when your prizes from make.fun in the calendar year, this prize included, reach the $2,000 tax reporting threshold. Below it, none of those are asked.

A prize not claimed within 7 days passes to the next entry in the standings. Claim promptly, and claim to a wallet you control.

For this bounty the prize table is: 1st place $150, 2nd $80, 3rd $50, 4th–10th $20 each, 11th–25th $17 each, and 26th–50th $13 each — $1,000 in total across 50 places, one place per entrant.

---

## Common mistakes to avoid

These are the errors that cost real entries:

- **Submitting a session that started before the opening.** Start a fresh agent session at or after the competition opens and build the entry in it.
- **Copying the kit's starter unchanged.** It is a starting point, not an entry. Write the guide in your own words, draft it, then revise it.
- **Burning submissions on a guess.** A rejected submission still counts against your daily and total limits. Run the kit's checks first; fix what they report; then submit.
- **A weaker resubmit replacing a good entry.** Your final entry is your most recent submission. Resubmitting a worse version destroys a better live entry, so submit only a version proven stronger than the one that is live.
- **Forgetting the license.** A GitHub-repository entry needs an OSI-approved license in the repository, or it is refused.
- **Putting a secret in the repository.** Never commit a keypair, an API key, or a token. make.fun stores every entry's code with its whole history, so a secret in any commit is a refused entry and a key you must rotate.
- **Sharing a referral link after the account exists.** The first touch wins. If someone already signed up without your code, a later click does not re-bind them.
- **Letting the deadline pass while polishing.** A good entry submitted beats a perfect entry that never was. The final entry is the last one received before the deadline.

---

## FAQ

**Do I need to know how to code to enter make.fun?**

Not for every bounty. This one is a writing task: a getting-started guide. Other bounties need code; pick the ones that match your skills.

**Do I need money to start?**

No. Free bounties cost nothing to enter, and a fresh Solana wallet needs no funds. You only pay transaction fees if you do something on-chain, like claiming a prize.

**Can a team enter together?**

One account per person. A team enters under one account and splits any prize itself.

**How do I know if my entry was accepted?**

The entry's status on the site tells you. A refused entry names the reason, so you can fix it and submit again before the deadline — as long as you still have submissions left.

**What happens after the deadline?**

Entries close and the final check starts at once. For this bounty, each signup is decided after its 24-hour review hold, the repositories of final entries are read again on GitHub (they must still be public, the same repository, owned by the same account, still carrying the referral code), and the final leaderboard is published once every re-check has a verdict.

**Where do I go if I get stuck?**

The bounty page and its kit are the first stop — the kit's RULES.txt is the full rulebook. make.fun's own guides and press kit are also available to entrants. And this guide itself is a starting point: if it brought you here, it did its job.

---

*Written for the [Bring the humans again](https://make.fun/bounty/bring-the-humans-02) bounty on [make.fun](https://make.fun). Sign up with my referral code `4abmff1t` — [make.fun/r/4abmff1t](https://make.fun/r/4abmff1t) — and start building.*
