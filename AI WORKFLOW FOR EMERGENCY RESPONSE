import cv2
import numpy as np
import random
import csv
import datetime

# ----------------- Disaster Detection Functions -----------------
def detect_fire(frame):
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
    lower = np.array([18, 50, 50], dtype="uint8")
    upper = np.array([35, 255, 255], dtype="uint8")
    mask = cv2.inRange(hsv, lower, upper)
    fire_pixels = cv2.countNonZero(mask)
    return fire_pixels > 2000, mask

def detect_flood(frame):
    hsv = cv2.cvtColor(frame, cv2.COLOR_BGR2HSV)
    lower = np.array([90, 50, 50], dtype="uint8")   # blue range
    upper = np.array([130, 255, 255], dtype="uint8")
    mask = cv2.inRange(hsv, lower, upper)
    water_pixels = cv2.countNonZero(mask)
    return water_pixels > 5000, mask

def detect_earthquake():
    return random.random() > 0.98   # simulated

def detect_accident():
    return random.random() > 0.97   # simulated

def detect_medical_sos():
    return random.choice([False, False, True])   # simulated

# ----------------- Severity & Resources -----------------
def predict_severity():
    return random.choice(["LOW", "MEDIUM", "HIGH"])

def allocate_resources(disaster, severity):
    resources = []
    if disaster == "FIRE":
        resources = ["🚒 Fire Truck", "🚑 Ambulance"]
    elif disaster == "FLOOD":
        resources = ["🚑 Ambulance", "🚨 Rescue Team"]
    elif disaster == "EARTHQUAKE":
        resources = ["🚑 Ambulance", "👮 Police", "Rescue Dogs"]
    elif disaster == "ACCIDENT":
        resources = ["🚑 Ambulance", "👮 Police"]
    elif disaster == "MEDICAL SOS":
        resources = ["🚑 Ambulance", "🏥 Hospital Alert"]

    if severity == "HIGH":
        resources.append("🚁 Helicopter Support")
    return resources

# ----------------- Incident Logging -----------------
def log_incident(disaster, severity, resources):
    timestamp = datetime.datetime.now().strftime("%Y-%m-%d %H:%M:%S")
    with open("incident_log.csv", mode="a", newline="") as file:
        writer = csv.writer(file)
        writer.writerow([timestamp, disaster, severity, ", ".join(resources)])
    print(f"[{timestamp}] {disaster} - {severity} → {resources}")

# ----------------- Main Workflow -----------------
def main():
    cap = cv2.VideoCapture(0)

    while True:
        ret, frame = cap.read()
        if not ret:
            break

        # Detection
        fire, fire_mask = detect_fire(frame)
        flood, flood_mask = detect_flood(frame)
        earthquake = detect_earthquake()
        accident = detect_accident()
        sos = detect_medical_sos()

        alert_msg, disaster_type = None, None

        if fire:
            disaster_type, alert_msg = "FIRE", "🔥 FIRE DETECTED!"
        elif flood:
            disaster_type, alert_msg = "FLOOD", "🌊 FLOOD DETECTED!"
        elif earthquake:
            disaster_type, alert_msg = "EARTHQUAKE", "🌪️ EARTHQUAKE DETECTED!"
        elif accident:
            disaster_type, alert_msg = "ACCIDENT", "🚗 ACCIDENT DETECTED!"
        elif sos:
            disaster_type, alert_msg = "MEDICAL SOS", "🏥 MEDICAL SOS DETECTED!"

        if alert_msg:
            severity = predict_severity()
            resources = allocate_resources(disaster_type, severity)

            # Overlay on video
            cv2.putText(frame, alert_msg, (30, 50),
                        cv2.FONT_HERSHEY_SIMPLEX, 1, (0, 0, 255), 3)
            cv2.putText(frame, f"Severity: {severity}", (30, 90),
                        cv2.FONT_HERSHEY_SIMPLEX, 0.8, (255, 0, 0), 2)

            # Console log
            print(f"ALERT: {alert_msg} | Severity: {severity} | Resources: {resources}")

            # Save incident log
            log_incident(disaster_type, severity, resources)

        # Show windows
        cv2.imshow("Disaster Detection", frame)
        cv2.imshow("Fire Mask", fire_mask)
        cv2.imshow("Flood Mask", flood_mask)

        if cv2.waitKey(1) & 0xFF == ord('q'):
            break

    cap.release()
    cv2.destroyAllWindows()

if __name__ == "__main__":
    # Initialize log file with headers
    with open("incident_log.csv", mode="w", newline="") as file:
        writer = csv.writer(file)
        writer.writerow(["Timestamp", "Disaster", "Severity", "Resources"])
    main()
