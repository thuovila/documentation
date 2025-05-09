---
layout: doc
title: doveadm-mailbox-cryptokey
dovecotComponent: core
---

# doveadm-mailbox-cryptokey(1) - Mail crypt plugin management

## SYNOPSIS

**doveadm**
  [**-o** *crypt_user_key_password=password*]
  [*GLOBAL OPTIONS*]
  *mailbox cryptokey export|generate|list|password*
  [*options*]
  [*arguments*]

## DESCRIPTION

Generate new keypair for user or folder. The new keypair is marked as
active.

## OPTIONS

**doveadm mailbox cryptokey** can be used to manage user's cryptographic keys.

<!-- @include: include/global-options-formatter.inc -->

**-o** *crypt_user_key_password=password*
:   Dovecot option, needed if you use password protected keys

## OPTIONS

<!-- @include: include/option-A.inc -->

<!-- @include: include/option-F-file.inc -->

<!-- @include: include/option-no-userdb-lookup.inc -->

<!-- @include: include/option-S-socket.inc -->

<!-- @include: include/option-u-user.inc -->

## SUBCOMMANDS

**export** [**-U**] | *mailbox-mask*

**-U**
:   Operate on user keypair only

Exports user's or folder's keypair(s) in PEM format. If the keys are
password protected, -o is needed.

**generate** [**-Rf** [**-U**] | *mailbox-mask*]

**-U**
:   Operate on user keypair only

**-R**
:   Re-encrypt all folder keys with current active user key

**-f**
:   Force keypair creation, normally keypair is only created if none
    found

Generates new keypair for user or folder. If you want to generate new
user key and use it to secure your folder keys, use generate -u username
-UR.

If you want to password-protect your key here, use -o.

**list** [**-U**] | *mailbox-mask*

**-U**
:   Operate on user keypair only

List all keys for user or folder. No password is required.

**password** [**-N** | **-n** *password*] [**-O**|**-o** *password*] [**-C**]

**-O**
:   Ask for old password

**-o old-password**
:   Provide old password

**-N**
:   Ask for new password

**-n new-password**
:   Provide new password

**-C**
:   Clear (unset/remove) password. Your key will not be protected by password.

Set, change or clear password from your user key.

## SEE ALSO

[[man,doveadm]], [[man,doveadm-mailbox]]
