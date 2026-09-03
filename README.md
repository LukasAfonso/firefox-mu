![Firefox Browser](./docs/readme/readme-banner.svg)

# Firefox MultiUser

Firefox MultiUser is a Windows-focused fork of [Mozilla Firefox](https://github.com/mozilla-firefox/firefox). It is intended for shared computers where Windows administrators control browser policy and each browser session must start without data from the previous session.

## Differences from upstream Firefox

| Area | Firefox MultiUser | Upstream Firefox |
| --- | --- | --- |
| Windows build default | Multi-user mode is enabled for Windows desktop builds. Use `--disable-multiuser` to build normal Firefox behavior. | Standard profile and policy behavior. |
| Profiles | Creates a unique profile in the current user's Windows temporary directory for each browser process. It does not register the profile in `profiles.ini`. | Uses persistent, registered profiles by default. |
| Data after exit | Removes the temporary profile at shutdown. A later launch removes unlocked temporary profiles left by crashes after they are at least one minute old. | Keeps profile data such as history, cookies, sessions, and saved preferences. |
| User settings | Blocks Settings, `about:config`, profile management, add-on management, troubleshooting mode, and developer tools. It ignores per-user policies under `HKEY_CURRENT_USER`. | Users can normally change settings and use supported `about:` pages. |
| Administrator settings | Reads machine policy from `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Mozilla\Firefox` and installation policy from `distribution\policies.json`. Administrators can set and lock supported preferences. | Supports machine, installation, and per-user policy sources with normal precedence. |
| Privacy defaults | Forces private browsing and shutdown sanitization. Disables telemetry, studies, accounts, password saving, form history, Pocket, recommendations, and other user messaging. | Uses normal Firefox defaults and user choices. |
| Extensions | Blocks extension installation by default. Administrators can define a different machine-wide extension policy. | Allows normal user extension installation. |
| Updates and reporting | Compiles out the application updater, maintenance service, update agent, and crash reporter. Policies also disable background and system add-on updates. | Includes the normal Mozilla update and crash-reporting components. |

The required privacy, update, private-browsing, sanitization, and settings-page policies cannot be overridden by another policy source.

Files saved outside the profile, including downloads, remain on disk. Profile removal is best-effort during shutdown, with stale-profile cleanup on a later launch. This fork is not an operating-system sandbox and does not replace Windows account permissions or other endpoint controls.

Automatic Mozilla updates are intentionally unavailable. Administrators must build and deploy a newer release to receive upstream security fixes.

## Build for Windows

Install [MozillaBuild](https://firefox-source-docs.mozilla.org/setup/windows_build.html), then run these commands from its shell:

```bash
./mach --no-interactive bootstrap --application-choice browser
./mach build
./mach package
```

Windows desktop builds enable multi-user mode automatically. To produce a build with upstream profile and update behavior, add this line to your `mozconfig`:

```text
ac_add_options --disable-multiuser
```

The multi-user changes are intended for Windows desktop builds. Other targets retain their normal defaults.

## Contributing

Report fork-specific problems in this repository's [GitHub issue tracker](https://github.com/LukasAfonso/firefox-mu/issues). For upstream Firefox development guidance, read the [Firefox Contributors' Quick Reference](https://firefox-source-docs.mozilla.org/contributing/contribution_quickref.html).

## Upstream resources

- [Firefox Source Docs](https://firefox-source-docs.mozilla.org/)
- [Firefox Nightly](https://www.mozilla.org/firefox/channel/desktop/#nightly)
- [Mozilla Matrix](https://chat.mozilla.org/#/room/#introduction:mozilla.org)
