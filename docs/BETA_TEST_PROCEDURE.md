# Standard Operating Procedure: BRIEF Beta Verification (`test beta`)

This document defines the automated verification protocol for the **BRIEF** application (`com.thewhitecarp.brief`).

Whenever you enter **`test beta`** (or `run beta test`, `beta test`), Antigravity executes all 6 verification phases end-to-end, testing the currently active Android device, running bug scans and unit tests, verifying legal compliance, synchronizing privacy policies to GitHub Pages, and outputting a standardized **Beta Verification Report**.

*For the generalized multi-app version of this procedure, see [ANDROID_BETA_TEST_PROCEDURE.md](file:///C:/Users/CARP/Desktop/productivity/antigravity/general/ANDROID_BETA_TEST_PROCEDURE.md).*

---

## 1. Project Configuration

* **App Name:** BRIEF
* **Package Name:** `com.thewhitecarp.brief`
* **Main Activity:** `com.thewhitecarp.brief.MainActivity`
* **Project Root:** `c:\Users\CARP\Desktop\productivity\antigravity\AI briefing app\gemini-clone`
* **GitHub Pages Repo:** `c:\Users\CARP\Desktop\productivity\antigravity\AI briefing app\BRIEF.github.io`
* **Public URL:** `https://thewhitecarp.github.io/BRIEF.github.io/`
* **Target SDK:** 36 (Android 16)
* **Compile SDK:** 36

---

## 2. Dynamic Device Targeting

The test runner detects all attached ADB devices at test launch using:
```powershell
& "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe" devices -l
```
* Priority:
  1. Connected physical hardware device (e.g. Sony Xperia `HQ653Q07A7`).
  2. Running Android Studio Emulator (e.g. `emulator-5554`).
* If multiple devices are present, it selects the first online device and logs its model and OS version.

---

## 3. The 6 Verification Phases

### Phase 1: Code, Compilation & Deep Bug/Error Audit
1. **Lifecycle & Leaks**: Ensure no Activity contexts are retained by Singletons (`SettingsRepository`, `AppOpenAdManager`, `NativeAdManager`) or background CoroutineScopes.
2. **Persistence & Thread Safety**: Ensure `ChatHistoryManager` uses `AtomicFile` write operations to prevent file corruption during power loss.
3. **Timeout Guards**: Ensure `BroadcastReceiver.goAsync()` delegates long operations to WorkManager rather than exceeding Android's 10-second limit.
4. **Encrypted Storage Guard**: Confirm `SettingsRepository` has recovery handling for corrupted KeyStore/EncryptedSharedPreferences.
5. **R8 / ProGuard Keep Rules**: Verify `proguard-rules.pro` retains `@Serializable` models and AdMob classes.
6. **Compilation**: Run `./gradlew.bat assembleDebug` and verify debug symbols (`SYMBOL_TABLE`).

### Phase 2: Automated Unit & Domain Logic Tests
1. Run:
   ```powershell
   .\gradlew.bat testDebugUnitTest
   ```
2. Verify all unit tests pass with 0 failures and 0 errors:
   - `ErrorMessageFormatterTest`
   - `ReportedIssueSerializationTest`

### Phase 3: Dynamic Device UI & Functional Testing
1. Install debug APK:
   ```powershell
   & "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe" -s $targetDevice install -r -d "app\build\outputs\apk\debug\app-debug.apk"
   ```
2. Launch app and verify startup:
   ```powershell
   & "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe" -s $targetDevice shell am start -n "com.thewhitecarp.brief/.MainActivity"
   ```
3. Dump and verify UI hierarchy via `uiautomator dump /sdcard/window_dump.xml`.
4. Test key UI flows:
   - Floating Automations button / Navigation between Chat and Automations tab.
   - Multi-language switching (Korean, Japanese, English, System Default) to ensure no recreation crash.
   - Text input responsiveness.
5. Inspect Logcat for fatal runtime exceptions or ANRs:
   ```powershell
   & "$env:LOCALAPPDATA\Android\Sdk\platform-tools\adb.exe" -s $targetDevice logcat -d -v time "*:E"
   ```

### Phase 4: Legality, Google Play Policy & Data Safety Audit
1. Verify `targetSdk = 36` in `app/build.gradle.kts`.
2. Review permissions in `AndroidManifest.xml` against `docs/PRIVACY_POLICY.md`.
3. Check in-app legal dialogs (Terms of Service, Privacy Policy, Open Source Licenses in `SettingsScreen.kt`).
4. Reconcile `docs/GOOGLE_PLAY_DATA_SAFETY.md` with actual data transits (BYOK direct Google AI Studio API calls, AdMob GAID collection, zero first-party data storage).
5. Verify test ad unit IDs in `AdConfig.kt` during debug builds and proper `[Ad]` attribution.

### Phase 5: Privacy Policy & Docs Sync + Deployment
1. Compare `gemini-clone/docs/` with `BRIEF.github.io/docs/` (`index.html`, `PRIVACY_POLICY.md`, `GOOGLE_PLAY_DATA_SAFETY.md`).
2. Copy updated files to `BRIEF.github.io/docs/`.
3. Git commit with author `TheWhiteCarp <thewhitecarp@protonmail.com>` and push to `origin main`.
4. Run `curl -I https://thewhitecarp.github.io/BRIEF.github.io/` to verify HTTP 200 live status.

### Phase 6: Standardized Beta Verification Report
Generate a complete, structured summary containing:
- Phase status table (`PASS` / `FAIL` / `WARN`).
- Active device hardware and OS version details.
- Unit test and bug scan results.
- Live URL deployment confirmation.
- Release readiness verdict (`GO` / `NO-GO`).
