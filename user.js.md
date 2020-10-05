# Thunderbird user.js

- [Maildir](#maildir)
- [Global Search](#global-search)
- [Web Beacon](#web-beacon)


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


## Global Search

```js
user_pref("mailnews.database.global.indexer.enabled", true);
```

`true` by default.

`Preferences` > `General` > `Enable Global Search and Indexer`: `ON`

> Global search queries all of your messages, regardless of the account the message is associated with or the folder where the message is stored.
[[Thunderbird Help](https://support.mozilla.org/en-US/kb/global-search)]

[Rebuilding the Global Database - Thunderbird Help](https://support.mozilla.org/en-US/kb/rebuilding-global-database)


## Web Beacon

```js
user_pref("beacon.enabled", false);
```

> A web beacon is an object embedded in a web page or email, which unobtrusively (usually invisibly) allows checking that a user has accessed the content. Common uses are email tracking and page tagging for web analytics.
[[Wikipedia](https://en.wikipedia.org/wiki/Web_beacon)]

> Example use cases of the Beacon API are logging activity and sending analytics data to the server.
[[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API)]

