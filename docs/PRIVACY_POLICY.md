# Privacy Policy for BRIEF

**Effective Date:** September 7, 2026  
**Application Name:** BRIEF  
**Application ID / Package:** `com.carp.brief`  
**Developer / Publisher:** TheWhteCarp  
**Contact:** `carp.brief.app@gmail.com`  

---

## 1. Introduction & Core Privacy Philosophy

BRIEF ("we", "us", or "our", operated by TheWhteCarp) is designed from the ground up with a **local-first, zero-telemetry architecture**. We believe that your personal automations, private thoughts, and daily intelligence summaries should remain strictly under your control.

* **No Proprietary Servers:** We do not operate external application servers, central user databases, or cloud syncing infrastructure for BRIEF.
* **No Account Required:** You do not need to register an account, log in, or provide an email address to use the core features of BRIEF.
* **No Covert Tracking:** We do not track your location, read your contacts, monitor your clipboard, or profile your behavior.

This Privacy Policy explains how information is handled, secured, and transmitted when you use the BRIEF mobile application on Android.

---

## 2. Bring-Your-Own-Key (BYOK) Model & API Key Security

BRIEF operates on a **Bring-Your-Own-Key (BYOK)** model. To generate AI summaries and perform automated briefings, you supply your personal Google AI Studio (Gemini) API key.

* **On-Device Cryptographic Storage:** Your API key is encrypted immediately upon entry and saved to Android's `EncryptedSharedPreferences`. Encryption keys are generated and managed within the hardware-backed **Android Keystore System** using AES-256 GCM encryption.
* **Zero Developer Access:** Your API key is stored exclusively on your physical device. It is never transmitted, copied, logged, or visible to the developers of BRIEF.
* **Direct Transit:** When an AI request or automation runs, the application sends your API key directly over an encrypted HTTPS connection to Google's official Generative Language API endpoint (`https://generativelanguage.googleapis.com`).

---

## 3. Local Data Storage & User Custody

All conversation data, automation schedules, and application preferences are stored strictly inside the private, sandboxed internal storage of your Android device:

* **Chat Transcripts:** Messages between you and the AI are written locally to JSON files utilizing atomic write operations (`android.util.AtomicFile`) to guard against data corruption.
* **Automation Profiles:** Your daily topic configurations, notification conditions, and scheduled trigger times are stored in local preferences.
* **Complete User Custody:** Because this data resides entirely on your device, you have complete control over its retention:
  * You can clear chat history at any time using the in-app chat menu.
  * You can delete individual automation profiles or reset them in Settings.
  * Clearing the app’s data via Android System Settings or uninstalling BRIEF permanently erases all stored keys, transcripts, and automations immediately.

---

## 4. Third-Party Data Transits & Service Providers

While BRIEF maintains no servers of its own, specific app features communicate directly with third-party service providers to function.

### A. Google AI Studio / Google Generative Language API
When you send a message or when a scheduled automation executes, your text prompt and conversation context are transmitted to Google:
* **Endpoints:** Direct HTTPS calls to Google's official API servers.
* **Google's Privacy Governance:** Your interaction with Google's API is governed by [Google's Privacy Policy](https://policies.google.com/privacy) and the [Google AI Studio Terms of Service](https://ai.google.dev/terms).
* **Free Tier vs. Paid Tier Notice:**
  * **Free of Charge Tier:** Under Google's API terms, if you use a free-tier Google AI Studio API key, Google may log and review your API input prompts and generated responses to improve Google products. Human reviewers may read these logs.
  * **Paid Tier:** If you attach a billing account to your Google Cloud / AI Studio project, Google treats your data under Google Cloud enterprise privacy commitments and does not use your prompts to train models.
  * *Recommendation:* Users desiring complete training privacy should configure a paid billing tier in their Google AI Studio console.

### B. Public Web Search & Content Synthesis
To deliver up-to-date daily briefings (such as weather, financial updates, or breaking news), BRIEF performs search queries against public web indices (e.g., DuckDuckGo HTML / public search feeds):
* Only the search keyword or topic specified by your automation prompt is transmitted.
* No personal identifying information, device ID, or user profile is attached to web search requests.

### C. Advertising Networks (Google AdMob / Google Mobile Ads)
BRIEF displays banner advertisements supported by Google AdMob to fund ongoing maintenance.
* **Data Processed by AdMob:** The Google Mobile Ads SDK may collect and process:
  * Device information (device model, OS version, carrier, screen size).
  * The **Google Advertising ID (GAID)**, a resettable identifier provided by Google Play services.
  * IP address (used for approximate geographic ad targeting and fraud prevention).
  * Ad interaction metrics (impressions, clicks).
* **Ad Personalization & Opt-Out:**
  * Users within the European Economic Area (EEA) and the UK will be presented with a consent dialog (via Google User Messaging Platform / UMP) to choose between personalized ads, non-personalized ads, or ad rejection.
  * You can reset your Google Advertising ID or opt out of personalized ads at any time via Android device settings: `Settings > Google > Ads > Delete advertising ID` or `Opt out of Ads Personalization`.
  * For more information on how Google uses data in advertising, visit [How Google uses information from sites or apps that use our services](https://policies.google.com/technologies/partner-sites).

---

## 5. Android Device Permissions & Technical Justification

BRIEF requests only the minimum permissions required for operation:

| Permission | Technical Purpose |
| :--- | :--- |
| `android.permission.INTERNET` | Required to send AI prompts to Google AI Studio, fetch public web information for daily briefings, and display advertisements. |
| `android.permission.POST_NOTIFICATIONS` | Required on Android 13 (API 33)+ to deliver scheduled daily briefings, alert notifications, and background task progress updates. |
| `android.permission.SCHEDULE_EXACT_ALARM` | Required to wake the device at the exact minute specified by your automation schedules (e.g., 07:00 AM briefing). |
| `android.permission.RECEIVE_BOOT_COMPLETED` | Required to restore your local automation schedules in Android's `AlarmManager` after your device reboots. |

BRIEF **does not** request or access:
* Precise or background GPS location
* Camera, microphone, or audio recordings
* Contact lists, phone state, or SMS
* Files outside the app's sandboxed private storage directory

---

## 6. Generative AI Safety & Content Reporting

In compliance with Google Play's Generative AI Policy:
* AI outputs are generated dynamically and may occasionally produce inaccurate, misleading, or unexpected responses.
* **In-App Reporting Mechanism:** Every response generated by the AI includes a **Report** option. If you encounter inaccurate, harmful, or objectionable output, you can tap "Report" to submit a categorization flag. Reports are logged locally to aid in safety tuning and prompt refinement.

---

## 7. Children's Privacy (COPPA & Age Restrictions)

BRIEF is not directed at children under the age of 13 (or under 16 within the European Economic Area). We do not knowingly collect or solicit personal information from children.

Furthermore, pursuant to Google AI Studio's Terms of Service, **users must be at least 18 years of age** (or the age of majority in their jurisdiction) to create a Google AI Studio account, generate an API key, and use generative AI features.

---

## 8. International Privacy Rights & Regulatory Compliance

### A. European Economic Area (EEA) & UK Users (GDPR)
* **Data Controller vs. Processor:** You maintain direct control over all personal data stored locally on your device. For queries transmitted to Google's API, Google acts as an independent data processor/controller under Google's Cloud Data Processing Addendum.
* **Legal Basis for Processing:** Processing is conducted on the basis of **Contract Performance** (fulfilling your request to generate summaries or execute your configured automations) and **Legitimate Interest** (ensuring application stability and displaying contextual advertisements).
* **Your GDPR Rights:** You have the right to access, rectify, and erase your data. Because BRIEF holds no remote databases, you can exercise your right to erasure immediately by clearing app storage or deleting chat history inside the app.

### B. California Residents (CCPA / CPRA)
* **No Sale of Personal Information:** BRIEF has not sold, does not sell, and will not sell or share personal information to third-party data brokers.
* **Non-Discrimination:** We will not discriminate against any user for exercising their privacy rights under California law.

---

## 9. Changes to This Privacy Policy

We may update this Privacy Policy from time to time to reflect changes in our practices, new legal standards, or feature additions. When changes are made:
* The "Effective Date" at the top of this document will be updated.
* Significant updates will be accompanied by an in-app notice or release notes.
* The latest policy will always remain accessible at our public URL:  
  **https://thewhtecarp.github.io/brief/**

---

## 10. Contact Us

If you have questions, feedback, or concerns regarding this Privacy Policy or our privacy practices, please contact us at:

* **Email:** `carp.brief.app@gmail.com`
* **Developer / Publisher:** TheWhteCarp
* **Application:** BRIEF (`com.carp.brief`)
