---
template: post
title: MSAL authentication with Vue 2
slug: msal-authentication-with-vue-2
socialImage: /media/vue.js_logo_2.svg
draft: true
date: 2020-11-16T20:43:27.244Z
description: It is an example of implementation of an MSAL authentication with
  Vue2 and msal-browser. In this example we will setup a VueJS plugin, load it
  and use it to authenticate our user on an SPA. The authentication is in
  redirect mode, does not require a backend server but will provide a JWT access
  token that we will be able to use in our Authorization header to communicate
  with our APIs.
category: VueJS
---
It is an example of implementation of an MSAL authentication with [Vue2](https://vuejs.org/) and [msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser). In this example we will setup a VueJS plugin, load it and use it to authenticate our user on an SPA. The authentication is in redirect mode, does not require a backend server but will provide a [JWT access token v2.0](https://docs.microsoft.com/en-us/azure/active-directory/develop/access-tokens) that we will be able to use in our Authorization header to communicate with our APIs.

## Pre-requisites
You need to have a vue app up and running and the Node Packet Manager installed on your computer.

## Azure portal setup

To setup your Azure portal for MSAL 2.0 you can follow [this guideline](https://docs.microsoft.com/en-us/azure/active-directory/develop/scenario-spa-app-registration).

## Modules installation

We need the following module to set up our authentication flow:

* [msal-browser](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-browser) which will be the base of our authentication flow
* [msal](https://github.com/AzureAD/microsoft-authentication-library-for-js/tree/dev/lib/msal-core) which will allow use to catch the `InteractionRequiredAuthError` generated when we try to acquire a token for a non-authenticated user

You can install them by using NPM with the following command:

`npm install @azure/msal-browser msal`

## Plugin setup

## Header Level 2

* Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
* Aliquam tincidunt mauris eu risus.

Donec non enim in turpis pulvinar facilisis. Ut felis. Praesent dapibus, neque id cursus faucibus, tortor neque egestas augue, eu vulputate magna eros eu erat. Aliquam erat volutpat. 

<figure>
	<blockquote>
		<p>Lorem ipsum dolor sit amet, consectetur adipiscing elit. Vivamus magna. Cras in mi at felis aliquet congue. Ut a est eget ligula molestie gravida. Curabitur massa. Donec eleifend, libero at sagittis mollis, tellus est malesuada tellus, at luctus turpis elit sit amet quam. Vivamus pretium ornare est.</p>
		<footer>
			<cite>— Aliquam tincidunt mauris eu risus.</cite>
		</footer>
	</blockquote>
</figure>

### Header Level 3

* Lorem ipsum dolor sit amet, consectetuer adipiscing elit.
* Aliquam tincidunt mauris eu risus.

Pellentesque habitant morbi tristique senectus et netus et malesuada fames ac turpis egestas. Vestibulum tortor quam, feugiat vitae, ultricies eget, tempor sit amet, ante. Donec eu libero sit amet quam egestas semper. Aenean ultricies mi vitae est. Mauris placerat eleifend leo. Quisque sit amet est et sapien ullamcorper pharetra.

```css
#header h1 a {
  display: block;
  width: 300px;
  height: 80px;
}
```

Donec non enim in turpis pulvinar facilisis. Ut felis. Praesent dapibus, neque id cursus faucibus, tortor neque egestas augue, eu vulputate magna eros eu erat. Aliquam erat volutpat. Nam dui mi, tincidunt quis, accumsan porttitor, facilisis luctus, metus.