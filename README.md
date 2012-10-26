# Local Dovecot Starter Kit

This repository contains instructions and configuration for setting up
a kick-ass local [Dovecot](http://wiki2.dovecot.org) IMAP server that
synchronizes with your remote IMAP server.

These instructions use GMail as a remote IMAP server and assume you're
on a Mac using [Homebrew](http://mxcl.github.com/homebrew/).  

## MacPorts Users

If I was starting over from scratch, I'd probably use
[MacPorts](http://macports.org) instead of Homebrew to install Dovecot
and other tools because of its far greater maturity, but I didn't know
that before I tried Homebrew.  If you're already a MacPorts user, rest
assured that Homebrew and MacPorts [can
coexist](https://github.com/mxcl/homebrew/wiki/Homebrew-0.9.3).  I'm
using them that way now, but I may eventually transition to using
Homebrew as a better [stow](http://www.gnu.org/software/stow/).

## Strategy

* Build and install Dovecot with powerful search/indexing capability enabled
* Use Dovecot's super-efficient
  [mdbox](http://wiki2.dovecot.org/MailboxFormat/dbox) mail storage format
* Use [mbsync](http://isync.sourceforge.net/mbsync.html) to keep your
  local and remote mailboxes in sync

