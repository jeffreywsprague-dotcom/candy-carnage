# GT's Candy Carnage: setup guide

You'll need about 15–20 minutes and a Google account. Everything here is free.

What's in this folder:

- `site/` is the website. This is the only folder you upload to Netlify.
- `firestore-rules.txt` holds the database security rules. Paste them into Firebase. Don't upload this file, because your admin passcode goes in it.
- `SETUP.md` is this guide.

## Part 1: Create the database (Firebase)

1. Go to https://console.firebase.google.com and sign in with your Google account.
2. Click **Create a project**. Name it something like `gts-candy-carnage`. You can turn Google Analytics off. Wait for it to finish.
3. In the left menu, open **Build → Firestore Database** and click **Create database**. Pick a location near you (any US location is fine) and choose **production mode**.
4. Open the **Rules** tab and delete everything in the editor. Then paste in the whole contents of `firestore-rules.txt`.
5. In the rules you just pasted, find `CHANGE-THIS-PASSCODE` and replace it with your own admin passcode. Keep the quote marks around it. A few words strung together works well, like `'ghost-pumpkin-taffy-42'`. Click **Publish**.
6. In the left menu, open **Build → Authentication** and click **Get started**. On the **Sign-in method** tab, choose **Anonymous**, switch it on, and click **Save**. This gives each phone its own ID so there's one ballot per device and nobody has to sign up.
7. Click the gear icon next to **Project Overview**, then choose **Project settings**. Under **Your apps**, click the web icon (`</>`). Give it a nickname, leave Firebase Hosting unchecked, and click **Register app**.
8. Firebase shows a block of code containing `const firebaseConfig = { ... }`. Open `site/config.js` in any text editor (Notepad or TextEdit is fine). Replace each placeholder value with the matching value from Firebase, then save. These values are meant to be public, so it's fine that they ship with the website.

## Part 2: Put the site online (Netlify)

1. Go to https://app.netlify.com and sign up for a free account. You need an account, or a dropped site disappears after a short while.
2. Go to **Add new site → Deploy manually** (also at https://app.netlify.com/drop) and drag the **`site`** folder onto the page.
3. Netlify gives you a random address. To change it, open **Site configuration → Change site name** and enter something like `gts-candy-carnage`. Your link becomes `https://gts-candy-carnage.netlify.app`, if that name is free.
4. Back in Firebase, open **Authentication → Settings → Authorized domains**. Click **Add domain** and enter your Netlify address without `https://`, for example `gts-candy-carnage.netlify.app`.

## Part 3: Open the first region and test

1. On your phone, open your site with `/#admin` at the end, for example `https://gts-candy-carnage.netlify.app/#admin`.
2. Enter your admin passcode. That device stays unlocked, and a **Manage** button appears on the home screen.
3. Under **Open for voting**, tap **Chocolate Crypt**. (It's also the default before you pick anything.)
4. Vote through a few matchups, then open **Bracket** to check that your picks show up.
5. To clear test votes before sending the link out, go to Firebase **Firestore Database → Data**. Open the `ballots` collection and delete the documents in it.
6. Text the link to your friends.

## Running the tournament

- **Next region:** After a few days, open Manage and tap the next region. Votes in the earlier region stop, and its results stay on the bracket.
- **Final Four:** Once all four regions are done, tap **Final Four**. It shows the four region champions and asks you to confirm before locking them in.
- **Pausing:** **Pause voting** closes voting without opening anything new.
- **Your other devices:** To manage from another phone or computer, open `/#admin` there and enter the passcode again.

## Adding candy photos

1. Name each photo exactly as listed in `site/images/FILE-NAMES.txt`, for example `reeses.jpg` or `candycorn.png`, and put it in `site/images/`. Square photos look best. Any candy without a photo keeps its drawing.
2. In Netlify, open your site, go to the **Deploys** tab, and drag the `site` folder onto it again. The update is live in a few seconds, and votes aren't affected.

## If something's wrong

The site shows a short message at the bottom of the screen when it can't save votes:

- **"Add your Firebase settings to config.js":** `config.js` still has placeholder values, or the updated file wasn't uploaded.
- **"Turn on Anonymous sign-in":** redo Part 1, step 6.
- **"Check that the Firestore rules … are published":** redo Part 1, steps 4–5.
- **"That passcode didn't work":** the passcode you typed doesn't exactly match the one in your published rules. Capitals and spaces count.

## Costs

Firebase's free Spark plan needs no credit card. Its daily limits are tens of thousands of reads and writes, which is far more than a friend-group bracket uses. Netlify's free plan covers a site like this many times over.
