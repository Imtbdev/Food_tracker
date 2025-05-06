# Meal Journal App

---

## 🚀 Getting Started

### Prerequisites

Make sure you have the following installed:

- Node.js and npm
- Expo CLI (`npm install -g expo-cli` or use `npx`)
- Expo Go app on your mobile device (iOS/Android)

### Installation

Clone the repository and install dependencies:

```bash
git clone https://github.com/Imtbdev/Food_tracker/
cd Food_tracker
npm install
```

### Running the App

Start the development server:

```bash
npx expo start
```

Then:

- Scan the QR code with Expo Go on your device
- Or launch the app in an emulator

---

## 📷 Features

- User registration and authentication
- Add meals with photo, description, and category
- Take a photo using the camera or select one from the gallery
- View and edit existing meal entries
- Smart category selection based on time of day

---

## 🛠 Built With

- React Native
- Expo
- SQLite (for local storage)
- Expo Image Picker & Camera

## Report

This is a React Native app built with Expo that allows users to track their meals with photos, descriptions, and categories. Users can register and log in, choose a photo from the gallery or take a new one using their phone camera, add a description of the dish, and select a category such as Breakfast, Lunch, Dinner, or Snacks.

All meal entries can be viewed and edited later, giving users a simple and visual food journaling experience.

### 🔥 Unique Feature

The app automatically selects the appropriate meal category based on the current time of day on the user's device, making the experience more seamless and intuitive.

## ✅ Changes and Additions

### 1. Added `<SQLiteProvider>` in `App.js`

Wrapped the main application with the `SQLiteProvider` component from `expo-sqlite/next`:

```jsx
<SQLiteProvider databaseName='FoodJournal.db' onInit={initDatabase}>
  <NavigationContainer>
    <Stack.Navigator>
      <Stack.Screen name="Auth" component={AuthScreen} />
      <Stack.Screen name="Home" component={HomeScreen} />
    </Stack.Navigator>
  </NavigationContainer>
</SQLiteProvider>
```

**Purpose**: This allows any component within the app to access the local SQLite database by providing a shared context. It ensures the database is initialized once and remains available throughout the app lifecycle, enabling persistent storage of user data like meal entries.

---

### 2. Updated `app.json` with Expo Plugins

Added plugins to `app.json` for database and camera functionality:

```json
"plugins": [
  [
    "expo-sqlite",
    {
      "enableFTS": true,
      "useSQLCipher": true,
      "android": {
        "enableFTS": false,
        "useSQLCipher": false
      },
      "ios": {
        "customBuildFlags": [
          "-DSQLITE_ENABLE_DBSTAT_VTAB=1 -DSQLITE_ENABLE_SNAPSHOT=1"
        ]
      }
    }
  ],
  [
    "expo-camera",
    {
      "cameraPermission": "Allow app to access your camera",
      "microphonePermission": "Allow app to access your microphone",
      "recordAudioAndroid": true
    }
  ]
]
```

**Purpose**:

- The `expo-sqlite` plugin enables advanced SQLite features like full-text search (FTS) and encryption (SQLCipher), improving both performance and security of local data storage.
- The `expo-camera` plugin requests the necessary permissions for using the device camera and microphone, which are required for capturing meal photos and enabling media-related functionality on both iOS and Android platforms.

### 3. Refactored SQLite Database Initialization (`database.js`)

#### 🔄 Before:
- The database was initialized and accessed through two exported functions: `initDatabase` and `executeSql`.
- SQL commands were executed using `executeSql`, which handled database initialization internally.
- Transactions were managed via `tx.execAsync` inside `withTransactionAsync`.

#### ✅ After:
- Removed the `executeSql` helper function to simplify database usage.
- All SQL queries are now run directly through the `db` instance after explicit initialization.
- Streamlined the transaction block by using `db.execAsync` directly without the `tx` object.
- Added a `console.log` after opening the database to help with debugging and confirming the connection.

**Purpose**:  
This refactor simplifies the structure by focusing on explicit database control. It makes it easier to reason about the timing of initialization and avoids hidden automatic behaviors from `executeSql`, improving clarity and maintainability.

### 4. Refactored Authentication Screen (`AuthScreen.js`)

#### 🔄 Before:
- Used `executeSql` from an external `database.js` module for all SQL operations.
- `AuthScreen` had no direct connection to the database instance.
- `executeSql` implicitly initialized the database on each call.

#### ✅ After:
- Removed `executeSql` import and replaced it with `useSQLiteContext()` from `expo-sqlite` to directly access the database instance.
- New API methods used:
  - `db.getAllAsync()` for fetching data (login and email existence checks).
  - `db.runAsync()` for inserting a new user during registration.
- Improved transparency and predictability by explicitly working with the `db` instance provided via context.
- Enhanced compatibility with the updated `expo-sqlite` 3.x structure.

**Benefits**:  
This refactoring improves integration with `expo-sqlite`, offers better debugging and maintenance, and ensures a more stable and explicit database access pattern.

### 5. Refactored Home Screen (`HomeScreen.js`)

#### 🔄 Before:
- Used an external `executeSql` helper from `database.js` to run all SQL queries.
- Relied on legacy `Camera` API from `expo-camera` (via `ref`).
- Basic state management for category selection (single category state).
- No use of `expo-sqlite` context, leading to hidden database access logic.

#### ✅ After:
- Switched to the new `useSQLiteContext()` API from `expo-sqlite`:
  - Used `db.getAllAsync()` for querying journal entries.
  - Used `db.runAsync()` for insert, update, and delete operations.
- Replaced deprecated `Camera` API with modern `CameraView` from `expo-camera`.
- Split category into two distinct states:
  - `category1` for assigning during journal creation (auto-suggested by time).
  - `category2` for filtering existing journals.
- Added a `getCategoryByTime()` function to auto-assign categories (e.g. Breakfast, Lunch).
- Improved permission handling with `useCameraPermissions()` hook.
- Code is now cleaner, more modular, and aligned with the latest Expo APIs.

**Benefits**:  
The refactoring brings full compatibility with `expo-sqlite` 3.x and the latest `expo-camera` APIs. The use of contextual DB access improves transparency, while enhanced category logic and UI state separation improve UX and maintainability.

