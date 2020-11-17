---
template: post
title: Use Cypress with VueJS and MSAL 2.0
slug: cypress-vue-msal
socialImage: /media/vue.js_logo_2.svg
draft: true
date: 2020-11-17T22:22:52.051Z
description: A few explanations on how to authenticate with Cypress on Azure
  MSAL 2.0 in a VueJS 2 application.
category: VueJS
tags:
  - Handwriting
  - Learning to write
---
Due to the multiple redirects and iframes, it can be pretty painful to authenticate with Cypress on an app with MSAL 2.0. However, it is possible to pass the authentication step by using an Azure client secret. Here is how.

## Pre-requisites
To start we need an up and running VueJS application using the msal-browser library to authenticate with MSAL 2.0.
In our example we will take as an hypothesis that the MSAL cacheLocation is set to "localstorage"
To get some useful commands to manipulate the localstorage with Cypress we need to install the [Cypress localstorage commands module](https://www.npmjs.com/package/cypress-localstorage-commands).  

It can be done with  

`npm i --save-dev cypress-localstorage-commands`

## 


Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo. Quisque sit amet est et sapien ullamcorper pharetra. Vestibulum erat wisi, condimentum sed, commodo vitae, ornare sit amet, wisi. Aenean fermentum, elit eget tincidunt condimentum, eros ipsum rutrum orci, sagittis tempus lacus enim ac dui. Donec non enim in turpis pulvinar facilisis. Ut felis. 

Praesent dapibus, neque id cursus faucibus, tortor neque egestas augue, eu vulputate magna eros eu erat. Aliquam erat volutpat. Nam dui mi, tincidunt quis, accumsan porttitor, facilisis luctus, metus.