# Responsive, always-available messaging for hackers

This repository contains instructions and configuration for setting up
a kick-ass local [Dovecot](http://wiki2.dovecot.org) IMAP server, that
synchronizes with your remote IMAP server, and a local
[leafnode](http://leafnode.sourceforge.net) server for your NNTP
newsgroups.

These instructions use GMail as a remote IMAP server and assume you're
on a Mac, but they can easily be adapted to other platforms and
configurations.  They'll be especially useful if you're a Gnus user.

## Strategy

* Build and install Dovecot with powerful search/indexing capability enabled
* Use Dovecot's super-efficient
  [mdbox](http://wiki2.dovecot.org/MailboxFormat/dbox) mail storage format
* Use [mbsync](http://isync.sourceforge.net/mbsync.html) to
  *efficiently* keep your local and remote mailboxes in sync.
* Run a daemon that uses IMAP IDLE to be notified of changes to the
  remote mailbox requiring a sync.

## Installation on MacOS

1. Install [HomeBrew](https://brew.sh)
2. Review the [brew-install script](https://github.com/onmsg/onmsg/blob/master/scripts/brew-install) and make sure it isn't doing anything specific to my setup that you want to change.  The script contains several constants at the beginning that you may want to tweak, and one (my GMail username) that you'll *definitely* want to tweak.
3. Run the [brew-install script](https://github.com/onmsg/onmsg/blob/master/scripts/brew-install) from the root of this repository: `scripts/brew-install`

## Certificates

https://wiki.archlinux.org/index.php/Isync describes how to set up
some certs for your .mbsync file
