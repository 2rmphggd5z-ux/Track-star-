# Track Dash: getting it on the App Store without a Mac

Codemagic's Macs build the iPhone app from these files and upload it to Apple. You do everything else in a web browser.

## What's in this folder

| File | What it is |
|---|---|
| `track-dash.html` | The whole game: 3D engine, music and sounds built in, works offline |
| `icon.png` | App icon (1024 × 1024) |
| `splash.png` | Launch screen |
| `codemagic.yaml` | Build instructions for Codemagic |
| `privacy.html` | Privacy policy page, which Apple requires |
| `capacitor.config.json`, `package.json` | Settings that wrap the game as an iPhone app, including the vibration add-on |

Keep all of these files together at the top level of your repository. Don't put them in a subfolder.

## 1. Sign up for three accounts

1. **Apple Developer Program** at developer.apple.com/programs. It costs £79 / $99 a year and approval can take a day or two. You must be 18 or over, or have a parent or guardian enrol.
2. **GitHub** at github.com (free). This stores the files.
3. **Codemagic** at codemagic.io (free). Sign up with your GitHub account. Personal accounts get 500 free Mac build minutes a month, and one build takes about 10 to 15 minutes.

## 2. Choose your app's ID

Every app needs a unique ID written like a reversed web address, for example `com.alexsmith.trackdash`. Use lowercase letters and no spaces. Once you choose it, you can't change it.

Open `codemagic.yaml` and replace `com.yourname.trackdash` on the two lines marked **CHANGE ME**.

## 3. Put the files on GitHub

1. On GitHub, tap **+**, then **New repository**. Name it `track-dash`; it can be private. Create it.
2. Tap **uploading an existing file** (or **Add file**, then **Upload files**) and add all the files from this folder.
3. Tap **Commit changes**.

## 4. Set up the app with Apple

1. **Register the ID.** On developer.apple.com, go to **Certificates, IDs & Profiles**, then **Identifiers**, then **+**. Choose **App IDs**, then **App**. Enter the description `Track Dash` and choose **Explicit Bundle ID**. Paste your ID and register it.
2. **Create the app.** At appstoreconnect.apple.com, go to **Apps**, then **+**, then **New App**. Choose iOS, enter a name, choose your language, select your ID and use `trackdash` as the SKU.
   The name has to be unique on the App Store. If "Track Dash" is taken, try something like "Track Dash Runner". The name only affects the store listing.
3. **Make an API key** so Codemagic can upload for you. In App Store Connect, go to **Users and Access**, then **Integrations**, then **App Store Connect API**. You may need to request access first. Generate a key with **Admin** access.
   Download the `.p8` file, because you can only download it once. Also note the **Issuer ID** and **Key ID**.

## 5. Connect Codemagic to Apple

1. In Codemagic, open your personal **Settings**, then **Integrations**, then **Developer Portal**, and choose **Connect**.
   Name the key exactly `codemagic`, enter the Issuer ID and Key ID, and upload the `.p8` file.
2. Still in Settings, open **Code signing identities**, then **iOS certificates**, and **Generate certificate**. Choose an **Apple Distribution** certificate using that key.
3. Back on developer.apple.com, go to **Profiles**, then **+**, and choose **App Store Connect**. Pick your Track Dash ID and the certificate Codemagic made. Name it `Track Dash App Store`, then generate it.
4. In Codemagic, go to **Code signing identities**, then **iOS provisioning profiles**, then **Fetch profiles**, and select the new one.
5. Go to **Apps**, then **Add application**, then **GitHub**, and choose your `track-dash` repository. Codemagic will find `codemagic.yaml`.

## 6. Build it

In Codemagic, open the app, tap **Start new build**, and choose **Track Dash for iPhone**.

When the build turns green, the app has been sent to App Store Connect. Apple takes another 15 to 30 minutes to process it.

If the build fails, open the red step and copy its last 30 or so lines. Paste them to Claude to work out the fix.

## 7. Play it on your iPhone with TestFlight

1. In App Store Connect, open your app, then **TestFlight**. Add yourself under **Internal Testing**.
2. Install Apple's **TestFlight** app on your iPhone, open the invite, and install Track Dash.

Turn off your silent switch to hear the music.

## 8. Send it to the App Store

In App Store Connect, open the **App Store** tab and fill in:

- **Screenshots** of the game on an iPhone. Take them while playing the TestFlight version.
- **Description**, **keywords** and a **support URL**.
- A **privacy policy URL**: `https://2rmphggd5z-ux.github.io/Track-star-/privacy.html` (it's in this folder, so it goes live with GitHub Pages). Under **App Privacy**, choose **Data Not Collected**: the game stores progress only on the phone and doesn't collect or send any data.
- **Age rating** questionnaire and **price** (Free).
- **Build**: choose the one Codemagic uploaded.

Then tap **Add for Review**, then **Submit**. Apple's review usually takes a day or two.

## Sending an update later

1. Replace `track-dash.html` on GitHub with the new version.
2. In `codemagic.yaml`, raise `APP_VERSION`, for example from `1.0.0` to `1.0.1`.
3. Start a new build in Codemagic. The build number goes up automatically.

## Good to know

- The store uses coins earned in the game, not real money, so you don't need Apple's in-app purchase setup. If you later want to sell coins for real money, that needs Apple In-App Purchases. It's a bigger job, but it can be added.
- Progress (high scores, coins, upgrades) is saved on the phone. Deleting the app resets it.
- Vibration uses the iPhone's haptic engine through Capacitor's Haptics add-on. It's switched on automatically in the app build, and players can turn it off from the main menu.
- New players get a short tutorial on their first run. It can be replayed from **How to play**.
- The 3D engine is three.js (MIT licence). Its licence notice is kept at the top of the engine code inside `track-dash.html`.
