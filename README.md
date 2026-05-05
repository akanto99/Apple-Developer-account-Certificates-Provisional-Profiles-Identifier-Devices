🍎 iOS App Store Submission — Complete Guide (Bengali)

Apple Developer Certificate, Identifier, Profile, Device, Push Notification (P12) — সম্পূর্ণ A to Z গাইড বাংলায়।


📋 Table of Contents

Mac এ Client Apple ID Login
Xcode ও macOS Version Check
CSR File বানাও (Keychain Access)
Identifier Create করো
iOS Distribution Certificate Create
iOS Development Certificate Create
Test Device Add করো
Provisioning Profile Create করো
Push Notification P12 Certificate
Xcode Final Setup ও Archive


STEP 1 — Mac এ Client Apple ID Login
কোথায় যাবে: Xcode → Settings → Accounts
1. Xcode খোলো
2. Menu bar: Xcode → Settings → Accounts
3. বাম নিচে + (plus) button click করো
4. "Add Apple ID" select করো
5. Client এর Apple ID (email) দাও → Continue
6. Client এর iPhone এ 6-digit 2FA code যাবে
7. সেই code Xcode এ বসাও → Sign In
Verify করো:
Accounts list এ client এর নাম দেখা যাবে এবং Team এর নিচে Apple Developer Program দেখাবে।

STEP 2 — Xcode ও macOS Version Check
Terminal খুলে run করো:
bashxcodebuild -version
sw_vers -productVersion
Minimum requirement:

Xcode: 15 বা তার বেশি
macOS: 13 (Ventura) বা তার বেশি

Update দরকার হলে: App Store → search "Xcode" → Update
Flutter project open করো:
bashcd /your/project/path
open ios/Runner.xcworkspace

⚠️ সবসময় .xcworkspace open করবে — .xcodeproj না


STEP 3 — CSR File বানাও (Keychain Access)
Certificate বানাতে হলে আগে CSR (Certificate Signing Request) file দরকার।
কোথায় যাবে: Keychain Access → Certificate Assistant
1. Mac এ Keychain Access app খোলো
   (Spotlight: Cmd + Space → "Keychain Access" লিখো)

2. Menu bar থেকে:
   Keychain Access → Certificate Assistant
   → Request a Certificate From a Certificate Authority

3. Form fill করো:
   ┌─────────────────────────────────────────────┐
   │ User Email Address : তোমার/client এর email  │
   │ Common Name        : Humayan Islam           │
   │ CA Email Address   : (খালি রাখো)            │
   │ Request is         : ✅ Saved to disk        │
   └─────────────────────────────────────────────┘

4. Continue → Desktop এ save করো
Output: CertificateSigningRequest.certSigningRequest ফাইল তৈরি হবে

💡 এই একটা CSR file দিয়ে Development ও Distribution দুটো certificate বানানো যাবে


STEP 4 — Identifier Create করো
কোথায় যাবে: developer.apple.com → Certificates, Identifiers & Profiles → Identifiers
1. Identifiers → + (plus) button click করো
2. Select: App IDs → Continue
3. Select: App → Continue
4. Fill করো:
   ┌──────────────────────────────────────────────────────────────┐
   │ Description : Dinmajur Customer                              │
   │ Bundle ID   : com.dinmajurplatformservice.dinmajurcustomer   │
   │ (Explicit select করো, Wildcard না)                          │
   └──────────────────────────────────────────────────────────────┘

5. Capabilities section এ scroll করো এবং check করো:
   ✅ Push Notifications     (push notification লাগলে)
   ✅ Sign in with Apple     (Apple login লাগলে)
   ✅ Maps                   (map লাগলে)

6. Continue → Register

⚠️ Bundle ID টা Xcode এর Bundle Identifier এর সাথে exact match করতে হবে


STEP 5 — iOS Distribution Certificate Create
App Store এ submit করার জন্য Distribution Certificate দরকার।
কোথায় যাবে: developer.apple.com → Certificates → +
1. Certificates → + button click করো
2. Software section থেকে:
   ✅ Apple Distribution → Continue
3. CSR Upload:
   "Choose File" → Step 3 এ বানানো .certSigningRequest file select করো
   → Continue
4. Download করো → distribution.cer file আসবে
5. সেই .cer file এ double click করো
   → Keychain Access এ automatically install হবে
Verify করো:
Keychain Access → My Certificates → "Apple Distribution: Humayan Islam" দেখা যাবে

STEP 6 — iOS Development Certificate Create
Real device এ test run করার জন্য Development Certificate দরকার।
কোথায় যাবে: developer.apple.com → Certificates → +
1. Certificates → + button click করো
2. Software section থেকে:
   ✅ Apple Development → Continue
3. CSR Upload:
   Same .certSigningRequest file আবার upload করো → Continue
4. Download → double click → Keychain এ install

⚠️ Development + Distribution দুটোই লাগে। একটা দিয়ে কাজ হবে না।


STEP 7 — Test Device Add করো
Development profile এ device add না থাকলে real device এ app run হবে না।
কোথায় যাবে: developer.apple.com → Devices → +
UDID বের করার উপায়:
1. iPhone টা Mac এ cable দিয়ে connect করো
2. Finder খোলো (Cmd + Space → Finder)
3. Sidebar এ iPhone এর নাম দেখা যাবে → click করো
4. Serial Number এ click করো → UDID এ switch হবে
5. Right click → Copy করো
Device Register করো:
Platform    : iOS
Device Name : SAIFUL's iPhone
Device ID   : (copied UDID paste করো)

→ Continue → Register

STEP 8 — Provisioning Profile Create করো
Profile দুই ধরনের বানাতে হবে।
কোথায় যাবে: developer.apple.com → Profiles → +
8A — Distribution Profile (App Store Submit এর জন্য)
1. Profiles → + click করো
2. Distribution section:
   ✅ App Store Connect → Continue
3. App ID select করো:
   com.dinmajurplatformservice.dinmajurcustomer → Continue
4. Certificate select করো:
   iOS Distribution certificate (Step 5 এ বানানো) → Continue
5. Profile Name দাও:
   Humayun Islam Distribution → Generate → Download
6. .mobileprovision file এ double click করো → Xcode এ install হবে
8B — Development Profile (Device Testing এর জন্য)
1. Profiles → + click করো
2. Development section:
   ✅ iOS App Development → Continue
3. App ID select করো:
   com.dinmajurplatformservice.dinmajurcustomer → Continue
4. Certificate select করো:
   iOS Development certificate (Step 6 এ বানানো) → Continue
5. Device select করো:
   ✅ SAIFUL's iPhone (Step 7 এ add করা) → Continue
6. Profile Name দাও:
   Humayun Islam Development → Generate → Download → double click

⚠️ Profile যদি "Invalid" দেখায় তাহলে delete করে নতুন বানাও — কারণ পুরনো/expire হওয়া certificate দিয়ে profile বানানো হয়েছিল।


STEP 9 — Push Notification P12 Certificate বানাও
OneSignal / FCM এ push notification এর জন্য P12 file দরকার।
9A — Apple Push Services Certificate বানাও
কোথায় যাবে: developer.apple.com → Certificates → +
1. Certificates → + click করো
2. Services section থেকে:
   ✅ Apple Push Notification service SSL (Sandbox & Production) → Continue
3. App ID select করো:
   com.dinmajurplatformservice.dinmajurcustomer → Continue
4. CSR upload করো (.certSigningRequest file) → Continue
5. Download করো → double click করে Keychain এ install করো
9B — Keychain থেকে P12 Export করো
1. Keychain Access খোলো
2. বাম দিকে: My Certificates click করো
3. খোঁজো:
   "Apple Push Services: com.dinmajurplatformservice.dinmajurcustomer"
4. সেটার উপর Right click করো
5. Export "Apple Push Services: com..." click করো
6. Format: Personal Information Exchange (.p12) select করো
7. Save করো → একটা password দাও (মনে রাখো!)
8. Mac এর login password দাও → Done
Output: push_certificate.p12 file তৈরি হবে
9C — OneSignal এ Upload করো
OneSignal Dashboard
→ App Settings
→ Platforms → Apple iOS
→ .p12 file upload করো
→ Password দাও (9B তে যেটা দিয়েছিলে)
→ Save

STEP 10 — Xcode Final Setup ও Archive
কোথায় যাবে: Xcode → Runner → Signing & Capabilities
1. Runner.xcworkspace open করো
2. Left sidebar → Runner → Targets → Runner
3. Signing & Capabilities tab এ যাও
4. Fill করো:
   ┌──────────────────────────────────────────────┐
   │ Team           : Humayan Islam (CGYVL2WY5G)  │
   │ Bundle ID      : com.dinmajurplatformservice  │
   │                  .dinmajurcustomer            │
   │ Auto Signing   : ✅ Automatically manage      │
   └──────────────────────────────────────────────┘
5. Xcode automatically provisioning profile নেবে
Archive ও Upload করো:
1. Top bar এ device dropdown → "Any iOS Device (arm64)" select করো
2. Menu: Product → Archive
3. Archive শেষ হলে Organizer window আসবে
4. "Distribute App" click করো
5. "App Store Connect" select করো → Next
6. "Upload" select করো → Next → Next → Upload
App Store Connect এ:
1. appstoreconnect.apple.com এ যাও
2. Apps → তোমার app → TestFlight (beta test) অথবা
3. App Store → Submit for Review

📊 Quick Status Check
SectionItemStatusCertificatesApple Push Services✅ DoneCertificatesiOS Development✅ DoneCertificatesiOS Distribution✅ DoneCertificatesDistribution Managed✅ DoneIdentifiersDinmajur Customer✅ DoneIdentifiersOneSignal Extension✅ DoneDevicesSAIFUL's iPhone✅ DoneProfilesApp Store Distribution⚠️ Invalid — RecreateProfilesDevelopment⚠️ Invalid — RecreateP12 ExportPush Notification🔄 Export needed

⚠️ Common Errors ও Solutions
"Communication with Apple failed"
Solution: Manual flow ব্যবহার করো
1. Web এ Identifier, Certificate, Profile বানাও
2. Xcode এ manual signing select করো
3. Profile manually select করো
Profile "Invalid" দেখাচ্ছে
Solution:
1. Developer site এ যাও → Profiles
2. Invalid profile delete করো
3. নতুন profile বানাও — নতুন certificate select করো
4. Download → double click → Xcode refresh করো
Archive এ error আসছে
Solution:
1. Xcode → Product → Clean Build Folder (Cmd+Shift+K)
2. Derived Data delete করো:
   Xcode → Settings → Locations → Derived Data → Arrow click
3. সেই folder delete করো → Xcode restart
"No accounts with iTunes Connect access"
Solution:
1. appstoreconnect.apple.com এ login করো
2. App টা create করা আছে কিনা check করো
3. Bundle ID match করছে কিনা confirm করো

🔗 Useful Links

Apple Developer Portal
App Store Connect
Apple Certificate Guide
Flutter iOS Deployment


Guide prepared for Dinmajur Customer App — Bundle ID: com.dinmajurplatformservice.dinmajurcustomer
