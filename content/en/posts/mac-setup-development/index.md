---
title: Mac Setup 2022
description: How I set up my M1 MacBook Pro software development...
toc: true
authors:
  - roman-kurnovskii
tags:
  ["mac setup development", "mac setup web developer", "mac setup javascript"]
categories:
series:
date: "2022-05-18"
lastmod: "2022-05-18"
featuredImage:
draft: false
---

## MacBook Pro Specification

- 13-inch
- Apple M1 Pro M1 2020
- 16 GB RAM
- 512 GB SSD
- QWERTY = English/Hebrew
- macOS Monterey

## Homebrew

Install [Homebrew](https://brew.sh) as package manager for macOS:

```bash
## paste in terminal and follow the instructions
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"
```

Update everything in Homebrew to recent version:

```bash
brew update
```

Install GUI applications (read more about these in GUI Applications):

```bash
brew install --cask \
  google-chrome  \
  firefox \
  visual-studio-code \
  all-in-one-messenger \
  sublime-text \
  docker \
  rectangle \
  discord \
  vlc \
  figma \
  grammarly \
  macx-youtube-downloader \
  notion \
  postman \
  tor-browser \
  transmission \
  utm \
  viber \
  yandex-disk \
  zoom \
  mongodb-compass \
```

Install terminal applications (read more about these in Terminal Applications):

```bash
brew install \
  git \
  ffmpeg \
  nvm \

```

## GUI Applications

- [Google Chrome](https://www.google.com/chrome/) (web development, web browsing)
  - Preferences
    - set default browser
    - always show bookmarks
    - import bookmarks from previous machine
  - Chrome Developer Tools
    - Network -> only "Fetch/XHR"
  - Chrome Extensions
    - [React Developer Tools](https://chrome.google.com/webstore/detail/react-developer-tools/fmkadmapgofadopljbjfkapdkoienihi?hl=en)
    - [Pocket](https://chrome.google.com/webstore/detail/save-to-pocket/niloccemoadcdkdjlinkgdfekeahmflj?hl=en)
- [Firefox](https://www.google.com/chrome/) (web development)

* [Visual Studio Code](https://code.visualstudio.com/) (web development IDE)
* [Sublime Text](https://www.sublimetext.com/) (editor)
* [Docker](https://www.docker.com/products/docker-desktop) (Docker, see [setup](/docker-macos/))
  - used for running databases (e.g. PostgreSQL, MongoDB) in container without cluttering the Mac
  - Preferences
    - enable "Use Docker Compose"
* [VLC](https://www.videolan.org/vlc/) (video player)
  - use as default for video files
* [Maccy](https://maccy.app/) (clipboard manager)
  - enable "Launch at Login"

## Terminal Applications

- [nvm](https://github.com/nvm-sh/nvm) (node version manager)

## NVM for Node/npm

The [node version manager (NVM)](https://github.com/nvm-sh/nvm) is used to install and manage multiple Node versions. After you have installed it via Homebrew in a previous step, type the following commands to complete the installation:

```text
echo "source $(brew --prefix nvm)/nvm.sh" >> ~/.zshrc

source ~/.zshrc
## or alias
## zshsource
```

Now install the latest LTS version on the command line:

```text
nvm install <latest LTS version from https://nodejs.org/en/>
```

Afterward, check whether the installation was successful and whether the [node package manager (npm)](https://www.npmjs.com/) got installed along the way:

```text
node -v && npm -v
```

Update npm to its latest version:

```text
npm install -g npm@latest
```

And set defaults for npm:

```text
npm set init.author.name "your name"
npm set init.author.email "you@example.com"
npm set init.author.url "example.com"
```

If you are a library author, log in to npm too:

```text
npm adduser
```

That's it. If you want to list all your Node.js installation, type the following:

```text
nvm list
```

If you want to install a newer Node.js version, then type:

```text
nvm install <version> --reinstall-packages-from=$(nvm current)
nvm use <version>
nvm alias default <version>
```

Optionally install [yarn](https://yarnpkg.com/) if you use it as alternative to npm:

```text
npm install -g yarn
yarn -v
```

If you want to list all globally installed packages, run this command:

```text
npm list -g --depth=0
```

That's it. You have a running version of Node.js and its package manager.

[inspiration](https://www.robinwieruch.de/mac-setup-web-development/)