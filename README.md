# Chat Assistant (Reviactyl port)

-# Coverted by shadowhacker597398

AI chat assistant for Reviactyl: log/config analysis, server-aware answers, optional human handoff via Discord webhook, staff replies from the admin page. Ported from the Blueprint `aichat` extension (YadduNetwork).

## Install

```bash
cd /var/www/reviactyl
php artisan extension:install chat-assistant
php artisan extension:enable chat-assistant
php artisan optimize:clear
```

Configure under Admin Panel -> Settings -> Extensions -> Chat Assistant (providers, Discord webhook, support sessions). `data/` must be writable by the web user. Back up `data/settings.json` before updating: replacing extension files can overwrite it.

## Where it appears

- Server sidebar: "AI Assistant" (server auto-selected, console context included)
- Dashboard: "AI Assistant" (pick a server from the dropdown)

## Unverified against a live panel

1. The `dashboardRouter` entry uses the same keys as `serverRouter`; if install rejects it, delete that block from `extension.json`.
2. `/admin-reply` (external bot webhook) only works if extension routes are not behind session auth.
3. Identity and server access are checked through the panel's own `/api/client/account` and `/api/client/servers/{id}` with forwarded cookies (same method as DB Manager).
4. Server context reads the `servers`, `eggs`, `server_variables`, `egg_variables` tables (standard Pterodactyl schema).
5. Nav icon `RiChat3Line`.

## Notes

- API keys and the webhook secret are encrypted at rest with the panel APP_KEY. Changing APP_KEY invalidates them (re-enter in admin).
- Env var values whose names contain PASS/SECRET/TOKEN/KEY/PRIVATE are masked before being sent to the AI provider.
