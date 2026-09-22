# MyHandyPlus app-config

Keyless runtime configuration for the MyHandyPlus app, served via GitHub Pages:

    https://myhandyplusorg.github.io/app-config/app-config.json

The app fetches it at startup and on resume (`ForceUpdateService` in
`MyHandyPlusOrg/app`). A build whose Android/iOS build number is **below
`min_build`** is blocked behind an update screen pointing at `store_url`.

## Rules

- **This file must stay keyless and public** — it is the mechanism that lets us
  retire Supabase keys, so it can never depend on one.
- **Never set `min_build` above what is live or in store review** (store
  rejection risk + you'd lock out users who cannot update yet).
- Raise `min_build` only after telemetry shows the target build is widely
  installed. The gate fails open on network errors, so raising it only stops
  app usage, it never bricks devices.
- `min_build: 0` = gate present but dormant.
- **Surgical recall**: an optional `"blocked_builds": [10140]` list per platform
  forces those exact builds onto the update screen regardless of `min_build` —
  for pulling one bad release without walling the whole older install base.
  Only builds ≥ the first release shipping the parsing respond to it (10136 and
  10137 respond to `min_build` only).
