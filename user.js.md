# Thunderbird user.js

- [Maildir](#maildir)
- [Global Search](#global-search)
- [Display HTML](#display-html)
- [Web Beacon](#web-beacon)
- [Telemetry](#telemetry)
  - [Health Report](#health-report)
  - [Data Submission](#data-submission)
- [Crash Reports](#crash-reports)


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


## Display HTML

```js
user_pref("mailnews.display.prefer_plaintext", false);
user_pref("mailnews.display.disallow_mime_handlers", 3);
user_pref("mailnews.display.html_as", 1);

```

Convert HTML, inline images and some other uncommon types to text and then back to HTML again.

Options of `prefer_plaintext`:\
`false` (default) - display a message as HTML when there is both a HTML and a plain text version of a message body,\
`true` - display a message as plain text when ...

Options of `disallow_mime_handlers`:\
`0` (default) - all classes can process incoming data,\
`1` - use hard-coded blacklist to avoid rendering HTML,\
`2` - ... and inline images,\
`3` - ... and some other uncommon types,\
`100` - use hard-coded whitelist.

Options of `html_as`:\
`0` (default) - display HTML normally,\
`1` - convert HTML to text and then back again,\
`2` - display HTML source,\
`3` - sanitize HTML,\
`4` - display all body parts.

[[HTML Sanitizer - Ben Bucksch](https://www.bucksch.org/1/projects/mozilla/108153/)]

[[Mail and news settings - mozillaZine](http://kb.mozillazine.org/Mail_and_news_settings)]

Original HTML = `prefer_plaintext`:`false` + `disallow_mime_handlers`:`0` + `html_as`:`0`

Simple HTML = `prefer_plaintext`:`false` + `disallow_mime_handlers`:>=`1` + `html_as`:`3`

Plain Text = `prefer_plaintext`:`true`

Note: applying `html_as` to `1` doesn't change the status of menu items "Original HTML / Simple HTML / Plain Text" and none of them is selected after restart of Thunderbird.


## Web Beacon

```js
user_pref("beacon.enabled", false);
```

> A web beacon is an object embedded in a web page or email, which unobtrusively (usually invisibly) allows checking that a user has accessed the content. Common uses are email tracking and page tagging for web analytics.
[[Wikipedia](https://en.wikipedia.org/wiki/Web_beacon)]

> Example use cases of the Beacon API are logging activity and sending analytics data to the server.
[[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API)]


## Telemetry

> The Telemetry feature provides this capability by sending performance and usage info to Mozilla. Telemetry measures and collects non-personal information, such as performance, hardware, usage and customizations. It then sends this information to Mozilla on a daily basis.
[[mozilla wiki](https://wiki.mozilla.org/Telemetry)]

[Telemetry - Firefox Source Docs](https://firefox-source-docs.mozilla.org/toolkit/components/telemetry/index.html)

[Telemetry - Firefox Data Documentation](https://docs.telemetry.mozilla.org/)


### Health Report

[Health Report (Firefox)](https://github.com/DonQuixoteI/Firefox-UserGuide/blob/master/doc/user.js.md#health-report)

```js
user_pref("datareporting.healthreport.uploadEnabled", false);
```


### Data Submission

```js
user_pref("datareporting.policy.dataSubmissionEnabled", false);
```

> This is the data submission master kill switch. If disabled, no policy is shown or upload takes place, ever.
[[Mozilla Source Tree Docs](https://firefox-source-docs.mozilla.org/main/65.0/toolkit/components/telemetry/telemetry/internals/preferences.html)]


## Crash Reports

```js
user_pref("browser.crashReports.unsubmittedCheck.autoSubmit2", true);
```

Default:
	n/a

`true` if
`Preferences` > `Privacy & Security` > `Allow Thunderbird to send backlogged crash reports on your behalf`: `ON`

```js
user_pref("breakpad.reportURL", "");
```

Default:
	"https://crash-stats.mozilla.com/report/index/"

Submitted crash reports are listed in
`Help` > `Troubleshooting Information`

[Mozilla Crash Reporter TB - Thunderbird Help](https://support.mozilla.org/en-US/kb/mozilla-crash-reporter-tb)
