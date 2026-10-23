# Kryft Mail

**This is a modified version of [Bulwark Webmail](https://github.com/bulwarkmail/webmail)**,
branded for Kryft and served at <https://webmail.kryft.dev> as a Stalwart
Application. It is distributed under the same licence, the GNU AGPL v3 only
(see `LICENSE` and `NOTICE`, both unchanged). Copyright in the upstream code
stays with its authors.

Modified by Kryft, starting 2026-10-23, from upstream tag `1.17.1`.

## What differs from upstream

Branding only — no application code is changed, so that every upstream release
can be taken with a rebase.

| Change | Files |
| --- | --- |
| Name, logos, favicon, login-page company link, source-code link | `lite-config.overlay.json` |
| Kryft logo and favicon artwork (from <https://kryft.dev/brand/>) | `public/branding/kryft/` |
| Install-as-app icons, rendered from the Kryft favicon | `public/icon-*.png`, `public/apple-touch-icon.png` |
| Monochrome palette in place of upstream's blue | one appended block at the end of `app/globals.css` |
| `LITE_CONFIG_OVERLAY` — merges the overlay file into the built `config.json` | `scripts/lite/lib.mjs` (`readConfigOverlay`), one env line in `.github/workflows/static-lite.yml` |
| Upstream's image-publishing, demo-deploy, standalone-release, integration and perf workflows removed — they publish under upstream's names or need upstream's secrets | `.github/workflows/` |

Strings that name Bulwark inside the app (the About page, the mobile app,
the push relay) are left alone: they are attribution, or describe Bulwark
features this build does not ship.

## Releasing

The `kryft` branch is the default branch. **The fork's `main` is never synced**
— it would run upstream's workflows from upstream's files.

    git fetch upstream --tags
    git rebase --onto <new-tag> <old-tag> kryft
    # a conflict on a deleted workflow: keep it deleted (git rm)
    git push --force-with-lease origin kryft
    gh release create <new-tag>-kryft.1 --target kryft --title "Kryft Mail <new-tag>-kryft.1" --notes "Bulwark <new-tag>, Kryft branding."

Publishing the release runs `Lite: Static bundle`, which attaches
`bulwark-lite-stalwart.zip` (plus `.sha256` and a provenance attestation) to
it. Stalwart is then pointed at
`https://github.com/kryft-dev/kryft-mail/releases/download/<tag>/bulwark-lite-stalwart.zip`.
