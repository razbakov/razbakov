---
title: Vue + Google Firebase Analytics
description: How to integrate Google Analytics App + Web and Firebase Analytics into Vue (Quasar)?
permalink: /vue-google-firebase-analytics
date: 2020-01-12
image: https://thepracticaldev.s3.amazonaws.com/i/z96uk4p7dwsx7nx9f3jm.png
tags: 
  - Vue
  - Analytics
  - Quasar
  - Firebase
category: WebDev
tweet: https://twitter.com/razbakov/status/1233891527916691463
---

**Google Analytics App + Web** is available when you enable **Google Firebase Analytics.**

It's Google Analytics v2 with a new representation of data, which focuses on users and events rather than sessions, user properties and event parameters rather than hit, session and user scoped definitions, on audiences rather than segments. It has a powerful live time view and DebugView. It's a [new way to unify app and website measurement in Google Analytics](https://www.blog.google/products/marketingplatform/analytics/new-way-unify-app-and-website-measurement-google-analytics/).

I am using [Quasar Framework for Vue](https://quasar.dev/) to create a Single Page Application (SPA), Progressive Web App (PWA) and planning to build native applications with Cordova and Electron, with build scripts provided by Quasar. While building [my new budgeting app](https://dev.to/razbakov/am-i-safe-to-spend-3dpo) recently I discovered Firebase Analytics and decided to give it a try and enable analytics. I haven't tested it yet in native apps though.

## Setup

I will show you how to track events in Vue application, but first you will need to [activate Firebase Analytics](https://www.lovesdata.com/blog/google-analytics-app-web-property). You will find there a video tutorial how to do it.

Next thing is to configure Firebase, I just followed setup instructions from [vuex-easy-firestore](https://mesqueeb.github.io/vuex-easy-firestore/setup.html#setup), library which I use as SDK for Firestore, because it's very fast to kick-off.

## Which library to choose?

My previous experience with Google Tag Manager and gtag made me go in a wrong direction and spend a lot of time on debugging and trying to understand what's going on.

### What didn't work for me?

- [vue-gtm](https://github.com/mib200/vue-gtm) - for integration with Google Tag Manager (GTM), which will send events to GTM, where you have to forward them to Google Analytics. As a Developer you will need to do configuration twice. That was my first attempt and here I shared [my opinion why gtag is better than GTM](https://dev.to/razbakov/gtm-vs-gtag-1em7).
- [vue-gtag](https://github.com/MatteoGabriele/vue-gtag) - for custom gtag implementation which will send events directly to Google Analytics. I was already convinced that gtag is better, but something went wrong and for a long time I didn't get why. Debugger showed me that gtag is initialized twice. I assumed it is cache from GTM, even though GTM was injecting only while building on Netlify, so locally it shouldn't have been there. So I checked official documentation and finally realized that I have firebase.analytics() which adds gtag automatically. Custom gtag is possible with some changes, but I decided to see how far I can go with already existing one.

### What works?

Following [**recommended setup**](https://firebase.google.com/docs/analytics/get-started?platform=web) the easiest way is to **remove your own gtag** snippets and track events with:

```js
firebase.analytics().logEvent("notification_received");
```

Let's check versions of node:

```bash
node -v
v12.14.1

npm -v
6.13.6

vue -V
@vue/cli 4.2.2
```

First, let's setup vue app:

```bash
npm install -g @vue/cli
vue create myapp
cd myapp
npm install firebase --save
```

Your `package.json` should look like this:

```json
{
  "dependencies": {
    "core-js": "^3.6.4",
    "firebase": "^7.9.1",
    "vue": "^2.6.11"
  },
  "devDependencies": {
    "@vue/cli-plugin-babel": "~4.2.0",
    "@vue/cli-plugin-eslint": "~4.2.0",
    "@vue/cli-service": "~4.2.0",
    "babel-eslint": "^10.0.3",
    "eslint": "^6.7.2",
    "eslint-plugin-vue": "^6.1.2",
    "vue-template-compiler": "^2.6.11"
  }
}

```

Go to the [Firebase console](https://console.firebase.google.com/) and setup new app with activated Analytics.

Now let's add firebase with analytics and create an alias for it, so that you don't need to pollute each script with imports:

```js
// main.js

import Vue from 'vue'
import App from './App.vue'

import * as firebase from "firebase/app";
import "firebase/firestore";
import "firebase/analytics";
    
const firebaseConfig = {
  apiKey: '<your-api-key>',
  authDomain: '<your-auth-domain>',
  databaseURL: '<your-database-url>',
  projectId: '<your-cloud-firestore-project>',
  storageBucket: '<your-storage-bucket>',
  messagingSenderId: '<your-sender-id>'
  appId: '<your-app-id>',
  measurementId: '<your-measurement-id>'
};

firebase.initializeApp(firebaseConfig);
firebase.analytics();

Vue.config.productionTip = false

// alias
Vue.prototype.$analytics = firebase.analytics();

new Vue({
  render: h => h(App),
}).$mount('#app')
    
```

Now you can track events in your component methods like this:

```js
this.$analytics.logEvent("notification_received");
```

## How to track events?

Before introducing new event, make sure it's not already tracked by [automatic events](https://support.google.com/analytics/answer/9234069).

[Activate enhanced measurement](https://support.google.com/analytics/answer/9216061?hl=en&ref_topic=9228654) to track data from on-page elements such as links and embedded videos.

After that check if your event listed in [recommended events](https://support.google.com/analytics/answer/9267735), if so follow recommended parameters.

Otherwise you create your own custom events, but those will have reduced capabilities in analytics.

## How to track page views?

Page views expect different document titles for different pages. The easiest way to implement it is to use [vue-meta](https://vue-meta.nuxtjs.org/).

```js
// add to each of your page components
metaInfo() {
  return {
    title: "Screen Name"
  };
},
```

Normally you would track page views on `router.afterEach`, but `vue-meta` changes document title later and it would record a previous page name on navigation instead of new one. So we have to trigger on right timing after title updates.

```js
// util.js
// this function defines if app is installed on homescreen as PWA
function isPWA() {
  return window && window.matchMedia("(display-mode: standalone)").matches;
}

// App.vue
import * as firebase from "firebase/app";
import { version } from "../package.json";
import { isPWA } from "./util";

// add to App component
export default {
  metaInfo: {
    changed(metaInfo) {
      firebase.analytics().setCurrentScreen(metaInfo.title);
      firebase.analytics().logEvent("page_view");
      firebase.analytics().logEvent("screen_view", {
        app_name: isPWA() ? "pwa" : "web",
        screen_name: metaInfo.title,
        app_version: version
      });
    }
  },
  mounted() {
    firebase.auth().onAuthStateChanged(user => {
      if (user) {
        this.$analytics.setUserId(user.uid);
        this.$analytics.setUserProperties({
          account_type: "Basic" // can help you to define audiences
        });
      }
    });
  }
}
```

## Debugging

I used the following browser extensions to debug tracking:

- [GoogleAnalyticsDebugger](https://chrome.google.com/webstore/detail/google-analytics-debugger/jnkmfdileelhofjcijamephohjechhna) - activates DebugView realtime in GA App + Web and logs communication in console log with errors
- [GTM/GA Debug](https://chrome.google.com/webstore/detail/gtmga-debug/ilnpmccnfdjdjjikgkefkcegefikecdc) - tab in inspector which shows all triggered events with parameters

## Open Questions

### How to set app version?

App version is [automatically collected user property](https://support.google.com/analytics/answer/9268042?hl=en&ref_topic=9228654), but not in case if it is a web app.

[gtag screenview](https://developers.google.com/analytics/devguides/collection/gtagjs/screens) has an `app_version`, somehow Analytics doesn't use it in reporting. 

Other option is to add it to user properties, at least it becomes visible and filterable in reporting, but it is not clear if it works as intended.
