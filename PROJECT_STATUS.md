# AlphaFx implementation and verification status

## Implemented in this repository
- Supabase SSR browser/server clients and server-side service-role client.
- Email/password signup and sign-in, email confirmation callback, protected root dashboard when Supabase env is configured, sign out.
- Authenticated hosting-instance GET/create APIs, plan/subscription checks, resource-limit checks, and queued start/stop/restart/health-check jobs.
- Private signed EA upload URLs, file listing, platform/size validation, and a scanner-result endpoint. Deployment is blocked unless a trusted scanner marks a file clean.
- Stripe Checkout subscription creation for database-backed ZAR/USD development plans, signed webhook verification, subscription lifecycle updates and invoice payment records.
- Authenticated support-ticket submission and billing history APIs.
- Agent device registry, SHA-256 bearer-token validation, atomic typed-job claim RPC, region/capacity reservation, result reporting and capacity release on failed provisioning.
- .NET 8 Windows-agent prototype with typed action allowlist, template-copy provisioning, process start/stop/restart, health check and copying scanned EA binaries into terminal Experts directories.
- Dependency-free agent-contract test.

## Tests performed in this environment
- `node scripts/test-agent-contract.mjs`: PASS (4 contract cases).
- `node --check scripts/test-agent-contract.mjs`: PASS.
- JSON parsing for `package.json` and all JSON files: PASS.
- TypeScript parser run: no syntax diagnostics found; full type-check is blocked because dependencies are not installed in this environment.
- `npm install --no-audit --no-fund`: timed out while trying to access the npm registry; offline installation also failed because the cache does not contain required packages.
- `npm run build`: NOT VERIFIED; failed with `next: not found` because dependencies could not be installed.
- Windows agent build/runtime test: NOT RUN; this environment has no .NET SDK/Windows runtime.
- Supabase migrations, Stripe webhooks, storage signed URLs, scanner callbacks, and live MT4/MT5 deployments: NOT RUN; they require user-owned external accounts, credentials and a Windows test host.

## Not production-complete yet
- Agent is a prototype console worker, not a hardened Windows Service.
- Provisioning copies preinstalled terminal templates; it does not install vendor terminals or bypass licensing.
- EA deployment copies a file into the Experts directory but does not attach it to a chart or prove the EA is trading.
- True resource metrics, broker-login status, EA heartbeat, automated recovery/orphan-job handling and admin console are not complete.
- No malware scanner is connected; uploaded files remain `pending` until an external scanner calls the protected callback.
- Public commercial use requires per-customer VM isolation, OS/network hardening, code signing, backups, monitoring, security review, legal review, billing cancellation/refund workflows and end-to-end tests.
