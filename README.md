# Build your Plan B before you need it

![Plan B logo](https://mirajobs.com/img/planb-logo.png)

Always keep a Plan B.

Many engineers only start looking when the pressure is already high: layoffs, reorgs, PIPs, surprise low ratings, promo fog, or toxic teams.

I’m Anthony, a software engineer previously at Amazon and Microsoft, and I created this project to help engineers build private options before workplace pressure turns into a rushed or desperate job search.

The mission is bigger than one tool: make the tech job market a little less one-sided, so engineers have more options, more negotiating power, and less fear when things start going sideways.

The idea is simple: build a database of anonymous professional profiles that recruiters and employers can reach with job proposals, while each engineer keeps control of their identity until they choose to reveal it.

How it works: use your own AI agent with the open instructions in this repo, generate your profile locally, and submit it for review when you are ready. Once it is live, recruiters can reach out to you by email with job proposals, without seeing your identity.

This workflow only became practical recently, with agentic AI now widely available: your own agent can turn your resume or background into a structured anonymous profile locally, before anything is submitted.

This repo is the public entry point for an idea I started building in 2018, originally as Mirajobs.
This version is open and agent-based, and lets engineers create profiles locally before publishing them.

## Why this exists

Too often, engineers only start looking once their options have already narrowed.

Put differently, this is about improving your [BATNA](https://en.wikipedia.org/wiki/Best_alternative_to_a_negotiated_agreement): your best alternative if your current job situation deteriorates.

Staying ready takes effort. Interview skills get rusty, the market changes, and rebuilding momentum after a sudden job loss can be much harder than keeping your options open while you are still employed.

You do not need to be actively interviewing all the time. But this has become the new normal: letting your options go completely cold for years can become very expensive very quickly.

Common situations:
- layoffs
- reorgs
- PIPs
- promo fog
- unfair performance narratives
- toxic teams
- rushed job search under pressure

The goal is to make it easier to build a private Plan B before you urgently need one.

## What this is

This is a private Plan B network for engineers.

One user can maintain multiple anonymous profiles, each tailored to a different role, specialty, or skill cluster.

It is not:
- a public `#opentowork` signal
- a public social profile
- a generic recruiter funnel

It is:
- an anonymous public profile
- a private email channel for recruiter proposals
- a candidate-first way to stay reachable for better opportunities

## Getting started

TL;DR:

Use your own AI agent to generate an anonymous profile locally, then submit it for review when you are ready.

Recommended first step:

Copy this prompt into your own AI agent:

`Read https://github.com/mirajobs/planb/blob/main/AGENT.md and use it to create my anonymous Plan B jobseeker profile from my resume/background. Save the draft locally as profiles/profile.yaml, check it for public PII, and then follow SUBMIT.md to submit it for moderator review.`

## How it works

1. Read the open instructions in `AGENT.md`.
2. Use your own AI agent with your resume, LinkedIn summary, or freeform background.
3. Generate `profile.json` or `profile.yaml` locally.
   A good default is to save local drafts under `profiles/`, which is ignored by git.
4. Review it yourself or have your agent check that it contains no public PII.
5. Verify your email ownership.
6. Create the anonymous profile draft and submit it for review with your private email using `SUBMIT.md`.
7. Recruiters can send proposals without seeing your identity unless you choose to reveal it later.

You can repeat this flow for multiple tailored profiles, for example:
- one backend profile
- one platform or DevOps profile
- one engineering manager profile

## Trust principles

- Public profile is anonymous
- Private contact stays private
- Publish only when ready
- The instructions are open
- You can draft locally before publishing anything
- Personal data is not sold

## Profile format

See:
- `AGENT.md`
- `SUBMIT.md`
- `schema/profile.schema.json`
- `schema/profile.template.yaml`
- `schema/job-categories.json`
- `examples/`

Supported local formats:
- JSON
- YAML

JSON is the canonical machine payload.

YAML is supported as a human-friendly editing format.

## Why use your own agent

Using your own AI agent means:
- you control the drafting process
- you can keep the first draft local
- you decide what gets published
- you can inspect the instructions before using them

## Why this is different from LinkedIn

LinkedIn starts with your public identity and your public professional graph.

Features like `#opentowork` may offer some visibility controls, but they still sit on top of a public social network first.

In practice, that means you can never be fully sure that teammates, HR, or your manager will not see signals that you are open to leaving.

That means you are still depending on a platform that was not designed around anonymous job search.

This project starts from the opposite direction: anonymous profile first, identity revealed only if you decide to proceed.

The point is not to announce that you are looking. The point is to create options before you are desperate.

## Built on an existing network

Plan B is new. The network behind it is not.

As of August 2026, Mirajobs has been operating for years and currently includes:
- 30,000+ approved jobseeker profiles
- [2,700+ registered recruiters globally](https://mirajobs.com/recruiters)
- [600+ registered employers](https://mirajobs.com/employers)

## Current status

Early beta.

The flow, schema, and tooling will evolve. The current repo now covers local draft generation plus the current Mirajobs-backed draft-create and submit-for-review path.

Feedback is welcome.

## Powered by Mirajobs

[Mirajobs](https://mirajobs.com) provides the existing network behind this project: profile review, publishing, recruiter and employer access, and the private email contact flow.

## Support

If this resonates:
- create and publish your anonymous profile
- help make the tech job market a little fairer
- spread the word if you believe engineers should build private options before they are under pressure
- star the repo
- follow updates

Always be interviewing? Maybe not. Always keep a Plan B? Definitely.
