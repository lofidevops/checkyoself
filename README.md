# Check yo' self (dibbity deb remix)

Which online repositories does my system rely on?

## Status

Very early prototype.

## Usage

```
Usage: checkyoself [OPTIONS]

  Before you wreck yo' self. Cause unknown sources are bad for your health.

  Returns a list of online repositories that your system relies on. Verbose
  mode additionally lists the software that uses them.

Options:
  --help  Show this message and exit.
```

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

## Development environment

### Setup

```
git checkout https://gitlab.com/lofidevops/checkyoself.git
cd checkyoself
pipenv --site-packages  # see note 1
PIP_IGNORE_INSTALLED=1 pipenv install -e . --dev  # see note 2
pipenv shell
```

You can use a text editor, or any IDE that supports virtualenv / pipenv.

For pipenv details see <https://pipenv.pypa.io/en/latest/>

**Note 1:** We must access system packages (aka site packages) so that we can `import apt`.

**Note 2:** ...but we prefer virtualenv packages to system packages. See
<https://pipenv.pypa.io/en/latest/advanced/#working-with-platform-provided-python-components> for details.

### Code quality

All cleanup and validation tasks should succeed without error or modification.

```
# coding conventions
black .
# unit tests
coverage run -m pytest
# coverage report (don't let it drop, aim for 90+)
coverage report > coverage.txt
# licensing metadata
reuse lint
# confirm no changes have occurred
git diff --exit-code
```

If all tasks pass, your changes are ready for submission. Otherwise you need
to fix, commit and validate again.

You can invoke these tasks in any CI/CD pipeline.

### Build

Build packages:

```
python setup.py sdist bdist_wheel
```

If everything works as expected you should end up with the files:

* `dist/<name>-<version>-py3-none-any.whl`
* `dist/<name>-<version>.tar.gz`

You can now optionally upload to PyPI:

```
twine upload dist/*
```

You can invoke these tasks in any CI/CD pipeline, but be aware of your threat model and the
security implications.

## Sharing and contributions

Check yo' self (dibbity deb remix) <br />
<https://gitlab.com/lofidevops/checkyoself> <br />
Copyright 2020 David Seaward and contributors <br />
SPDX-License-Identifier: GPL-3.0-or-later

Shared under GPL-3.0-or-later. We adhere to the Contributor Covenant
2.0 without modification, and certify origin per DCO 1.1 with a
signed-off-by line (`git -s`). Contributions under the same terms
are welcome.

For details see:

* [GPL-3.0-or-later.txt], full license text
* [CODE_OF_CONDUCT.md], full conduct text (report via a private ticket)
* [CONTRIBUTING.DCO.txt], full origin text

<!-- Links -->

[GPL-3.0-or-later.txt]: LICENSES/GPL-3.0-or-later.txt
[CODE_OF_CONDUCT.md]: CODE_OF_CONDUCT.md
[CONTRIBUTING.DCO.txt]: CONTRIBUTING.DCO.txt
