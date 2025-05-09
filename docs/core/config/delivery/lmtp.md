---
layout: doc
title: LMTP
dovecotlinks:
  lmtp: LMTP Server
---

# LMTP Server

LMTP is a network-available service that handles local delivery of messages.

It is defined by [[rfc,2033]].

The main difference from LDA is that the LDA is a short-running process,
started as a binary from command line, while LMTP is a long-running process
started by Dovecot's master process.

::: tip
LMTP is the recommended method for mail delivery for most installations.
:::

<!-- @include: include/delivery_common.inc -->

## Envelope Addresses

Compared to dovecot-lda parameters, the addresses are taken from:

| LDA Flag | LMTP Command | Description |
| -------- | ------------ | ----------- |
| `-f` | `MAIL FROM:` | Envelope sender address |
| `-r` | `RCPT TO:` | Final envelope recipient address |
| `-a` | `RCPT TO:`, but may be overridden by [[setting,lda_original_recipient_header]] | Original envelope recipient address |
| `-d` | `RCPT TO:`, but with the `+extension` part removed when [[setting,recipient_delimiter]] is enabled | Destination username. If usernames differ from recipient email addresses, the userdb must handle the translation. |

## Listeners

You can configure LMTP to be listening on TCP or UNIX sockets:

::: tip
By general convention, LMTP is expected to listen on port 24.
:::

```[dovecot.conf]
# add lmtp to protocols, otherwise its listeners are ignored
protocols = {
  lmtp = yes
}

service lmtp {
  inet_listener lmtp {
    address = 192.168.0.24 127.0.0.1 ::1
    port = 24
  }

  unix_listener lmtp {
    #mode = 0666
  }
}
```

The UNIX listener on `$base_dir/lmtp` is enabled by default when
protocols setting contains lmtp.

## Security

Unfortunately LMTP process currently needs to run as root, and only
temporarily drop privileges to users. Otherwise it couldn't handle mail
deliveries to more than a single user with different UID.

If you're using only a single global UID/GID (i.e. virtual users), you can
improve security by running lmtp processes as that user:

```[dovecot.conf]
service lmtp {
  user = vmail
}
```

## LMTP Proxying

It's possible to use Dovecot LMTP server as a proxy to remote LMTP or
SMTP servers.

The configuration is similar to [[link,authentication_proxies]], but you'll
need to tell Dovecot LMTP to issue passdb lookups: [[setting,lmtp_proxy,yes]].

## Performance

For higher volume sites, it may be desirable to increase the number of
active listener processes. A range of 5 - 20 is probably good for most sites:

```[dovecot.conf]
service lmtp {
  process_min_avail = 5
}
```

## Logging

If you want to store LMTP delivery logs to a different file, you can do
it with:

```[dovecot.conf]
service lmtp {
  executable = lmtp -L
}

protocol lmtp {
  info_log_path = /var/log/dovecot-lmtp.log
}
```

For rawlogs, please see [[link,rawlog]].

## Plugins

* Most of the Dovecot plugins work with LMTP.

* Virtual quota can be enforced using [[plugin,quota]].

  * [[setting,lmtp_rcpt_check_quota,yes]] enables quota checking already
    at RCPT TO stage. This check isn't done for proxied connections.

* Sieve language support can be added with [[link,sieve]].

## Address Extension Delivery

To make address extension work with LMTP you must check that these variables
are set:

* [[setting,lmtp_save_to_detail_mailbox,yes]]
* [[setting,recipient_delimiter,+]]

## Using LMTP with different MTAs

* Browse the How To section: [[link,howto]]
* [Halon](https://docs.halon.io/kb/delivery/lmtp)
