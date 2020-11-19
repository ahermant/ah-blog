---
template: post
title: Vue MSAL Browser plugin
slug: vue-msal-browser-plugin
socialImage: /media/vue.js_logo_2.svg
draft: false
date: 2020-11-19T10:19:17.461Z
description: A wrapper to use the full capabilities of the msal-browser library with VueJS. 
category: VueJS
tags:
  - MSAL
  - VueJS
  - VueX
  - VueMSAL
  - authentication
  - msal-browser
---

If you need to authenticate your users through MSAL 2.0, want to use the MSAL authentication in your VueX Store and want to be able to use all the methods of the [Microsoft msal-browser library](https://github.com/AzureAD/microsoft-authentication-library-for-js), you can now use the [vue-msal-browser plugin](https://www.npmjs.com/package/vue-msal-browser).

## Installation

You can install it easily with yarn or npm

```bash
npm install vue-msal-browser
or
yarn add vue-msal-browser
```

## Usage

Then, initialize the plugin in your main.js

``` javascript
import { default as msalPlugin } from "msal-browser";
const msalConfig = {
  auth: {
    tenant: '<your-tenant>',
    clientId: '<your-client-id>',
    authority: '<your-tenant-address>',
    redirectUri: '<your-redirect-url>', // It has to be configured on your Azure tenant
    scopes: ['<your-scopes>']
  }
}
Vue.use(msalPlugin, msalConfig);
```

Use the plugin from a Vue component

``` javascript

<template>
    <a v-if="$msal.isAuthenticated()" class="toolbar-items" @click="$emit('logout')">
        <v-icon color="tertiary">mdi-logout</v-icon>
    </a>
</template>

<script>
    export default {
        created() {
            if (!this.$msal.isAuthenticated()) {
                try {
                    this.$msal.loginRedirect({});
                } catch (err) {
                    // handle error
                }
            }
        }
    }
</script>

```

Or from your store actions

``` javascript
import { msalInstance } from "vue-msal-browser"
export default {
  async AzureAuthentication() {
    msalInstance.loginRedirect({});
  }
}
```

Do not hesitate to create an issue on [my github page](https://github.com/ahermant/vue-msal-browser) if you have some feedbacks or evolution requests.  
Your MR are also welcome ! 