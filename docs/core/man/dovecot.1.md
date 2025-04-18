---
layout: doc
title: dovecot
dovecotComponent: core
---

# dovecot(1) - A secure and highly configurable IMAP and POP3 server

## SYNOPSIS

**dovecot** [**-Fp**] [**-c** *config-file*]

**dovecot -a** [**-c** *config-file*]

**dovecot -n** [**-c** *config-file*]

**dovecot -\-build-options**

**dovecot -\-help**

**dovecot -\-hostdomain**

**dovecot -\-version**

**dovecot reload**

**dovecot stop**

## DESCRIPTION

Dovecot is an open source IMAP and POP3 server for Linux/UNIX-like
systems, written with security primarily in mind. Dovecot is an
excellent choice for both small and large installations. It's fast,
simple to set up, requires no special administration and it uses very
little memory.

## OPTIONS

**-a**
:   Dump all configuration settings to stdout and exit successfully. The
    same as *doveconf -a*.

**-c** *config-file*
:   Start **dovecot** with an alternative configuration.

**-F**
:   Run **dovecot** in foreground, do not daemonize.

**-n**
:   Dump non-default settings to stdout and exit successfully. The same
    as *doveconf -n*.

**-p**
:   Prompt for the ssl key password for the configured *ssl_server_key* on startup.

**-\-build-options**
:   Show Dovecot's build options and exit successfully.

**-\-help**
:   Print a usage message to stdout and exit successfully.

**-\-hostdomain**
:   Shows the current *host*.*domain* name of the system. If the domain
    lookup should fail for some reason, only the hostname will be shown.

**-\-version**
:   Show Dovecot's version and exit successfully.

## COMMANDS

**reload**
:   Force **dovecot** to reload its configuration.

**stop**
:   Shutdown **dovecot** and all its child processes.

When *shutdown_clients* is set to **no**, existing sessions will
continue to use the old settings, after a **dovecot reload**. Also all
sessions will keep alive after a **dovecot stop**.

By default all active sessions will be shut down.

## SIGNALS

Dovecot handles the following *signals* as described:

**HUP**
:   Force **dovecot** to reload its configuration.

**INT**
:   Shutdown **dovecot** and all its child processes.

**TERM**
:   Shutdown **dovecot** and all its child processes.

**USR1**
:   Force **dovecot** to reopen all configured log files (*log_path*,
    *info_log_path* and *debug_log_path*).

The *signals* **ALARM** and **PIPE** are ignored.

## FILES

*/etc/dovecot/dovecot.conf*
:   Dovecot's main configuration file.

*/etc/dovecot/conf.d/*.conf*
:   Configuration files of different services and settings.

<!-- @include: include/reporting-bugs.inc -->

## SEE ALSO

[[man,doveadm]]
