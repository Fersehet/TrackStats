<h1>
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/header-dark.svg?v=4">
    <img src="assets/header-light.svg?v=4" alt="TrackStats" height="72">
  </picture>
</h1>

Small KAPPS add-on that shows live track stats in iRacing: wetness, rain, humidity, fog, wind and track usage.

<p align="center">
  <img src="assets/preview.gif?v=10" alt="TrackStats overlay running through the wetness levels, fog, wind and track usage" width="300">
</p>

## What it shows

| Stat | Details |
| --- | --- |
| **Track wetness** | Six-step bar from *Dry* to *Extremely Wet*, with a *getting wetter* / *getting drier* trend. *getting wetter* only shows while it rains |
| **Declared wet** | Small LED in front of the wetness label. It lights up blue while race control has the session declared wet (rain tyres allowed) and goes out when that ends |
| **Weather icon** | Top right: ☀️ 🌤️ ⛅ ☁️ by cloud cover, 🌧️ while it rains (5 % or more) |
| **Rain** | Current precipitation in % |
| **Humidity** | Relative humidity in % |
| **Fog** | Fog level in % |
| **Wind** | Arrow, direction word and speed in km/h, relative to your car: orange *Headwind*, green *Tailwind*, blue *Crosswind*. The arrow shows where the wind blows: up means tailwind, down means headwind |
| **Track usage** | Rubber state of the current session as reported by iRacing, with an estimated percentage (for example *Low Usage (17 %)*). For *Carry Over* it shows the usage carried over from the session before |

The overlay is designed for 260 × 222 px and scales with its window, so resizing it in KAPPS never adds scroll bars. It uses the Roboto font.

## Install

1. Download this repository (**Code → Download ZIP**), unzip it and rename the folder to `TrackStats` (GitHub adds a `-main` suffix to the ZIP folder).
2. Put the folder into your custom apps folder, for example:
   ```
   %USERPROFILE%\Documents\iRacing\CustomApps\TrackStats
   ```
3. Start **KAPPS as administrator**. Open **App → Settings → Add App Folder** and select the `CustomApps` folder.
   If *Add App Folder* is missing, copy the `TrackStats` folder to
   `%AppData%\Kapps\iRacingBrowserApps\apps\` instead.
4. Open **Racing Overlay**, scroll to the bottom and click **Add Custom Overlay**.
5. Enter a name and this URL, then tick the options you need:
   ```
   http://127.0.0.1:8182/TrackStats/
   ```

The folder name in the URL must match the folder name on disk.

Set the overlay window in KAPPS to **260 × 222 px** to show it 1:1. Any other size scales it to fit.

## Options

Add options to the end of the URL. Combine several with `&`.

| Option | Example | Effect |
| --- | --- | --- |
| `opacity` | `?opacity=70` | Background opacity, 0 to 100 (or 0 to 1). Default is solid |
| `color` | `?color=0f0f0f` | Background color as hex without `#`. Default `0f0f0f` |
| `flip` | `?flip=1` | Reverses the rotation of the wind arrow |
| `norden` | `?norden=1` | Wind arrow shows the wind direction relative to north (north is up) instead of relative to your car |
| `debug` | `?debug=1` | Shows the raw data KAPPS sends, useful when a value stays empty |

Example: `http://127.0.0.1:8182/TrackStats/?opacity=80&color=000000`

## Wetness levels

| Level | Label |
| --- | --- |
| 1 | Dry |
| 2 | Mostly Dry |
| 3 | Very Lightly Wet |
| 4 | Lightly Wet |
| 5 | Moderately Wet |
| 6 | Very Wet |
| 7 | Extremely Wet |

## Demo

Download the repository and open `demo/index.html` in any browser (GitHub only shows its source). It runs on simulated weather and has sliders for wetness, sky, rain and fog plus a *Declared wet* switch, so you can preview every state without iRacing.

## Troubleshooting

| Problem | Fix |
| --- | --- |
| Overlay is blank or shows a 404 page | The folder name in the URL must match the folder on disk (`TrackStats`, not `TrackStats-main`). Check that the `CustomApps` folder is added in KAPPS, or that the folder sits in `%AppData%\Kapps\iRacingBrowserApps\apps\` |
| Status stays *Connecting To ws://…* or *Disconnected – Retrying…* | KAPPS is not running or its local server is not reachable. Start KAPPS (as administrator) and check that the URL uses `127.0.0.1:8182` |
| Stuck on *Waiting For iRacing…* | KAPPS is connected but iRacing sends no data yet. Join a session (not just the menu) and wait a few seconds |
| A single value shows `–` | iRacing is not sending that value right now. Add `?debug=1` to the URL to see the raw data |
| Wind arrow turns the wrong way | Add `?flip=1`. Add `?norden=1` if you want north up instead of relative to your car |
| Overlay looks blurry or too small | Set the overlay window to **260 × 222 px** to show it 1:1. Other sizes are scaled to fit |
| Font looks different | Roboto loads from Google Fonts. Without internet it falls back to Segoe UI or Arial |
| Weather icons look different from the preview | The icons are emojis and use your system's emoji font, so they look slightly different on each OS |

## Good to know

- iRacing reports one wetness value for the whole track, not per section.
- Track usage is the text label iRacing provides. iRacing does not send a percentage, so the overlay estimates one by spreading the seven steps evenly: Clean 0 %, Low 17 %, Moderately Low 33 %, Moderate 50 %, Moderately High 67 %, High 83 %, Max 100 %. Long labels get a slightly smaller font so they fit on one line.
- The overlay shows *Waiting For iRacing…* until data arrives. A single value shows `–` when iRacing is not sending it, for example outside a session.

## How it works

TrackStats is a plain `index.html` with no build step. Its only external request is the Roboto font from Google Fonts; without it the overlay falls back to Segoe UI or Arial. It reads the live iRacing data from the local KAPPS server over a WebSocket and renders it in the browser source.

---

Released under the [MIT License](LICENSE).

TrackStats is an unofficial fan project and is not affiliated with iRacing.com Motorsport Simulations or KAPPS.
