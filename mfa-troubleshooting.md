---
title: Troubleshooting Multi-Factor Authentication
category: troubleshooting
summary: Identify and resolve issues with MFA.
---

# Troubleshooting Multi-Factor Authentication

## Lost Device

If you use the Google Authenticator app for multi-factor authentication, losing your device could lock you out of your account. If you lose your device, you will need to enable the email mode, login to your account, and then disable the authenticator mode. Start by heading to the [MFA activation page](https://product.datica.com/account/mfa/activate?type=email). Then follow these steps

1. Choose the email mode.
2. Enter your credentials and `Submit`.
3. Check your email for a one time password (OTP).
4. Enter your one time use code.
5. Login to your account from the [dashboard](https://product.datica.com/account).
6. Open your account settings by clicking your username in the top right. Or navigate to [https://product.datica.com/account/view](https://product.datica.com/account/view).
7. Expand the `MFA` section.
8. Click `Disable` next to the authenticator mode.

You'll now have full access to your Datica account. If you want to enable the authenticator mode again, just follow [this guide](/compliant-cloud/articles/guides/enable-multi-factor-auth#adding-additional-factors) to repeat the setup process.

## CLI Uses the Incorrect Mode

If your CLI is using the incorrect mode for MFA, you'll need to update your [preferred mode](/compliant-cloud/articles/guides/enable-multi-factor-auth#preferred-mode) in the dashboard. The CLI is capable of only using a single mode, specifically your preferred mode, while the [dashboard](https://product.datica.com/account) allows you to choose between all of your enabled modes. To set your preferred mode, please follow [this guide](/compliant-cloud/articles/guides/enable-multi-factor-auth#preferred-mode).

## My Authenticator no Longer Works

When choosing the authenticator mode for MFA, your device and the Datica servers will need to be kept in sync with [NTP](http://www.ntp.org). Without an accurate system clock, the authenticator app will generate invalid OTPs. Mobile devices will often synchronize automatically as long as an active network connection is available. If you are using another type of device, be sure to check you have an active network connection available and check the documentation for your device on how to keep in sync with NTP.

# Still Having Issues?

If you're unable to login to your Datica account and you have reviewed the steps above, please contact Datica Support via [Zendesk](https://catalyzeio.zendesk.com/hc/en-us/requests/new)
