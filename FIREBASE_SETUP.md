# Aceapp Histology — Firebase setup

## Recommendation

Do **not** create a second Firebase project.

Reuse the existing Aceapp Anatomy Firebase project:

- Project ID: `aceapp-70837`
- Authentication: shared with Anatomy
- Firestore: shared project, but Histology progress must live in its own subject document

Recommended path:

`users/{firebaseUid}/subjects/histology`

Keep Anatomy exactly where it already is. Do not write Histology progress into
`users/{firebaseUid}.progress`, because Anatomy already uses that field.

## Why shared Firebase is better

- One Aceapp account works across Anatomy and Histology.
- No duplicate user accounts.
- One Firebase Authentication user list.
- Future Aceapp dashboard can show progress across subjects.
- Easier to add Parasitology and Biochemistry later.

## Required Firebase Console steps

1. Keep Email/Password Authentication enabled in the existing project.
2. Add the new Histology Netlify/custom domain under:
   Authentication → Settings → Authorized domains.
3. Replace/publish Firestore rules with the included `firestore.rules`.
   These rules preserve Anatomy access and add:
   `users/{uid}/subjects/histology`
4. Recommended before public launch:
   - enable App Check for both Anatomy and Histology domains;
   - keep anonymous auth disabled unless the product plan changes.

## Histology cloud document

Suggested structure:

```json
{
  "subject": "histology",
  "schemaVersion": 1,
  "progress": {
    "seen": [],
    "items": {},
    "batchPasses": [],
    "misses": [],
    "streak": 0,
    "totalAttempts": 0,
    "totalCorrect": 0
  },
  "profileName": "",
  "updatedAt": "server timestamp"
}
```

## Migration rule

The current Histology build stores progress locally under:

`ace-histology-progress-v1`

When Firebase is wired in:

- if the user logs in and the cloud Histology document is empty, merge/upload local progress;
- if cloud progress exists, prefer the newer `updatedAt` record and merge non-conflicting mastery data;
- debounce Firestore writes;
- never write on every tap.

## Security

The Firebase web API configuration is public client configuration by design.
Never place Firebase Admin service-account credentials in `index.html`.
