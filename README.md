# 🚀 NSISBI for Electron Builder 

[![NSISBI Version](https://img.shields.io/badge/NSISBI-3.10.3-blue?style=flat-square)](https://sourceforge.net/projects/nsisbi/)  [![Platforms](https://img.shields.io/badge/Supports-macOS%20|%20Linux%20|%20Windows-lightgrey?style=flat-square)]()

> **The Big Boy Installer Solution** 💪 For when your Electron App exceeds the puny 2GB maximum size of normal windows installers - *Pushes the theoretical max to 8EB!*

At it's core, this is pretty much a mirror of sourceforge project [NSISBI](https://www.sourceforge.net/projects/nsisbi/) a build tool for creating windows installers >2Gib. This repo also provides supplemental nicities for sane compatability with [Electron-Builder](https://github.com/electron-userland/electron-builder)'s windows-flavored build pipeline. 

## ⚡️ Plug and Play
In your Electron-Builder setup, simply configure [customNsisBinary](https://www.electron.build/nsis#customnsisbinary) to enable creation of windows installers larger than 2GiB:

```json
{
  // Your config
  // ...
  "build": {
    "nsis": {
      "customNsisBinary": {
        "url":"https://release-link-here-soon.co.uk",
        "checksum":"sha512-hex-here-soon"
      }
    }
  }
}
```

## 🚧 Warning for >4GB Installers


If your final installer.exe size exceeds 4GB, NSISBI generates a two-file installer:


> - 1️⃣ < Small installer.exe file >
> - 2️⃣ < Large data-file with  your app data >

Both files will appear in your normal electron-builder output directory. To distribute your two file installer simply `.zip` both files together and distribute the `.zip` file. 

The two files must remain in the same directory in order for the installer to work. 


### Why not one installer file? 
> While NSISBI is capable of producing windows `.exe` installers all the way up to 8EB, it must produce a two-file installer after 4Gib is exceeded. The NSISBI author [goes into more detail](https://sourceforge.net/projects/nsisbi/files/nsisbi3.01.1/) on this topic if you're interested.

## 🍎 Cross-Platform Builds 🐧


> **🚨 You must have wine installed on your machine** in order to build for windows on a MacOS/Linux environment.

### Install Wine
- 🍎 MacOS [**[Brew]**](https://brew.sh/) `brew install --cask --no-quarantine wine-stable`
- 🐧 Linux [**[Debian]**](https://wiki.debian.org/Wine)  `sudo apt install wine wine32 libwine fonts-wine`
- 🐧 Linux [**[Arch]**](https://wiki.archlinux.org/title/Wine) `sudo pacman -S wine`

### Why Wine?
> The decade old version of NSIS that ships with electron builder by default has native compiled MacOS/Linux NSIS binaries.
>
> NSISBI and pretty much all recent releases of NSIS do not ship cross-platform binaries. It is much easier and reliable to persue cross-platform builds via wine.

### How Wine?
> We put shell-scripts in the binary-target paths Electron-Builder uses for NSIS on Mac/Linux. The scripts invoke `wine` to execute our NSISBI flavored `./Bin/makensis.exe` and wine-ify all the local file paths transmitted to the process.
> 
> This uses the wine installed on the machine you're building with `which wine` and not any static-versions of wine packaged with electron-builder.

## 📜 Honorable Mentions
- NSISBI Author @JasonFriday13 [**[SourceForge]**](https://sourceforge.net/u/jasonfriday13/profile/)
- Electron-Builder [**[GitHub]**](https://github.com/electron-userland/electron-builder)

## ❤️ Contributing
Is the NSISBI version out of date? Found a Bug? Want more features?  [Open an issue](https://github.com/your/repo/issues) or PR!

## Creating Releases
Electron-Builder has a pretty *interesting* escape hatch for using custom NSIS binaries in your build, namely:

1. You must provide a remote URL for your custom NSIS binary inside a 7zip archive.
2. You must provide a the base64 encoded sha512 checksum of the the aforementioned .7z archive

#### Release Generation CMDs
```bash
7z a -r nsisbi-electronbuilder-VERSION.7z *
cat  ./nsisbi-electronbuilder-VERSION.7z | openssl dgst -sha512 -binary | base64
# To use with electron-builder:
# [ ] -> You must host .7z at a remote URL
# [ ] -> You must include sha512 checksum in your electron-builder config
```
---

*Sheparded with ❤️ by the Electron-Builder community*

