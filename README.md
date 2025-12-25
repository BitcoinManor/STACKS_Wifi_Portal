# STACKS_WiFi_Portal

A lightweight, mobile-friendly, captive-style Wi-Fi setup portal for **STACKSWORTH ESP32 devices**, served directly from SPIFFS using `ESPAsyncWebServer`.

This portal is designed to be **fast, reliable, and memory-efficient**, using a pre-compressed `.html.gz` file to minimize flash usage and load time on both desktop and mobile devices.

---

## ✨ Purpose

The STACKS Wi-Fi Portal provides a clean, branded setup experience for STACKSWORTH devices when:

- The device is in **Access Point (AP) mode**
- Wi-Fi credentials need to be entered or updated
- Device settings (SSID, location, display text, etc.) are configured

It replaces generic captive portals with a **custom UI aligned with the STACKSWORTH brand**.

---

## 📦 Repository Contents
portal_src/ └─ STACKS_WiFi_Portal.html        # Editable source HTML (human-readable)
data/ └─ STACKS_WiFi_Portal.html.gz     # Pre-compressed file for SPIFFS upload
docs/ └─ BUILD_AND_UPDATE.md            # Step-by-step build & update instructions

### Key Files
- **`portal_src/STACKS_WiFi_Portal.html`**  
  The master source file. All edits should be made here.

- **`data/STACKS_WiFi_Portal.html.gz`**  
  Gzipped version of the portal, uploaded to the ESP32’s SPIFFS filesystem.

- **`docs/BUILD_AND_UPDATE.md`**  
  Explains how to edit, gzip, and upload the portal correctly.

---

## 🚀 Serving the Portal on ESP32

This portal is intended to be served from SPIFFS using `ESPAsyncWebServer`.

### Example: Serve the Portal as the Root Page

```cpp
#include <SPIFFS.h>
#include <ESPAsyncWebServer.h>

AsyncWebServer server(80);

void setup() {
  SPIFFS.begin(true);

  // Serve the Wi-Fi portal as the homepage
  server.on("/", HTTP_GET, [](AsyncWebServerRequest *req) {
    req->send(SPIFFS, "/STACKS_Wifi_Portal.html.gz", String(), true);
  });

  server.begin();
}

void loop() {}
