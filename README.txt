# ئەلشىفا نومۇر ئېلىش تور سىستېمىسى — تارقىتىشقا تەييارلاش

## ھۆججەتلەر
- `patient.html`: بىمارلارنىڭ نومۇر ئىلتىماس قىلىش بېتى.
- `admin.html`: باشقۇرغۇچى سۇپىسى (Firebase Authentication email/password).
- `database.rules.json`: Firebase Realtime Database بىخەتەرلىك قائىدىسى.
- `firebase.json`: Firebase Hosting تەڭشىكى.

## 1) Firebase Authentication نى ئېچىش
Firebase Console → Authentication → Sign-in method → Email/Password → Enable.
Authentication → Users → Add user ئارقىلىق باشقۇرغۇچى ئېلخەت/پارول ھېساباتى قۇرۇڭ.

## 2) باشقۇرغۇچى UID
Authentication → Users → قۇرغان ھېسابات → UID نى كۆچۈرۈڭ.
`database.rules.json` ئىچىدىكى ھەر بىر `PUT_ADMIN_UID_HERE` نى UID بىلەن ئالماشتۇرۇڭ.

## 3) Database Rules
Realtime Database → Rules نىڭ ھازىرقى قائىدىلىرىنى backup قىلىڭ، ئاندىن تەييار `database.rules.json` نى rules غا كۆچۈرۈپ Publish قىلىڭ.
ئەسكەرتىش: بۇ قائىدىلەر public نىڭ settings نى ئوقۇشى ۋە pending ئىلتىماس قوشۇشىغا يول قويىدۇ، ئەمما pending/approved/history بىمار ئۇچۇرلىرىنى ئوقۇشنى admin غا چەكلەيدۇ. Public ticket يولىدا پەقەت نومۇر، كېلىش ۋاقتى ۋە كۈن ساقلىنىدۇ.

## 4) Hosting
Node.js نى قاچىلاپ، بۇ ھۆججەتلەرنى بىر فولدېرغا قويۇڭ. Terminal:
```bash
npm install -g firebase-tools
firebase login
firebase init hosting
```
Init دا:
- Existing project: `alshifa-573-6bf98`
- Public directory: `.` (current folder)
- Configure as single-page app: No
- Overwrite index.html: No (patient.html نى index.html قىلىپ كۆچۈرۈش ياكى `firebase.json` تەڭشەش)

ئاسان تەڭشەش ئۈچۈن `patient.html` نى `index.html` قىلىپ copy قىلىڭ. `admin.html` admin بەت بولۇپ قالىدۇ.
ئاندىن:
```bash
firebase deploy --only hosting
```

## 5) بىخەتەرلىك/چەكلىمە
- Admin پارول HTML دا ساقلانمايدۇ؛ Firebase Auth ئىشلىتىلىدۇ.
- Database rules admin UID غا باغلانغان. UID نى rules غا قويماي Publish قىلماڭ.
- Patient ticket ID random Firebase push ID بولۇپ، approvedPublic دا PII (ئىسىم/تېلېفون) يوق. بىمارنىڭ ئىسمى ئۇنىڭ ئۈسكۈنىسىدىكى localStorage دىن كۆرۈنىدۇ.
- بىمارنىڭ duplicate phone request نى پۈتۈنلەي server-side توسۇش ئۈچۈن Cloud Functions ياكى باشقا trusted backend لازىم. ھازىرقى نۇسخا تەكرار ئىلتىماسنى admin كۆرۈپ رەت قىلالايدۇ.
- Firebase Rules نىڭ admin UID ئورنىنى توغرا ئالماشتۇرۇپ، publish قىلغاندىن كېيىن test قىلىڭ.
