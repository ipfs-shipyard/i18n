# IPFS Translation Project  🌐✍️🖖

> Adapting IPFS apps and websites for a specific language by translating text and adding locale-specific components

## Lead

[Marcin Rataj](https://github.com/lidel)


## How can I contribute translation for my language?

Go to https://app.transifex.com/ipfs/, select languages you want to help with and start translating!  

Transifex is localization platform for crowdsourcing translations from IPFS Community:
- Everyone can contribute translations.
- Be sure to change your Transifex notification settings to be notified when translation sources change.
- Missing language? Request it via Transifex UI
- Night Owl? Try [Dark Mode](#dark-mode) :owl:

That is all!

## I am a developer, how can I enable translation of my project?

Continue reading! 


- [Developer Tools and Best Practices](#developer-tools-and-best-practices)
  - [Adding i18n to Your Project](#adding-i18n-to-your-project)
  - [Locale File Formats](#locale-file-formats)
  - [Suggested Libraries](#suggested-libraries)
  - [Transifex 101](#transifex-101)
    - [Documentation Hightlights](#documentation-hightlights)
    - [Language Mapping](#language-mapping)
    - [Automatically Updating Source Locale at Transifex](#automatically-updating-source-locale-at-transifex)
    - [Sharing Translation Memory](#sharing-translation-memory)
    - **Note:** [Someone submitted a PR with translations instead of using Transifex
](#someone-submitted-a-pr-with-translations-instead-of-using-transifex)

## What does i18n mean?

> **Internationalization** (I18N) is the process of designing a software application so that it can be adapted to various languages and regions without engineering changes. 
>
> **Localization** (L10N) is the process of adapting internationalized software for a specific region or language by translating text and adding locale-specific components. 
>
> Localization (which is potentially performed multiple times, for different locales) uses the infrastructure or flexibility provided by internationalization (which is ideally performed only once, or as an integral part of ongoing development).

More: https://en.wikipedia.org/wiki/Internationalization_and_localization:

----


# Developer Tools and Best Practices

## Adding i18n to Your Project

1. Extract strings into separate files(s) in [JSON-ICU or PO formats](#locale-file-formats) and wire them up to be loaded by your app.
   - See sections below for platform-specific notes
1. Create a PR with above changes in your repository
1. Create an issue in this repo to: [Add a new project to Transifex](https://github.com/ipfs/i18n/issues/new/choose)

Next steps, after your project is added to Transifex:

1. Add `.tx/config` to the PR (read _Transifex_ section below)
1. Smoke-test by pushing source locale with `tx push -s` 
1. Merge the PR to `master`
1. Set up automated sync of source locale (read _Transifex_ section below)

That's it!

After this initial setup, the only manual step going forward is fetching new translations with `tx pull -a` as a part of release dance of your project.

## Locale File Formats

The most important is to use future-proof format for locale files, such as ICU, JSON-ICU or gettext (`.po`). 

Make sure to read and understand [how plurals and genders impact translations](https://docs.transifex.com/projects/plurals-and-genders), and [how to define plurals in JSON-ICU](https://docs.transifex.com/formats/json#plurals-support).

## Suggested Libraries

### JavaScript

We've had some success with [i18next](https://www.i18next.com/) with [i18next-icu](https://github.com/i18next/i18next-icu) or  using [yahoo/intl-messageformat](https://github.com/yahoo/intl-messageformat) manually.

## Transifex 101
 
Transifex is localization platform for crowdsourcing translations from IPFS Community.
 
### Documentation Hightlights
 
- [Installing the Transifex Client](https://docs.transifex.com/client/installing-the-client)
- [Understanding `.tx/config` file](https://docs.transifex.com/client/client-configuration#section-tx-config)
- [Using Transifex with GitHub in Your Development Workflow](https://docs.transifex.com/integrations/github)
  - [Syncing a local project to Transifex with the Transifex Client](https://docs.transifex.com/integrations/github#section-using-the-client)
- [Understanding User Roles](https://docs.transifex.com/teams/understanding-user-roles)
- [How Translation Memory works](https://docs.transifex.com/setup/translation-memory/)

### Language Mapping

Transifex use POSIX style `_` underscores when in it's locale tags to separate language tag and ISO region code, so `en_GB`
denotes `en` or English as the language, and `GB` or Great Britain as the region.

Browsers and the IETF standard use hyphens as the separator, like `en-GB`. Some libraries such as i18next look up languages based on the hyphenated IETF language code, with hyphens rather than undercores so we tell the `tx` client to map those underscored country specific locales to the hypenated version in the config file `.tx/config` by adding:

```ini
lang_map = zh_CN: zh-CN, ko_KR: ko-KR
```

#### `zh-CN` and  `zh-TW` vs `zh-HANS` and `zh-HANT`

Summary from [mozilla/addons-server/issues/5829]( https://github.com/mozilla/addons-server/issues/5829#issuecomment-325935847):
> In practice, `zh-CN` and `zh-SG` are subsets of `zh-Hans`; `zh-TW` and `zh-HK` are subset of `zh-Hant`. So `zh-CN` if not resolved directly, should be resolved as `zh-Hans`.
> 
> The difference within the subsets are usually vocabularies and translation costumes (much like the difference between `en_US` and `en_UK`). But the scripts used are common within the sets.

More: https://www.w3.org/International/articles/bcp47/

###  Automatically Updating Source Locale at Transifex

>  Manually updating source files isn't fun or scalable if you've got frequent updates. To avoid this, you can have Transifex automatically check for updates to your source file. You simply need to provide Transifex with the public URL of the file. The file can be hosted on any service, such as GitHub 

[Project Manager](https://docs.transifex.com/teams/understanding-user-roles) is able to set up [source file sync automation](https://docs.transifex.com/projects/updating-content/#automatically-updating-source-files).  Every new text that lands in `master` branch will be fetched by Transifex. Sync happens twice a day, so translation team gets notification within 24h.

Having this, the only manual step is to fetch fresh translations via `tx pull -a` as a part of release dance :dancer:

> Example: Setting up IPFS Companion to sync source locale using raw URL:    
> https://github.com/ipfs-shipyard/ipfs-companion/raw/master/add-on/_locales/en/messages.json
> ![Setting up IPFS Companion i18n sync](https://user-images.githubusercontent.com/157609/45259536-88d40a80-b3cf-11e8-9944-38f1836f275b.png)

### Sharing Translation Memory

> Each project in Transifex has its own Translation Memory. However, if you have projects which have similar or related content, e.g. [IPFS app and a website], you can share TMs across those projects by creating a TM group. – [sharing-tm](https://docs.transifex.com/setup/translation-memory/sharing-tm)

Right now we have a single TM group named "ipfs".


### Someone submitted a PR with translations instead of using Transifex

We don't accept direct PRs with translated strings coming from users other than Transifex itself, as those changes can be lost.

Translations need to be submitted using Transifex project at https://app.transifex.com/ipfs/

PRs that  bypass Transifex should be **closed without merging**, with thank you note and ask to apply changes via Transifex or just a link to [ipfs/i18n#how-can-i-contribute-translation-for-my-language](https://github.com/ipfs/i18n#how-can-i-contribute-translation-for-my-language) ([example](https://github.com/ipfs-shipyard/ipfs-webui/pull/950)).  


### Dark Mode 

Transifex supports _Dark Mode_ now, it can be enabled in _Editor Preferences_:

> ![Enabling Dark Mode on Transifex](https://user-images.githubusercontent.com/157609/66070351-6fa97900-e551-11e9-8ff0-dbb24a907732.png)

In the past, we had to use dedicated extensions:
   - [Dark Reader](https://darkreader.org) (Universal Browser Extension)
