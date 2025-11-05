# STACKS_Wifi_Portal

A lightweight, gzipped captive-style Wi-Fi setup portal for STACKSWORTH ESP32 devices.

## What this repo contains
- `portal_src/STACKS_WiFi_Portal.html` — editable HTML source.
- `data/STACKS_Wifi_Portal.html.gz` — pre-compressed file for SPIFFS.
- `docs/BUILD_AND_UPDATE.md` — how to edit, gzip, and upload to device.

## Serve on ESP32 (AsyncWebServer + SPIFFS)
```cpp
#include <SPIFFS.h>
#include <ESPAsyncWebServer.h>
AsyncWebServer server(80);

void setup() {
  SPIFFS.begin(true);

  // Option A: serve this specific file as home
  server.on("/", HTTP_GET, [](AsyncWebServerRequest *req){
    req->send(SPIFFS, "/STACKS_Wifi_Portal.html.gz", String(), true);
  });

  // Option B: use index.html(.gz) as default
  // server.serveStatic("/", SPIFFS, "/").setDefaultFile("index.html");
  // (If you rename to index.html.gz, AsyncWebServer will serve it automatically)
  server.begin();
}

void loop() {}
