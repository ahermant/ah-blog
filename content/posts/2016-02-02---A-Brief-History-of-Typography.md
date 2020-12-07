---
template: post
title: "Semantic-release: Build different Changelog files for DEV, STAGING and PROD"
slug: semantic-release-multiple-changelog
socialImage: /media/semantic-release-logo.png
draft: true
date: 2020-12-07T19:47:02.519Z
description: Explanation of a trick to get one CHANGELOG file per environment
  with semantic-release
category: Semantic Release
tags:
  - Semantic
  - release
  - multiple
  - changelog
---
[Semantic-release](https://github.com/semantic-release/semantic-release) is a great tool to manage automatically your semver versions. On top of it it can update your changelogs automatically thanks to the [changelog plugin](https://github.com/semantic-release/changelog).
However some problems may occur if you want to manage several changelogs in parallel: for example one for your DEV branch, one another for your STAGING branch and one last one for your PRODUCTION branch.

## Base config

Let's start from a simple semantic-release config file. For example, this `.releaserc` file.

```
{

}
```