# Roundcube Catch-all Plugin

Roundcube plugin for **catch-all mailboxes** — inboxes that receive mail at
many different local-parts on a domain (e.g. `anything@example.com`).

## What it does

- **Identity auto-create on reply** — when you reply to a message that was
  delivered to `foo@example.com`, the plugin creates a matching Roundcube
  identity (if missing) and preselects it as the `From:` header so your
  reply goes out as `foo@`, not the base mailbox address.
- **Catch-all SMTP password** — when a domain-wide catch-all password is
  configured, the plugin authenticates SMTP as the identity's email address
  so ForwardEmail (and similar providers) accept the send.
- **Optional [Forward Email](https://forwardemail.net) API integration** —
  when an API key is set, the plugin additionally provisions per-alias SMTP
  credentials via the Forward Email API so each reply authenticates as
  its own alias.

For persistent login / autologin, see the companion plugin
[`teh-hippo/roundcube-remember-me`](https://github.com/teh-hippo/roundcube-remember-me).

## Installation

### Via Composer (once published)

```bash
composer require teh-hippo/roundcube-catchall
```

Then add `roundcube_catchall` to the `plugins` array in your Roundcube config.

### Manual

```bash
cd /path/to/roundcube/plugins
git clone https://github.com/teh-hippo/roundcube-catchall.git roundcube_catchall
```

Add `roundcube_catchall` to `$config['plugins']` in `config/config.inc.php`.

## Configuration

### Catch-all password (recommended)

The simplest setup: generate a domain catch-all password from your mail
provider's settings (e.g.
[Forward Email advanced settings](https://forwardemail.net/my-account/domains)),
then enter it in **Settings > Catch-all** in the Roundcube UI, or set it in
config:

```php
$config['catchall_domain'] = 'example.com';
$config['catchall_identity_autocreate'] = true;
$config['catchall_fe_catchall_password_plain'] = '…';
```

### Per-alias API provisioning (alternative)

For fine-grained control, use a Forward Email API key to auto-create
per-alias SMTP passwords:

```php
$config['catchall_domain'] = 'example.com';
$config['catchall_fe_api_key_plain'] = '…';
$config['catchall_fe_auto_delete']   = false;
```

Users can also set per-user credentials under **Settings > Catch-all** in
the Roundcube UI.

## Requirements

- Roundcube 1.6+
- PHP 8.0+ with curl extension (Forward Email API features only)

## License

MIT
