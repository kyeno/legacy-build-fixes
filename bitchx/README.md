# BitchX 1.2.1 Legacy Patches 💻💬

![Gentoo-Tested](https://img.shields.io/badge/Gentoo-tested-purple?style=flat-square)
![GCC-15](https://img.shields.io/badge/GCC-15.3-blue?style=flat-square)

This repository contains modern compatibility patches to compile the legendary **BitchX 1.2.1** IRC client on contemporary Linux distributions. 

BitchX was originally released in an era of vastly different compiler standards. This patch resolves compilation failures introduced by strict modern toolchains, modern C standards, and updated core system libraries (GCC 15+, OpenSSL 3.x, etc.)—allowing you to run this legendary client inside a modern terminal emulator without virtualization.

---

## 📦 Resources & Downloads

* **Official SourceForge Home:** [bitchx.sourceforge.net](https://bitchx.sourceforge.net/)
* **Original Source Archive:** [bitchx-1.2.1.tar.gz](https://sourceforge.net/projects/bitchx/files/ircii-pana/bitchx-1.2.1/bitchx-1.2.1.tar.gz/download)
* **Required Patch:** [bitchx121-modernlinux.patch](./bitchx121-modernlinux.patch)

---

## 🛠️ Build Instructions

Follow these steps to apply the patch and compile BitchX from source:

1. **Download and extract** the official BitchX 1.2.1 source tarball.
2. **Move the patch file** (`bitchx121-modernlinux.patch`) directly into the root of the extracted directory.
3. **Apply the patch** using the utility:
   ```bash
   patch -p1 < bitchx121-modernlinux.patch
   ```
4. **Configure the build tree** using the verified flags described below.
5. **Compile and install** the binaries:
   ```bash
   make
   sudo make install
   ```

### Verified Configuration Flags

The following `./configure` layout is confirmed to build cleanly:
```bash
./configure \
  --enable-ipv6 \
  --with-tcl \
  --with-plugins
```

---

## 🖥️ Tested Environment (Build Stack)

This patch was successfully compiled, linked, and verified on a modern rolling-release environment:

* **OS:** Gentoo Linux (Profile 23.0, `amd64`)
* **Compiler:** GCC 15.3.0
* **TCL Version:** 8.6.15

---

## ⚠️ Scope Limits (Untested & Unfixed)

Because this patch was created with the sole intention of running BitchX natively inside modern Linux terminal emulators, several outdated, graphical, or platform-specific subsystems were **not** verified or fixed:

* **Graphical/Media Components:** GTK interface, XMMS, audiofile, cdrom
* **Non-Linux Platforms:** Win32, OS/2, BSD
* **Known Unresolved Issues:** The legacy SOCKS5 implementation remains broken and was not patched. Contributions to resolve this are welcome.
