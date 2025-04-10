---
title: "Privacy focused note taking setup"
description: "How I configured my Boox device for privacy, avoiding cloud accounts and integrating notes with Obsidian using Syncthing."
pubDate: "2025-04-09"
---

## Introduction

For my second blog post, I wanted to show you my note taking setup, and for that, I mainly use two tools: Obsidian and my Boox tablet.

Boox devices are great e-ink android tablets, but I avoid vendor cloud accounts and Google services. I set up my Boox to work independently and connect its notes to my self-hosted [Obsidian](https://obsidian.md/) vault. Here's how I did it.

## Avoiding cloud accounts

The first step was to minimize reliance on external accounts during the initial setup:

1.  **Skip Boox Account:** I opted out of creating a Boox account at all. "Create an account?" just say no
2. **Bypass Google:** To avoid needing a Google account for apps, I downloaded the [F-Droid](https://f-droid.org/) APK directly from their website and installed it.
3. **Install Aurora Store:** I installed F-Droid mainly to get the [Aurora Store](https://auroraoss.com/). Aurora Store is an open-source client for the Google Play Store that lets you download apps without a Google account.

With this setup, I could install any Android applications while maintaining maximal privacy. My data stays where it belongs—with me!

## The self-hosted syncing strategy

The main challenge was getting notes off the Boox and into my Obsidian vault automatically. My solution involves a self-hosted server and [Syncthing](https://syncthing.net/).

### Server setup

I run a home server that acts as the central hub for my files.
For secure remote access, I protect the server using a [Cloudflare Tunnel](https://www.cloudflare.com/products/tunnel/) and route connections through Cloudflare Warp VPN.

*Note: You don't need a server. You can run Syncthing on your local network. The server option just lets you sync files from anywhere.*

### Sync topology

I use a star pattern for Syncthing. My Boox, laptop, phone, and other devices all sync with the central server, not with each other.

**Why?** This reduces sync conflicts. Since I usually edit notes on only one device at a time, syncing through a central point prevents problems that can happen in a network where devices sync directly with multiple others.

## The meaty part: integrating Boox notes with Obsidian

Alright, buckle up! This is where the real magic (and slight Boox-induced headache) happens. Let's get those precious notes flowing into Obsidian.

### 1. Boox auto-export
I enabled the built-in auto-export feature in the Boox Notes app, configuring it to export notes as PDFs automatically.
How? Dive into the Notes app settings. Look for an "Automatically export as PDF after exiting a notebook" option. This tells your Boox, "Hey, whenever I'm done scribbling, turn that masterpiece into a PDF, pronto!"

### 2. The export location limitation
A significant limitation is that the Boox Notes app forces exports into a specific folder named `note` in the device's storage. Unfortunately, you cannot change this default location.
Yeah, you read that right. Boox decided the `note` folder is the place. It's like that one drawer in your kitchen where everything ends up.

### 3. The workaround
This limitation isn't a deal-breaker. My solution was to structure my Obsidian vaults within this `note` folder. For example:
`storage/note/MyObsidianVault1/`.

### 4. Syncthing configuration
**Get Syncthing Running:** Install Syncthing-Fork (the original Syncthing is no longer maintained) on your Boox using Aurora Store.

**Add Folders (The Smart Way):** Now, fire up Syncthing on the Boox. Instead of just syncing the entire chaotic `note` folder (unless you're a fan of digital entropy), add each specific Obsidian vault folder as a separate Syncthing folder.
When adding a folder in Syncthing on the Boox, you can give it a label (e.g., "MyObsidianVault1") and set the "Path" to `storage/note/MyObsidianVault1/` (adjust the path based on your actual storage layout if needed).
Share this folder with your central Syncthing server (or other devices). Repeat for any other vaults inside the `note` folder (e.g., add another Syncthing folder pointing to `storage/note/MyObsidianVault2/`).

**Why separate shares?** This keeps things tidy and ensures only your vault content syncs, not random files in the main `note` folder.

### 5. Boox notes app
**Internal Structure vs. Export:** The Boox Notes app internally organizes your notes differently (in its own folder structure, hidden away). The structure we care about is the one inside the Notes app itself that you create using folders. This internal structure dictates where the exported PDFs land within that `note` folder.

**Mirroring is Key:** To make the auto-exported PDFs land within your Obsidian vault's subfolders (inside the `note` directory), you need to replicate your desired Obsidian folder structure within the Boox Notes app.

**Example:** Let's say your Obsidian vault on your computer (and synced via Syncthing) looks like this: `MyObsidianVault1/folder1/file1.md`. Then you have to create the same folder structure in the Boox Note app, for the pdf export to work with your Obsidian vault:

In order to do that, get inside the Boox Notes app and create a folder named `folder1`.
Put your Boox notes into the corresponding folder within the app: `folder1/note1`.

Now when `note1` (which lives in `folder1` inside the Notes app) gets auto-exported, Boox will place the resulting `note1.pdf` inside `storage/note/MyObsidianVault1/folder1/`. Boom! Syncthing sees this new PDF in the shared folder and whisks it away to your server and other devices, right where it belongs in your Obsidian vault structure.

## Conclusion

And voilà! With a bit of setup gymnastics, you've wrestled your Boox into submission, creating a privacy-respecting, self-hosted note pipeline directly into Obsidian. No cloud accounts needed, just your Syncthing server doing the heavy lifting.

If you have any questions or suggestions, feel free to leave a comment in this reddit post. I'll be there, probably syncing some notes while I wait.