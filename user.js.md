# Thunderbird user.js

- [Maildir](#maildir)

## Maildir

> Optional support for [Maildir](https://en.wikipedia.org/wiki/Maildir) allows a single unique file per email (using the [EML](https://www.loc.gov/preservation/digital/formats/fdd/fdd000388.shtml) data format), unlike the default monolithic single file format of one file per folder known as [mbox](https://en.wikipedia.org/wiki/Mbox).
[[Thunderbird Help](https://support.mozilla.org/en-US/kb/maildir-thunderbird)]

To turn on Maildir:

```js
user_pref("mail.serverDefaultStoreContractID", "@mozilla.org/msgstore/maildirstore;1")
```

Or `Preferences` > `General` > `Message Store Type for new account`: `File per message (maildir)`

> Any email account created from now on will use Maildir. Existing accounts will continue to use mbox. If you want to use mbox again for new accounts, just reset the config variable.
[[Thunderbird Help](https://support.mozilla.org/en-US/kb/maildir-thunderbird)]

