---
layout: post
title: Using Steam Deck as Development PC
date: 2024-07-24 15:09:00
description: How to install environment tools for development
tags: code linux steamOS steamdeck
categories: posts
featured: true
---


Hello! Long story short: I no longer have my standard computer and am left only with my Steam Deck. I thought, “I’ll just use it as a PC and start programming since it’s based on Arch Linux, right?” WRONG! I had to make many changes to the system to get everything working. Here, I’ll list the modifications I made and provide references for you (and my future self). Some of these might not apply to you, so just use what you need:

> Some of those changes will be reverted after a system upgrade


### Create a password for default user

Otherwise, you cannot use sudo!

```bash
passwd
```

[Ref](https://www.ibm.com/docs/en/zos/2.4.0?topic=commands-using-passwd-command&source=post_page-06f6df15c0e5)


### Enable changes in system files

Protection created by Valve for gaming-only users

```bash
sudo steamos-readonly disable # Allows you to install packages
sudo steamos-readonly enable # Revert default config
sudo steamos-readonly status # Current status
```

[Ref](https://www.reddit.com/r/SteamDeck/comments/t6w9at/how_to_get_rid_of_read_only_filesystem_folders/?utm_content=title&utm_medium=post_embed&utm_name=586be786354945a0a7f5dbfe42b91a3b&utm_source=embedly&utm_term=t6w9at)


### Config keyring

Is a collection of PGP (Pretty Good Privacy) keys used to verify the authenticity and integrity of pacman’s packages.

```bash
sudo pacman-key --init #Initializes pacman-key
sudo pacman-key --populate archlinux #Install standard archlinux keys
sudo pacman-key --populate holo #Install holo keys
```

[Ref](https://wiki.archlinux.org/title/Pacman/Package_signing?source=post_page-----06f6df15c0e5--------------------------------)


### Re-install libraries

Some “unnecessary” library files were removed for size reduction rootfs, so you need to re-install then.

This is a fix that you need if you find yourself with an error like: “fatal error: stdlib.h: No such file or directory”

```bash
pacman -Qk # List missing files on the system
# glibc: "X" total files, "Y" missing files
# git: 792 total files, 200 missing files

# Example of reinstallation of some libraries
sudo pacman -Sy glibc linux-api-headers #Reinstallation of C packages
sudo pacman -Sy git #Reinstallation of git

pacman -Qk # List again the missing files
# glibc: 1600 total files, 0 missing files
# git: 792 total files, 0 missing files

# NOTE: glibc was necessary for me to compile C/C++ code! Maybe you won't need it
```

[Ref](https://www.reddit.com/r/SteamDeck/comments/t92ozw/for_compiling_c_code/?utm_content=title&utm_medium=post_embed&utm_name=33509299737d48cdaab913274cb5fc07&utm_source=embedly&utm_term=t92ozw)

### Other interesting links/comments

1. Some similar notes

    [Ref](https://jimmyhub.net/article/548?source=post_page-----06f6df15c0e5--------------------------------)
    [Ref](https://steamdecki.org/SteamOS/Read-only_Filesystem?source=post_page-----06f6df15c0e5--------------------------------)

2. Checking SteamOS pacman mirrors

    [Ref](https://steamdeck-packages.steamos.cloud/archlinux-mirror/holo-main/os/x86_64/?source=post_page-----06f6df15c0e5--------------------------------)
    [Ref](https://www.reddit.com/r/SteamDeck/comments/wre33e/desktop_mode_users_which_pacman_mirror_steamdeck/?utm_content=title&utm_medium=post_embed&utm_name=94be7c6279af496a936685b289413583&utm_source=embedly&utm_term=wre33e)

3. HoloISO (SteamOS alternative) Operating System

    [Ref](https://holoiso.ru.eu.org/?source=post_page-----06f6df15c0e5--------------------------------)

4. Steam Deck plugins

    [Ref](https://steamdecklife.com/category/steam-deck-plugins/?source=post_page-----06f6df15c0e5--------------------------------)
