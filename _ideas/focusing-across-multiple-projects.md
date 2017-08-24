---
layout: post
title: Focusing Across Multiple Projects
---

I have a couple of side projects going on right now and I have found that staying focused on any one project can be pretty hard when I'm working at my computer. There are always distractions: notifications, windows that I haven't closed yet, etc. Sometimes the distractions are personal and sometimes the distractions spring from another side project.

I have a set up that I've been using for a few months that helps combat this and I find it works quite well: I have set up multiple user accounts on my iMac. I have one personal account and then one account for every major project that I am working on. It takes some time to set up when I start a new project, but the benefit is that I have a focused environment for my work.

My personal user account has my personal email, iMessage, iCloud account, financial data, photos, games, music library, etc. It's the account that I log into when I want to do things like shop on Amazon, surf the Internet, review my [OmniFocus](https://www.omnigroup.com/omnifocus/) projects, manage my family photos, or reconcile my bank statements.

Every other major project that I work on gets its own macOS user account created. In addition to the macOS user account, ideally every project gets a new email address as well. I also get a new iCloud account registered for the project's email address. The main benefit of the new iCloud account is that it allows for keychain sharing across multiple machines. I prefer not to use my personal iCloud account for keychain sharing between my projects because the passwords become unwieldy (there would be multiple Apple IDs, multiple Gmail passwords, etc.).

## Shared Items

There are a few Internet accounts that I reuse across all macOS user accounts:

- Apple ID for iTunes store and Apple Music
- Apple ID for Mac App Store
- Apple ID for iMessage and Contacts (I try to avoid getting sucked into distracting conversations when I'm focusing on non-personal work so I typically turn off notifications and badges for iMessage on my non-personal user accounts. If I don't, the interruption from a message can derail me. Even if I recover quickly, the disruptions add up throughout the day. However, I have found that it's also too inconvenient to completely disable iMessage because there are times that I do need to send a text out.)
- OmniFocus sync account (All of my projects go into a single OmniFocus account. I might have tried splitting this up too if it were possible to support multiple sync accounts on iOS. It's not possible though, so I never explored this option.)

## 1Password

I use [1Password](https://1password.com) to manage my passwords for every project. I have found it helpful to create a new 1Password vault for every project that gets its own macOS user account. Even though I want independent vaults for the projects, I have found it necessary to also sign in to some of my personal accounts for projects from time to time (for things like OmniFocus, GitHub, etc.). I have found a way to leave my personal 1Password vault available while generally being out of the way.

The trick is to first log in to my personal vault and then log in to the project's vault. After I have signed into both vaults, I uncheck my personal vault in the All Vaults preferences. What this does for me:

- The project vault is shown and the personal vault is hidden when I have All Vaults selected. This is the preferred default for my set up.
- I can always switch to my personal vault when I need to.
- Because I signed into my personal vault first, I use the master password for my personal vault to unlock 1Password. This way I can have a unique password for my project vault but still only need to remember the password for my personal vault. I tend to create a random password for the project vaults and store those in my personal vault. They remain secure and I never have to remember them.

## Slack

One of the great features of the native Slack clients is that they allow you to sign into multiple teams. This is crucial when you have a single macOS user and are part of multiple Slack teams. However, it can be a significant distraction if you sign into all of your Slack teams in all macOS user accounts. I have found that it is best to only sign into the single team

## Other Thoughts

It takes a lot of work to set up a new user account. I only do it if I am working on a project that is going to be a long-lasting project. Otherwise, it's too much work without enough value.

Here are a list of things that I have to do when I create a new user account that may not be obvious at first:

- create a new email account
- create a new Apple ID
- adjust keyboard settings
- adjust mouse settings
- adjust mission control settings
- adjust `.gitconfig` settings
- register my license keys for my development tools and other software, even if they already registered for other user accounts