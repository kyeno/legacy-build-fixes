# PHP Legacy Patches

![Gentoo tested](https://img.shields.io/badge/Gentoo-tested-purple?style=flat-square)
![GCC 15](https://img.shields.io/badge/GCC-15.3-blue?style=flat-square)

These patches target the final PHP 7.4.33 and PHP 5.4.45 releases. Get the original archives from [php.net](https://www.php.net/releases/).

| Release | Patch | Contents |
| --- | --- | --- |
| PHP 7.4.33 | [php74-modernlinux.patch.xz](./php74-modernlinux.patch.xz) | Modern Linux build fixes and CVE-2026-109618 / CVE-2026-109619 backports |
| PHP 5.4.45 | [php54-modernlinux.patch.xz](./php54-modernlinux.patch.xz) | Modern Linux build fixes only |
| PHP 5.4.45 | [php54-modernlinux-cve.patch.xz](./php54-modernlinux-cve.patch.xz) | The same build fixes plus the CVE-2026-109618 / CVE-2026-109619 backport |

The PHP 7.4 patch uses the 7.4.33 release archive as its base and retains the PHP team's CVE branch versioning. After build configuration, the patched binary reports **7.4.34-dev**.

Choose **one** PHP 5.4 patch and apply it to a fresh 5.4.45 tree. The CVE archive already contains the build fixes. The PHP 5.4 security changes are a community backport adapted from [PHP's 7.4 CVE branch](https://github.com/php/php-src/tree/PHP-7.4-CVE-2026-109618-109619). PHP 5.4 is outside upstream support, so this backport is not an upstream security release or a guarantee that every vulnerability is fixed. Its multipart mitigation disables session.upload_progress tracking. See the [Qualys advisory](https://blog.qualys.com/vulnerabilities-threat-research/2026/10/20/php-announces-critical-security-release-for-two-vulnerabilities) for the CVEs and supported fixed versions.

## Apply and build PHP 7.4.33

From the extracted php-7.4.33 directory:

~~~sh
xzcat /path/to/php74-modernlinux.patch.xz | patch -p1
./buildconf --force
./configure \
  --enable-fpm --with-fpm-user=nobody --with-fpm-group=nobody \
  --disable-cgi --enable-bcmath --with-curl --enable-calendar \
  --with-mhash --with-gettext --enable-mbstring \
  --with-mysqli --with-pdo-mysql --enable-sockets \
  --with-zlib --with-zip --with-pear \
  --enable-gd --with-webp --with-jpeg --with-freetype \
  --enable-exif --enable-intl --with-openssl
make -j"$(nproc)"
~~~

Add --with-mysql-sock=/path/to/mysql.sock if your database setup needs a specific socket. Run make install when you are ready to install into the configured prefix.

## Apply and build PHP 5.4.45

From a fresh extracted php-5.4.45 directory, use **one** of these commands:

~~~sh
# Build fixes only:
xzcat /path/to/php54-modernlinux.patch.xz | patch -p1

# Or, in a separate fresh tree, build fixes plus the CVE backport:
xzcat /path/to/php54-modernlinux-cve.patch.xz | patch -p1
~~~

The PHP 5.4 patches include modified configure, acinclude.m4, and aclocal.m4 files. Run the patched ./configure directly. Running ./buildconf --force would regenerate those files and replace the patched configuration.

For example, this uses PHP 5.4 option names:

~~~sh
./configure \
  --enable-fpm --with-fpm-user=nobody --with-fpm-group=nobody \
  --disable-cgi --enable-bcmath --with-curl --enable-calendar \
  --with-gettext --enable-mbstring --with-mysqli=mysqlnd \
  --with-pdo-mysql=mysqlnd --enable-sockets --with-zlib \
  --enable-zip --with-gd --with-jpeg-dir=/usr \
  --with-png-dir=/usr --with-freetype-dir=/usr \
  --enable-exif --enable-intl --with-icu-dir=/usr \
  --with-openssl --without-pear
make -j"$(nproc)"
~~~

PHP 5.4 uses --with-gd, --with-jpeg-dir, --with-png-dir, --with-freetype-dir, and --enable-zip where the PHP 7.4 example uses --enable-gd, --with-jpeg, --with-freetype, and --with-zip. PHP 5.4 has --with-vpx-dir for its older WebP support, not --with-webp. If you use --disable-all, explicitly add --enable-session when session support is needed. Adjust library prefixes, MySQL socket, and enabled extensions for your system. The PHP 5.4 example is an option-name guide; the exact feature combination depends on installed libraries.

## Tested build environment

The build fixes were developed on Gentoo Linux (Profile 23.0, amd64), GCC 15.3.0, OpenSSL 3.5.8, libxml2 2.15.4, MariaDB 10.6.27, oniguruma 6.9.10, ICU 78.3, zlib 1.3.2-r1, libzip 1.11.4-r2, libcurl 8.20.0-r1, FreeType 2.14.3, libwebp 1.6.0, libjpeg-turbo 3.1.4.1, and libpng 1.6.58.
