# Build & Update Guide (Windows)

This guide shows how we edit the portal HTML, convert it to a **.gz** for faster serving from SPIFFS, and upload it to the ESP32 using **Arduino IDE** on Windows.

---

## Repo layout


STACKS_Wifi_Portal/
├─ portal_src/
│ └─ STACKS_WiFi_Portal.html ← edit this file
├─ data/
│ └─ STACKS_Wifi_Portal.html.gz ← device-ready gzip lives here
└─ docs/
└─ BUILD_AND_UPDATE.md


> For Arduino, the **sketch** that serves the page must have a `data/` folder next to the `.ino`. Copy the `.gz` there before uploading to SPIFFS

## Why gzip?
- Faster load times, smaller flash footprint.
- `ESPAsyncWebServer` auto-sends `Content-Encoding: gzip` if the filename ends with `.gz`.

## 1) Edit the HTML
- Open and edit: `portal_src/STACKS_WiFi_Portal.html`

---

## 2) Create the `.gz` on Windows (right-click method)
We used Windows with **7-Zip** installed.

1. Right-click `portal_src/STACKS_WiFi_Portal.html`
2. Choose **7-Zip → Add to archive…**
3. In the dialog:
   - **Archive format:** `gzip`
   - **Compression level:** `Ultra`
4. Click **OK** to produce `STACKS_WiFi_Portal.html.gz`
5. Move the new file into:
data/STACKS_Wifi_Portal.html.gz



*(Keep the exact filename extension `.html.gz`.)*

> Optional: If you want this to be the site’s default page, rename it to **`index.html.gz`** and adjust your server code accordingly (see below).

---

## 3) Upload the `data/` folder to the ESP32 (SPIFFS)
In **Arduino IDE**:

1. Make sure you have the **ESP32FS (SPIFFS) Uploader** installed.
2. Open your **Arduino sketch** (the `.ino` that serves the portal).
3. Ensure your sketch folder contains:
YourSketch/
├─ YourSketch.ino
└─ data/
└─ STACKS_Wifi_Portal.html.gz


Upload to device using Arduino IDE 

4. Go to **Tools → ESP32 Sketch Data Upload**
5. When prompted, **choose `SPIFFS`** and confirm.
6. Wait for the upload to complete, then the device will **reboot**, if it does not by chance, unplug it and plug it back in

---

## 4) Serving patterns (ESPAsyncWebServer)

**Option A — Serve this specific filename**
```cpp
server.on("/", HTTP_GET, [](AsyncWebServerRequest *req){
req->send(SPIFFS, "/STACKS_Wifi_Portal.html.gz", String(), true);
});

Option B — Use index.html.gz as default
server.serveStatic("/", SPIFFS, "/").setDefaultFile("index.html");
// If index.html.gz exists, it will be used automatically.

Troubleshooting

Browser shows gibberish: Make sure you’re serving the .gz file (and not the plain .html), and that the filename ends with .gz.

404 Not Found: Confirm the file was inside the sketch’s data/ folder before running “ESP32 Sketch Data Upload”.
