# Enterprise Policies

## Introduction

Enterprise policies control Firefox behavior and let you centrally manage various aspects of Firefox across devices.
Policies can be applied using Group Policy, Microsoft Intune, or by creating a file called `policies.json` and defining policies within the JSON.

The reference documentation for each policy, including guides for applying and managing them, is in the [Firefox Admin Documentation](https://firefox-admin-docs.mozilla.org/).
For other resources for deploying Firefox in an organization, see the [Firefox Enterprise](https://www.firefox.com/en-US/browsers/enterprise/) page.

## Kiosk Mode

Firefox Kiosk Mode is a basic full-screen mode intended for environments where the content displayed in the browser is controlled by a kiosk owner.
It's designed for cases where users have no keyboard access or where keyboard access is restricted (particularly <kbd>Ctrl</kbd> and <kbd>Alt</kbd>).
Kiosk administrators are responsible for ensuring that content displayed on the device cannot unexpectedly navigate users away.

To run Kiosk Mode, start Firefox from the command line with the `--kiosk` option:

```bash
firefox --kiosk
# Or provide a URL
firefox --kiosk 'https://example.com/my-dashboard'
```

To put the kiosk window on a particular monitor, use `--kiosk-monitor` with the monitor number, instead:

```bash
# --kiosk-monitor implies --kiosk
firefox --kiosk-monitor 1 'https://example.com/my-dashboard'
```

Kiosk Mode does three main things:

1. Main browser windows (not popup windows) switch to full-screen mode that can't be exited within Firefox.
2. The context menu isn't shown.
3. Status for URLs and page loading isn't shown.

Two policies that are important for for a kiosk are
[UserMessaging](https://firefox-admin-docs.mozilla.org/reference/policies/usermessaging/) and
[DisableFirefoxStudies](https://firefox-admin-docs.mozilla.org/reference/policies/disablefirefoxstudies/).
Together they stop Firefox from interrupting the kiosk content with recommendations, onboarding, What's New, and studies.

Kiosk mode also won't suppress updates, the notifications and restart prompts for them, block `about:` pages, developer tools, and other behavior, so you should use policies for controlling these, too.

## Windows multi-user mode

Firefox desktop builds enable multi-user mode by default when the target is Windows. Pass `--disable-multiuser` to produce a normal Windows build.

Multi-user mode creates a unique profile under the current user's Windows temporary directory for each browser process. It does not register the profile in `profiles.ini`. Firefox removes the directory after shutdown. At the next launch, it also removes unlocked temporary profiles left by crashes once they are at least one minute old. Files that users download outside the profile remain on disk.

The mode compiles out the application updater, maintenance service, update agent, and crash reporter. Required policies also disable application and system add-on updates, force private browsing, sanitize browsing data at shutdown, keep downloads in the configured download directory, and block Settings, `about:config`, profile management, add-on management, troubleshooting mode, and developer tools.

Administrators configure remaining settings through `HKEY_LOCAL_MACHINE\SOFTWARE\Policies\Mozilla\Firefox` or `distribution\policies.json` under the installation directory. Multi-user builds ignore policies under `HKEY_CURRENT_USER`. The required privacy, update, and settings-page restrictions cannot be overridden by either policy source. Standard users therefore cannot make persistent browser configuration changes, while administrators can set and lock supported preferences through the machine-wide `Preferences` policy.
