---
layout: post
title:  "Use 1Password as MFA Method for AzureAD"
author: tim
categories: [AzureAD, MFA, 1Password, Quick]
image: assets/images/using1passwordasMFAMethod\key.jpg
description: "Using 1Password as MFA Method for login into Azure Active Directory"
featured: false
hidden: false
---

# Use 1Password as MFA Method for AzureAD

Unlocking your Device every time for agree the MFA prompt could be really annoying, even when you are working like a consult for different companies.
During my late night session working for different customers, it was a real pain to unlock, hold my face in front of the device and press "agree" every time i log into my customer environment, especially then you need to do MFA every time you sign in over CLI.

## Security Concerns

Yes there are some security benefits not to fall back to software token and using Microsoft Authentificator app or something similar, i think its save enough to use Software Token, unless you are not working with the highest privileged account like Global Administrator.

### Step by Step

1. Log into [aka.ms/mysecurityinfo](aka.ms/mysecurityinfo) and register a new MFA Method. Choose **"Authenticator app"**

    ![Overview](/assets/images/using1passwordasMFAMethod/choseMFAMethod.png)

2. Click on **"I want to use a different authenticator app"**

    ![App](/assets/images/using1passwordasMFAMethod/differentapp.png)

3. **"Next"**
4. **"Can´t scan image?"**
5. Copy **Secret key**

    ![QR](/assets/images/using1passwordasMFAMethod/cantscan.png)

6. Now goto your 1password (Android, Windows, MacOs, IOS)
7. Create a new login or edit your existing login entry
8. Add **OTP** as a bew field

    ![OTP](/assets/images/using1passwordasMFAMethod/OTP.png)

9. paste the copied **Secret key**
10. **Save**
11. Now copy the new provided **OTP** from 1password and go back to your MFA Method registration this is open the Browser
12. Select **Next** and paste your 6-digit **OTP** into the field.

    ![EnterOTP](/assets/images/using1passwordasMFAMethod/enterotp.png)

13. If its accepted, your finished!

If you like it, it would be great if you can share it or if you have any question ping me on [Twitter](https://twitter.com/ti_stock)
