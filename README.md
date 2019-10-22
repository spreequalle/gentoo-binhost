# gentoo binhost

Providing [gentoo](https://gentoo.org/) binary packages using [github](https://github.com/) infrastructure.

<div style="display: inline"><img src="https://raw.githubusercontent.com/wiki/spreequalle/gentoo-binhost/images/gentoo-logo.png" alt="gentoo-logo" width="160" /></div>

This repo provides various gentoo [binary packages](https://wiki.gentoo.org/wiki/Binary_package_guide) for a variety of different architectures (checkout branches for details). This branch contains the script that is used for GitHub upload.

## Concept

The package upload is realized using a small upload script thats executed via portage [hooks](https://wiki.gentoo.org/wiki//etc/portage/bashrc). For every package that is being merged via portage the Gentoo *Packages* manifest file is committed to Git. The binary packages itself are not stored into repository there are uploaded as [GitHub release](https://developer.github.com/v3/repos/releases) artifacts.

To make everything work the following nomenclature has to apply:

Gentoo idiom|GitHub entity
------------|-------------
[CATEGORY](https://wiki.gentoo.org/wiki//etc/portage/categories)|GitHub release
[PF](https://devmanual.gentoo.org/ebuild-writing/variables/)|GitHub release asset
[CHOST](https://wiki.gentoo.org/wiki/CHOST)|Git branch name
[CHOST](https://wiki.gentoo.org/wiki/CHOST)/[CATEGORY](https://wiki.gentoo.org/wiki//etc/portage/categories)|Git release tag

## Usage

Setup a gentoo binhost Github and provide the following.

### Dependencies

The upload script uses Python3 and [PyGithub](https://github.com/PyGithub/PyGithub) module.

```shell
emerge dev-python/PyGithub
```

### Configuration

github upload can be easily configured.

#### make.conf

Enable gentoo binhost by adding the following lines.
```python
# enable binhost
PORTAGE_BINHOST_HEADER_URI="https://github.com/spreequalle/gentoo-binhost/releases/download/${CHOST}"
FEATURES="${FEATURES} buildpkg"
USE="${USE} bindist"
ACCEPT_LICENSE="-* @BINARY-REDISTRIBUTABLE"
```

Since github releases are used to store the packages *PORTAGE_BINHOST_HEADER_URI* has to be set here.

#### bashrc

Add the [/etc/portage/bashrc ](https://wiki.gentoo.org/wiki//etc/portage/bashrc) file below, if you use your own file make sure to call the **gh-upload.py** script during **postinst** phase.

```bash
#!/bin/env bash

if [[ ${EBUILD_PHASE} == 'postinst' ]]; then
  # FIXME come up with a more sophisticated approach to detect if binary package build is actually requested
  # commandline args like -B or --buildpkg-exclude and other conditionals are not supported right now.
  grep -q 'buildpkg' <<< {$PORTAGE_FEATURES}
  if [ $? -eq 0 ]; then
    /etc/portage/binhost/gh-upload.py
  fi
fi
```

#### gh-upload.py

Add the [/etc/portage/binhost/gh-upload.py](/etc/portage/binhost/gh-upload.py) script and add your github settings accordingly.
You need to create a [github access token](https://help.github.com/en/articles/creating-a-personal-access-token-for-the-command-line) that is able to access repository and create releases.

```python
gh_repo = 'spreequalle/gentoo-binhost'
gh_token = '<your github access token>'
```

## Disclaimer

Although this software is released under [JSON](/LICENSE) license, the binary packages come with their respective license according to *Packages* Manifest file. Refer to [gentoo license](https://devmanual.gentoo.org/general-concepts/licenses/index.html) for details.
