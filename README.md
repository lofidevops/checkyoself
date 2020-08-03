# Check yo' self

Which online repositories does my system rely on?

Returns a list of inferred repositories. Verbose mode
additionally lists the installed software that uses them.

## Status

Very early prototype.

## Run (prototype)

1. Copy `checkyoself.py`
2. `apt install python3-parse`
3. `python3 -m checkyoself`

## Sample output

These are the repositories that your system might download
additional components from.

```
* deb.debian.org
* dl.flathub.org
* http://ppa.launchpad.net
* pypi.org
```

## Sample output (verbose)

These are the sources that might access the repositories:

```
* deb.debian.org
   * /etc/apt/sources.list.d/buster-backports.list
* dl.flathub.org
   * flatpak
* http://ppa.launchpad.net
   * /etc/apt/sources.list.d/micahflee-ubuntu-ppa-bionic.list
   * /etc/apt/sources.list.d/papirus.list
* pypi.org
   * apt:python3-pip
```

## Scope

* Packages installed by `apt` [1]
* Knowledge base limited to packages found in Debian stable, PureOS stable
  and/or Ubuntu LTS [1]
* Best effort. We don't know what we don't know.
* No advisory stance (libre? security practices? domain owner?)
* Packages with hard-coded repositories or default configurations, not
  packages like `wget` that perform arbitrary downloads or packages like
  `telnet` that perform arbitrary communication.
* Known default repositories only [2]

##### Exceptions and additions

* [1] We report on the online repositories of non-deb packages from
  very significant software authors. For example `aws` from Amazon relies
  on `aws.amazon.com`
* [2] We report on the configuration values of `apt` and `flatpak`, not
  just their defaults.

## Additional notes

* Inspired by, but not quite the same as, [vrms](https://salsa.debian.org/debian/vrms)

* Not a substitute for inspecting which domains your machine is *actually*
  connecting to.

* Intended to inspire recording this information at the time of packaging, and
  alerting the user at the time of installation: "foo relies on bar.com, do you
  want to install it? Y/N"

## Sharing

Check yo' self <br />
<https://gitlab.com/lofidevops/checkyoself> <br />
Copyright 2020 David Seaward and contributors <br />
SPDX-License-Identifier: GPL-3.0+

Shared under GPLv3-or-later, see [COPYING.GPL.md](COPYING.GPL.md)
for details. Contributions under the same terms are welcome.
