# Thunderbird user.js

- [Setup](#setup)
  - [Account Setup](#account-setup)
  - [Maildir](#maildir)
  - [Global Search](#global-search)
  - [Start Page](#start-page)
  - [Wrap Long Lines](#wrap-long-lines)
- [Appearance](#appearance)
  - [Condensed Addresses](#condensed-addresses)
  - [Show User Agent](#show-user-agent)
  - [Show Sender](#show-sender)
- [Hardening Thunderbird](#hardening-thunderbird)
  - [Display HTML](#display-html)
  - [Inline Attachments](#inline-attachments)
  - [Remote Content](#remote-content)
  - [Web Beacon](#web-beacon)
  - [Online Storage](#online-storage)
  - [Cookies](#cookies)
  - [JavaScript](#javascript)
  - [Send Referer](#send-referer)
  - [Show Punycode](#show-punycode)
  - [First-Party Isolation](#first-party-isolation)
  - [Google Safe Browsing](#google-safe-browsing)
  - [Telemetry](#telemetry)
    - [Health Report](#health-report)
    - [Data Submission](#data-submission)
  - [Crash Reports](#crash-reports)
  - [Privacy](#privacy)
    - [Your IP Address](#your-ip-address)
    - [Your User Agent](#your-user-agent)
- [Extensions](#extensions)
  - [Extension Blocklist](#extension-blocklist)
  - [Add-on Metadata Updates](#add-on-metadata-updates)
  - [Extension Updates](#extension-updates)


## Setup

### Account Setup

> All the lookup mechanisms use the email address domain as base for the lookup. For example, for the email address fred@example.com, the lookup is performed as (in this order):
> 1. (_thunderbird-install-dir_)`/isp/example.com.xml` on the harddisk
> 2. check for autoconfig.example.com - https://autoconfig.example.com/mail/config-v1.1.xml?emailaddress=fred@example.com
> 3. look up of "example.com" in the ISPDB - https://autoconfig.thunderbird.net/v1.1/
> 4. look up "MX example.com" in DNS, and for mx1.mail.hoster.com, look up "hoster.com" in the ISPDB
> 5. try to guess (imap.example.com, smtp.example.com etc.)
[[Autoconfiguration in Thunderbird - MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Thunderbird/Autoconfiguration)]

[Privacy-freundliche Konfiguration des Thunderbird Assistenten - Privacy Handbuch](https://www.privacy-handbuch.de/handbuch_31m.htm)


### Maildir

> Optional support for [Maildir](https://en.wikipedia.org/wiki/Maildir) allows a single unique file per email (using the [EML](https://www.loc.gov/preservation/digital/formats/fdd/fdd000388.shtml) data format), unlike the default monolithic single file format of one file per folder known as [mbox](https://en.wikipedia.org/wiki/Mbox).
[[Thunderbird Help](https://support.mozilla.org/en-US/kb/maildir-thunderbird)]

To turn on Maildir:

```js
user_pref("mail.serverDefaultStoreContractID", "@mozilla.org/msgstore/maildirstore;1")
```

Or `Preferences` > `General` > `Message Store Type for new account`: `File per message (maildir)`

> Any email account created from now on will use Maildir. Existing accounts will continue to use mbox. If you want to use mbox again for new accounts, just reset the config variable.
[[Thunderbird Help](https://support.mozilla.org/en-US/kb/maildir-thunderbird)]


### Global Search

```js
user_pref("mailnews.database.global.indexer.enabled", true);
```

`true` by default.

`Preferences` > `General` > `Enable Global Search and Indexer`: `ON`

> Global search queries all of your messages, regardless of the account the message is associated with or the folder where the message is stored.
[[Thunderbird Help](https://support.mozilla.org/en-US/kb/global-search)]

[Rebuilding the Global Database - Thunderbird Help](https://support.mozilla.org/en-US/kb/rebuilding-global-database)


### Start Page

```js
user_pref("mailnews.start_page.enabled", false);
user_pref("mailnews.start_page.url", "");
```

[[Privacy Handbuch](https://www.privacy-handbuch.de/handbuch_31d.htm)] (in German)


### Wrap Long Lines

```js
user_pref("mailnews.wraplength", 0);
```

Default:
`72`

`0` - wrap to window,
`-1` - does not wrap.


## Appearance

### Condensed Addresses

```js
user_pref("mail.showCondensedAddresses", false);
```

> `true` (default) - show just the display name for people in the address book,\
`false` - show both the email address and display name.
[[mozillaZine](http://kb.mozillazine.org/Mail_and_news_settings)]


### Show User Agent

To show User-Agent header in message pane:

```js
user_pref("mailnews.headers.showUserAgent", true);
```

By default Thunderbird is sending information about your OS and version of your E-mail client included into the User Agent. To prevent sending User-Agent info, see [Your User Agent](#your-user-agent).


### Show Sender

To show Sender header in message pane:

```js
user_pref("mailnews.headers.showSender", true);
```

`false` by default.

> `Sender:` Address of the sender acting on behalf of the author listed in the `From:` field (secretary, list manager, etc.).
[[Wikipedia](https://en.wikipedia.org/wiki/Email#Message_header)]


## Hardening Thunderbird

### Display HTML

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


### Inline Attachments

```js
user_pref("mail.inline_attachments", false);
```

> `true` - show inlinable attachments (text, images, messages) after the message,\
`false` - do not display any attachments with the message.
[[mozillaZine](http://kb.mozillazine.org/Mail_and_news_settings)]

The option should be set to `false` in order to avoid automatic opening dangerous attachments during reading emails.
[[Privacy Handbuch](https://www.privacy-handbuch.de/handbuch_31d.htm)] (in German)


### Remote Content

> Email messages can contain remote content such as images or stylesheets. To protect your privacy, Thunderbird does not load remote content automatically, but instead shows a notification bar to indicate that it blocked remote content.
[[Thunderbird Help](https://support.mozilla.org/en-US/kb/remote-content-in-messages)]

```js
user_pref("mailnews.message_display.disable_remote_image", true);
```

`true` by default.

`Preferences` > `Privacy & Security` > `Allow remote content in messages`: `OFF`


### Online Storage

> Thunderbird allows you to upload attachments to an online storage service and then replaces the attachment in the message with a link.
[[Thunderbird Help](https://support.thunderbird.net/kb/filelink-large-attachments)]

```js
user_pref("mail.cloud_files.enabled", false);
```

[[Thunderbird Filelink - Privacy Handbuch](https://privacy-handbuch.de/handbuch_31h.htm)] (in German)


### Web Beacon

```js
user_pref("beacon.enabled", false);
```

> A web beacon is an object embedded in a web page or email, which unobtrusively (usually invisibly) allows checking that a user has accessed the content. Common uses are email tracking and page tagging for web analytics.
[[Wikipedia](https://en.wikipedia.org/wiki/Web_beacon)]

> Example use cases of the Beacon API are logging activity and sending analytics data to the server.
[[MDN](https://developer.mozilla.org/en-US/docs/Web/API/Beacon_API)]


### Cookies

```js
user_pref("network.cookie.cookieBehavior", 2);
```

`0` (default) - accept all cookies,
`1` - only accept from the originating site (block third-party cookies),
`2` - block all cookies,
`3` - block third-party cookies from unvisited sites,
`4` - block cookies from trackers.

[Cookies Preferences in Mozilla - MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Cookies_Preferences)

`Preferences` > `Privacy & Security` > `Accept cookies from sites`: `OFF`

Note: [CardBook](https://addons.thunderbird.net/en-US/thunderbird/addon/cardbook/) extension changes `cookieBehavior` to `1`.


### JavaScript

```js
user_pref("javascript.enabled", false);
```

> JavaScript is disabled for message content but not for RSS news feeds.
[[ArchWiki](https://wiki.archlinux.org/index.php/thunderbird),
[MDN](https://developer.mozilla.org/en-US/docs/Mozilla/Thunderbird/Releases/3#JavaScript)]


### Send Referer

```js
user_pref("network.http.sendRefererHeader", 0);
```

> Controls whether or not to send a referrer regardless of origin.\
`0` - never send the Referer header,\
`1` - send the Referer when clicking on a link,\
`2` (default) - send the Referer header when clicking on a link or loading an image.
[[mozillaZine](http://kb.mozillazine.org/Network.http.sendRefererHeader), [mozilla wiki](https://wiki.mozilla.org/Security/Referrer)].

[Improve online privacy by controlling referrer information - gHacks Tech News](https://www.ghacks.net/2015/01/22/improve-online-privacy-by-controlling-referrer-information/)


### Show Punycode

> Using Punycode, host names containing Unicode characters are transcoded to a subset of ASCII consisting of letters, digits, and hyphens [[Wikipedia](https://en.wikipedia.org/wiki/Punycode)].

```js
user_pref("network.IDN_show_punycode", true);
```

[Internationalized Domain Name (IDN) homograph attack - wikipedia](https://en.wikipedia.org/wiki/IDN_homograph_attack)

[Punycode Phishing Attacks - thehackernews.com](https://thehackernews.com/2017/04/unicode-Punycode-phishing-attack.html)


### First-Party Isolation

> The feature restricts cookies, cache and other data access to the domain level so that only the domain that dropped the cookie or file on the user system can access it.
[[gHacks](https://www.ghacks.net/2017/11/22/how-to-enable-first-party-isolation-in-firefox/)]

```js
user_pref("privacy.firstparty.isolate", true);
```


## Google Safe Browsing

> Google's Safe Browsing service aims to protect against phishing websites and malicious downloads. It works by sending hashes of some visited website addresses to Google. If Google reports that the page is unsafe, the page or file will not be downloaded or displayed to protect the user against malware and data theft. Google states that it cannot derive the full website addresses from the information submitted as it only sends a partial URL fingerprint. Safe Browsing can be disabled entirely if the trade-off between privacy and security is not acceptable.
[[GOV.UK](https://www.gov.uk/government/publications/browser-security-guidance-mozilla-firefox/browser-security-guidance-mozilla-firefox#enterprise-considerations)]

```js
user_pref("browser.safebrowsing.blockedURIs.enabled", false);
user_pref("browser.safebrowsing.downloads.enabled", false);
user_pref("browser.safebrowsing.downloads.remote.block_dangerous", false);
user_pref("browser.safebrowsing.downloads.remote.block_dangerous_host", false);
user_pref("browser.safebrowsing.downloads.remote.block_potentially_unwanted", false);
user_pref("browser.safebrowsing.downloads.remote.block_uncommon", false);
user_pref("browser.safebrowsing.downloads.remote.enabled", false);
user_pref("browser.safebrowsing.downloads.remote.url", "");
user_pref("browser.safebrowsing.malware.enabled", false);
user_pref("browser.safebrowsing.phishing.enabled", false);
```


### Telemetry

> The Telemetry feature provides this capability by sending performance and usage info to Mozilla. Telemetry measures and collects non-personal information, such as performance, hardware, usage and customizations. It then sends this information to Mozilla on a daily basis.
[[mozilla wiki](https://wiki.mozilla.org/Telemetry)]

[Telemetry - Firefox Source Docs](https://firefox-source-docs.mozilla.org/toolkit/components/telemetry/index.html)

[Telemetry - Firefox Data Documentation](https://docs.telemetry.mozilla.org/)


#### Health Report

[Health Report (Firefox)](https://github.com/DonQuixoteI/Firefox-UserGuide/blob/master/doc/user.js.md#health-report)

```js
user_pref("datareporting.healthreport.uploadEnabled", false);
```


#### Data Submission

```js
user_pref("datareporting.policy.dataSubmissionEnabled", false);
```

> This is the data submission master kill switch. If disabled, no policy is shown or upload takes place, ever.
[[Mozilla Source Tree Docs](https://firefox-source-docs.mozilla.org/main/65.0/toolkit/components/telemetry/telemetry/internals/preferences.html)]


### Crash Reports

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


### Privacy

#### Your IP Address

```js
user_pref("mail.smtpserver.default.hello_argument", "localhost");
```

> Lets you replace your IP address with the specified string in Received: headers when your IP address is not a "fully qualified domain name" (FQDN). Typically you only need to do this when you have a NAT box to prevent it from using the NAT boxes IP address. If you don't set it to something in your SMTP server's domain it may increase your spam score.
[[mozillaZine](http://kb.mozillazine.org/Mail_and_news_settings)]


#### Your User Agent

The [User-Agent](https://en.wikipedia.org/wiki/User_agent) string includes the following details:\
`Mozilla/[version] ([system and browser information]) [platform] ([platform details]) [extensions]`.

For example,\
`Mozilla/5.0 (X11; Linux x86_64; rv:68.0) Gecko/20100101 Thunderbird/68.10.0`

```js
user_pref("general.useragent.override", "");
```

[User-Agent - Privacy Handbuch](https://www.privacy-handbuch.de/handbuch_31f.htm) (in German)

To view the User Agent, see [how to show them](#show-user-agent).


## Extensions

### Extension Blocklist

> Mozilla maintains a list of extensions that are not approved for use with Thunderbird. Thunderbird downloads this list from the Internet... _Warning_: Disabling blocklist updating is not recommended and may result in you using extensions known to be untrustworthy.
[[Thunderbird Help](https://support.mozilla.org/en-US/kb/thunderbird-makes-unrequested-connections#w_extension-blocklist-updating)]

[Add-ons Blocking Process - Firefox Extension Workshop](https://extensionworkshop.com/documentation/publish/add-ons-blocking-process/)

```js
user_pref("extensions.blocklist.enabled", false);
```

`false`: don't allow Mozilla to deactivate installed Add-ons.
[Privacy Handbuch](https://www.privacy-handbuch.de/handbuch_31d.htm) (in German)


### Add-on Metadata Updates

```js
user_pref("extensions.getAddons.cache.enabled", false);
```

`false`: don't send information on installed extensions to Mozilla once a day.
[Mozilla Add-ons Blog](https://blog.mozilla.org/addons/how-to-opt-out-of-add-on-metadata-updates/)


### Extension Updates

By default, the extensions are checked and updated automatically. To prevent the automatic updating:

```js
user_pref("extensions.update.autoUpdateDefault", false);
```

To prevent the check for add-on updates:

```js
user_pref("extensions.update.enabled", false);
```

[Firefox Bug #701987](https://bugzilla.mozilla.org/show_bug.cgi?id=701987#c6)
