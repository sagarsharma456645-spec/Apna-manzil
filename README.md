# Apna-manzil
a
Folder Structure
apna-manzil/
│── App.js
│── firebase.js
│── package.json
│── screens/
│    ├── SplashScreen.js
│    ├── Onboarding.js
│    ├── LoginScreen.js
│    ├── HomeScreen.js
│    ├── DriverScreen.js
│── assets/
     ├── logo.png
     ├── splash.png

📄 package.json
{
  "name": "apna-manzil",
  "version": "1.0.0",
  "main": "node_modules/expo/AppEntry.js",
  "scripts": {
    "start": "expo start",
    "android": "expo start --android",
    "ios": "expo start --ios",
    "web": "expo start --web"
  },
  "dependencies": {
    "expo": "~51.0.0",
    "expo-location": "~16.0.0",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "react-native": "0.74.0",
    "react-native-maps": "1.14.0",
    "react-native-linear-gradient": "^2.6.2",
    "react-native-vector-icons": "^10.0.0",
    "@react-navigation/native": "^6.1.9",
    "@react-navigation/stack": "^6.3.20",
    "firebase": "^10.12.2"
  }
}

📄 firebase.js
// firebase.js
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);

📄 App.js
import React from "react";
import { NavigationContainer } from "@react-navigation/native";
import { createStackNavigator } from "@react-navigation/stack";
import SplashScreen from "./screens/SplashScreen";
import Onboarding from "./screens/Onboarding";
import LoginScreen from "./screens/LoginScreen";
import HomeScreen from "./screens/HomeScreen";
import DriverScreen from "./screens/DriverScreen";

const Stack = createStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator screenOptions={{ headerShown: false }}>
        <Stack.Screen name="Splash" component={SplashScreen} />
        <Stack.Screen name="Onboarding" component={Onboarding} />
        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Driver" component={DriverScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

📄 screens/SplashScreen.js
import React, { useEffect } from "react";
import { View, Text, StyleSheet, Image } from "react-native";

export default function SplashScreen({ navigation }) {
  useEffect(() => {
    setTimeout(() => {
      navigation.replace("Onboarding");
    }, 2000);
  }, []);

  return (
    <View style={styles.container}>
      <Image source={require("../assets/logo.png")} style={{ width: 120, height: 120 }} />
      <Text style={styles.text}>Apna Manzil 🚖</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#000" },
  text: { color: "#fff", fontSize: 24, fontWeight: "bold", marginTop: 20 }
});

📄 screens/Onboarding.js
import React from "react";
import { View, Text, StyleSheet, TouchableOpacity } from "react-native";

export default function Onboarding({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Welcome to Apna Manzil 🚘</Text>
      <Text style={styles.subtitle}>Luxury rides, premium experience!</Text>
      <TouchableOpacity style={styles.btn} onPress={() => navigation.replace("Login")}>
        <Text style={styles.btnText}>Get Started</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#111" },
  title: { fontSize: 28, color: "#fff", fontWeight: "bold" },
  subtitle: { fontSize: 16, color: "#aaa", marginVertical: 20 },
  btn: { backgroundColor: "#ffcc00", padding: 15, borderRadius: 10 },
  btnText: { fontSize: 18, fontWeight: "bold" }
});

📄 screens/LoginScreen.js
import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, StyleSheet } from "react-native";
import { auth } from "../firebase";
import { signInWithEmailAndPassword, createUserWithEmailAndPassword } from "firebase/auth";

export default function LoginScreen({ navigation }) {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleLogin = async () => {
    try {
      await signInWithEmailAndPassword(auth, email, password);
      navigation.replace("Home");
    } catch (error) {
      alert("Login Failed: " + error.message);
    }
  };

  const handleSignup = async () => {
    try {
      await createUserWithEmailAndPassword(auth, email, password);
      navigation.replace("Home");
    } catch (error) {
      alert("Signup Failed: " + error.message);
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Login to Apna Manzil</Text>
      <TextInput style={styles.input} placeholder="Email" value={email} onChangeText={setEmail} />
      <TextInput style={styles.input} placeholder="Password" secureTextEntry value={password} onChangeText={setPassword} />
      <TouchableOpacity style={styles.btn} onPress={handleLogin}>
        <Text style={styles.btnText}>Login</Text>
      </TouchableOpacity>
      <TouchableOpacity style={[styles.btn, { backgroundColor: "#333" }]} onPress={handleSignup}>
        <Text style={styles.btnText}>Sign Up</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#000" },
  title: { fontSize: 24, color: "#fff", marginBottom: 20 },
  input: { backgroundColor: "#222", color: "#fff", width: "80%", padding: 10, marginVertical: 10, borderRadius: 8 },
  btn: { backgroundColor: "#ffcc00", padding: 15, borderRadius: 8, marginVertical: 10, width: "80%", alignItems: "center" },
  btnText: { color: "#000", fontWeight: "bold" }
});

📄 screens/HomeScreen.js
import React from "react";
import { View, Text, StyleSheet, TouchableOpacity } from "react-native";

export default function HomeScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Book your Ride 🚖</Text>
      <TouchableOpacity style={styles.btn} onPress={() => navigation.navigate("Driver")}>
        <Text style={styles.btnText}>Find Driver</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#111" },
  title: { fontSize: 24, color: "#fff", marginBottom: 20 },
  btn: { backgroundColor: "#ffcc00", padding: 15, borderRadius: 10 },
  btnText: { color: "#000", fontWeight: "bold" }
});

📄 screens/DriverScreen.js
import React from "react";
import { View, Text, StyleSheet } from "react-native";
import MapView, { Marker } from "react-native-maps";

export default function DriverScreen() {
  return (
    <View style={styles.container}>
      <MapView
        style={styles.map}
        initialRegion={{
          latitude: 28.6139,
          longitude: 77.2090,
          latitudeDelta: 0.09,
          longitudeDelta: 0.04,
        }}
      >
        <Marker coordinate={{ latitude: 28.6139, longitude: 77.2090 }} title="Pickup Point" />
      </MapView>
      <View style={styles.bottom}>
        <Text style={{ color: "#fff" }}>Driver is on the way...</Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1 },
  map: { flex: 1 },
  bottom: { padding: 20, backgroundColor: "#000", alignItems: "center" }
});

🚀 Next Steps (tumhare liye)

GitHub pe naya repo banao → naam apna-manzil

Ye saare code files repo me upload karo.

Terminal me:

git clone <tumhara-repo-link>
cd apna-manzil
npm install
npx expo startFolder Structure
apna-manzil/
│── App.js
│── firebase.js
│── package.json
│── screens/
│    ├── SplashScreen.js
│    ├── Onboarding.js
│    ├── LoginScreen.js
│    ├── HomeScreen.js
│    ├── DriverScreen.js
│── assets/
     ├── logo.png
     ├── splash.png

📄 package.json
{
  "name": "apna-manzil",
  "version": "1.0.0",
  "main": "node_modules/expo/AppEntry.js",
  "scripts": {
    "start": "expo start",
    "android": "expo start --android",
    "ios": "expo start --ios",
    "web": "expo start --web"
  },
  "dependencies": {
    "expo": "~51.0.0",
    "expo-location": "~16.0.0",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "react-native": "0.74.0",
    "react-native-maps": "1.14.0",
    "react-native-linear-gradient": "^2.6.2",
    "react-native-vector-icons": "^10.0.0",
    "@react-navigation/native": "^6.1.9",
    "@react-navigation/stack": "^6.3.20",
    "firebase": "^10.12.2"
  }
}

📄 firebase.js
// firebase.js
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);

📄 App.js
import React from "react";
import { NavigationContainer } from "@react-navigation/native";
import { createStackNavigator } from "@react-navigation/stack";
import SplashScreen from "./screens/SplashScreen";
import Onboarding from "./screens/Onboarding";
import LoginScreen from "./screens/LoginScreen";
import HomeScreen from "./screens/HomeScreen";
import DriverScreen from "./screens/DriverScreen";

const Stack = createStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator screenOptions={{ headerShown: false }}>
        <Stack.Screen name="Splash" component={SplashScreen} />
        <Stack.Screen name="Onboarding" component={Onboarding} />
        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Driver" component={DriverScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

📄 screens/SplashScreen.js
import React, { useEffect } from "react";
import { View, Text, StyleSheet, Image } from "react-native";

export default function SplashScreen({ navigation }) {
  useEffect(() => {
    setTimeout(() => {
      navigation.replace("Onboarding");
    }, 2000);
  }, []);

  return (
    <View style={styles.container}>
      <Image source={require("../assets/logo.png")} style={{ width: 120, height: 120 }} />
      <Text style={styles.text}>Apna Manzil 🚖</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#000" },
  text: { color: "#fff", fontSize: 24, fontWeight: "bold", marginTop: 20 }
});

📄 screens/Onboarding.js
import React from "react";
import { View, Text, StyleSheet, TouchableOpacity } from "react-native";

export default function Onboarding({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Welcome to Apna Manzil 🚘</Text>
      <Text style={styles.subtitle}>Luxury rides, premium experience!</Text>
      <TouchableOpacity style={styles.btn} onPress={() => navigation.replace("Login")}>
        <Text style={styles.btnText}>Get Started</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#111" },
  title: { fontSize: 28, color: "#fff", fontWeight: "bold" },
  subtitle: { fontSize: 16, color: "#aaa", marginVertical: 20 },
  btn: { backgroundColor: "#ffcc00", padding: 15, borderRadius: 10 },
  btnText: { fontSize: 18, fontWeight: "bold" }
});

📄 screens/LoginScreen.js
import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, StyleSheet } from "react-native";
import { auth } from "../firebase";
import { signInWithEmailAndPassword, createUserWithEmailAndPassword } from "firebase/auth";

export default function LoginScreen({ navigation }) {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleLogin = async () => {
    try {
      await signInWithEmailAndPassword(auth, email, password);
      navigation.replace("Home");
    } catch (error) {
      alert("Login Failed: " + error.message);
    }
  };

  const handleSignup = async () => {
    try {
      await createUserWithEmailAndPassword(auth, email, password);
      navigation.replace("Home");
    } catch (error) {
      alert("Signup Failed: " + error.message);
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Login to Apna Manzil</Text>
      <TextInput style={styles.input} placeholder="Email" value={email} onChangeText={setEmail} />
      <TextInput style={styles.input} placeholder="Password" secureTextEntry value={password} onChangeText={setPassword} />
      <TouchableOpacity style={styles.btn} onPress={handleLogin}>
        <Text style={styles.btnText}>Login</Text>
      </TouchableOpacity>
      <TouchableOpacity style={[styles.btn, { backgroundColor: "#333" }]} onPress={handleSignup}>
        <Text style={styles.btnText}>Sign Up</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#000" },
  title: { fontSize: 24, color: "#fff", marginBottom: 20 },
  input: { backgroundColor: "#222", color: "#fff", width: "80%", padding: 10, marginVertical: 10, borderRadius: 8 },
  btn: { backgroundColor: "#ffcc00", padding: 15, borderRadius: 8, marginVertical: 10, width: "80%", alignItems: "center" },
  btnText: { color: "#000", fontWeight: "bold" }
});

📄 screens/HomeScreen.js
import React from "react";
import { View, Text, StyleSheet, TouchableOpacity } from "react-native";

export default function HomeScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Book your Ride 🚖</Text>
      <TouchableOpacity style={styles.btn} onPress={() => navigation.navigate("Driver")}>
        <Text style={styles.btnText}>Find Driver</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#111" },
  title: { fontSize: 24, color: "#fff", marginBottom: 20 },
  btn: { backgroundColor: "#ffcc00", padding: 15, borderRadius: 10 },
  btnText: { color: "#000", fontWeight: "bold" }
});

📄 screens/DriverScreen.js
import React from "react";
import { View, Text, StyleSheet } from "react-native";
import MapView, { Marker } from "react-native-maps";

export default function DriverScreen() {
  return (
    <View style={styles.container}>
      <MapView
        style={styles.map}
        initialRegion={{
          latitude: 28.6139,
          longitude: 77.2090,
          latitudeDelta: 0.09,
          longitudeDelta: 0.04,
        }}
      >
        <Marker coordinate={{ latitude: 28.6139, longitude: 77.2090 }} title="Pickup Point" />
      </MapView>
      <View style={styles.bottom}>
        <Text style={{ color: "#fff" }}>Driver is on the way...</Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1 },
  map: { flex: 1 },
  bottom: { padding: 20, backgroundColor: "#000", alignItems: "center" }
});

🚀 Next Steps (tumhare liye)

GitHub pe naya repo banao → naam apna-manzil

Ye saare code files repo me upload karo.

Terminal me:

git clone <tumhara-repo-link>
cd apna-manzil
npm install
npx expo startFolder Structure
apna-manzil/
│── App.js
│── firebase.js
│── package.json
│── screens/
│    ├── SplashScreen.js
│    ├── Onboarding.js
│    ├── LoginScreen.js
│    ├── HomeScreen.js
│    ├── DriverScreen.js
│── assets/
     ├── logo.png
     ├── splash.png

📄 package.json
{
  "name": "apna-manzil",
  "version": "1.0.0",
  "main": "node_modules/expo/AppEntry.js",
  "scripts": {
    "start": "expo start",
    "android": "expo start --android",
    "ios": "expo start --ios",
    "web": "expo start --web"
  },
  "dependencies": {
    "expo": "~51.0.0",
    "expo-location": "~16.0.0",
    "react": "18.2.0",
    "react-dom": "18.2.0",
    "react-native": "0.74.0",
    "react-native-maps": "1.14.0",
    "react-native-linear-gradient": "^2.6.2",
    "react-native-vector-icons": "^10.0.0",
    "@react-navigation/native": "^6.1.9",
    "@react-navigation/stack": "^6.3.20",
    "firebase": "^10.12.2"
  }
}

📄 firebase.js
// firebase.js
import { initializeApp } from "firebase/app";
import { getAuth } from "firebase/auth";
import { getFirestore } from "firebase/firestore";

const firebaseConfig = {
  apiKey: "YOUR_API_KEY",
  authDomain: "YOUR_PROJECT.firebaseapp.com",
  projectId: "YOUR_PROJECT_ID",
  storageBucket: "YOUR_PROJECT.appspot.com",
  messagingSenderId: "YOUR_SENDER_ID",
  appId: "YOUR_APP_ID"
};

const app = initializeApp(firebaseConfig);
export const auth = getAuth(app);
export const db = getFirestore(app);

📄 App.js
import React from "react";
import { NavigationContainer } from "@react-navigation/native";
import { createStackNavigator } from "@react-navigation/stack";
import SplashScreen from "./screens/SplashScreen";
import Onboarding from "./screens/Onboarding";
import LoginScreen from "./screens/LoginScreen";
import HomeScreen from "./screens/HomeScreen";
import DriverScreen from "./screens/DriverScreen";

const Stack = createStackNavigator();

export default function App() {
  return (
    <NavigationContainer>
      <Stack.Navigator screenOptions={{ headerShown: false }}>
        <Stack.Screen name="Splash" component={SplashScreen} />
        <Stack.Screen name="Onboarding" component={Onboarding} />
        <Stack.Screen name="Login" component={LoginScreen} />
        <Stack.Screen name="Home" component={HomeScreen} />
        <Stack.Screen name="Driver" component={DriverScreen} />
      </Stack.Navigator>
    </NavigationContainer>
  );
}

📄 screens/SplashScreen.js
import React, { useEffect } from "react";
import { View, Text, StyleSheet, Image } from "react-native";

export default function SplashScreen({ navigation }) {
  useEffect(() => {
    setTimeout(() => {
      navigation.replace("Onboarding");
    }, 2000);
  }, []);

  return (
    <View style={styles.container}>
      <Image source={require("../assets/logo.png")} style={{ width: 120, height: 120 }} />
      <Text style={styles.text}>Apna Manzil 🚖</Text>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#000" },
  text: { color: "#fff", fontSize: 24, fontWeight: "bold", marginTop: 20 }
});

📄 screens/Onboarding.js
import React from "react";
import { View, Text, StyleSheet, TouchableOpacity } from "react-native";

export default function Onboarding({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Welcome to Apna Manzil 🚘</Text>
      <Text style={styles.subtitle}>Luxury rides, premium experience!</Text>
      <TouchableOpacity style={styles.btn} onPress={() => navigation.replace("Login")}>
        <Text style={styles.btnText}>Get Started</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#111" },
  title: { fontSize: 28, color: "#fff", fontWeight: "bold" },
  subtitle: { fontSize: 16, color: "#aaa", marginVertical: 20 },
  btn: { backgroundColor: "#ffcc00", padding: 15, borderRadius: 10 },
  btnText: { fontSize: 18, fontWeight: "bold" }
});

📄 screens/LoginScreen.js
import React, { useState } from "react";
import { View, Text, TextInput, TouchableOpacity, StyleSheet } from "react-native";
import { auth } from "../firebase";
import { signInWithEmailAndPassword, createUserWithEmailAndPassword } from "firebase/auth";

export default function LoginScreen({ navigation }) {
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");

  const handleLogin = async () => {
    try {
      await signInWithEmailAndPassword(auth, email, password);
      navigation.replace("Home");
    } catch (error) {
      alert("Login Failed: " + error.message);
    }
  };

  const handleSignup = async () => {
    try {
      await createUserWithEmailAndPassword(auth, email, password);
      navigation.replace("Home");
    } catch (error) {
      alert("Signup Failed: " + error.message);
    }
  };

  return (
    <View style={styles.container}>
      <Text style={styles.title}>Login to Apna Manzil</Text>
      <TextInput style={styles.input} placeholder="Email" value={email} onChangeText={setEmail} />
      <TextInput style={styles.input} placeholder="Password" secureTextEntry value={password} onChangeText={setPassword} />
      <TouchableOpacity style={styles.btn} onPress={handleLogin}>
        <Text style={styles.btnText}>Login</Text>
      </TouchableOpacity>
      <TouchableOpacity style={[styles.btn, { backgroundColor: "#333" }]} onPress={handleSignup}>
        <Text style={styles.btnText}>Sign Up</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#000" },
  title: { fontSize: 24, color: "#fff", marginBottom: 20 },
  input: { backgroundColor: "#222", color: "#fff", width: "80%", padding: 10, marginVertical: 10, borderRadius: 8 },
  btn: { backgroundColor: "#ffcc00", padding: 15, borderRadius: 8, marginVertical: 10, width: "80%", alignItems: "center" },
  btnText: { color: "#000", fontWeight: "bold" }
});

📄 screens/HomeScreen.js
import React from "react";
import { View, Text, StyleSheet, TouchableOpacity } from "react-native";

export default function HomeScreen({ navigation }) {
  return (
    <View style={styles.container}>
      <Text style={styles.title}>Book your Ride 🚖</Text>
      <TouchableOpacity style={styles.btn} onPress={() => navigation.navigate("Driver")}>
        <Text style={styles.btnText}>Find Driver</Text>
      </TouchableOpacity>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1, justifyContent: "center", alignItems: "center", backgroundColor: "#111" },
  title: { fontSize: 24, color: "#fff", marginBottom: 20 },
  btn: { backgroundColor: "#ffcc00", padding: 15, borderRadius: 10 },
  btnText: { color: "#000", fontWeight: "bold" }
});

📄 screens/DriverScreen.js
import React from "react";
import { View, Text, StyleSheet } from "react-native";
import MapView, { Marker } from "react-native-maps";

export default function DriverScreen() {
  return (
    <View style={styles.container}>
      <MapView
        style={styles.map}
        initialRegion={{
          latitude: 28.6139,
          longitude: 77.2090,
          latitudeDelta: 0.09,
          longitudeDelta: 0.04,
        }}
      >
        <Marker coordinate={{ latitude: 28.6139, longitude: 77.2090 }} title="Pickup Point" />
      </MapView>
      <View style={styles.bottom}>
        <Text style={{ color: "#fff" }}>Driver is on the way...</Text>
      </View>
    </View>
  );
}

const styles = StyleSheet.create({
  container: { flex: 1 },
  map: { flex: 1 },
  bottom: { padding: 20, backgroundColor: "#000", alignItems: "center" }
});

🚀 Next Steps (tumhare liye)

GitHub pe naya repo banao → naam apna-manzil

Ye saare code files repo me upload karo.

Terminal me:

git clone <tumhara-repo-link>
cd apna-manzil
npm install
npx expo start
