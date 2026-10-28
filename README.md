# Legacy Software Patches & Build Guides

Software development moves at a breakneck pace. Major versions change overnight, APIs are deprecated, dependencies shift, and backward compatibility is frequently dropped.

Yet, there are times when you absolutely *need* to run a legacy application on modern hardware, only to find out that the source code no longer compiles on today's toolchains.

This repository aims to bridge that gap by providing patches, workarounds, and non-structural modifications to legacy software, enabling it to be successfully built on modern operating systems.

> ⚠️ **Security Warning:**
> Compiling legacy software on a modern stack does **not** fix inherent security vulnerabilities within the application itself. Deploying these programs may expose your system to risks. Use at your own discretion, preferably within isolated or sandboxed environments.

**PHP patches:** The PHP 7.4 patch includes backports for CVE-2026-109618 and CVE-2026-109619. PHP 5.4 has separate build-only and build-plus-CVE variants; see the [PHP guide](php/README.md) for their limits and build instructions.

## License & Disclaimer

The patches, build guides, and configuration scripts provided in this repository are licensed under the [MIT License](LICENSE).

**Upstream Software Licenses:**
Please note that the legacy software projects targeted by these patches (such as PHP, MPlayer, etc.) are governed by their own respective upstream licenses (e.g., GPL, PHP License, BSD). Applying these patches does not alter, override, or grant any additional rights to the original software.
