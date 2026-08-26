# AGENT.md

Use this file to generate an anonymous Mirajobs-compatible engineer profile.

The goal is to create a public professional summary without public PII. The profile should be useful to recruiters and employers, but it must not expose the candidate's identity by default.

## Output files

Prefer one of these formats:
- `profiles/profile.yaml`
- `profiles/profile.json`

Use:
- [`schema/profile.schema.json`](./schema/profile.schema.json) for the canonical field shape
- [`schema/profile.template.yaml`](./schema/profile.template.yaml) for the human-friendly template
- [`schema/job-categories.json`](./schema/job-categories.json) for exact `JobCategory` labels
- [`examples/`](./examples/) for sample outputs

JSON is the canonical machine payload.

YAML is the preferred human-editable format because it can carry comments and guidance.

## Submission model

This workflow has two separate data paths:

- public anonymous profile data
- private owner email used for authentication and future recruiter notifications

Do not put the private email inside the public profile payload.

The intended flow is:

1. authenticate the user by email with a one-time code
2. generate `profile.yaml` or `profile.json`
3. check the profile for public PII
4. create an initial draft on Mirajobs
5. update that draft with the full public profile
6. submit the profile for moderator review
7. keep the private email separate from the public profile content

One user may maintain multiple profiles, each tailored to a different role or skill cluster.
Examples:
- backend engineer
- platform or DevOps engineer
- engineering manager

When generating multiple profiles for the same user:
- keep each profile meaningfully differentiated
- use a distinct `Title` for each profile
- tailor `Summary`, `Skills`, and `Expectations` to that profile's target role

For the concrete auth and publish flow, use [`SUBMIT.md`](./SUBMIT.md).

## Source material you can use

You may use:
- a resume or CV
- a LinkedIn summary or exported profile text
- a freeform background summary written by the user
- a list of skills, projects, domains, and role preferences

## Hard privacy rules

Do not include public PII in the generated public profile.

Do not include:
- full name
- email address
- phone number
- exact street address
- links to personal social profiles
- direct links to a personal LinkedIn profile
- GitHub username if it clearly identifies the person
- portfolio links if they clearly identify the person
- company-internal identifiers
- exact dates of birth

Avoid unnecessarily identifying details, including:
- extremely specific project names that uniquely identify the candidate
- confidential product names
- client names that should not be disclosed
- internal-only tooling names if they reveal the employer or team

Allowed:
- professional summary
- skills
- generalized domain experience
- anonymous career history summaries
- level, preferences, and compensation targets
- broad company/background descriptors such as `top-tier tech company`, `startup`, or `enterprise`

When in doubt, generalize instead of exposing.

## Writing rules

- Write in clear professional English.
- Be concrete, not fluffy.
- Prefer short factual sentences over self-promotion.
- Do not write like a recruiter, marketer, or brand guide.
- Keep the profile useful for search and screening.
- Do not invent experience, credentials, or preferences that are not supported by the source material.

## Field rules

Generate these fields:

- `ProfileID`
  - optional for a new local draft
  - include it only if one is already assigned by Mirajobs
- `Title`
  - desired target title
  - concise and specific
- `Summary`
  - executive summary in paragraph form
  - usually 60-140 words
- `Skills`
  - list of specific technical or professional skills
  - keep it relevant and scannable
- `Expectations`
  - what the candidate wants next
  - role shape, team quality, tech scope, growth, flexibility, or mission
- `JobCategory`
  - must use an exact backend label from [`schema/job-categories.json`](./schema/job-categories.json)
  - example: `backend`, not `Back-End / Server Programming`
- `ExpLevel`
  - one of: `junior`, `intermediate`, `senior`
- `ExpTotal`
  - total professional years of experience
- `ExpStartup`
  - years in startups
- `ExpTop`
  - years in top-tier companies
- `ExpEnterprise`
  - years in large enterprises
- `Degree`
  - one of: `none`, `associate`, `bachelors`, `masters`, `doctoral`
- `Clearance`
  - one of: `none`, `confidential`, `secret`, `topsecret`
- `Permanent`
  - `true` or `false`
- `Contract`
  - `true` or `false`
- `Office`
  - `true` or `false`
- `Remote`
  - `true` or `false`
- `Relocate`
  - `true` or `false`
- `Salary`
  - desired salary number if known
  - otherwise `null`
- `TotalCompensation`
  - desired total compensation number if known
  - otherwise `null`
- `HourlyRate`
  - desired contract hourly rate if known
  - otherwise `null`

## Normalization rules

- Use lowercase enum values exactly as documented in the schema.
- Use `JobCategory` labels exactly as documented in `schema/job-categories.json`.
- Use numbers for experience and compensation fields, not strings.
- Use booleans for preference fields, not `"yes"` or `"no"`.
- If a numeric value is unknown, use `null`.
- If a boolean preference is unknown, make a reasonable best-effort inference or set it conservatively to `false`.

## Good summary pattern

A good summary usually covers:
- level and years of experience
- main technical scope
- environments worked in
- notable strengths
- what kinds of roles the person wants next

## Good expectations pattern

A good expectations section usually covers:
- target role or scope
- preferred environment or team quality
- remote/office preference if relevant
- growth, ownership, or mission preferences

## Final check before returning the draft

Before producing the final profile:

1. Check that there is no public PII.
2. Check that `JobCategory` is one of the exact labels in `schema/job-categories.json`.
3. Check that enum values match the schema exactly.
4. Check that the text is useful to a recruiter without revealing identity.
5. Check that no important claims were invented.

## Submission handoff

After the public profile draft is ready:

1. ask for the private email separately
2. authenticate with the one-time email code
3. create the initial draft
4. update it with the full public profile object
5. return the created or updated `ProfileID`

For the exact endpoint details, request/response examples, and submit/update flow, use [`SUBMIT.md`](./SUBMIT.md).

## Output contract

If the task is draft generation only, return only the final profile object or YAML document.

If the task includes submission, follow [`SUBMIT.md`](./SUBMIT.md) and return:

- the final submitted profile object
- the created or updated `ProfileID`
- the short profile URL if returned
- the backend status message, including that the profile is pending moderator review until approved

Do not include secrets or tokens in the final output.

Do not include:
- explanations
- commentary
- Markdown fences
- extra notes before or after the profile

## Current beta note

The public repo flow will continue to evolve.

The current authenticated submit/update model is documented in [`SUBMIT.md`](./SUBMIT.md).
