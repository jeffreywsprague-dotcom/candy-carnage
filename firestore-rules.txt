rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    function isAdmin() {
      return request.auth != null
        && exists(/databases/$(database)/documents/admins/$(request.auth.uid));
    }

    // Which region is open. Everyone can read it; only unlocked admin devices can change it.
    match /config/{doc} {
      allow read: if true;
      allow write: if isAdmin();
    }

    // One ballot per device. Everyone can read them (for the live bracket);
    // each device can only write its own. Admins can delete ballots.
    match /ballots/{uid} {
      allow read: if true;
      allow create, update: if request.auth != null
        && request.auth.uid == uid
        && request.resource.data.keys().hasOnly(['stages', 't'])
        && request.resource.size() < 20000;
      allow delete: if isAdmin();
    }

    // Admin unlock. Replace CHANGE-THIS-PASSCODE with your own passcode
    // (a few words is best, e.g. 'ghost-pumpkin-taffy-42').
    // Nobody can read this collection, so the passcode stays private.
    match /admins/{uid} {
      allow read: if false;
      allow create, update: if request.auth != null
        && request.auth.uid == uid
        && request.resource.data.keys().hasOnly(['pin'])
        && request.resource.data.pin == 'CHANGE-THIS-PASSCODE';
    }
  }
}
