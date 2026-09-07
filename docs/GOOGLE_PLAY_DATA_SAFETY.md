# Google Play Console: Data Safety Form Walkthrough for BRIEF

**Application Name:** BRIEF  
**Package Name:** `com.carp.brief`  
**Target:** Google Play Console > Policy > App Content > **Data Safety**

Use this guide to complete the Google Play Data Safety declaration form. The answers below align with BRIEF's on-device architecture, Bring-Your-Own-Key (BYOK) model, and AdMob banner integration.

---

## Screen 1: Data Collection & Security

| Question | Recommended Answer | Explanation |
| :--- | :--- | :--- |
| **Does your app collect or share any of the required user data types?** | **Yes** | Even though BRIEF maintains no developer servers, user prompts are sent off-device to Google's API, and AdMob processes device identifiers. |
| **Is all of the user data collected by your app encrypted in transit?** | **Yes** | All network communications (Google AI Studio and AdMob) use HTTPS / TLS 1.3 encryption. |
| **Do you provide a way for users to request that their data is deleted?** | **Yes** | Users can delete chat history and automation profiles directly within the app, or delete all app data / uninstall the app. |

---

## Screen 2: Data Types Selection

Select only the following categories and data types:

### 1. Messages
* Check: **Other in-app messages**
  * *Explanation:* Represents the user's chat prompts sent to Google AI Studio.

### 2. App Info and Performance
* Check: **Crash logs** (if using Firebase/Play Console automated crash reports)
* Check: **Diagnostics** (for Google Mobile Ads / AdMob SDK)

### 3. Device or Other Identifiers
* Check: **Device or other identifiers**
  * *Explanation:* Required for the Google Mobile Ads (AdMob) SDK (Google Advertising ID / GAID).

---

## Screen 3: Details for Each Selected Data Type

### A. Messages > Other in-app messages (User Prompts)
1. **Is this data collected, shared, or both?**
   * Select: **Shared** *(Data is transferred off-device directly to Google Generative Language API without being retained on your personal servers)*
2. **Is this data processed ephemerally?**
   * Select: **Yes** *(BRIEF's internal pipeline processes it in memory to deliver the immediate response without storing it remotely)*
3. **Is this data required for your app, or can users choose whether it's collected?**
   * Select: **Data collection is required** *(The app cannot generate summaries without sending the prompt to the AI)*
4. **Why is this user data shared?**
   * Check: **App functionality**

### B. Device or Other Identifiers (AdMob)
1. **Is this data collected, shared, or both?**
   * Select: **Shared** *(or "Collected" by the Google Mobile Ads SDK)*
2. **Is this data processed ephemerally?**
   * Select: **No**
3. **Is this data required for your app, or can users choose whether it's collected?**
   * Select: **Data collection is required** *(or optional if consent SDK allows refusal)*
4. **Why is this user data collected/shared?**
   * Check: **Advertising or marketing**
   * Check: **Analytics**
   * Check: **Fraud prevention, security, and compliance**

---

## Screen 4: Financial, Health, Location, and Personal Info
* **Location:** Select **No** (BRIEF does not access fine/coarse GPS location; approximate location is derived by Google/AdMob via IP address on the network layer).
* **Personal info (Name, Email, Address, User IDs):** Select **No**.
* **Financial info:** Select **No**.
* **Health and fitness:** Select **No**.
* **Photos and videos / Audio / Files / Contacts:** Select **No**.

---

## Summary for Privacy Policy URL Field in Play Console
When Google Play Console asks for:
`Policy > App Content > Privacy Policy > Privacy policy URL`

Provide your deployed GitHub Pages URL:
```text
https://thewhtecarp.github.io/brief/
```
*(Or the direct link to the published `index.html` page).*
