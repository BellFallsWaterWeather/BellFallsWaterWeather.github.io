# BellFallsWaterWeather

Winter road weather page for a snow plow driver, centred on Chemin de Kilmar at Chemin de la Rivière-Rouge, Grenville-sur-la-Rouge, QC (lat 45.77, lon -74.615).

## Two copies, always kept in sync

- **GitHub Pages (main, public, no login):** `index.html` in this repo → https://bellfallswaterweather.github.io
- **Claude artifact:** https://claude.ai/artifact/X53bCZRPwRg4vGCNKM9cQq

**Rule from the owner:** any change made to the app in Claude must also be made here, and the reverse. Apply every edit to both copies in the same turn.

Differences between the copies are only the wrapper: `index.html` is a full document (doctype, `<head>` with viewport, home-screen metas and a small script that reloads the page after 15 minutes, but only when nobody has touched it for 5 minutes or when it comes back into view); the artifact is the same page body without that skeleton.

The live radar map (Leaflet + Environment Canada GeoMet WMS RADAR_1KM_RRAI / RADAR_1KM_RSNO, 2-hour loop, no key) and the forecast maps (Windy embed: rain, ptype, snowAccu) only run on github.io; the Claude artifact's sandbox blocks map tiles and iframes, so there the radar box shows a link to the GitHub site instead.

## Hourly forecast refresh

A Claude scheduled task ("Grenville weather refresh", hourly) fetches MET Norway Locationforecast for the location and replaces ONLY the JSON inside `<script type="application/json" id="fc">…</script>` in both copies, then pushes/publishes.

When editing the page, keep exactly one `id="fc"` JSON block and keep its data shape:
`{updatedAt, modelUpdatedAt, source, lat, lon, hours:[{t,temp,wind(m/s),dir,rh,cloud,sym,mm}], blocks:[{t,temp,wind,sym,mm}]}`.
