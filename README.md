# GraffBans

**Punishment management built for modern Paper servers and Velocity networks.**
Bans, mutes, warnings and kicks with punishment ladders, staff rank limits, alt detection and
network-wide sync, all in one jar.

> **Status:** in development, not released yet. This repository holds the documentation
> ([Wiki](https://github.com/g-raffie/GraffBans/wiki)) and the default configuration files ([`configs/`](configs)).

---

## Why GraffBans

- **One jar, every setup.** The same file runs on a single Paper server and on a Velocity proxy. On a
  network you put it on the proxy and on the backends, point them at one database, and you are done.
- **Network sync without extra software.** A ban, unban or mute reaches every server within about two
  seconds over the shared database. No Redis, no messaging plugin, no duplicate announcements.
- **Punishment ladders that think for you.** `/ban Steve hacking` picks the right step on its own:
  1 day the first time, 7 days the second, then 30 days, then permanent. Each step can show its own
  message, run commands and carry its own reason.
- **Staff with guard rails.** Longest ban per rank, cooldowns, "templates only" for new helpers, a full
  undo for a compromised staff account (`/staffrollback`), and changes you can trace in `/staffhistory`.
- **Private by default.** Player IP addresses are visible only from the console, so a hijacked
  operator account cannot leak them.
- **Modern text.** Every message uses MiniMessage: hex colours, gradients, hover text and clickable
  links, in English and Turkish out of the box, reloadable without a restart.
- **Never blocks your server.** Database work runs off the main thread; if the database goes down you
  choose whether players may still join.
- **Easy to switch to.** Staff keep the commands they already know, and an existing LiteBans setup
  moves over with one command (see [Switching from LiteBans](https://github.com/g-raffie/GraffBans/wiki/Switching-from-LiteBans)).

**Coming next:** a built-in web panel, ban appeals, player reports and a Discord bot.

---

## Features

| Area | What you get |
|---|---|
| Punishments | Bans, mutes, warnings, kicks; permanent or timed; IP and IP-range (`1.2.3.*`) punishments; silent and console-only modes |
| Warnings | Expiring warnings, and automatic consequences at a number of warnings (e.g. a kick at 3, a 1-day ban from 4 on) |
| Editing | Change reason or duration in place, delete from history, replace a shorter punishment with a longer one |
| Targets | Player name, UUID (also for players who never joined) or punishment number (`#123`) |
| Templates | Ladders for bans, mutes, warnings and kicks; per-step reason, screen, announcement and commands; shared ladders with weights; offences that expire |
| History | Player history, staff history, active and recent lists, punishment lookup |
| Alts | Accounts sharing an IP, online IP report, IP history, name history, exceptions for shared households |
| Staff | Rank limits and cooldowns, staff notices (banned join, muted chat, banned alt joined), rollback |
| Server | Lockdown that survives restarts, per-server or network-wide |
| Networks | Velocity proxy + backends, server-scoped punishments, move-to-another-server instead of disconnect, an optional lobby that banned players may still join, sync across instances |
| Chat | Muted players cannot dodge with `/msg`, `/r`, `/mail` and similar (configurable, blocks `/plugin:command` too) |

---

## Compatibility

| | Supported |
|---|---|
| Server | Paper 1.20.6 and newer (tested on 1.20.6 and 26.x), Folia (experimental) |
| Proxy | Velocity 3.5+ and 4.x |
| Java | 21 or newer (Paper 26.x and Velocity 4.x need Java 25 themselves) |
| Database | H2 (built in, nothing to set up), MariaDB 10.6+, MySQL 8; move from H2 to MariaDB / MySQL later with one command |
| Permissions | Any permission plugin |

Not supported: Spigot / CraftBukkit, BungeeCord / Waterfall, Fabric, PostgreSQL, Minecraft older than 1.20.6.

| Your setup | Install on | Database |
|---|---|---|
| One Paper server | That server | H2 (default) |
| Velocity network | The proxy **and** every backend | One shared MariaDB / MySQL |

---

## Quick start

1. Drop `GraffBans.jar` into `plugins/` (on a network: proxy and every backend) and start once.
2. In `plugins/GraffBans/config.yml` set `server-name`, `language` (`en` / `tr`) and, on a network, the
   shared `database`.
3. Restart. Messages, templates and most settings reload with `/graffbans reload`.

A few commands to try:

```
/ban Steve 7d Griefing          /tempmute Alex 30m Spam        /warn Sam Caps lock
/ban Steve hacking              # uses the "hacking" ladder from templates.yml
/history Steve                  /dupeip Steve                  /lockdown Maintenance
```

Full lists: [Commands](https://github.com/g-raffie/GraffBans/wiki/Commands) ·
[Command syntax](https://github.com/g-raffie/GraffBans/wiki/Command-Syntax) ·
[Permissions](https://github.com/g-raffie/GraffBans/wiki/Permissions).

---

## Documentation

| Page | |
|---|---|
| [Installation](https://github.com/g-raffie/GraffBans/wiki/Installation) | Single server and network setup |
| [Configuration](https://github.com/g-raffie/GraffBans/wiki/Configuration) | Every `config.yml` section explained, including automatic actions on warnings |
| [Commands](https://github.com/g-raffie/GraffBans/wiki/Commands) | All commands |
| [Command Syntax](https://github.com/g-raffie/GraffBans/wiki/Command-Syntax) | Durations, flags, targets, punishment numbers |
| [Permissions](https://github.com/g-raffie/GraffBans/wiki/Permissions) | All permission nodes |
| [Templates](https://github.com/g-raffie/GraffBans/wiki/Templates) | Punishment ladders |
| [Networks](https://github.com/g-raffie/GraffBans/wiki/Networks) | Velocity, server scopes, sync |
| [Staff Limits](https://github.com/g-raffie/GraffBans/wiki/Staff-Limits) | Rank limits and cooldowns |
| [Messages](https://github.com/g-raffie/GraffBans/wiki/Messages) | Editing messages, placeholders, hover and click |
| [Switching from LiteBans](https://github.com/g-raffie/GraffBans/wiki/Switching-from-LiteBans) | One-command import |
| [Troubleshooting](https://github.com/g-raffie/GraffBans/wiki/Troubleshooting) | Common problems |

Default files: [`config.yml`](configs/config.yml) · [`messages_en.yml`](configs/messages_en.yml) ·
[`messages_tr.yml`](configs/messages_tr.yml) · [`templates.yml`](configs/templates.yml)

---

© graffie. All rights reserved.
