# GraffBans

**Ban, mute, warn and kick management for Paper and Velocity, with LiteBans-compatible commands.**

GraffBans is a standalone punishment plugin. It uses the same commands, flags and permission nodes as
LiteBans, so staff do not have to relearn anything, and it moves an existing LiteBans setup over with
one command: punishments, history, templates and permissions included.

> **Status:** in development, not released yet. This repository holds the documentation and the default
> configuration files (`configs/`). Planned next: a built-in web panel, a ban appeal system, player
> reports and Discord integration.

---

## Contents
- [Features](#features)
- [Compatibility](#compatibility)
- [Installation](#installation)
- [Moving from LiteBans](#moving-from-litebans)
- [Commands](#commands)
- [Flags](#flags)
- [Permissions](#permissions)
- [Configuration files](#configuration-files)

---

## Features

**Punishments**
- Bans, mutes, warnings and kicks: permanent or temporary (`1d`, `1.5h`, `5 days`, `1mo`).
- IP bans and IP mutes, including whole ranges (`/ipban 192.168.1.*`).
- Server-scoped punishments on a network (`server:survival`), or network-wide.
- Warnings can expire; expired ones stop counting.
- Change a punishment in place (`-m`), delete it from history (`-d`), or replace a shorter one (`-N`).
- Silent (`-s`) and extra silent (`-S`) punishments.
- Players can be targeted by name, by UUID (even if they never joined) or by punishment ID (`#123`).

**Templates (punishment ladders)**
- `/ban Steve hacking`: 1st offence 1 day, 2nd 7 days, 3rd 30 days, then permanent. You decide the steps.
- Ban, mute, warn and kick templates. Each step can have its own reason, ban screen message, broadcast,
  flags and commands to run (`actions`).
- Template groups share one ladder, with weights. Old offences can be forgotten (`expire-ladder`).
- Templates can require their own permission.

**Staff tools**
- Full history: `/history`, `/staffhistory`, `/warnings`, `/checkban`, `/banlist` and more.
- Alt detection: `/dupeip` (`/alts`), `/ipreport`, IP history, name history.
- `/staffrollback`: undo everything a staff member did, all time or only recently (for a compromised
  or abusive staff account).
- Staff group limits: longest ban / mute per rank, cooldowns, "templates only" for helpers.
- Staff notices: a banned player tried to join, a muted player tried to chat, someone joined from the
  same IP as a banned account.
- `/lockdown`: keep everyone without a bypass permission from joining (stays on after a restart).
- Muted players cannot dodge the mute with `/msg`, `/tell`, `/r`, `/mail send` and similar commands
  (configurable list, also blocks `/plugin:command` forms).

**Networks**
- **One jar** for Paper and Velocity. On a network, the same jar goes on the proxy and on every
  backend server, all sharing one MySQL / MariaDB database.
- The proxy checks bans when players join and kicks them network-wide; a server-scoped ban sends the
  player to another server instead of disconnecting them.
- Punishments, unbans and mutes reach every server within about 2 seconds, with no Redis or extra
  setup.

**Privacy & safety**
- Player IP addresses are hidden from players by default (only the console sees them), so a hijacked
  operator account cannot leak them.
- If the database is unreachable, you choose whether players may still join.
- No database work on the main server thread.

**Languages**
- English and Turkish included. Every message can be edited (MiniMessage format: colours, gradients,
  hover text) and reloaded with `/graffbans reload`.

---

## Compatibility

| | Supported |
|---|---|
| **Server software** | Paper 1.20.6 and newer (tested on 1.20.6 and 26.x), Folia (experimental) |
| **Proxy** | Velocity 3.5+ and 4.x |
| **Java** | 21 or newer (Paper 26.x and Velocity 4.x themselves need Java 25) |
| **Database** | H2 (built in, no setup, for a single server), MariaDB 10.6+, MySQL 8 |
| **Minecraft versions** | 1.20.6 and newer |
| **Permission plugins** | Any (LuckPerms etc.); LiteBans permission nodes keep working |

**Not supported:** Spigot / CraftBukkit, BungeeCord / Waterfall, PostgreSQL, versions older than 1.20.6.

**Which setup do I need?**

| Your setup | Where to install | Database |
|---|---|---|
| A single Paper server | That server | H2 (default) is fine |
| A Velocity network | The proxy **and** every backend server | One shared MySQL / MariaDB |

On a network, backends need GraffBans too: modern Minecraft chat is signed, so mutes can only be
enforced reliably on the backend servers.

---

## Installation

1. Put `GraffBans.jar` in the `plugins` folder (on a network: proxy and every backend).
2. Start the server once. `plugins/GraffBans/` now contains `config.yml`, `messages_en.yml`,
   `messages_tr.yml` and `templates.yml`.
3. Edit `config.yml`:
   - `server-name`: on a network, each backend's name must match its name in `velocity.toml`.
   - `language`: `en` or `tr`.
   - `database`: leave `h2` for a single server; on a network use `mysql` / `mariadb` everywhere with
     the same database.
4. Restart (or `/graffbans reload` for messages, templates and most settings).

---

## Moving from LiteBans

```
/graffbans import litebans --dry-run   # shows what would be imported, changes nothing
/graffbans import litebans             # imports
```

- Reads LiteBans' own `config.yml` and finds its data by itself (its H2 file or MySQL / MariaDB).
  LiteBans does not need to be running; it is better if it is not.
- Imports bans, mutes, warnings, kicks, IP history, name history and LiteBans' `templates.yml`
  (ladders, messages, broadcasts, actions, template groups). Your existing ladders continue where
  they were.
- Running it again is safe: already imported rows are skipped.
- LiteBans permission nodes (`litebans.ban`, `litebans.admin`, ...) keep working, so your permission
  setup does not need to change. Staff group limits from LiteBans' config can be pasted as they are.
- Tested on a live network database: 776 bans, 864 kicks and 245,000 players imported in 12 seconds,
  every active ban still enforced.

---

## Commands

`<player>` can also be a UUID; removal and check commands also take a punishment ID (`123` or `#123`).
Durations: `30s`, `10m`, `2h`, `1d`, `1w`, `1mo`, `1y`, also `1.5h` or `5 days`.

### Punishing
| Command | Description |
|---|---|
| `/ban <player> [duration] [reason]` | Ban, permanently or for a time |
| `/tempban <player> <duration> [reason]` | Temporary ban |
| `/ipban <player\|ip> [duration] [reason]` | IP ban (aliases `/banip`, `/ban-ip`); ranges like `1.2.3.*` |
| `/tempipban <player\|ip> <duration> [reason]` | Temporary IP ban (`/tempbanip`, `/iptempban`) |
| `/mute <player> [duration] [reason]` | Mute |
| `/tempmute <player> <duration> [reason]` | Temporary mute |
| `/ipmute <player\|ip> [duration] [reason]` | IP mute (`/muteip`) |
| `/tempipmute <player\|ip> <duration> [reason]` | Temporary IP mute |
| `/warn <player> [duration] [reason]` | Warn (works offline: shown on next join) |
| `/kick <player> [reason]` | Kick |
| `/ban <player> <template> [reason]` | Use a template (same for mute, warn, kick, ipban...) |

### Removing
| Command | Description |
|---|---|
| `/unban <player\|ip\|id>` | Remove a ban (every scope, or only `server:<name>`) |
| `/unmute <player\|ip\|id>` | Remove a mute |
| `/unwarn <player\|id>` | Remove the newest warning, or a given one |
| `/staffrollback <staff> [duration]` | Undo a staff member's punishments (all time needs `confirm`) |
| `/prunehistory <player> [duration]` | Delete a player's finished punishments from history |

### Information
| Command | Description |
|---|---|
| `/history <player> [bans\|mutes\|warnings\|kicks] [page]` | Punishments a player received (`/hist`) |
| `/staffhistory <staff> [type] [page]` | Punishments a staff member issued (`/staffhist`) |
| `/warnings [player]` | Your own warnings, or another player's |
| `/checkban <player\|ip\|id>` | The ban in effect now |
| `/checkmute <player\|ip\|id>` | The mute in effect now |
| `/checkwarn <player\|id>` | Warnings in effect now |
| `/banlist [page]`, `/mutelist [page]`, `/warnlist [page]` | Newest punishments |
| `/dupeip <player>` | Accounts that joined from the same IP (`/alts`, `/checkalts`) |
| `/ipreport` | Online players sharing an IP with other accounts |
| `/iphistory <player>` | IPs a player joined from (console only by default) |
| `/namehistory <player>` | Names a player has used |
| `/lastuuid <name>` | UUID of the last player with that name |

### Server
| Command | Description |
|---|---|
| `/lockdown <reason> [server:<name>]` | Nobody without bypass can join |
| `/lockdown end [server:<name>]` | End the lockdown |
| `/lockdown` | Show the lockdown status |

### Administration (`/graffbans ...`)
| Command | Description |
|---|---|
| `/graffbans reload` | Reload config, messages and templates |
| `/graffbans import litebans [--dry-run]` | Import from LiteBans |
| `/graffbans info` | Plugin, database and server information |
| `/graffbans servers` | Servers connected to the shared database |
| `/graffbans broadcast <text> [broadcast-type:<type>]` | Announce to everyone on the network |
| `/graffbans allow add\|check\|remove <player>` | Let a player join past an IP ban on another account |
| `/graffbans unlink <player>` | Forget a player's IPs (removes them from `/dupeip`) |
| `/graffbans add-login <name> <uuid> <ip>` | Record a join by hand |
| `/graffbans reset-templates` | Reset template ladders (console, needs confirm) |
| `/graffbans reset-database [server:] [mode:delete\|deactivate\|prune] [exclude:]` | Clear punishments (console, needs confirm) |

---

## Flags

Same meanings as LiteBans. They can go before or after the player name: `/ban -s Steve 7d spam`.

| Flag | Meaning |
|---|---|
| `-s` | Silent: only staff with `graffbans.notify.silent` see the announcement |
| `-S` | Extra silent: only the console sees it |
| `-p` | Public (overrides a template's silent setting) |
| `-g` / `server:global` | Network-wide |
| `server:<name>` / `server:local` | Only on that server / this server |
| `-I` | Punish the player's IP |
| `-d` | Delete the punishment from history (on `/unban`, `/unmute`, `/unwarn`) |
| `-m` | Change an existing punishment's reason or duration |
| `-N` | Replace an existing punishment only if the new one is longer |
| `duration:permanent` | Permanent |
| `--hide`, `--skip`, `--server-origin=` | Admin flags (`graffbans.admin`) |
| `--sender=`, `--sender-uuid=` | Issue as someone else (console only) |
| `--` | Everything after this is the reason |

---

## Permissions

Every command uses `graffbans.<command>` (e.g. `graffbans.ban`, `graffbans.history`). `graffbans.*`
grants everything. The matching `litebans.*` nodes work as well while
`compatibility.litebans-permissions` is on (default).

| Permission | Meaning |
|---|---|
| `graffbans.<unban\|unmute\|unwarn>.own` | Remove only punishments you issued |
| `graffbans.delete[.own]`, `graffbans.modify[.own]` | Use `-d` / `-m` |
| `graffbans.override` | Replace a ban / mute already in effect |
| `graffbans.ipban.wildcard`, `graffbans.ipmute.wildcard` | IP range punishments |
| `graffbans.warnings` | See your own warnings (everyone by default) |
| `graffbans.warnings.others` | See other players' warnings |
| `graffbans.notify.silent` | See silent punishments |
| `graffbans.notify.banned-join`, `.muted-chat`, `.alts` | Staff notices |
| `graffbans.notify.broadcast` | See `/graffbans broadcast` (everyone by default) |
| `graffbans.extrasilent` | Use `-S` |
| `graffbans.exempt` / `graffbans.exempt.<ban\|mute\|warn\|kick>` | Cannot be punished by players (console still can) |
| `graffbans.exempt.bypass` | Punish exempt players |
| `graffbans.exempt.alts` | Never trigger the alt notice |
| `graffbans.group.<name>` | Staff group for duration limits (see `limits` in config.yml) |
| `graffbans.group.unlimited` | No duration limits |
| `graffbans.cooldown.bypass[.type]` | No cooldowns |
| `graffbans.lockdown.bypass` | Join during a lockdown |
| `graffbans.import`, `graffbans.reload`, `graffbans.admin` | Administration |

---

## Configuration files

The files in [`configs/`](configs) are exactly what GraffBans creates on first start:

| File | What it holds |
|---|---|
| [`config.yml`](configs/config.yml) | Server name, language, database, muted-command list, IP privacy, staff group limits, staff notices |
| [`messages_en.yml`](configs/messages_en.yml) | English messages (MiniMessage) |
| [`messages_tr.yml`](configs/messages_tr.yml) | Turkish messages (MiniMessage) |
| [`templates.yml`](configs/templates.yml) | Punishment templates / ladders, with examples and every option explained |

---

© graffie. All rights reserved.
