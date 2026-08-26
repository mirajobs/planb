# SUBMIT.md

Use this file for the Mirajobs authentication and submit-for-review flow after the public profile has already been generated.

Keep two data paths separate:

- public anonymous profile data
- private owner email used for authentication and recruiter notifications

Do not put the private email inside the public profile payload.

## API base

Use:

- `https://mirajobs.com/api/v1`

## Client identification

For the current portable API login flow, send:

- `X-Client-Id: agent`

This is currently important for email-code authentication via `/api/v1/auth/email/start`.

## 1. Start email login

Request:

- `POST /auth/email/start`

JSON body:

```json
{
  "email": "owner@example.com",
  "scopes": "default offline_access"
}
```

Expected response shape:

- `login_id`
- `expires_in`
- `attempts_remaining`
- `code_length`

The backend sends a one-time code to the private email address.

Recommended headers:

- `Content-Type: application/json`
- `X-Client-Id: agent`

## 2. Verify the one-time code

Request:

- `POST /auth/email/verify`

JSON body:

```json
{
  "login_id": "returned-login-id",
  "token": "code-from-email"
}
```

Expected response shape:

- `access_token`
- `refresh_token`
- `expires_in`
- `token_type`

Use the returned access token in this header:

- `X-Access-Token: Bearer <access_token>`

Recommended headers:

- `Content-Type: application/json`
- `X-Client-Id: agent`

## 3. Create the initial draft

Request:

- `POST /user/profiles`

Headers:

- `Content-Type: application/json`
- `X-Client-Id: agent`
- `X-Access-Token: Bearer <access_token>`

JSON body:

- submit at least a `Title`
- or submit a `Resume` payload if using backend-assisted draft generation
- do not include the private email inside this object
- this endpoint currently creates an initial draft, it does not reliably accept the full public profile object as a one-shot create payload

Example:

```json
{
  "Title": "Senior Backend Engineer"
}
```

Expected response shape includes:

- `ProfileID`
- `Title`
- `Category`
- `ShortUrl`
- `Created`
- `Yaml`

It may also include:

- `SubmissionStatus`
- `Visibility`
- `StatusMessage`

Treat the response `Yaml` as backend-generated draft output from the create step, not as proof that your full local profile content has already been saved.

Use the returned `ProfileID` for the next step.

## 4. Update the draft with the full public profile

Request:

- `PUT /user/profiles/<ProfileID>`

Headers:

- `Content-Type: application/json`
- `X-Client-Id: agent`
- `X-Access-Token: Bearer <access_token>`

JSON body:

- full public profile object
- include `ProfileID` in the body
- use the exact field names and enum values from `schema/profile.schema.json`

Example:

```json
{
  "ProfileID": "returned-profile-id",
  "Title": "Senior Backend Engineer",
  "Summary": "Senior backend engineer with experience building distributed systems, APIs, and data-heavy services across startup and enterprise environments.",
  "Skills": [
    "Go",
    "TypeScript",
    "PostgreSQL",
    "Kafka",
    "AWS",
    "Kubernetes"
  ],
  "Expectations": "Looking for backend or platform-focused roles with strong engineering culture, meaningful ownership, and remote-friendly teams.",
  "JobCategory": "backend",
  "ExpLevel": "senior",
  "ExpTotal": 8,
  "ExpStartup": 3,
  "ExpTop": 2,
  "ExpEnterprise": 5,
  "Degree": "bachelors",
  "Clearance": "none",
  "Permanent": true,
  "Contract": true,
  "Office": false,
  "Remote": true,
  "Relocate": false,
  "Salary": null,
  "TotalCompensation": null,
  "HourlyRate": null
}
```

This step applies the actual public profile content.

In the current backend flow, the profile may already show pending-review or pending-approval state immediately after the create step. Do not treat that as completion. The `PUT` step is still required to persist the full public profile payload.

## 5. Optional follow-up requests

Useful authenticated endpoints:

- `GET /user/profiles`
- `GET /user/profiles/<ProfileID>`
- `DELETE /user/profiles/<ProfileID>`

## Agent behavior during submission

When acting for a user:

1. Ask for the private email separately.
2. Start email login.
3. Ask the user for the one-time code from email.
4. Verify the code and store tokens securely.
5. Create the initial draft.
6. Update the draft with the full public profile payload.
7. Confirm that the full profile content was saved and that the profile is pending moderator review.
8. Return the created `ProfileID` and any returned YAML/template copy to the user.

## Current backend note

The current Mirajobs `/api/v1` profile flow is:

1. authenticate
2. `POST /user/profiles` to create a draft
3. `PUT /user/profiles/<ProfileID>` to populate the draft fully

Practical note:

- the create response may already include `pending-review` or `pending-approval` metadata
- response objects may use UI-oriented fields such as `Category` in addition to the schema field names used in the submitted payload
- the `PUT` step is still the required step for saving the full public profile content

If the backend is later refactored to support one-shot create from the full public profile object, this document should be simplified to match.

Do not:

- place the private email into the public profile JSON or YAML
- print access or refresh tokens into logs or final user-facing output
- invent unsupported fields
