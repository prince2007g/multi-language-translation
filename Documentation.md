# Multi-Language Translation Script - Documentation #

## Contents ##



## Introduction ##

The Multi-Language Translation script automatically translates a web-page (containing text in more than one language) into any single language requested by the user. This can be particularly useful for sites involving a lot of user-generated content (which might have originally been written by a number of users who speak different native languages) - e.g. blogs, forums, or other community sites. The script harnesses the [Google AJAX Language API](http://code.google.com/intl/en/apis/ajaxlanguage/) to perform translation and language detection.

This page provides full instructions on how to use the script and [deploy it on your website](#Deployment_and_Usage_Instructions.md), providing [examples](#Further_Examples.md) where necessary. It also attempts to answer any [additional questions](#FAQs_and_Further_Information.md) you may have, as well as to outline ways you can [support the project](#How_Can_I_Contribute_Towards_This_Project?.md) if you wish to do so. All feedback - both on this documentation and on the script itself - would be much appreciated.

Note: for a much quicker guide on how to use the script, please see the readme file included with the latest release, along with the simple example. Have a play around with this, and see if you can understand how it works. If you run into any difficulties, it is suggested that you consult the far more detailed documentation below...

## Deployment and Usage Instructions ##

This section explains how to deploy the script on your website. Before following these instructions, please first [download](http://code.google.com/p/multi-language-translation/downloads/list) the latest script release, and extract all files from the archive into a new folder on your computer.

### Step 1: ###

Firstly, you need to include the following 3 HTML lines somewhere within all web-pages that use the script:

```
<script type="text/javascript" src="http://www.google.com/jsapi"></script>
<script type="text/javascript" src="translate.js"></script>
<div id="mlt_languagelist"></div>
```

If you use a _Content Management System_ to maintain your site (either a generic one such as [Drupal](http://www.drupal.org), or a bespoke one), you could place this code in a _Block_, _Widget_, or top banner; so that the script runs on every page on your site. If your site is made up of static HTML pages however, you will need to repeat this step for all pages you wish the translation functionality to work on.

_Note:_ If you wish, you could include the first 2 lines between the `<head>` tags of all pages on your site instead. This is down to personal preference, but it should work fine in both cases.

Please ensure that if you intend to upload the script to a different folder from where your web-pages are located, you update the `src` value of line 2 to the location of your script (e.g `src="scripts/translate.js"` or `src="http://www.example.com/scripts/translate.js"`)

When a page on your site loads, the script will create a listbox at the location where you have placed the above code, containing all the languages that your site can currently be translated into (note: each language will be auto-translated to allow native speakers to easily find their language; and the list will show additional languages automatically, as and when Google supports them). If the user changes the language, the specified content (see [Step 2](#Step_2:.md)) will be translated into that language. The script will also remember the language they pick; so that if they visit the page again later, or visit another page on your site which includes the translation functionality, the relevant page can be (re)translated automatically.

There is also an _Original Languages_ option at the top of the listbox, which will return the user to the original untranslated site. If a language has not yet been chosen, this option will be selected (by default) and the site will thus remain untranslated, but the user will normally see a message asking them if they want to translate pages into their language (detected based on their browser / system settings - see [Step 2(d)](#Step_2(d):.md)).

Note: if the user has JavaScript disabled, the listbox will not appear (which does prevent further confusion from arising, as it prevents such users from trying to change the language - which would in any case not work unless JavaScript was enabled). If you wish, you can display a message to the user between the `<div>` tags - e.g.

```
<div id="mlt_languagelist">Enable JavaScript!</div>
```

This message will not be shown unless JavaScript is disabled or not supported by the user's browser...

Directly below the listbox containing the languages, a _Powered By Google_ attribution message will be shown. This is a requirement of the [Google AJAX Language API Terms of Use](http://code.google.com/intl/en/apis/ajaxlanguage/terms.html).

### Step 2: ###

Open the file _translate.js_. You need to make some configurations to the top of this script file to enable it to work with your website:

#### Step 2(a): ####

Specify the _Base Language_ for your website. This is the language the site was originally written in (i.e. before any user-generated content is added), and is normally the language used for all regions which appear on every page and do not change throughout the site - e.g. navigational elements such as links to other parts of the website. The base language should be entered in the form of a 2 character language code - you can view a [Complete List of Available Language Codes](http://code.google.com/intl/en/apis/ajaxlanguage/documentation/reference.html#LangNameArray) here (note that you do also need to check if [translation is supported](http://code.google.com/intl/en/apis/ajaxlanguage/documentation/#SupportedPairs) from the applicable language).

For example, if the base language is English, you should configure the script to read as follows:

```
var GL_baseLang = "en";
```

#### Step 2(b): ####

A typical modern website is divided into a number of regions - e.g. a top banner, sidebar(s), a content area (which is usually divided into further regions - such as individual blog post entries), and a footer. These regions are usually enclosed by `<div>` _HTML Elements/Tags_ - or alternatively by `<h1>`,`<ul>`,`<p>` elements etc. Specific elements on a page can be identified through the usage of element _IDs_ or _Class Names_ - e.g. `<div class="region1">`.

**It is important to remember when creating a new site that an ID can only be used to identify one single element on any one webpage - if an element is intended to appear multiple times on a page (e.g. to surround blog post entries), Class Names should be used instead!**

For the script to work correctly, you need to specify all regions on each page of your site that you wish to have translated - [why do I need to do this?](#Why_Do_I_Need_to_Specify_the_Regions_to_Translate_on_Each_Page?.md). This is achieved by identifying the IDs and Class Names of all elements that enclose these regions. Please note that if any regions are **not** enclosed by an element which can be identified by a Class Name or an ID, you would unfortunately have to modify your theme / site code to support this - most sites are already designed to work in this manner however, and any changes that _are_ required should be very minor.

Firstly, all applicable element IDs must be specified. If you have some existing knowledge of JavaScript, you will see that this is achieved by storing them in an Array, made up of a number of sub-Arrays (do not worry if you do not understand this though - simply follow the instructions below).

**i)** If an element encloses content that is written in the _[Base Language](#Step_2(a):.md)_ of the site, you need to specify the element by using the following format (_exactly as shown_):

```
	["elementID"],
```

... where _elementID_ is the ID of the element enclosing the relevant region.

**ii)** If an element encloses content written in a **known** language which is **different** from the base language, specify the element using the following format:

```
	["elementID", "fr"],
```

... where _fr_ is the [language code](http://code.google.com/intl/en/apis/ajaxlanguage/documentation/reference.html#LangNameArray) for the source language the contents are written in.

**iii)** If the element encloses content written in an **unknown** language (which therefore needs to be detected automatically to ensure correct translation when the script runs), use the format:

```
	["elementID", ""],
```

**iv)** The script can also optionally append some text to the bottom of a specified element which indicates the source language of the enclosed region. If you wish to enable this for an element, use the format:

```
	["elementID", "fr", true],
```

... this will add the line

_Automatic translation from French: Powered By Google -_<u>View Source Text (+)</u>

to the bottom of the relevant content. Note that this option _will_ work if a source language is manually specified, _or_ if it has been detected automatically - but _**not**_ if the Base Language format above was used (you can get around this limitation by manually specifying the Base Language as the Source Language (i.e. use `["elementID", "en", true],` instead of `["elementID", true],` assuming English is the Base Language).).

If the user clicks on the <u>View Source Text (+)</u>_link shown on the line, the original untranslated text for the relevant region will be shown below it (highlighted in a green font). The user can then click on the link again to hide this content if they wish. This functionality is only available if the webmaster has chosen to display the source language below a region - it is assumed that the user would not want to view the source text of less 'important' regions such as navigational elements._

**v) Putting It All Together:** A full example is shown below of the correct syntax to use when specifying the IDs of elements enclosing regions to be translated:

```
var GL_classIds = [
	["navigation"],
	["left_sidebar", ""],
	["right_sidebar", "fr", true],
	["footer"]
];
```

This will translate the regions enclosed by the 4 elements identified with the IDs shown above. The contents of element `<div id="navigation">` are written in the Base Language; the `left_sidebar` element contents will be auto-detected during translation; the `right_sidebar` element contents are written in French (and a line revealing this will be displayed at the bottom of the region); and finally the `footer` element contents are written in the Base Language. The contents of each of these elements will be translated separately, into the language requested by the user. Any elements specified above which do **not** appear on any individual page that includes the script will be ignored and skipped out - so feel free to include such elements in the list if required by your site...

The example above will work with the _example.htm_ file distributed with the script - so you will be able to see how the page interacts with this in real life.

#### Step 2(c): ####

This step is very similar to [Step 2(b)](#Step_2(b):.md) - you now need to specify the Class Names of all elements enclosing regions to translate. The Class Names should be specified using exactly the same format as for the element IDs (see [Step 2(b)](#Step_2(b):.md)). As noted previously, Class Names may be used to identify multiple elements on any one page. Please bear in mind that the script expects the contents of any one element to be in a single language, but another element with the same class name can contain text in a completely different language. E.g. a blog may contain multiple entries, each written in a single language, but some entries may be written in different languages from others.

When the script runs, the script will search for each occurrence of each specified element class name (if present - any specified elements not present will be skipped), and translate the contents of each separately (automatically detecting the language of the applicable region if necessary).

A full example is shown below of the correct syntax to use when specifying the Class Names of elements enclosing regions to be translated:

```
var GL_classNames = [
	["entry_baselang"],
	["entry_french", "fr"],
	["entry", "", true],
	["quotes", "de", true]
];
```

This will translate all regions enclosed by any of the 4 elements identified with the Class Names shown above. The contents of element `<div class="entry_baselang">` are always written in the Base Language; the `entry_french` element contents are always written in French; the language of each individual region enclosed by an `entry` element class name will be detected automatically during translation (and a line of text containing the detected language will be displayed at the bottom of each applicable element); and finally the `quotes` element contents are always written in German (and a line showing this will be displayed at the bottom of each applicable element). The example shown here also works with the _example.htm_ file distributed with the script.

#### Step 2(d): ####

Specify the language that the site will be displayed in for new visitors (i.e. users who are yet to select a new language). The default setting is to leave this blank:

```
var GL_curLang = "";
```

With this setting, all content on the page will remain untranslated; however every time a page containing the translation functionality is visited, new users will see the following message at the top:


_Translate This Page into English?_<u>Yes</u> | <u>No</u>

... Where English appears to be their main language. This language is detected based on the user's browser / system (if they are using Internet Explorer this will be based on the regional settings on their computer; otherwise it will be based on the default language of their current browser). If the user clicks on the 'Yes' link, the page will automatically be translated into the detected language (and this will also be performed on all future pages without prompting them again); if they click on 'No', the page will remain untranslated (and the user will no longer be prompted to switch languages when they visit future pages - they can of course change the site language manually however!). Note that the message will only ever appear if Google can actually translate a site into the detected language (or a non-region-specific variation of it).

The webmaster can alternatively configure this setting so that it behaves differently. If a language code is specified, then the site will be automatically translated into that language when new users visit, regardless of whether or not they speak that language. This may be useful for sites which are predominantly in a single language, and where a high proportion of visitors speak that language. E.g. to display the site in English to new visitors by default, use the following syntax:

```
var GL_curLang = "en";
```

Finally, if the original untranslated site should always be displayed to new users, and a message should NOT ever be displayed at the top suggesting that they switch to their detected language, use the following syntax (this was the default setting for script versions prior to 0.2.4):

```
var GL_curLang = "orig";
```

No further changes to the script are necessary (unless you want to alter the functionality); so save and close the file.

### Step 3: ###

Upload the file _translate.js_ (as well as any web-pages or files on your site that have been changed) to your server. If the above instructions have successfully been followed, the script should work in the manner expected. Your work is now complete!

## Further Examples ##

The archive containing the latest release of the script includes a file entitled _example.htm_ which shows the functionality in action. This is a very simple working example, but should help you understand how to configure and use the script. Please ensure that you have extracted both _example.htm_ and _translate.js_ from the archive into a new folder before opening the example (if you do not do this, it will not work properly). If you view the source of _example.htm_ (Right Click on the page and choose _View Source_ in Internet Explorer, or _View Page Source_ in Firefox) you will see some additional comments describing how the script interacts with the page.

In addition to this, a fully-functioning real-life example of the script in action can be found at: http://www.mobi-blog.eu - feel free to test out the translation functionality on the site, and also view the script [source](http://mobi-blog.eu/wp-content/themes/cutline-3-column-split-11/translate.js) to see how it has been configured...

See also this independent [Lyrics & Meanings](http://lyricsmeaning.davidsmit.za.net/home.aspx) website designed to be accessed from inside Winamp - it uses the script to translate song lyrics and associated comments from users.

We are always very interested in how the script is being used. If you have deployed or adapted it for your site, please consider providing a link to it in a [new comment](#commentform.md) below this page - this would be very useful both to offer additional support for new script users, as well as to provide further "case studies" of successful implementations. Many thanks!

## Script Licence ##
The script as a whole is released as Open Source under the [MIT License](http://www.opensource.org/licenses/mit-license.php). This essentially allows you to use or adapt it for any purpose; however no warranty is provided if you choose to do so.

Please note however that portions directly interact with (_Invoke_) functionality of the [Google AJAX Language API](http://code.google.com/intl/en/apis/ajaxlanguage/). Google has not released this functionality under an Open Source licence; instead it is provided with the understanding that all usage should be in accordance with the [Google AJAX API Terms of Use](http://code.google.com/intl/en/apis/ajaxlanguage/terms.html). For details of all relevant functionality invoked by this script, please see the [Translation API Class Reference](http://code.google.com/intl/en/apis/ajaxlanguage/documentation/reference.html).

Therefore, whilst you are free to use or adapt the script for any purpose, **you also assume full responsibility for ensuring that your usage of this script (and/or any derivatives that continue to utilise the [Google AJAX Language API](http://code.google.com/intl/en/apis/ajaxlanguage/)) on your website is fully in compliance with these [Terms of Use](http://code.google.com/intl/en/apis/ajaxlanguage/terms.html).**

The developer(s) of the script are in no way affiliated with Google, and this project is run completely independently from them.

Whilst not actually required by the licence, if you decide to use or adapt this script, we would be extremely grateful if you could somewhere retain or include a link to our project homepage - http://code.google.com/p/multi-language-translation/ - either as a comment inside the script itself, or within any documentation you write, or as a link on your website. Even better, let us know if you have successfully deployed the script on your site (by adding a [comment](#commentform.md) below this page), and we'll add a link from here back to you! Many thanks!

# FAQs and Further Information #

## What Things Do I Need to Use and/or Deploy This Script? ##

### Standard Users: ###

  * A reasonably modern browser. This script has been tested successfully on the following browsers:
    * _Internet Explorer_ (6, 7 & 8)
    * _Mozilla Firefox_ (2, 3 & 3.5)
    * _Safari_ (3 & 4)
    * _Google Chrome_ (3)
    * _Opera_ (9 & 10)
  * JavaScript must be enabled and supported by the browser
  * The browser must be able to accept cookies for your site if the current language is to be remembered when the user next visits

### Website Maintainers and Developers: ###
  * A website
    * This script should work on most modern sites.
    * You do need to ensure that the site is coded in such a way that distinct regions of each page (e.g. headers, sidebars, content areas in separate languages, etc) are enclosed by HTML elements (e.g. `<div>` tags) that can be identified using specific IDs and/or class names - which should already be the case for the majority of sites!
    * It is recommended that your site is written using [Standards Compliant](http://validator.w3.org/) (X)HTML; using an encoding which can cope with unusual characters (such as _UTF-8_). The script will still work if these are not the case, but some items may not necessarily appear as expected...
  * Access to the server that the site is located on
  * A text editor (e.g. _Windows Notepad_)
  * Some basic familiarity with HTML and JavaScript would be helpful

## Why Do I Need to Specify the Regions to Translate on Each Page? ##

### Short Answer: ###

Because it increases translation quality, reliability and flexibility.

### Long Answer: ###

A typical modern website is divided into a number of regions - e.g. a top banner, sidebar(s), a content area (which is usually divided into further regions - such as individual blog post entries), and a footer. Some of these regions are likely to appear on every page of your site, and will contain the same known content each time. For example, many sites have a navigation menu which appears on all pages. Other regions may vary from page to page - e.g. user-submitted content.

For translation of any content to be successful, the source language must be known. If required however, this can be achieved by automatically detecting the language of a block of text during translation. Whilst this usually works reliably, occasionally the Google translator is unable to determine the source language correctly (e.g. for very small sections of text, or text written in very similar languages) - which can reduce translation quality (i.e. if text is mis-translated) or reliability (i.e. if an error occurs because the source language of a section cannot be determined at all). Therefore, it is important to specify the language of a region of source text whenever this is already known. Requiring the webmaster to specify the regions to translate provides a mechanism to achieve this - as the regions containing text in a known language can be manually identified as being written in that language.

In the case of regions which appear on every page, the source language is usually already known in advance (as the webmaster will have needed to physically add the content of such regions to the site in the first place). The source language of these regions can therefore be specified, to ensure they are translated reliably and correctly each time.

By contrast, the language of regions of content submitted by users cannot always be easily determined (except through automatic detection); as it depends on the native language(s) of the user(s) in question. In these cases however, each individual region is likely to be written in a single language, e.g. on a blog site, a single entry will be written in just one language; even if other entries are in different languages. The script works by finding each specified region (e.g. each blog entry), and translating the contents separately from all other regions (whilst automatically detecting the source language if necessary as part of this process).

A human can intuitively determine the regions of a page that are likely to contain text written entirely in a single language, as most web pages are designed in such a way that different regions are visually separated from each other when shown on screen. It is far harder for a computer script to achieve this however. This is because larger regions are typically split into smaller nested sub-regions (e.g. a blog post may be made up of a title, the entry itself, and additional metadata). As a result, it is hard to determine which regions on a page are entirely written in the same language, without already knowing the source language - choose too large a region, and the content MAY be written in multiple languages (which causes translation problems, as discussed above); choose too small a region, and there may not be enough content to detect the language during translation in the first place (as well as making the script run more slowly)! By contrast, a human can use their judgement to pick sensible regions for the script to translate separately from each other.

The issues raised in the above paragraph are made more complicated by the fact that due to restrictions imposed by modern browsers (as well as by Google), all content must be split up into small 'chunks' beforehand, and submitted for translation separately. To achieve this as efficiently as possible, large regions may be split up into multiple chunks; and a single chunk may also contain multiple small regions (or may not, depending on the content being translated). This increases the chance that any individual chunk could contain text in multiple languages, making potential translation problems more likely.

However, by asking a human to manually specify the regions, the script can ensure that all text in a specified region is kept completely separate from text from other regions whilst chunking the content, thus making sure that each chunk just contains text written in a single language. After translation, the chunks for each specified region can then be correctly reassembled and displayed on screen.

There is one other major benefit of requiring the regions to be manually specified - it significantly increases the script's flexibility. For example, a webmaster may decide that they do not wish a particular region to be translated - either because it contains very little translatable content (e.g. a banner image that just contains some ALT text) so that translation is not really necessary; or because it contains so much relatively unimportant text that the benefits of translation are not justified by the additional time required to complete it. Such decisions are therefore placed firmly in the hands of the webmaster.

Similarly, the webmaster can optionally choose to display a line of text below specified regions containing their original source language (as well as a capability allowing the user to view the original source text). However, the webmaster may not wish to enable this for regions where the feature is unnecessary or unhelpful. For example, details of the source language are unlikely to be important for regions simply containing a navigation menu etc - and indeed, including the line of text in such cases is more likely to be an inconvenient distraction for the user.

By contrast, such a feature can be extremely helpful in regions containing user-generated content, helping the user identify text which has been translated (and potentially also helping them understand it better, if they have some understanding of the original language). Because this optional capability is fully integrated with the existing requirement for webmasters to specify regions to translate, they therefore have full control over determining what regions (if any) should support this functionality - thus increasing flexibility.

## I've Read the Above Answer, and Still Don't Want to Specify the Regions - Do I Have To? ##

Not if you really want to avoid it. A workaround is available - simply configure the top of the script to read as follows:

```
var GL_baseLang = "en";
var GL_classIds = [["translate", ""]];
var GL_classNames = [];
var GL_curLang = "";
```

Then modify the `<body>` tag of all pages that use the script, to read:

```
<body id="translate">
```

The script will then do its best to automatically detect the language of all content on the page. It is however highly recommended that you avoid this approach, [for the reasons outlined above](#Why_Do_I_Need_to_Specify_the_Regions_to_Translate_on_Each_Page?.md) - the chances of content being incorrectly translated (or of translation errors occurring) are dramatically increased.

The technique could however be useful when working with pages that are written in a single language (though remember that if you just want a tool to translate pages between individual languages, [better alternatives](http://translate.google.com/translate_tools) are probably available); or if used briefly for testing purposes.

If the above technique does not meet with much success on a multi-language site, changing the size of chunks to translate MAY help improve matters. To do this, find the function `createChunks(srcContent)`, and alter the code below:

```
var chars = 1000;
```

... where `chars` contains the maximum number of characters to include per chunk. Try making minor changes to this number first, to see if they have any effect. **Note: Make this change at your own risk - changing chunk size may cause other problems!**

For best results though, always use the script as intended, and manually specify all the regions to be translated. It is easy to do, and the steps only need to be performed once in any case!

## What are the Benefits/Trade-offs of Using JavaScript to Perform Translations? ##

This script utilises _AJAX_ (_Asynchronous JavaScript and XML_) as the means to perform automatic translations. This is partly because the main translation functionality offered to developers by Google is provided through their [AJAX Language API](http://code.google.com/intl/en/apis/ajaxlanguage/) - which is primarily designed to be used in this manner. However, the approach does also offer a number of benefits to users and web developers.

It is worth noting that an alternative method could have been used - i.e. to perform translations directly on the server (e.g. using a _PHP_ or _ASP.NET_ script), before the user actually sees the web page on screen. Both approaches have their own advantages and disadvantages; these are outlined below - and should be taken into account when making a decision about whether or not to use this script...

### Benefits: ###

Performing the translations _client-side_ through _JavaScript_ (with _AJAX_ functionality used as necessary) does offer a number of benefits. Perhaps the main one for developers, is that it greatly increases portability. The script will work on virtually any modern website, regardless of the underlying platform (e.g. a generic _Content Management System_ such as _[Drupal](http://www.drupal.org)_ or _[WordPress](http://www.wordpress.org)_; a bespoke script that is written in _PHP_, _ASP.NET_, or another server-side language; or simply a set of static _HTML_ pages placed on a server). This is because it can easily be embedded within existing pages, without interfering with the underlying behaviour of the site.

By contrast, a _server-side_ script which offered similar translation capabilities would probably have to be tailored to the specific site(s) or _Content Management System(s)_  in question (e.g. by creating plug-ins for various different platforms), in order to avoid 'breaking' existing functionality - and therefore the script could not (potentially) be deployed anywhere near as widely. Similarly, because it is designed to run within the current page, the script will continue to work whether or not the user is logged-in to the site (which again is not always the case with certain existing server-side implementations of translation functionality - such as the original [Google Translate](http://translate.google.com) tool), thus making it possible to provide users with a fully integrated experience in their own language.

Another benefit is that translations are performed dynamically on screen as the user waits. They can see a status message displaying the progress that has been made so far; and the page does not have to reload if the current site language changes (always required in the case of server-side scripts) - thus interfering less with their current activities. Regions on a page are automatically updated as they are translated, meaning that the user can read some content in their own language as they wait for the rest of translation to complete. This gives translation a far more 'fluid' and visually 'exciting' feel than would be the case through a server-side script.

### Trade-offs: ###

These benefits do have some trade-offs however. Most significantly, the user must have JavaScript enabled within their browser, for the script to work correctly. Whilst this will be the case for most users, some will choose to disable it (or are using a browser and/or assistive technologies which do not support it well). Although the script is designed in such a way that the functionality will be completely invisible to those with JavaScript disabled (and will therefore not interfere with their activities); this does mean that a small subset of users may potentially be excluded. Whether or not this is a problem depends on the proportion of such users on your site.

Another trade-off to consider is the speed of translation. Whilst the script has been optimised to perform translations as quickly as possible, the entire content of each page has to be submitted to Google each time a new language is requested after the page has loaded - which obviously takes (a relatively small amount of) time to complete. The time taken is influenced by a number of factors; including the speed of the user's internet connection, and the speed of their computer and browser.

By contrast, a server-side script could potentially store online 'caches' of translated pages in different languages; meaning that after the content has been translated once, it could be served to all users requesting that language very quickly. It should be noted however that because the quality of automatic translations can be variable and tend to improve in the long term as time passes, this could mean that users are not presented with the best possible translation at any one time, and may even always be shown a fluke translation which happened to be particularly 'bad' on that occasion (which is not an issue if the content is submitted to Google every time - as the user will always see the latest - and theoretically the best - translation).

JavaScript cannot cache content as easily. Whilst the script does temporarily store all successfully completed translations of a page (meaning that future translations into the same language can be performed and displayed almost instantaneously), this process can only take place on a per-user basis (rather than for multiple users); and the stored content will always be lost next time the page is reloaded/revisited (unless functionality such as [Google Gears](http://gears.google.com/) were used for offline storage, which would add additional complications) - so that all content must then be re-translated from scratch. As a result, client-side translation will inevitably be a bit slower overall. Because the translation takes place dynamically though, this is less of an issue - as the user will always have something to watch or do whilst they wait for a short period.

## How Stable is the Script - Can I Safely Use it On My Site? ##

The script has been tested reasonably thoroughly, and should be safe to use in most cases. Naturally though, scripts are rarely perfect; so if you do spot a problem or are struggling to get it to work on your site, please do [get in touch with us](#How_Can_I_Contribute_Towards_This_Project?.md); and we will try to help you ASAP. We are particularly interested in cases where the script suddenly stops working (or does not work in the first place), or where the browser displays a script error - such events may imply a deeper problem to resolve, so please [let us know](#How_Can_I_Contribute_Towards_This_Project?.md) about them in the unlikely event they occur!

## The Script Seems to 'Hang' Part-Way Through Translation - How Can I Solve This? ##

Very occasionally, it is possible that translation may appear to 'hang' or 'freeze' part-way through - in such cases the '_Translating..._' status message at the top remains on screen, and will usually eventually be replaced by a '_Translating (Slowly!)..._<u>Retry</u>_' mesage. In the unlikely event that this happens on your site, here are some instructions to try to solve this:_

  1. Firstly try refreshing the page in your browser - often such issues are just caused by a temporary problem.
  1. Check to see if there are any script errors: For example, if you use _Internet Explorer_, ensure that errors will be displayed by default (go to _Tools > Internet Options > Advanced_, and make sure the _Disable Script Debugging_ option is not ticked, and the _Display a notification about every script error_ option is ticked). If you use _Mozilla Firefox_, you can use a debugging tool such as [Firebug](http://getfirebug.com/). If any script errors are shown, please report the problem to us, following the instructions in the [Report Any Problems](#Report_Any_Problems:.md) section...
  1. The script works by breaking content down into very small 'chunks' of text, and submitting each separately to _Google_. By default, each chunk contains up to 1000 characters, which works fine in the vast majority of cases. Very occasionally though, this number is a little too high, owing to limitations associated with current browsers and servers. This may be the case for example on a webpage containing a lot of line breaks. If the steps above do not solve the problem therefore, please search through the script until you have found the top of the function `createChunks(srcContent)`, and alter the code below:

```
var chars = 1000;
```

... where `chars` contains the maximum number of characters to include per chunk. Try reducing the value of `chars` - for example, to 990 characters. If that does not work, try reducing it a bit further.  **Note: It is however important not to reduce the value by too much, as this could potentially cause other problems**.

If none of the steps above solve your problem, please report it to us, following the instructions in the [Report Any Problems](#Report_Any_Problems:.md) section...

## Are There Any Element IDs or Class Names I Should Avoid Using on My Site? ##

This script uses a number of hard-coded HTML element IDs and class names to identify interface components added by it. In the very unlikely event that a page already contains HTML tags which use these names, this could cause conflicts with the script, and cause things to appear in unexpected locations on page. In a nutshell, as of script version 0.2.4., all element IDs and class names are prepended with `mlt_` (and this convention will continue to be followed if any elements should be added in future versions) - so if possible, try to avoid using element IDs / class names that start with those 4 letters on your site.

For reference, the complete list of HTML element IDs and class names currently used by the script is shown below (note: the `#` symbol is substituted for any single numeric value):

  * `mlt_errsrctxt`
  * `mlt_langattr`
  * `mlt_langstring`
  * `mlt_language`
  * `mlt_languagelist`
  * `mlt_perc`
  * `mlt_srctxt#`
  * `mlt_srctxtbracket#`
  * `mlt_srctxtcontent#`
  * `mlt_srctxtlnk#`
  * `mlt_transerrlist`
  * `mlt_transerrmsg1`
  * `mlt_transerrmsg2`
  * `mlt_transerrtxt`
  * `mlt_translatemsg`

If you know that your site already uses one or more of these names to identify elements, you will either have to rename all instances within your site to something else; or alternatively modify the script to use a different name to identify that element whenever necessary (which may be easier - using a _Find and Replace_ tool to substitute the names could be helpful here!).

## A New Version of This Script Has Been Released - How Do I Upgrade? ##

Upgrading from an older version to a newer version of this script is usually extremely straightforward. Follow these simple steps:

  1. [Download](http://code.google.com/p/multi-language-translation/downloads/list) the newest version of this script.
  1. Open both _translate.js_ files (both the older version you already have, and the new version you have just downloaded).
  1. Copy the top section (i.e. the part you previously configured when first setting everything up) from your old version of the script, and paste it in the place of the equivalent section of the new version.
  1. Save and upload the new _translate.js_ file to the correct location on your server (overwriting the old file).
  1. Your upgrade is now complete!

These steps should always work, unless radical changes have been made to the script since a previous version. Before upgrading, it is always worth checking this section of the documentation first, as it will be updated should any more significant changes ever be required as part of an upgrade.

### Alterations for Script Version 0.2.4 ###

If you are upgrading from a version of the script older than 0.2.4 to this or a higher version, please bear the following in mind:

  * All HTML elements inserted by the script are now prepended with `mlt_` to reduce the likelihood of conflicts - see  [section above](#Are_There_Any_Element_IDs_or_Class_Names_I_Should_Avoid_Using_on.md) for details. This includes the element `<div id="languagelist"></div>` which you will have previously manually inserted somewhere on each of your pages. Therefore, for the script to continue working, you MUST adjust this code wherever it appears on your site to read as follows: `<div id="mlt_languagelist"></div>`.
  * The variable `GL_curLang` has now been moved from the global variables into the configurable section of the script, allowing the webmaster to specify an initial site language for new users (see [Step 2(d)](#Step_2(d):.md) for details). When upgrading, you may wish to move this manually yourself and make any desired configurations. The script will still work if you do not do this; however it will behave slightly differently from older versions - a message will now appear at the top asking the user if they wish to translate the site into their current language. To restore the behaviour of older versions, use `GL_curLang = "orig"`.

### Alterations for Script Version 0.2.5 ###

Version 0.2.5 of the script fixes a bug that could cause script errors and crashes on certain sites. Please note that as part of this fix, the configurable variables `GL_classIds` and `GL_classNames` can no longer contain commas after the last specified regions. If you are upgrading from an older version by following the instructions above, you may therefore need to remove these manually... E.g. When using the script with the example file, the variables should now read:

```
var GL_classIds = [
	["navigation"],
	["left_sidebar", ""],
	["right_sidebar", "fr", true],
	["footer"]
];

var GL_classNames = [
	["entry_baselang"],
	["entry_french", "fr"],
	["entry", "", true],
	["quotes", "de", true]
];
```

## Why Was This Project Started? ##

This project was started in the early summer of 2009 by Patrick Hathway, a researcher at [OdinLab](http://redgloo.sse.reading.ac.uk/odinlab/), at the [University of Reading](http://www.reading.ac.uk), UK. At the time, an EU project, known as [Mobi-Blog](http://mobi-blog.eu), was in progress. The main purpose of this project was to encourage Exchange Students to write posts on a blog about their experiences of studying in a different country. To improve participation, it was decided that students should be allowed to post entries in their own native language.

This did however present problems with users who did not speak the same language as the content author(s). It was agreed that it would be impractical to create multiple versions of the site for different languages (manually translating content into each language) - due to the anticipated volume of content, as well due to as resource constraints. Therefore, automated translation was seen as the best option - even if the translations were imperfect, they would help users gain a better understanding of what was being said.

Initially, the [Google 'Translate My Page' Gadget](http://www.google.co.uk/ig/directory?type=gadgets&url=www.google.com/ig/modules/translatemypage.xml) was used, as at the time this was the only existing technique of embedding Google translation functionality into a site. Whilst the method did work well when the site content was written primarily in English, significant problems arose once more multi-lingual content was added. Indeed, a situation arose whereby the functionality even struggled to achieve something as rudimentary as translating English text into English!

As a result of this frustration, alternatives were investigated. It was found that the [Google AJAX Language API](http://code.google.com/intl/en/apis/ajaxlanguage/) did not suffer from similar limitations - partly because translating from the source language to the same destination language did not cause any problems; partly because it allowed ANY content to be submitted for translation, automatically detecting the language if required. This made it possible to create a script which submitted various regions of a page (written in different languages) entirely separately, thus making sure that the language of each region was detected and translated correctly.

A fully working prototype was created in around a week or so; this involved overcoming various technical challenges (such as a limit - imposed both by current browsers and by Google - to the number of characters which could be translated at any one time), as well as creating various interface components (e.g. the ability to view translation progress), and additional functionality (e.g. primitive error handling, the ability to 'remember' the current site language, the display of a line of text below regions showing the source language, etc).

The prototype was released on the [Mobi-Blog](http://www.mobi-blog.eu) site, and received very positive feedback. Once the functionality could actually be seen in action, it was realised that it could be applied in a whole range of other contexts - as many other websites would benefit from the ability of users to view them and contribute in their own native language. Although the prototype had very much been tailored towards the [Mobi-Blog](http://www.mobi-blog.eu) website, there were various ways it could adapted to work more easily with other sites.

Although Google subsequently made a number of improvements to their existing [translation tools](http://translate.google.com/translate_tools), it was felt that the prototype script still was very valuable - not only was it specifically designed to support translation of content on a page containing multiple languages, but also offered the webmaster with significantly more flexibility and control over how the translation should take place - and without being significantly more difficult to deploy (at least after subsequent alterations). Furthermore, the script offered various features unavailable in the translation tools provided by Google - such as the ability to display the source language of specified regions; as well as the ability to automatically remember and translate pages to the specified language every time any page on a site was visited.

Therefore, a few months later, substantial improvements to the existing script were made; with the intention of making it far more flexible, powerful, and easier to deploy on any website; and ultimately, with a view to releasing the functionality as Open Source so that it could be used and adapted by anyone as desired. This project ended up as the result!

## How Can I Contribute Towards This Project? ##

Thank you for your interest in this script. You can help us out in a number of ways, which will support future development and evolution of the functionality.

### Deploy the Script on Your Site: ###

Perhaps the most useful thing you can do, is to download the script and attempt to deploy it on your website. If you manage to implement it successfully, please do leave a [comment](#commentform.md) below this page; as this will provide a useful 'case study' for future users. Similarly, if you have any problems, please do let us know by reporting an issue on the [Issue Tracker](http://code.google.com/p/multi-language-translation/issues/list). By helping to deploy this script more widely, you are ensuring that it is tested in a wider range of environments and scenarios, including those which may not have originally envisaged by the developer. In turn, if you report your findings to us, this can lead to ongoing reliability and functionality improvements.

Feel free to also use the [Issue Tracker](http://code.google.com/p/multi-language-translation/issues/list) to suggest improvements to the script, or enhancements to the functionality and/or new features. You are also extremely welcome to use the Issue Tracker to make more general comments or feedback; as well as to make any enquiries about the script - including requests for help in deploying it. We'd also like to receive feedback about this documentation, including any suggestions for improvement - please leave your ideas as [Comments](#commentform.md) below this page.

### Report Any Problems: ###

Before you report any script problems, please first review all examples thoroughly (and see if they work for you), and read through the detailed documentation above. Check for 'obvious' errors - e.g. is the script located within the right folder on your server; have all translatable regions been defined correctly within the script; have you added any minor 'syntax errors' to the script; is JavaScript enabled? If you still are experiencing problems, please report them to us via the [Issue Tracker](http://code.google.com/p/multi-language-translation/issues/list).

When reporting an issue, it is helpful if you can provide as much detail as possible. For example, if Google returns one or more translation error(s) after translation; it is helpful to receive details of all error(s) that occurred, as well as the precise section(s) on a page which are affected (preferably by providing us with the HTML code of the section(s) in question). All these these details can easily be found, simply by clicking on the _'Details'_ link next to the Translation Error message.

If the script does not run at all, or seems to stop running or 'hang' part-way through; it is helpful if you can provide details of script errors (if any!) displayed by the browser. If you use _Internet Explorer_, ensure that script errors will be displayed by default (go to _Tools_ > _Internet Options_ > _Advanced_, and make sure the _Disable Script Debugging_ option is not ticked, and the _Display a notification about every script error_ option is ticked). If you use _Mozilla Firefox_, you can use a debugging tool such as [Firebug](http://getfirebug.com/). Please include the full text of any browser error(s) in your problem report.

The most useful thing you can do to help us resolve issues, is to provide us with a live online example which demonstrates the problem in action. If possible, the problem needs to be reproducible (rather than just a 'random' error), so that we can isolate the causes.  Please upload a copy of your site and script to a server somewhere, and include a link to it in your problem report. Also please explain as precisely as possible how we can reproduce the problem on your site. Any example is better than none however!

Finally, it is helpful if you can provide additional information about your computer, such as the connection speed and browser you are using. This can help us debug problems more effectively.

### Adapt the Script to Meet Your Needs: ###

Please feel free to adapt the script to meet the needs of your site more effectively. It is advised that you have a good knowledge of HTML and JavaScript before attempting this - a range of good tutorials are available online. It is also recommended that you first gain a good understanding of how the [Google AJAX Language API](http://code.google.com/intl/en/apis/ajaxlanguage/) works, by reviewing their [examples](http://code.google.com/intl/en/apis/ajaxlanguage/documentation/#Examples).

Once you have done this, review [the examples provided with this script](#Further_Examples.md) to gain an understanding of how they interact with it. Also carefully read the documentation on this page (particularly the [FAQs](#FAQs_and_Further_Information.md)), as it provides detailed background information about how the script works - which will make it easier for you to understand the code. Finally, review all code in the script, whilst trying to understand what it is doing. The code should be fairly self-explanatory; and the script is also fully commented, so it should not be too difficult to see how it works. However, if there is anything you do not understand, or if you need any other help adapting the script, please let us know via the [Issue Tracker](http://code.google.com/p/multi-language-translation/issues/list); and we will try to assist.

Some changes will of course be easier to make than others. For example, changing the visual appearance of various interface components is relatively easy - e.g. the styles of the translation messages at the top can be modified by finding and altering the `addTransMsgs()` function as desired. Other changes may be more time consuming - e.g. modifying the underlying functionality or adding new features. If you have made an alteration or have added/improved functionality which you feel may be beneficial to other users, please do let us know (by adding a [Feature Enhancement Request](http://code.google.com/p/multi-language-translation/issues/entry?template=feature%20enhancement%20request) Issue, and attaching a file containing the changes you have made), so that we can incorporate them into the 'official' version of the script!

### Become A Project Contributor: ###
If you are really interested in how this project develops, and would like to help us more deeply - e.g. by developing additional functionality to be incorporated directly into the official script version; please do get in touch, and we can add you as a project _Contributor_ or _Committer_. This will provide you with access to more functionality on this site (e.g. the ability to _Commit_ code changes to the repository), giving you a more direct say in how the project evolves. We would be extremely grateful for such support!

### In Summary: ###

Thank you again for your interest in this script - if you would like to see it evolve further, have any suggestions or problem reports, or would like to help out in other ways, please do get in touch (using the [Issue Tracker](http://code.google.com/p/multi-language-translation/issues/list) as the primary means, if possible), and we shall look forwards to hearing from you!