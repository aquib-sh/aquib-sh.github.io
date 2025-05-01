---
title: "Fixing 'pfx is not owned by you' Error for Red Dead Redemption 2 on Ubuntu"
date: 2025-05-01
categories: [Gaming, Linux, Technical]
tags: [red-dead-redemption-2, ubuntu, steam, proton, ntfs, permissions, wine]
permalink: /:year/:month/:title.html
description: "A step-by-step guide to resolving the 'pfx is not owned by you' error when running Red Dead Redemption 2 on Ubuntu from an NTFS drive, focusing on Wine prefix ownership and NTFS permissions."
author: Aquib Shaikh
toc: true
comments: true
---

As a Linux gamer, I recently ran into a frustrating issue while trying to play *Red Dead Redemption 2* (RDR2) on Ubuntu. The game was installed on an NTFS drive shared with my Windows setup, but when I launched it through Steam, I got this error:

```
wine: '/mnt/games/SteamLibrary/steamapps/compatdata/1174180/pfx' is not owned by you
```

This error happens because the Wine prefix (used by Steam’s Proton to run Windows games on Linux) has incorrect ownership, often due to the game being installed from another operating system (like Windows) or the NTFS drive’s permissions. Here’s how I fixed it to get RDR2 running on Ubuntu.

## My Setup
- **Drive 1**: Windows 11 (where RDR2 was originally installed)
- **Drive 2**: NTFS drive mounted at `/mnt/games` for storing Steam games
- **Drive 3**: Ubuntu with Steam installed
- **Game Location**: `/mnt/games/SteamLibrary/steamapps/common/Red Dead Redemption 2`
- **Steam Library**: Added `/mnt/games/SteamLibrary` as a library in Steam on Ubuntu

## The Issue
I mounted the NTFS drive on Ubuntu and added the Steam library, but launching RDR2 failed with the `pfx is not owned by you` error. The problem was that the Wine prefix directory (`/mnt/games/SteamLibrary/steamapps/compatdata/1174180/pfx`) was owned by a different user, likely from when I installed or ran the game on Windows.

## The Fix
The solution was simple: change the ownership of the Wine prefix to my Ubuntu user. Here’s what I did:

### Step 1: Verify the NTFS Mount
I ensured the NTFS drive was mounted correctly at `/mnt/games`. To check the mount, I ran:

```bash
mount | grep /mnt/games
```

If the drive lacks proper permissions, you can remount it with your user’s ownership. For example, assuming your username is `aquib`:

```bash
sudo umount /mnt/games
sudo mount -t ntfs -o uid=aquib,gid=aquib,umask=022 /dev/sdb1 /mnt/games
```

Replace `/dev/sdb1` with your NTFS drive’s device name (find it with `lsblk`).

### Step 2: Fix Wine Prefix Ownership
The key fix was updating the ownership of the Wine prefix directory. I ran:

```bash
sudo chown -R aquib:aquib /mnt/games/SteamLibrary/steamapps/compatdata/1174180
```

This command recursively sets the ownership of the Wine prefix (and its contents) to my Ubuntu user (`aquib`). After this, RDR2 launched perfectly!

## Extra Tips
- **Proton Version**: In Steam, go to **Properties > Compatibility** for RDR2 and select a recent Proton version (e.g., Proton 9.0 or Proton Experimental) for best compatibility.
- **Steam Library Setup**: Ensure `/mnt/games/SteamLibrary` is added in **