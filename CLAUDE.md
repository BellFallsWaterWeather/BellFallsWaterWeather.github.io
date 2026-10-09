# BellFallsWaterWeather

Winter road weather page for a snow plow driver, centred on Hydro Hill (510 Chemin Kilmar), Grenville-sur-la-Rouge; the page calls the spot "Hydro Hill", QC (lat 45.6804, lon -74.6551).

## Two copies, always kept in sync

- **GitHub Pages (main, public, no login):** `index.html` in this repo → https://bellfallswaterweather.github.io
- **Claude artifact:** https://claude.ai/artifact/X53bCZRPwRg4vGCNKM9cQq

**Rule from the owner:** any change made to the app in Claude must also be made here, and the reverse. Apply every edit to both copies in the same turn.

Differences between the copies are only the wrapper: `index.html` is a full document (doctype, `<head>` with viewport, home-screen metas and a small script that reloads the page after 15 minutes, but only when nobody has touched it for 5 minutes or when it comes back into view); the artifact is the same page body without that skeleton.

Radar tab: Leaflet map (Esri light/dark grey canvas base, OSM fallback) with Environment Canada GeoMet radar RADAR_1KM_RRAI / RADAR_1KM_RSNO, 2-hour loop, 6-minute frames, no key. Forecast tab: Windy embed, overlay rain, product ecmwf. Owner wants rain/snow only, no wind or other layers. Maps only run on github.io; the Claude artifact sandbox blocks map tiles and iframes, so there the radar box links to the GitHub site.

## Live forecast (every 10 minutes)

On page load and every 10 minutes (and when the page comes back into view), the page fetches a live forecast straight from Open-Meteo (`models=gem_seamless`, Environment Canada GEM) for the location and converts it to the same data shape. The Claude artifact's sandbox blocks that fetch, so it always shows the saved copy.

Official Environment Canada alerts are fetched the same way (api.weather.gc.ca weather-alerts, bbox around the spot) and shown above the road call.

## Hourly forecast refresh (backup copy)

A Claude scheduled task ("Grenville weather refresh", hourly) fetches MET Norway Locationforecast for the location and replaces ONLY the JSON inside `<script type="application/json" id="fc">…</script>` in both copies, then pushes/publishes.

When editing the page, keep exactly one `id="fc"` JSON block and keep its data shape:
`{updatedAt, modelUpdatedAt, source, lat, lon, hours:[{t,temp,wind(m/s),dir,rh,cloud,sym,mm}], blocks:[{t,temp,wind,sym,mm}]}`.

## Design

Owner wants it sleek and simple: vital weather only (no road/511 links, no extra radar links). Section order: warnings, road call, conditions now, next 24 hours (summary, worst-time tiles, periods), radar, hourly 48 h, 7-day.
