ssh-askpass
===========

ssh-askpass for macOS. Works under Sierra.

Used to accept (or deny) the use of the private key(s) added to the SSH authentication agent with `ssh-add -c`.

![Screenshot](https://github.com/theseal/ssh-askpass/raw/master/sample/ssh-askpass.png)

## Installation

### [Homebrew](http://brew.sh/)
* Run:

    ```
    brew update
    brew tap tmaher/ssh-askpass
    brew install ssh-askpass
    ```
* Follow caveats. To re-read the caveats after installation,
`brew info ssh-askpass`.

* To uninstall, run these and then logout/login or reboot.
    ```
    brew uninstall ssh-askpass
    brew services stop ssh-askpass
    ```

### Without Homebrew (NOT RECOMMENDED)

#### OS X Pre 10.11
```
$ sudo ln -s $PWD/ssh-askpass /usr/libexec/ssh-askpass
```
#### OS X 10.11+
* [Disable SIP (rootless)](http://www.imore.com/el-capitan-system-integrity-protection-helps-keep-malware-away)
* Run:

    ```
    $ sudo mkdir -p /usr/X11R6/bin
    $ sudo ln -s $PWD/ssh-askpass /usr/X11R6/bin/ssh-askpass
    ```
* [Enable SIP (rootless)](http://www.imore.com/el-capitan-system-integrity-protection-helps-keep-malware-away)

## Enabling keyboard navigation
For security reasons ssh-askpass defaults to cancel since it's too easy to
press spacebar and accept a connection or other actions which might use
ssh-keys. To make it easier to press `OK`:

* Go to `System Preferences` and then `Keyboard`.

#### OS X Pre 10.11
* Under the `Keyboard` tab, click on `All controls`.

#### OS X 10.11+
* Under the `Shortcuts` tab, click on `All controls`.

Now you can press ⇥+spacebar to press `OK`.

## About Sierra and auto-loading keys
Prior to Sierra (10.12), macOS would attempt to auto-load private keys into
`ssh-agent`. That functionality was removed in Sierra, and now users are
on the hook for loading keys themselves. Usually the advice is something
like adding `ssh-add -K` to `~/.bashrc`, or more sophisticated things like
[adding a custom LaunchAgent](https://github.com/jirsbek/SSH-keys-in-macOS-Sierra-keychain).

This Homebrew formula follows the LaunchAgent approach. In order for
confirmation-on-use to work, when a private key is loaded into agent, a
flag must be set indicating confirmation-on-use is required. After setting
the necessary environment variables and restarting `ssh-agent`, the command
`ssh-add -cA` is run. This auto-loads the keys (as was done prior to Sierra),
and marks them as requiring confirmation-on-use. The difference between
`ssh-add -A` and `ssh-add -K` is that while the latter will only load
keys if they're unencrypted or there's a passphrase in Keychain. `-K` is
similar, except upon encountering an encrypted key without a Keychain
passphrase, user is prompted via TTY for the passphrase, which is saved to
Keychain. `-A`, being non-interactive, is more appropriate for LaunchAgents.

See https://developer.apple.com/library/content/technotes/tn2449/_index.html
for some additional details on Apple's OpenSSH changes in Sierra.

## License
ISC license

## Contributors
* [theseal](https://github.com/theseal)
* [simmel](https://github.com/simmel)
* [tmaher](https://github.com/tmaher)
