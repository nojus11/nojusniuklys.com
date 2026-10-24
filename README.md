# nojusniuklys.com

Source for [nojusniuklys.com](https://nojusniuklys.com): plain HTML and
CSS, no JavaScript, no build step.

## How it's served

- `site/` is the web root. It lives in a private S3 bucket that only
  CloudFront can read (origin access control), served over HTTPS (TLS 1.2+)
  with security headers: HSTS, a content security policy, no framing.
- The infrastructure is Terraform:
  [`infra/live/nojusniuklys.tf`](https://github.com/nojus11/aws-org-hardening/blob/main/infra/live/nojusniuklys.tf)
  in my AWS organization project.

## How it's deployed

- Every push to `main` runs [`deploy.yml`](.github/workflows/deploy.yml).
  GitHub's OIDC token is exchanged for a short-lived AWS role that can only
  update this site's files and clear its cache; no AWS keys are stored
  anywhere. The role trusts only this repository's `main` branch.
- Actions are pinned to commit SHAs; Dependabot proposes updates weekly.
