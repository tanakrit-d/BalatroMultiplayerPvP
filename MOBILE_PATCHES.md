# Mobile branch maintenance

`mobile` follows `Balatro-Multiplayer/BalatroMultiplayerPvP` through the
`upstream` remote; `origin` is `tanakrit-d/BalatroMultiplayerPvP`.
No PvP runtime patch is currently needed. Steam-free Community login lives
in the `mobile` branch of `tanakrit-d/BalatroMultiplayerAPI`.

## Automated updates

The [sync workflow](.github/workflows/sync-mobile.yml) checks upstream `main`
daily at 00:00 UTC and can also be run from Actions → Sync mobile with upstream
→ Run workflow. It merges updates into `automation/sync-mobile`, runs every
`tests/unit/*_test.lua` file with Lua 5.1 and LuaJIT, and opens or updates one
pull request targeting `mobile`. Score tests use the API fork's `mobile`
branch checked out alongside PvP; the tested API commit is recorded in the PR.
With no new upstream commits, it opens no PR; manual runs still validate mobile.

All branches and pull requests are published only to
`tanakrit-d/BalatroMultiplayerPvP`. The original project is read-only; never
push or open a PR upstream. The workflow never force-pushes or writes to
`main` or `mobile`. Conflicts or failing tests stop publication; inspect the
failed run and resolve the merge manually. Reserve `automation/sync-mobile`
for the workflow and make mobile fixes on `mobile`.

The workflow also lives on the fork's default branch (`main`) so GitHub can
run its schedule. Actions must be enabled, with **Allow GitHub Actions to
create and approve pull requests** enabled under Settings → Actions → General
→ Workflow permissions. It uses the built-in token; no extra secret is needed.

Test the PR with the paired mobile API on devices, then use **Create a merge
commit** to merge it into `mobile`. Do not squash or rebase: future syncs need
upstream ancestry. Update the builder's pinned PvP SHA separately when ready
to package. The workflow does not publish builds or validate device gameplay.

## Manual updates

Update deliberately from a clean checkout:

```sh
git fetch upstream --tags
git switch mobile
git merge upstream/main
git diff --check
```

Test with the patched API on a Community server before pushing `mobile` to
`origin`. Use merges to preserve published history. No upstream pull request
is needed. Record the paired API/PvP commits in the API repository's
`MOBILE_BASELINE.md` for each device-tested build. Upstream release downloads
do not automatically include changes from either fork's `mobile` branch.
