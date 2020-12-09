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

## Pre-requisites

You need [semantic-release](https://github.com/semantic-release/semantic-release) with the plugins [release-notes-generator](https://github.com/semantic-release/release-notes-generator) to generate the changelog, [commit-analyzer](https://github.com/semantic-release/commit-analyzer/) to analyze the commits and [git](https://github.com/semantic-release/git) to send it to your repository.

You will also need to use a CI/CD tool. For this example, we will use [Gitlab](https://about.gitlab.com/).

## Base config

Let's start from a simple semantic-release config file. For example, this `.releaserc` file.

```
{
    "repositoryUrl":"https://git.ah-blog.com/ah-blog",
    "branches":[
        {
            "name":"master"  
        },
        {
            "name":"staging",
            "prerelease":"staging"
        },
        {
            "name":"dev",
            "prerelease":"dev"
        }
    ],

   "plugins":[

      "@semantic-release/commit-analyzer",
      "@semantic-release/release-notes-generator",
      [

         "@semantic-release/changelog",
         {

            "changelogFile":"CHANGELOG.md",
            "changelogTitle":"Semantic Release Changelog"
         }
      ],
      [
         "@semantic-release/git",
         {
            "assets":[
               "CHANGELOG.md"
            ]
         }
      ]
   ]
}
```

## Step 1: Create one config file per changelog

Duplicate the `.releaserc` file above for each of your environments and suffix `.releaserc` with the name of the environment.

![List of files](/media/files.png "Files")

Now change the `CHANGELOG.md` file name in each `.releaserc` file to the file name you need for the environment. For example `CHANGELOG-DEV.md`

## Step 2: Get the right file at the right time

Now, edit your CI pipeline to change the filename. To do so you can use a base job and extend it once for each environment