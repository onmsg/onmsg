# Local Dovecot Starter Kit

This repository contains instructions and configuration for setting up
a kick-ass local [Dovecot](http://wiki2.dovecot.org) IMAP server that
synchronizes with your remote IMAP server.

These instructions use GMail as a remote IMAP server and assume you're
on a Mac, but they can easily be adapted to other platforms and
configurations.

## Strategy

* Build and install Dovecot with powerful search/indexing capability enabled
* Use Dovecot's super-efficient
  [mdbox](http://wiki2.dovecot.org/MailboxFormat/dbox) mail storage format
* Use [mbsync](http://isync.sourceforge.net/mbsync.html) to keep your
  local and remote mailboxes in sync.
* Run a daemon that uses IMAP IDLE to be notified of changes to the
  remote mailbox requiring a sync.

## Packages

On some platforms (e.g. [MacPorts](http://macports.org) and
[Homebrew](http://mxcl.github.com/homebrew/), as of this writing, the
official build recipes and packages are not enough to create the
system described here.  Details below.

### MacPorts

If you're using [MacPorts](http://macports.org), you'll want to set up
a local portfile repository.  The
[instructions](](http://guide.macports.org/chunked/development.local-repositories.html)
suggest that you put the repository below your home directory, but
that will [lead](https://trac.macports.org/ticket/36950) either to
great frustration with permissions, or to you unnecessarily relaxing
permissions, potentially compromising security.

Adding the following line *before the default repository* in
`/opt/local/etc/macports/sources.conf` will tell MacPorts to look
first in a local repository at `/Library/Portfiles/localhost`:

```
file:///Library/Portfiles/localhost
```

I set mine up this way:

```sh
sudo mkdir -p /Library/Portfiles/localhost
chown -R "${USER}:everyone" /Library/Portfiles
```
For my convenience, I also added a symlink as follows (totally optional):

```sh
ln -s /opt/local/var/macports/sources/rsync.macports.org/release/tarballs/ports \
   /Library/Portfiles/rsync.macports.org
```

Then you'll want to copy
[`mail/dovecot2`](https://github.com/dabrahams/Portfiles/tree/master/mail/dovecot2) (),
[`mail/isync`](https://github.com/dabrahams/Portfiles/tree/master/mail/isync),
and
[`textproc/libstemmer`](https://github.com/dabrahams/Portfiles/tree/master/textproc/libstemmer)
from
[my personal Portfile repository](http://github.com/dabrahams/Portfiles)
to the corresponding locations under `/Library/Portfiles/`, and then,
in that directory, execute

```sh
portindex
```

And finally (anywhere),

```sh
sudo port install dovecot2 +lucene +libstemmer
sudo port install isync-devel
```

For the rest of these instructions, you should read `$PREFIX` as `/opt/local`.

Note: MacPorts tickets for the updated Portfiles are here:
[dovecot](https://trac.macports.org/ticket/36954),
[isync](https://trac.macports.org/ticket/36959),
[libstemmer](https://trac.macports.org/ticket/36952)

## Homebrew

I am not currently using Homebrew for this, but I did create the
necessary Formulae at one point:

* [dovecot](https://github.com/dabrahams/homebrew/blob/master/Library/Formula/dovecot.rb)
* [libstemmer](https://github.com/dabrahams/homebrew/blob/master/Library/Formula/libstemmer.rb)
* [isync](https://github.com/dabrahams/homebrew/blob/master/Library/Formula/isync.rb)

## Packages

I am using MacPorts, and as such have 
[Homebrew](http://mxcl.github.com/homebrew/).  

## IMAP Idle

Idle is the feature that notifies us when something changes on the
IMAP server.  We use it to get timely updates when there are changes.

* You'll need imaplib2.  I did `sudo easy_install pip` and then `pip
  install imaplib2`.  If you're fetishistic about purity, install it
  in a [virtualenv](http://www.virtualenv.org) environment.
  
* In the [scripts/ subdirectory](file://scripts) of this repository
  you'll find `mbsync-idle-trigger`, which monitors Gmail's
  “[Gmail]/All Mail” folder for changes, and initiates a sync whenever
  something changes there.  It also syncs when it starts up, and every
  5 minutes thereafter, just in case.

## MacPorts Users

If I was starting over from scratch, I'd probably use
[MacPorts](http://macports.org) instead of Homebrew to install Dovecot
and other tools because of its far greater maturity, but I didn't know
that before I tried Homebrew.  If you're already a MacPorts user, rest
assured that Homebrew and MacPorts [can
coexist](https://github.com/mxcl/homebrew/wiki/Homebrew-0.9.3).  I'm
using them that way now, but I may eventually transition to using
Homebrew as a better [stow](http://www.gnu.org/software/stow/).


