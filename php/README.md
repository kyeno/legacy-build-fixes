# PHP Legacy Patches

## PHP 7.4.x
* **Official Release Archives:** [php.net/releases](https://www.php.net/releases/)
* **Latest Source Archive:** [php-7.4.33.tar.bz2](https://www.php.net/distributions/php-7.4.33.tar.bz2)
* **Required Patch:** [php74-modernlinux.patch](./php74-modernlinux.patch)

This patch introduces the necessary modifications to compile PHP 7.4 on modern Linux distributions, resolving build failures caused by recent toolchains and updated core libraries (GCC 15+, OpenSSL 3.x, etc.).

---

## Build Instructions

1. **Download and extract** the latest PHP 7.4.33 source archive.
2. **Download the patch** and place it directly into the root of the extracted source directory.
3. **Apply the patch** by running:
   ```bash
   patch -p1 < php74-modernlinux.patch
   ```
4. **Regenerate the configure script**:
   ```bash
   ./buildconf --force
   ```
5. **Configure, build, and install** the binaries.

## Tested Configuration Flags

Below is a confirmed working `./configure` setup for a modern, feature-rich deployment (including FPM, MySQL, GD, and OpenSSL support):
```bash
./configure \
  --enable-fpm \
  --with-fpm-user=nobody \
  --with-fpm-group=nobody \
  --disable-cgi \
  --enable-bcmath \
  --with-curl \
  --enable-calendar \
  --with-mhash \
  --with-gettext \
  --enable-mbstring \
  --with-mysqli \
  --with-mysql-sock=/var/run/mysqld/mysqld.sock \
  --with-pdo-mysql \
  --enable-sockets \
  --with-zlib \
  --with-zip \
  --with-pear \
  --enable-gd \
  --with-webp \
  --with-jpeg \
  --with-freetype \
  --enable-exif \
  --enable-intl \
  --with-openssl
```

## Tested Environment (Build Stack)

This patch has been successfully compiled and verified against the following modern, rolling-release environment:

* **OS:** Gentoo Linux (Profile 23.0, amd64)
* **Compiler:** GCC 15.3.0
* **Crypto Library:** OpenSSL 3.5.7
* **XML Parser:** libxml2 2.15.3
* **Database Driver:** MariaDB 10.6.27
* **Regular Expressions:** oniguruma 6.9.10
* **Internationalization (i18n):** ICU 78.3
* **Compression Libraries:** 
  * zlib 1.3.2-r1
  * libzip 1.11.4-r2
* **HTTP Transfer Library:** libcurl 8.20.0-r1
* **Graphics & Imaging (GD):**
  * freetype 2.14.3
  * libwebp 1.6.0
  * libjpeg-turbo 3.1.4.1
  * libpng 1.6.58
