# 🏏 Boundary Buddies

**Boundary Buddies** is a web-based cricket team management application designed to help teams manage their squad, organize batting orders, select matches, and track live scores through a simple interface.

## ✨ Features

### 🏏 Squad Management
- Add and manage cricket team players.
- Store and retrieve player information using Firebase Firestore.
- Keep squad information accessible to users.

### 📋 Match Management
- Manage match details and match selection.
- Organize match-related information.
- Maintain batting order information for selected matches.

### 🥇 Batting Order
- Manage the batting order for a match.
- View the current batting order.
- Restrict changes when the primary admin is active.

### 📊 Live Score Management
- Manage match scores independently of squad and batting-order permissions.
- Allow authorized backup admins to manage scores when their permissions permit it.

### 🔐 Admin Authentication and Permissions
- Firebase Authentication for admin sign-in.
- Owner and backup admin roles.
- Configurable backup admin permissions.
- Owner-priority session management.
- Automatic owner-session renewal using a renewable lease.
- Read-only restrictions for backup admins when the owner is active.
- Permission enforcement through Firebase Firestore Security Rules.

### 🔄 Owner Session Management
- The owner session is stored in Firestore under `adminSessions/active`.
- The session lease expires approximately 90 seconds after renewal.
- The owner session is renewed every 25 seconds while the owner is active.
- The owner can take priority over a backup admin.
- Backup admins regain their assigned permissions when the owner session is no longer active, subject to the configured permissions.

## 🛠️ Technology Stack

- **Frontend:** HTML, CSS, JavaScript
- **Authentication:** Firebase Authentication
- **Database:** Cloud Firestore
- **Firebase SDK:** Version 10.14.1
- **Hosting:** Compatible with static web hosting, including Firebase Hosting

## 📁 Project Structure

The current application can be deployed as a single HTML file.

```text
Boundary-Buddies/
├── index.html
└── README.md
```

If the application is split into multiple files in the future, the structure can be updated accordingly.

## 🚀 Setup and Installation

### 1. Clone or download the project

Download the application source code or clone the repository:

```bash
git clone <your-repository-url>
cd Boundary-Buddies
```

Replace `<your-repository-url>` with your actual repository URL.

### 2. Create a Firebase project

1. Open the [Firebase Console](https://console.firebase.google.com/).
2. Create or select your Firebase project.
3. Register a web application.
4. Copy the Firebase web configuration into the application source code.

### 3. Enable Firebase Authentication

1. Open **Build → Authentication**.
2. Enable the **Email/Password** sign-in provider.
3. Create the owner and backup admin accounts.
4. Ensure each admin's Firebase Authentication UID matches the document ID in the Firestore `admins` collection.

### 4. Configure Cloud Firestore

Create or use the project's Firestore database and publish the application's security rules.

The application uses the following collections:

| Collection | Purpose |
|---|---|
| `admins` | Admin roles and backup permissions |
| `adminSessions` | Active owner session and lease |
| `squad` | Cricket team player records |
| `matches` | Match details and batting order |
| `scores` | Match score information |
| `counters` | Application counters used by squad management |

### 5. Run the application

For initial testing, open the HTML file in a browser that supports JavaScript modules.

For reliable deployment and testing, serve it through a local web server or deploy it using Firebase Hosting.

## 👥 Admin Roles

### Owner Admin

The owner has primary administrative control over the application, including:
- Squad management
- Match management
- Batting-order management
- Score management
- Backup admin management

The owner can take priority over a backup admin's squad and batting-order access.

### Backup Admin

Backup admins have permissions assigned by the owner, such as:
- `manageSquad`
- `manageMatches`
- `manageBattingOrder`
- `manageScore`

When the owner session is active, backup admins cannot modify the squad, matches, or batting order through the corresponding restricted permissions. Their score-management permission is evaluated independently.

If the owner is active when a backup admin attempts to sign in, the application is designed to block that sign-in and display:

> The primary admin is currently active. Please try again later.

If a backup admin is already signed in when the owner takes over, the application is designed to retain the backup admin's signed-in state while restricting the relevant actions and displaying a read-only banner.

## 🔒 Security

The application uses Firebase Authentication and Firestore Security Rules to protect administrative operations.

Security principles include:
- Only registered admins can access protected admin records.
- Only the owner can manage admin accounts.
- Backup admin permissions are checked before protected operations.
- Squad, match, and batting-order writes are restricted while an active owner session exists.
- Score permissions are managed separately.
- Unmatched Firestore paths are denied by default.

**Important:** Client-side UI restrictions are not sufficient by themselves. Firestore Security Rules must remain enabled and correctly configured because users can attempt database operations outside the application's interface.

## 🧪 Testing Checklist

- [ ] Owner can sign in successfully.
- [ ] Owner can manage the squad, matches, batting order, and scores.
- [ ] Owner session appears in `adminSessions/active`.
- [ ] `expiresAt` advances as the session renews.
- [ ] Backup admin cannot sign in while the owner session is active.
- [ ] Backup admin becomes read-only if the owner takes over an existing session.
- [ ] Read-only banner appears only when applicable.
- [ ] Backup admin regains assigned permissions after the owner session ends.
- [ ] Backup admin score permissions work independently of squad and batting-order restrictions.
- [ ] Firestore Security Rules reject unauthorized writes.
- [ ] Owner sign-out releases the owner session.

## ⚠️ Important Notes

- The owner session uses a renewable lease. A lease indicates that the session is considered active until its expiry; it is not absolute proof that the owner is still connected.
- Test session takeover, renewal failures, expiry, and sign-out in separate browser sessions.
- Never commit service-account credentials, private keys, or other server-side secrets to a public repository.
- Firebase web configuration values are intended to be used by web clients, but access must still be protected through Firebase Security Rules and appropriate Firebase project restrictions.
- Verify the application's behavior and security rules before using it for a live match.

## 📄 License

Add your preferred license here if you intend to distribute the project publicly.

---

**Boundary Buddies — Manage your squad. Organize your batting order. Keep the game moving.**
