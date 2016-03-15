# Brew Installations


```
eno:~ dave$ brew install dovecot --with-clucene --with-stemmer
...
==> Installing dovecot dependency: openssl
...
==> Caveats
A CA file has been bootstrapped using certificates from the system
keychain. To add additional certificates, place .pem files in
  /Users/dave/brew/etc/openssl/certs

and run
  /Users/dave/brew/opt/openssl/bin/c_rehash

This formula is keg-only, which means it was not symlinked into /Users/dave/brew.

Apple has deprecated use of OpenSSL in favor of its own TLS and crypto libraries

Generally there are no consequences of this for you. If you build your
own software and it requires this formula, you'll need to add to your
build variables:

    LDFLAGS:  -L/Users/dave/brew/opt/openssl/lib
    CPPFLAGS: -I/Users/dave/brew/opt/openssl/include
...
==> Installing dovecot
...
==> Caveats
For Dovecot to work, you may need to create a dovecot user
and group depending on your configuration file options.

To have launchd start dovecot at startup:
  sudo cp -fv /Users/dave/brew/opt/dovecot/*.plist /Library/LaunchDaemons
  sudo chown root /Library/LaunchDaemons/homebrew.mxcl.dovecot.plist
Then to load dovecot now:
  sudo launchctl load /Library/LaunchDaemons/homebrew.mxcl.dovecot.plist
...



Last login: Wed Jan  6 05:23:41 on ttys001
eno:~ dave$ brew install leafnode
...

eno:~ dave$ brew install isync
...
```










----------

* Create /Users/dave/Library/Data/mbsync
* dovecot can run entirely as me as long as all transports are disabled
* postfix should use transport to set up delivery to apple's smtp server for mail to apple.com addresses

* initsplit preloading isn't working; elhome is too complex.  In
  particular mime-settings aren't getting loaded up.  Possibly time to
  cut losses and merge all settings again.

* Maybe we should dump the Gnus "Download mark" since we are doing
  everything with local servers.
  
* the latest imaplib2 from pypi is still really outdated.

* TODO:
  * [ ] watch apple with idle as well as gmail
  * [ ] notifications
  * [x] Better insurance about the sender address
  * [ ] Smart word wrapping for articles that contain code.  See
    `gnus-article-foldable-buffer' and
    `gnus-article-fill-cited-article'.
  * [ ] eudc-ldap expands to last-name + email, omitting first name
  * [ ] automatically add "(off-list)" to subject lines
  * [ ] debug `A T' from a newsgroup buffer
  * [ ] make `W Q' indent code that it wraps.
  * [ ] handle mime viewers.  Consider qlmanage (quicklooks) as the
    default
  * [ ] why are scoring files still going into ~/News?
  * [ ] why is scoring not working in apple/INBOX, particularly for
    github commentary?
  * [ ] can I share score files across groups when threads cross INBOX/gmane?
  * [x] set up the disk image for leafnode, to conserve inodes.
  
    The leafnode launchagent can look for a file on the disk image, to
    know when to KeepAlive.
  * [ ] Create plists:
    * [ ] fetchnews: while true; do ~/brew/sbin/fetchnews -vvv ; sleep
      120 ; done
    * [ ] mbsync-idle: while true; do ~/src/onmsg/scripts/mbsync-idle
      ; sleep 10; done
    * [ ] dovecot: /Users/dave/brew/opt/dovecot/sbin/dovecot -F
    * [x] stunnel
