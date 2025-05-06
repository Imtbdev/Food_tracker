# Meal Journal App

This is a React Native app built with Expo that allows users to track their meals with photos, descriptions, and categories. Users can register and log in, choose a photo from the gallery or take a new one using their phone camera, add a description of the dish, and select a category such as Breakfast, Lunch, Dinner, or Snacks.

All meal entries can be viewed and edited later, giving users a simple and visual food journaling experience.

### 🔥 Unique Feature

The app automatically selects the appropriate meal category based on the current time of day on the user's device, making the experience more seamless and intuitive.

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
