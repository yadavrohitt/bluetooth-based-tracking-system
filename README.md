rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {

    // User profile: only that user can read/write their own document
    match /users/{userId} {
      allow read, write: if request.auth != null && request.auth.uid == userId;
    }

    // Floor plan: any authenticated user can read; only admins can write (optional)
    // If you don't use custom claims for admin, allow write for any authenticated user
    match /config/floorPlan {
      allow read: if request.auth != null;
      allow write: if request.auth != null;
    }

    // Beacons: any authenticated user can read; only admins can write (optional)
    match /beacons/{beaconId} {
      allow read: if request.auth != null;
      allow create, update, delete: if request.auth != null;
    }
  }
}
