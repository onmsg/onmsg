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

## Packages

The official build recipies and packages for
[MacPorts](http://macports.org) and
[Homebrew](http://mxcl.github.com/homebrew/) are not enough to create
the system described here.  Details below.

### MacPorts

If you're using [MacPorts](http://macports.org), you'll want to set up
a local portfile repository.  The
[instructions](http://guide.macports.org/chunked/development.local-repositories.html)
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
sudo chown -R "${USER}:everyone" /Library/Portfiles
```
For my convenience, I also added a symlink as follows (totally optional):

```sh
ln -s /opt/local/var/macports/sources/rsync.macports.org/release/tarballs/ports \
   /Library/Portfiles/rsync.macports.org
```

Then you'll want to copy
[`mail/dovecot2`](https://github.com/dabrahams/Portfiles/tree/master/mail/dovecot2),
and [`mail/isync`](https://github.com/dabrahams/Portfiles/tree/master/mail/isync),
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
[isync](https://trac.macports.org/ticket/36959).

## Homebrew

I am not currently using Homebrew for this, but I did create the
necessary Formulae at one point:

* [dovecot](https://github.com/dabrahams/homebrew/blob/master/Library/Formula/dovecot.rb)
* [libstemmer](https://github.com/dabrahams/homebrew/blob/master/Library/Formula/libstemmer.rb)
* [isync](https://github.com/dabrahams/homebrew/blob/master/Library/Formula/isync.rb)


## IMAP Idle

Idle is the feature that notifies us when something changes on the
IMAP server.  We use it to get timely updates when there are changes.

* You'll need imaplib2.  I did `sudo easy_install pip` and then `pip
  install imaplib2`.  If you have [Growl](http://growl.info)
  installed, also `pip install gntp` and you'll get notifications when
  new mail arrives.  If you're fetishistic about purity, install these
  in a [virtualenv](http://www.virtualenv.org) environment.
  
* In the [scripts/ subdirectory](file://scripts) of this repository
  you'll find
  [`mbsync-idle-trigger`](file://scripts/mbsync-idle-trigger), which
  monitors Gmail's “[Gmail]/All Mail” folder for changes and initiates
  a sync whenever something changes there.  It also syncs when it
  starts up, and every 5 minutes thereafter, just in case.

* The script contains several constants at the beginning that you may
  want to tweak, and one (my GMail username) that you'll *definitely*
  want to tweak.


