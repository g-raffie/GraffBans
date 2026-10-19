# Security Policy

GraffBans handles bans, staff accounts and a web panel that can face the internet, so security reports get
priority over everything else.

## Reporting a vulnerability

**Please do not open a public issue for a security problem.** Report it privately instead:

1. Open the [Security tab](https://github.com/g-raffie/GraffBans/security) of this repository.
2. Click **Report a vulnerability** and describe what you found: the GraffBans version, how to reproduce it, and
   what an attacker could do with it.

You will get an answer within a few days. Once a fix is out, the report can be made public, with credit to you
if you want it.

## What counts

Everything that lets someone do what they should not be able to, for example:
- get into the web panel without a valid login, or with fewer rights get more (another role's abilities);
- run scripts in someone else's browser through the panel (XSS), or make them act without knowing (CSRF);
- read or change the database in ways the panel or commands do not allow (SQL injection);
- see player IP addresses where GraffBans promises to hide them;
- appeal or act on behalf of another player.

Problems that need the server owner to be careless (e.g. a weak panel password, the panel on plain HTTP after
the warning) are still worth a normal issue, but are not vulnerabilities.

## Supported versions

Only the newest beta release gets fixes. Update before reporting, if you can.
