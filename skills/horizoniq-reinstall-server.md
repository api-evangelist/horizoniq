---
name: Reinstall a HorizonIQ bare metal server
description: Pick a fresh OS image and reinstall a provisioned bare metal server, then track install progress to completion.
api: openapi/horizoniq-compass-openapi-original.yml
operations: [getServers, getServersByServerUUIDImages, createServersByServerUUIDReinstall, getServersByServerUUIDInstallProgress, getServersByServerUUIDRootPassword]
---

# Reinstall a HorizonIQ bare metal server

Use the HorizonIQ Compass API (`https://api.compass.horizoniq.com/v1`) to wipe and reinstall a server with a new operating system image.

## Auth
Every request needs a Compass API token: `Authorization: Bearer <token>`. Issue one in the Compass portal under Profile > API Tokens. HTTPS is required.

## Steps
1. **Find the server.** `getServers` (GET `/servers`) lists provisioned inventory; capture the target `serverUUID`. Supports `limit`/`offset`/`sortby`/`direction` and filters for narrowing large fleets.
2. **List reinstall images.** `getServersByServerUUIDImages` (GET `/servers/{serverUUID}/images`) returns the OS images available for that specific server. Choose an image id from this list only — availability is per-server.
3. **Start the reinstall.** `createServersByServerUUIDReinstall` (POST `/servers/{serverUUID}/reinstall`) with the chosen image. This is destructive — it wipes the server. Confirm intent before calling.
4. **Poll progress.** `getServersByServerUUIDInstallProgress` (GET `/servers/{serverUUID}/install-progress`) until the install reports complete.
5. **Sync the root password.** After the OS is up, keep the portal in sync with `getServersByServerUUIDRootPassword` (GET) / the update operation — note the portal value does not set the actual server password.

## Rules
- Reinstall and reboot are irreversible fleet actions; never call on a `serverUUID` you did not resolve in step 1.
- Errors return an HTTP status + JSON `message` (not RFC 9457). Treat 400 as bad/unauthorized request, 404 as unknown server. See errors/horizoniq-problem-types.yml.
- No idempotency-key exists; do not blindly retry a POST reinstall — re-poll install-progress instead.
