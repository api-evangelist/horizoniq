---
name: Provision a HorizonIQ SSL certificate
description: Initialize, approve, and finalize an SSL certificate through the Compass certificate workflow.
api: openapi/horizoniq-compass-openapi-original.yml
operations: [createSslCertificatesInitialize, createSslCertificatesByCertificateIdResendApproval, createSslCertificatesByCertificateIdFinalize, getSslCertificatesByCertificateId, getSslCertificates]
---

# Provision a HorizonIQ SSL certificate

Drive the three-stage SSL certificate flow on the HorizonIQ Compass API (`https://api.compass.horizoniq.com/v1`).

## Auth
`Authorization: Bearer <token>` (Compass portal API token). HTTPS required.

## Steps
1. **Initialize.** `createSslCertificatesInitialize` (POST `/ssl-certificates/initialize`) with the hostname. On successful hostname validation the response returns a list of valid approver email addresses and a `certificateId`.
2. **(Optional) resend approval.** If the approver email is not received, `createSslCertificatesByCertificateIdResendApproval` (POST `/ssl-certificates/{certificateId}/resend-approval`).
3. **Finalize.** `createSslCertificatesByCertificateIdFinalize` (POST `/ssl-certificates/{certificateId}/finalize`) using one of the approver email addresses returned in step 1. A non-approved email is rejected.
4. **Confirm.** `getSslCertificatesByCertificateId` (GET `/ssl-certificates/{certificateId}`) to read final state; `getSslCertificates` (GET `/ssl-certificates`) lists all certificates.

## Rules
- You must pass an approver email returned by initialize; the API will not accept an arbitrary address at finalize.
- Errors are HTTP status + JSON `message`. See errors/horizoniq-problem-types.yml.
- List endpoints paginate with `limit`/`offset`.
