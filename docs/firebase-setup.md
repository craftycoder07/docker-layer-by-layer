# Firebase setup (sign-in + progress sync)

Sign-in stays hidden on the site until `FIREBASE_CONFIG` in `index.html` is filled in.
Everything below runs on Firebase's free Spark plan (no card needed).

Throughout, `PROJECT_ID` is your Firebase project ID and the **auth handler URL** is:
`https://PROJECT_ID.firebaseapp.com/__/auth/handler`

## 1. Create the project
1. https://console.firebase.google.com → **Create a project** → name it (e.g. `layer-by-layer`). Analytics can be off.
2. **Project settings → General → Your apps → Web (`</>`)** → register an app (no Hosting needed).
3. Copy the `firebaseConfig` object it shows. That goes into `FIREBASE_CONFIG` in `index.html`. (These values are designed to be public; the Firestore rules below are what protect data.)

## 2. Allow the site's domain
**Authentication → Settings → Authorized domains → Add domain:** `craftycoder07.github.io`

## 3. Turn on sign-in methods (Authentication → Sign-in method)
- **Google:** enable, pick a support email, save.
- **Email link:** enable **Email/Password**, then switch on **Email link (passwordless sign-in)**. Save.
- **GitHub:**
  1. On GitHub: **Settings → Developer settings → OAuth Apps → New OAuth App**
     - Homepage URL: `https://craftycoder07.github.io/docker-layer-by-layer/`
     - Authorization callback URL: the auth handler URL above
  2. Generate a client secret, then paste the **Client ID** and **Client secret** into Firebase's GitHub provider and save.
- **Microsoft:**
  1. https://portal.azure.com → **Microsoft Entra ID → App registrations → New registration**
     - Supported account types: *Accounts in any organizational directory and personal Microsoft accounts*
     - Redirect URI: platform **Web**, value = the auth handler URL above
  2. **Certificates & secrets → New client secret**. Copy the secret's **Value** (not its ID).
  3. In Firebase's Microsoft provider, paste the **Application (client) ID** and the secret value. Save.

Skip any provider you don't want: remove it from `AUTH_PROVIDERS` in `index.html`.

## 4. Create the database
1. **Build → Firestore Database → Create database** → production mode → pick a location.
2. **Rules** tab → replace everything with the contents of [`firestore.rules`](../firestore.rules) → **Publish**.

## What gets stored
One document per user at `users/{uid}`: completed lesson IDs, completed challenge IDs, chosen stack, display name, and a last-updated timestamp. Sandbox state stays in the browser.
