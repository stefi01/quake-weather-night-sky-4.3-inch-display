# Quake desk

A 4.3 inch touchscreen desk display. Home is the USGS earthquake list for one saved place. The clock follows that place. Weather, storm radar, storm alerts, rain, the night sky, launches, and the ISS are on the same screen.

The sketch is `quake_desk.ino` in this folder.

## Board

Arduino IDE, board package ESP32 by Espressif.

| Setting | Value |
| --- | --- |
| Board | ESP32S3 Dev Module |
| PSRAM | OPI PSRAM |
| Flash Size | 16MB |
| USB CDC On Boot | Enabled |
| Partition Scheme | Default 4MB with spiffs (1.2MB APP/1.5MB SPIFFS) |

The world picture is stored in the sketch (`iss_world.h`). Together with the program it uses most of that 1.2 MB app slot. A smaller app partition will not fit. The partition above has two app slots, which Install needs.

Libraries, from the Library Manager:

- GFX Library for Arduino
- TJpg_Decoder

Wi-Fi, HTTP, Preferences, LittleFS, and FFat come with the ESP32 board package.

This board is the Freenove FNK0115B, 4.3 inch TN, 480x272. Use the Type-C port that shows up as CH340. Leave OPI PSRAM on. The bounce buffer stays 272 * 20. The touch `.h` and `.cpp` stay in this folder.

Saved settings use the name `quake`. Factory reset on this desk does not clear sky weather or the plane tracker.

## Updates

Setup, then About. The page shows `version 1.0`. **Check for updates** reads `ota/version.txt` from this GitHub repo. If that number is the same, the page says newest firmware already installed. If GitHub has a higher number, tap **Install**.

This firmware looks for:

- `https://raw.githubusercontent.com/stefi01/quake-weather-night-sky-4.3-inch-display/main/ota/version.txt`
- `https://raw.githubusercontent.com/stefi01/quake-weather-night-sky-4.3-inch-display/main/ota/quake_43.bin`

On GitHub, `README.md` and the `ota` folder sit at the root of `quake-weather-night-sky-4.3-inch-display`, branch `main`.

`ota/version.txt` says `1.0`. You build `ota/quake_43.bin` yourself. When you ship a new build, raise `SKY_VER` in the sketch, compile, copy the bin to `ota/quake_43.bin`, and put that same number in `ota/version.txt`. Upload `ota/version.txt` and `ota/quake_43.bin`. Steps are in `ota/PUBLISH.txt`. The screen never reads `PUBLISH.txt`.

## What it shows

Home lists earthquakes from the last day. The big card is the one nearest the saved place, or a felt one when the distance says so. ALSO lists more. The list holds eight. A newer quake knocks off the oldest.

Tap a quake and the stored world picture opens. Each quake is marked where it happened, with the magnitude and the local date and time. A ring marks the one you tapped. Under the map: the city or the ocean, the distance from land, the country, the nearest place, and how long ago it happened. A pink dot is the saved place.

The bottom row is the sky word, temperature, high and low, moon, tide or air, and rain for the next 6 hours. The alert bar is above the quake cards.

The foot buttons are Place, Radar, Rain, Sky, Wi-Fi, and Set.

- **Radar** has three pictures. Satellite is the overhead photo. Clouds keeps that photo and adds the bright cloud. Storm keeps that photo and adds the colored radar.
- **Rain** shows the next 12 hours. The sky chip opens the next 7 days. The air chip opens the station page. The rain chip opens the next hours.
- **Sky** opens a menu. Clock, launches, night sky, and ISS. ISS uses the same stored world picture, with the orbit path and the passes.
- **Set** has a dark or light look, lamp dim, mid, or bright, F or C, About, and factory reset.

Quakes, storm alerts, and the weather numbers refresh every minute. The radar picture stays on a slower pull. That download is large.

It needs 2.4 GHz Wi-Fi and USB power. There is no account. Place search and tide stations are for the United States. Felt here is a distance estimate, not a seismometer. A tsunami line shows only when USGS marks that flag.

## Data

Earthquakes come from the USGS FDSN feed. City, ocean, country, nearest place, and distance from land come from USGS Geoserve. Forecast and air quality come from Open-Meteo. Alerts come from weather.gov. Aurora Kp comes from NOAA SWPC. Tides come from NOAA. Place search comes from Nominatim. Satellite pictures come from Esri. Clouds come from NASA GIBS (GOES-East). Storm radar comes from Iowa State (NEXRAD). The ISS path and passes use a Celestrak element set and wheretheiss.at. Launches come from The Space Devs.

The world picture is already in the sketch, so the quake map and the ISS page do not download the map. The quake list, the place details, the ISS path, and the pass list still update over Wi-Fi.
