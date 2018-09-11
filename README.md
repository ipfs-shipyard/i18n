# IPFS Translation Project

## TL;DR

https://en.wikipedia.org/wiki/Internationalization_and_localization:

> In computing, internationalization and localization are means of adapting computer software to different languages, regional differences and technical requirements of a target locale.
>
> **Internationalization** is the process of designing a software application so that it can be adapted to various languages and regions without engineering changes. 
> **Localization** is the process of adapting internationalized software for a specific region or language by translating text and adding locale-specific components. 
>
> Localization (which is potentially performed multiple times, for different locales) uses the infrastructure or flexibility provided by internationalization (which is ideally performed only once, or as an integral part of ongoing development).

## How can I contribute translation for my language?

### Go to https://www.transifex.com/ipfs/public/ and start translating!

Transifex is localization platform for crowdsourcing translations from IPFS Community:
- Everyone can join using GitHub account!
- Missing language? Join and request it via Transifex UI
- Translating at night? Check eyesavers for night owls :owl:  
   - [Dark Theme for OLED](https://userstyles.org/styles/161907/transifex-black)
   - [Geeko Dark Theme](https://userstyles.org/styles/164067/transifex-geeko-dark)

## I am a developer, how can I enable translation of my project?

1. Extract strings into separate files(s) in JSON-ICU or PO formats and wire them up to be loaded by your app.
   - See _Tools_ section below for platform-specific notes
1. Create a PR with above changes in your repository
1. Create an issue in this repo to: [Add a new project to Transifex](https://github.com/lidel/i18n/issues/new/choose)

Next steps, after your project is added to Transifex:

1. Add `.tx/config` to the PR (read _Transifex_ section below)
1. Smoke-test by pushing source locale with `tx push -s` 
1. Merge the PR to `master`
1. Set up automated sync of source locale (read _Transifex_ section below)

Thats it!

After this initial setup, the only manual step going forward is fetching new translations with `tx pull -a` as a part of release dance of your project.


### Developer Tools and Best Practices

The most important is to use future-proof format for locale files, such as ICU, JSON-ICU or gettext (`.po`).

Make sure to read and understand [how plurals and genders impact translations](https://docs.transifex.com/projects/plurals-and-genders), and [how to define plurals in JSON-ICU](https://docs.transifex.com/formats/json#plurals-support).

### JavaScript / Node.js

We've had some success with `i18next` with `i18next-icu` adapter.

#### Transifex 101
 
 Transifex is a localization platform.
 
- [Installing the Transifex Client](https://docs.transifex.com/client/installing-the-client)
- [Understanding `.tx/config` file](https://docs.transifex.com/client/client-configuration#section-tx-config)
- Manual sync via Transifex Client 
  -  [Using Transifex with GitHub in Your Development Workflow](https://docs.transifex.com/integrations/github)
     - [Syncing a local project to Transifex with the Transifex Client](https://docs.transifex.com/integrations/github#section-using-the-client)
     - [Integrating the Client with CI](https://docs.transifex.com/integrations/github#section-integrating-with-travis-ci) (Travis CI, but same will work with Jenkins)
- Source locale sync automation
  - [Automatically updating source files](https://docs.transifex.com/projects/updating-content/#section-automatic-updates)
    - Every Manager is able to set up sync automation. Every new text that lands in `master` branch will be fetched by Transifex (sync happens twice a day, so translation team gets notification within 24h)
    - > Example: Setting up IPFS Companion to sync source locale using raw URL:
      > https://github.com/ipfs-shipyard/ipfs-companion/raw/master/add-on/_locales/en/messages.json
      > ![Setting up IPFS Companion i18n sync](https://user-images.githubusercontent.com/157609/45259536-88d40a80-b3cf-11e8-9944-38f1836f275b.png)
   - Having this, the only manual step is to fetch fresh translations via `tx pull -a` as a part of release dance
- Bonus: eyesavers for night owls :owl:  
   - [Dark Theme for OLED](https://userstyles.org/styles/161907/transifex-black)
   - [Geeko Dark Theme](https://userstyles.org/styles/164067/transifex-geeko-dark)
