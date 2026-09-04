# Pictaria Server for Unraid

This repository contains the official [Unraid Community Applications](https://ca.unraid.net/) template for [Pictaria Server](https://pictaria.ai/server).

Pictaria Server is self-hosted photo intelligence, enrichment, curation, and automation for an [Immich](https://immich.app/) library. It can run as a standalone Immich application or power additional features in Pictaria Frame.

## Install from Community Applications

1. Open **Apps** in Unraid and search for **Pictaria Server**.
2. Select the app and keep the default port and appdata path unless they conflict with another container.
3. Enter:
   - **Immich URL** — the address Pictaria can reach from inside its container.
   - **Immich API Key** — create one using the permissions in the [Pictaria first-run checklist](https://github.com/pictaria-ai/pictaria-server/blob/main/docs/GETTING-STARTED.md#2-connect-immich).
   - **Pictaria Password** — a new password that will protect Pictaria's web interface and API.
4. Apply the template, wait for the container to become healthy, and select its **WebUI** link.

When Immich runs on the same Unraid server, use the Unraid server's LAN address and Immich's published port, for example `http://192.168.1.20:2283`. Do not use `localhost` or `127.0.0.1`: inside the Pictaria container, those addresses mean Pictaria itself. Omit a trailing `/api` from the Immich URL.

The template runs Pictaria without privileged access as Unraid's standard `nobody:users` account (`99:100`). Its only persistent filesystem access is the appdata path mapped to `/data`.

## Install the template before it is listed

For testing on an Unraid server, open its terminal and install the current template:

```sh
mkdir -p /boot/config/plugins/dockerMan/templates-user
curl -fsSL \
  https://raw.githubusercontent.com/pictaria-ai/pictaria-unraid/main/templates/pictaria-server.xml \
  -o /boot/config/plugins/dockerMan/templates-user/my-pictaria.xml
```

Then open **Docker → Add Container**, choose `pictaria` under **Template**, fill in the three required settings, and apply it.

## Data, updates, and recovery

All Pictaria state lives in `/mnt/user/appdata/pictaria` by default. Removing, updating, or recreating the container preserves this directory. Deleting the appdata directory deletes the installation's settings, databases, generated state, and built-in backups.

The Community Applications template follows `ghcr.io/pictaria-ai/pictaria-server:latest`. Pictaria advances that tag only for stable releases, so Unraid's normal **Check for Updates** flow can find stable updates. Before updating, follow Pictaria's [backup](https://github.com/pictaria-ai/pictaria-server/blob/main/docs/BACKUP.md) and [upgrade](https://github.com/pictaria-ai/pictaria-server/blob/main/docs/UPGRADING.md) guidance.

For a deliberate rollback, change the Repository field to a published numeric tag such as `ghcr.io/pictaria-ai/pictaria-server:1.1.0`, then apply the change. Use the version and restore procedure documented for the release you are leaving.

Pictaria's built-in backups default to `/data/backups`, which is inside appdata. That protects against application-level problems but not loss of the appdata pool. For off-pool protection, mount a separate Unraid share at `/backups`, set `BACKUP_DIR=/backups`, and adopt it as described in the [built-in backup instructions](https://github.com/pictaria-ai/pictaria-server/blob/main/docs/BACKUP.md#built-in-automatic-backups).

## Support

- [Getting started](https://github.com/pictaria-ai/pictaria-server/blob/main/docs/GETTING-STARTED.md)
- [Configuration reference](https://github.com/pictaria-ai/pictaria-server/blob/main/docs/CONFIGURATION.md)
- [Immich compatibility](https://github.com/pictaria-ai/pictaria-server/blob/main/docs/IMMICH-COMPATIBILITY.md)
- [Report a problem](https://github.com/pictaria-ai/pictaria-server/issues)

The application image and source are maintained in [`pictaria-ai/pictaria-server`](https://github.com/pictaria-ai/pictaria-server). This repository contains only its Unraid packaging metadata.
