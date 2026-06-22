# ⚽ Saved by the Ball

A football trivia site with two modes:

- **Random** — generates a random football fact
- **Competitive** — guess the missing detail in a fact, multiple choice, until you get one wrong. Easy questions are worth 5 points, hard ones 10. Top scores are saved to a shared leaderboard.

It's a single `index.html` file — no build tools, no frameworks.

## Leaderboard setup

The leaderboard uses [Firebase Firestore](https://firebase.google.com/docs/firestore) so scores are shared across everyone who visits.

1. Create a free project at [console.firebase.google.com](https://console.firebase.google.com) and enable **Firestore Database**
2. In the **Rules** tab, paste this and publish:

   ```
   rules_version = '2';
   service cloud.firestore {
     match /databases/{database}/documents {
       match /scores/{playerId} {
         allow read: if true;
         allow create: if request.resource.data.name is string
                       && request.resource.data.score is int;
         allow update: if request.resource.data.score is int
                       && request.resource.data.score > resource.data.score;
         allow delete: if false;
       }
     }
   }
   ```

3. Add a web app under Project Settings, copy the `firebaseConfig` it gives you, and paste it into the `firebaseConfig` object near the top of the `<script>` tag in `index.html`

To clear scores, delete documents from the `scores` collection in the Firebase console — there's no delete option on the site itself.

## Deploying

Push to GitHub with `index.html` at the root, then go to **Settings → Pages** and set it to deploy from the `main` branch, root folder. Your site will be live at `https://<username>.github.io/<repo>/`.
