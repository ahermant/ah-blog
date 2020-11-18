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
  - MSAL
  - Cypress
  - VueJS
  - authentication
---
Due to the multiple redirects and iframes, it can be pretty painful to authenticate with Cypress on an app with MSAL 2.0. However, it is possible to bypass the authentication step by using an Azure client secret.  
If you use the MSAL mode `redirect` and get an access token to authenticate to your backend APIs, here is a method. It is far from ideal since the changes in the msal-browser library but it can unblock a difficult situation.

## Pre-requisites
To start we need an up and running VueJS application using the msal-browser library to authenticate with MSAL 2.0.  
In our example we will take as an hypothesis that the MSAL cacheLocation is set to "localstorage".  
To get some useful commands to manipulate the localstorage with Cypress we need to install the [Cypress localstorage commands module](https://www.npmjs.com/package/cypress-localstorage-commands).  

It can be done with  

`npm i --save-dev cypress-localstorage-commands`

## Creation of a client secret

Finally create a client secret for your app by following [this process](https://docs.microsoft.com/en-us/azure/active-directory/develop/howto-create-service-principal-portal#option-2-create-a-new-application-secret). Keep it preciously for our next steps.

## Command setup

Next, if you do not already have one, create a `commands.js` file in the Cypress support folder (For more info about this folder location refer to the  [Cypress folder structure](https://docs.cypress.io/guides/core-concepts/writing-and-organizing-tests.html#Folder-Structure).

In the `commands.js` file, import `cypress-localstorage-commands` to be able to use it.

`import "cypress-localstorage-commands"`

Now create a Cypress command named `login`. We will use this command to call the [Microsoft OAuth2 token API](https://docs.microsoft.com/en-us/azure/active-directory/develop/v2-oauth2-auth-code-flow#request-an-access-token) to get an access token and then create some fake localstorage info including the token.

``` javascript
Cypress.Commands.add("login", () => {
})
```

Let us set some variables with the information we need to request the access token.

``` javascript
Cypress.Commands.add("login", () => {
    const tenantId = Cypress.env('TENANT_ID') // directory (tenant) ID displayed in the Overview of your Azure app
    const clientId = Cypress.env('CLIENT_ID') // The application client ID available in the overview of your app
    const clientSecret = Cypress.env('CLIENT_SECRET') // The client Secret that you have generated
    const scope = Cypress.env('SCOPE') // The scope you want to connect to. For example User.Read   
})
```

Now we do the request to get the token from the client secret

``` javascript
    cy.request({
        method: 'POST',
        url: `https://login.microsoftonline.com/${tenantId}/oauth2/token/`,
        form: true,
        body: {
            grant_type: 'client_credentials',
            client_id: clientId,
            client_info: 1,
            client_secret: clientSecret,
            scope: [scope]
        }
    }).then(response => {})
```

When we get the response we set the extra variables we will need in our local storage.  
The user variable is not needed by MSAL: you can replace it with any data your frontend needs to identify your test user (eg: name, email...)


``` javascript
    const localAccountId = Cypress.env('LOCAL_ACCOUNT_ID') // The localAccountId in the field mainTokenResponse of your localstorage when authenticated with the Cypress user account
    const username = Cypress.env('USERNAME') // The username displayed in the mainTokenResponse when you log in with this account
    const name = Cypress.env('NAME') // The name displayed in the mainTokenResponse when you log in with this account
    const user = Cypress.env('USER') // Do not forget to set the user data that your application needs to work properly because the client secret is not linked to any user
    const token = response.body.access_token;
    const clientInfo = response.body.client_info;
```

Then we set the local storage according to what MSAL is expecting

``` javascript        
        cy.setLocalStorage(`${localAccountId}.${tenantId}-login.windows.net-accesstoken-${clientId}-${tenantId}-${scope}`, `{"homeAccountId":"${localAccountId}.${tenantId}","credentialType":"AccessToken","secret":"${token}","cachedAt":"1602169142","expiresOn":"1602176233","extendedExpiresOn":"1602309832","environment":"login.windows.net","clientId":"${clientId}","realm":"${tenantId}","target":"${scope}"}`);
        cy.setLocalStorage(`${localAccountId}.${tenantId}-login.windows.net-${tenantId}`, `{"authorityType":"MSSTS","clientInfo":"${clientInfo}","homeAccountId":"${localAccountId}.${tenantId}","environment":"login.windows.net","realm":"${tenantId}","localAccountId":"${localAccountId}","username":"${username}","name":"${name}"}`);
        cy.setLocalStorage(`${localAccountId}.${tenantId}-login.windows.net-idtoken-${clientId}-${tenantId}-`, `{"credentialType":"IdToken","homeAccountId":"${localAccountId}.${tenantId}","environment":"login.windows.net","clientId":"${clientId}","secret":"${token}","realm":"${tenantId}"}`);
        cy.setLocalStorage(`mainTokenResponse`, `{"accessToken":"${token}", "account":{"homeAccountId": "${localAccountId}.${tenantId}","environment": "login.windows.net","tenantId": "${tenantId}","username": "${username}","name": "${name}"}}`);
        cy.setLocalStorage("user", user); 
```

The final command should look like this

``` javascript
Cypress.Commands.add("login", () => {
    const tenantId = Cypress.env('TENANT_ID') // directory (tenant) ID displayed in the Overview of your Azure app
    const clientId = Cypress.env('CLIENT_ID') // The application client ID available in the overview of your app
    const clientSecret = Cypress.env('CLIENT_SECRET') // The client Secret that you have generated
    const scope = Cypress.env('SCOPE') // The scope you want to connect to. For example User.Read   
    cy.request({
        method: 'POST',
        url: `https://login.microsoftonline.com/${tenantId}/oauth2/token/`,
        form: true,
        body: {
            grant_type: 'client_credentials',
            client_id: clientId,
            client_info: 1,
            client_secret: clientSecret,
            scope: [scope]
        }
    }).then(response => {
        const token = response.body.access_token;
        const clientInfo = response.body.client_info;
        const localAccountId = Cypress.env('LOCAL_ACCOUNT_ID') // The localAccountId in the field mainTokenResponse of your localstorage when authenticated with the Cypress user account
        const username = Cypress.env('USERNAME') // The username displayed in the mainTokenResponse when you log in with this account
        const name = Cypress.env('NAME') // The name displayed in the mainTokenResponse when you log in with this account
        const user = Cypress.env('USER') // Do not forget to set the user data that your application needs to work properly because the client secret is not linked to any user

        cy.setLocalStorage(`${localAccountId}.${tenantId}-login.windows.net-accesstoken-${clientId}-${tenantId}-${scope}`, `{"homeAccountId":"${localAccountId}.${tenantId}","credentialType":"AccessToken","secret":"${token}","cachedAt":"1602169142","expiresOn":"1602176233","extendedExpiresOn":"1602309832","environment":"login.windows.net","clientId":"${clientId}","realm":"${tenantId}","target":"${scope}"}`);
        cy.setLocalStorage(`${localAccountId}.${tenantId}-login.windows.net-${tenantId}`, `{"authorityType":"MSSTS","clientInfo":"${clientInfo}","homeAccountId":"${localAccountId}.${tenantId}","environment":"login.windows.net","realm":"${tenantId}","localAccountId":"${localAccountId}","username":"${username}","name":"${name}"}`);
        cy.setLocalStorage(`${localAccountId}.${tenantId}-login.windows.net-idtoken-${clientId}-${tenantId}-`, `{"credentialType":"IdToken","homeAccountId":"${localAccountId}.${tenantId}","environment":"login.windows.net","clientId":"${clientId}","secret":"${token}","realm":"${tenantId}"}`);
        cy.setLocalStorage(`mainTokenResponse`, `{"accessToken":"${token}", "account":{"homeAccountId": "${localAccountId}.${tenantId}","environment": "login.windows.net","tenantId": "${tenantId}","username": "${username}","name": "${name}"}}`);
        cy.setLocalStorage("user", user); 
    })
})
``` 

## Use the command

In order to do use the command, et the following in your `support/index.js` file

``` javascript
import './commands'

before(() => {
    cy.clearLocalStorageSnapshot();
});

afterEach(() => {
    cy.saveLocalStorage();
});

describe('Logging in - Azure authentication', function () {

    context('Logs to ADAL', function () {
        it('Sets the local storage to log to MSAL', function () {
            cy.login()
        })
    })
})

```

Now the login command should start before your tests and record the MSAL data in your localstorage. 

