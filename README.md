import warnings
warnings.filterwarnings("ignore")
import pyrebase
from datetime import datetime
import time
import random  # for simulating sensor data

# ✅ Firebase Configuration
firebaseConfig = {
    "apiKey": "AIzaSyDWMPoeRY-t5zfTGsDjH3v8pIthqMWVM9w",
    "authDomain": "renewable-488d8.firebaseapp.com",
    "databaseURL": "https://renewable-488d8-default-rtdb.firebaseio.com/solar_data/-Obheui-TH2tVUGTl1ww",
    "projectId": "renewable-488d8",
    "storageBucket": "renewable-488d8.appspot.com",
    "messagingSenderId": "620034468809",
    "appId": "1:620034468809:web:22ba050f5fd9ef23291618",
    "measurementId": "G-28KTX24S9F"
}

# ✅ Initialize Firebase
firebase = pyrebase.initialize_app(firebaseConfig)
db = firebase.database()

print("\n🚀 Firebase Realtime Database Connection Successful!\n")

try:
    while True:
        # Simulated data — replace with your sensor values
        solar_panel_data = {
            "timestamp": datetime.now().strftime("%Y-%m-%d %H:%M:%S"),
            "light_intensity": random.randint(700, 900),
            "temperature": round(random.uniform(28.0, 35.0), 2),
            "humidity": random.randint(50, 75),
            "voltage": round(random.uniform(17.5, 18.5), 2),
            "current": round(random.uniform(3.8, 4.3), 2),
            "power": 0  # will be calculated
        }

        # Calculate power (P = V × I)
        solar_panel_data["power"] = round(
            solar_panel_data["voltage"] * solar_panel_data["current"], 2
        )

        # Upload to Firebase
        db.child("solar_data").push(solar_panel_data)

        # ✅ Print to VS Code terminal
        print(f"[{solar_panel_data['timestamp']}] Uploaded -> "
              f"Light: {solar_panel_data['light_intensity']} | "
              f"Temp: {solar_panel_data['temperature']}°C | "
              f"Voltage: {solar_panel_data['voltage']}V | "
              f"Current: {solar_panel_data['current']}A | "
              f"Power: {solar_panel_data['power']}W")

        # 🕒 Wait for 1 minutes (60 seconds)
        print("\n⏳ Waiting 1 minutes before next upload...\n")
        time.sleep(1 * 60)

except KeyboardInterrupt:
    print("\n🛑 Data upload stopped by user.\n")

except Exception as e:
    print("\n❌ Error while uploading data:", e)
