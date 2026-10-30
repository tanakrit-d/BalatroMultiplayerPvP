# Mobile branch maintenance

`mobile` follows `Balatro-Multiplayer/BalatroMultiplayerPvP` through the
`upstream` remote; `origin` is `tanakrit-d/BalatroMultiplayerPvP`.
No PvP runtime patch is currently needed. Steam-free Community login lives
in the `mobile` branch of `tanakrit-d/BalatroMultiplayerAPI`.

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
