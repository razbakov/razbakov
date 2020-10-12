---
title: How to use Google Ads API in PHP
description: Step by step guide how to use Google Ads API
permalink: /google-ads-php
date: 2020-07-30
category: WebDev
---

## 5 steps guide how to use googleads-php-lib

If you already tried to follow [Make Your First API Call](https://developers.google.com/adwords/api/docs/guides/first-api-call) and video with **talk through** instead of **walkthrough of the steps** and you felt confused and unclear, then this tutorial is for you!

### Step 1. Create Manager Account

You must have a Google Ads Manager Account to apply for access to the API.

START USING MANAGER ACCOUNTS - [https://adwords.google.com/um/MccStartNow](https://adwords.google.com/um/MccStartNow).
This link will create an AdWords manager account.
This is the one you should use if you are going through the AdWords API Sign Up guide to request a developer token.

Manager Accounts cannot be created using the same email address as an existing Google Ads account.

You must therefore use an email address that hasn't already been associated with a Google Ads account to create your Manager Account.

Sign up for AdWords API access through your Manager Account.
Sign in, then navigate to TOOLS & SETTINGS > SETUP > API Center.
The API Center option will appear only for Google Ads Manager Accounts.

**Get Developer token**

![API Access Screenshot](/img/google-ads-php/api-access.png)

### Step 2. Generate Test Client Id

Create a test manager account

1. Go to [https://adwords.google.com/um/Welcome/?sf=mt](https://adwords.google.com/um/Welcome/?sf=mt)
2. Select NEW GOOGLE ADS ACCOUNT and then name the new account something like [test-manager@mycompany.example.com](mailto:test-manager@mycompany.example.com)
3. To create a test account, you must have a Google account that is not already linked to your production manager account. Create a new Google account.
4. Once your test manager account is established, you can proceed to make API calls against it. Use the production manager account's developer token when making requests againsts the test manager account.
5. When requesting an OAuth2 refresh token, make sure you're logged in as the test manager account user (for example, [test-manager@mycompany.example.com](mailto:test-manager@mycompany.example.com)).

You should see "Test account" in right top corner.

Take number client id (number above email)

![Test Account Screenshot](/img/google-ads-php/test-account.png)

### Step 3. Create credentials

1. Open the [Google API Console Credentials page](https://console.developers.google.com/apis/credentials).
2. Click **Select a project**, then **NEW PROJECT**, and enter a name for the project, and optionally, edit the provided project ID. Click **Create**.
3. On the Credentials page, select **Create credentials**, then **OAuth client ID**.
4. You may be prompted to set a product name on the Consent screen; if so, click **Configure consent screen**, supply the requested information, and click **Save** to return to the Credentials screen.
5. Select **Desktop Application** for the **Application Type**.
6. Click **Create**.
7. On the page that appears, copy the **client ID** and **client secret** to your clipboard, as you will need them when you configure your client library.

### Step 4. Generate refresh token

Following [API access using own credentials (installed application flow)](<https://github.com/googleads/googleads-php-lib/wiki/API-access-using-own-credentials-(installed-application-flow)#step-2---setting-up-the-client-library>)

1. Get [GetRefreshToken.php](https://github.com/googleads/googleads-php-lib/blob/master/examples/Auth/GetRefreshToken.php) from the `example` directory by following [this section](https://github.com/googleads/googleads-php-lib/blob/master/README.md#downloading-a-compressed-tarball).
2. In a terminal, navigate to [GetRefreshToken.php](https://github.com/googleads/googleads-php-lib/blob/master/examples/Auth/GetRefreshToken.php).
3. Run this file via the command line and follow instructions:

```jsx
Copy the following lines to your 'adsapi_php.ini' file:
clientId = "xxxx.apps.googleusercontent.com"
clientSecret = "L-xxxxxxx"
refreshToken = "1//xxxxx"
```

### Step 5. Execute test call.

1. Install the latest [googleads-php-lib](https://github.com/googleads/googleads-php-lib) version using Composer:

```bash
composer require googleads/googleads-php-lib
```

2. Move `adsapi_php.ini` to project folder.
3. Download [GetCampaigns.php](https://github.com/googleads/googleads-php-lib/blob/master/examples/AdWords/v201809/BasicOperations/GetCampaigns.php) example.
4. Change in that file `require` path to `/vendor/autoload.php`
5. Execute `php GetCampaigns.php`

There should be no errors.

Now you can move forward and implement [Conversion tracking](https://developers.google.com/adwords/api/docs/guides/conversion-tracking#setup) following official documentation and [examples](https://github.com/googleads/google-ads-php/tree/master/examples).

Please write down in comments below if it actually worked for you and if you have any questions. I spent around 3 days to figure out how to configure it just to make a first test call. Be careful with datetime format 😉
