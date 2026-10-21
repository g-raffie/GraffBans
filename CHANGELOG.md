# Changelog

## Next beta (not released yet)

**Reports**
- `/report <player> <reason>` for every player; staff with `graffbans.notify.report` see it on every server.
- `/reports` to list, claim, resolve or reject reports in game; a **Reports** page in the panel.
- Punishing the reported player closes their open reports and links them to the punishment.
- New: `reports` section in config.yml (`enabled`, `cooldown`), `discord.reports-webhook`.

**Staff report**
- A **Staff** page in the panel: what each staff member did in the last day, week, month or 90 days, including
  punishments later overturned on appeal.
- Optional weekly summary in Discord: `discord.staff-report-webhook`, `staff-report-day`, `staff-report-time`.

**Upgrading**
- New panel abilities `reports` (claim and close reports) and `staff-report`. They are in the default roles; if you
  wrote your own `web.roles`, add them where you want them.
- New permissions: `graffbans.report` (everyone), `graffbans.reports`, `graffbans.notify.report` (op).
- New messages work without touching your messages file: missing ones come from the built-in defaults.

## 0.1.0-beta.1 (first public beta)

The first version anyone can try. Free during the beta; see [LICENSE.md](LICENSE.md).

**Punishments**
- Bans, mutes, warnings and kicks; timed or permanent; IP and IP-range punishments; silent and console-only modes.
- The commands and flags staff know from LiteBans, including `-s`, `-S`, `-d`, `-m`, `-N`, `server:`, punishment numbers.
- Templates with ladders, per-step reasons, messages, broadcasts and commands; template groups.
- Staff rank limits and cooldowns, staff notices, staff rollback, lockdown.
- Automatic actions at a number of warnings; default warning duration.

**Networks**
- One jar for Paper and Velocity; punishments reach every server within about two seconds over the shared database.
- Server-scoped punishments; an optional lobby that banned players may still join.

**Moving over**
- `/graffbans import litebans`: punishments, history, names, templates, permission nodes (H2, MySQL, MariaDB).
- `/graffbans import h2`: move from the built-in database to MariaDB / MySQL.

**Web panel** (off by default)
- Staff accounts tied to Minecraft accounts, roles, two-step login.
- Player and punishment pages; punish, change, remove and delete with the same rules as in game.
- Appeals with a code from the ban screen, conversation with staff, accept / reduce / deny, Discord notices.
- Public ban list, can be shown on your own site; your name, logo and colours.

**Other**
- English and Turkish messages, all editable (MiniMessage).
- An update check tells you about newer betas (`update-check` in config.yml).

Back up your database before installing. Please report problems in the
[issues](https://github.com/g-raffie/GraffBans/issues).
