
```
 ______             _____  ______   __
|  ____|           |  __ \|  _ \ \ / /
| |__ _ __ ___  ___| |__) | |_) \ V /
|  __| '__/ _ \/ _ \  ___/|  _ < > <
| |  | | |  __/  __/ |    | |_) / . \
|_|  |_|  \___|\___|_|    |____/_/ \_\
Your Open Source Asterisk PBX GUI Solution
```

### What?

[FreePBX](http://www.freepbx.org/ "FreePBX Home Page") is an open source GUI (graphical user interface) that controls and manages [Asterisk©](http://www.asterisk.org/ "Asterisk Home Page") (PBX). FreePBX is licensed under GPL.

This is a FreePBX 17 installation script.

This script is to install FreePBX  on the top of vanilla Debian 12.x OS.

[FreePBX](http://www.freepbx.org/ "FreePBX Home Page") is a completely modular GUI for Asterisk written in PHP and Javascript. Meaning you can easily write any module you can think of and distribute it free of cost to your clients so that they can take advantage of beneficial features in [Asterisk](http://www.asterisk.org/ "Asterisk Home Page")

### Setting up a FreePBX system

[See our WIKI](https://sangomakb.atlassian.net/wiki/spaces/FP/pages/9732130/Install+FreePBX)

### Installing without Sangoma (TheTechNetwork fork)

This `custom/main` branch of TheTechNetwork's fork adds options to install from our own
builds instead of Sangoma's `deb.freepbx.org` repository. Without these options the
script behaves exactly like upstream.

| Option | Env variable | Meaning |
|---|---|---|
| `--asterisk-debs <src>` | `ASTERISK_DEBS` | Install Asterisk from our `.deb` files. `<src>` is a directory, a `.deb`, an archive of debs (`.tar.gz`, `.tgz`, `.tar.xz`, `.zip`) or an `http(s)` URL of one; several may be given separated by spaces. Every `*.deb` found is installed with `apt-get`, so Debian dependencies are resolved normally. |
| `--asterisk-deb-include <regex>` | `ASTERISK_DEB_INCLUDE` | Only install `.deb` files whose name matches (e.g. pick one Asterisk version from a release that has several). |
| `--asterisk-deb-exclude <regex>` | `ASTERISK_DEB_EXCLUDE` | Skip `.deb` files whose name matches. Default skips debug/dev packages and the IMAP/ODBC voicemail variants (they conflict with plain voicemail): `(-dbgsym\|-dbg\|-devel\|-dev\|-voicemail-imapstorage\|-voicemail-odbcstorage)_`. Set it to an empty string to install everything. |
| `--freepbx-tarball <src>` | `FREEPBX_TARBALL` | Install FreePBX from our `freepbx-17.0-full.tgz` (framework + modules, from a `freepbx-modules-17-*` release of TheTechNetwork/telephony-builds) with the classic `./start_asterisk start && ./install -n` instead of Sangoma's `freepbx17` package. File, directory or URL. |
| `--module-repo <url>` | `MODULE_REPO_URL` | Set FreePBX's `MODULE_REPO` (the server Module Admin downloads modules from) to our own module server. |
| `--no-sangoma` | | Refuse to use `deb.freepbx.org` at all; fails if something would still need it (e.g. `--dahdi`). Implied when both Asterisk and FreePBX come from our sources. |
| | `DOWNLOAD_AUTH_HEADER` | Extra HTTP header for the downloads above, e.g. `Authorization: token ...`. |
| | `INSTALLER_REPO_URL` | Where the version check fetches the latest copy of this script (default: this fork's `custom/main`). |

Example, with the release assets downloaded first (`gh release download ...`):

```bash
bash sng_freepbx_debian_install.sh --no-sangoma \
    --asterisk-debs /root/asterisk-debs \
    --freepbx-tarball /root/freepbx-modules/freepbx-17.0-full.tgz
```

In this mode the script does not add Sangoma's apt key, repository or pinning; it does
not install `sysadmin17`, `sangoma-pbx17`, `ioncube-loader-82` or `freepbx17`; it applies
the PHP/Apache settings FreePBX needs that `sangoma-pbx17` used to provide; and it skips
`fwconsole ma upgradeall` (unless `--module-repo` is given) and
`fwconsole ma refreshsignatures`, both of which would fetch Sangoma's module builds.
Packages that only exist in Sangoma's repository (e.g. `libtonezone`) are skipped.
DAHDI (`--dahdi`) is not available, because its kernel modules only exist there.
See `freepbx/INDEPENDENCE.md` in TheTechNetwork/telephony-builds.

### License

[This modules code is licensed as GPLv3+](https://www.gnu.org/licenses/gpl-3.0.txt)

### Contributing

To contribute code or modules back into the [FreePBX](http://www.freepbx.org/ "FreePBX Home Page") ecosystem you must fully read our Code License Agreement. We are not able to look at or accept patches or code of any kind until this document is filled out. To view and sign the contributor license agreement you can visit <https://oss-cla.sangoma.com/freepbx/sng_freepbx_debian_install>. Signing this contributor license agreement once allows you to contribute to all open source projects from Sangoma, including FreePBX. Please take a look at [https://sangomakb.atlassian.net/wiki/spaces/FP/pages/10682663/Code+License+Agreement](https://sangomakb.atlassian.net/wiki/spaces/FP/pages/10682663/Code+License+Agreement) for more information

### Issues

Please file bug reports at <https://github.com/FreePBX/issue-tracker/issues>

### How to execute the script

Steps -

1) ssh to the Debian system as 'root'

2) Download the file using `wget`:

```bash
wget https://github.com/FreePBX/sng_freepbx_debian_install/raw/master/sng_freepbx_debian_install.sh -O /tmp/sng_freepbx_debian_install.sh
```

3) Execute the script:

```bash
bash /tmp/sng_freepbx_debian_install.sh
```

The script will install the necessary dependencies for FreePBX, followed by the FreePBX software itself.

The installation duration may vary depending on your internet bandwidth and system capacity.

You can find detailed installation logs at `/var/log/pbx/freepbx17-install.log`.
