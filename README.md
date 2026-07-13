# public-assets

Publicly-served static assets for Natrio's Gemini Enterprise agents — primarily the **custom icons
(avatars)** shown for each agent in the Agent Gallery.

## Why this repo exists

Gemini Enterprise lets each custom agent display its own icon, but it sets that icon from an image
**URL** (the agent resource's `icon.uri` field), and **GE fetches that URL client-side, from the
public internet.** That means the icon image has to be reachable **anonymously over HTTPS** — a
private or authenticated URL simply won't render in the avatar.

We can't serve those images from our usual storage. Natrio's organization policy **blocks public
Google Cloud Storage objects org-wide** (public access to `allUsers` is denied by policy). To make a
GCS object public we'd have to file an **org-policy exception** — a Terraform/infrastructure change
with a real security blast radius, since relaxing that policy opens the door to *anything* in that
project being shared publicly.

**This repo avoids all of that.** Anything committed here is served publicly and anonymously via
`raw.githubusercontent.com` (and, if we want a CDN, jsDelivr), which gives us exactly the public URL
Gemini Enterprise needs — **without changing the org policy and without provisioning any cloud
infrastructure.** A public GitHub repo is the least-privilege, lowest-friction way to host these
assets: the only thing exposed is the handful of images we deliberately put here.

## What belongs here

- Static, **non-sensitive, safe-to-be-public** assets only: agent icons, logos, and similar.
- Padded square PNGs work best for agent icons — GE crops the image into a **circle**, so leave a
  transparent margin so the artwork isn't clipped at the corners.

> ⚠️ **This repo is public.** Never commit anything secret or internal-only — credentials, service
> configuration, internal identifiers, or private data. Assume anything pushed here is permanently
> visible to the internet and may be cached or indexed even after it's deleted.

## Using an asset as an agent icon

1. Add the image to this repo.
2. Its public URL is
   `https://raw.githubusercontent.com/<org>/public-assets/<branch-or-commit-SHA>/<file>`.
   Pin to a **commit SHA** (not `main`) for an immutable URL that a later commit can't change.
3. Set it on the registered agent by patching the agent's `icon.uri` in Gemini Enterprise — see the
   agents repo's `docs/deploy-and-register.md` ("Setting an agent icon") for the exact API call.
