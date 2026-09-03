# Site Weather for Grafana

Outdoor weather at your sites (data centers, offices, plants) over the
dashboard time range, plus the official weather warnings active right now.
Built for reading a cooling or power incident against the weather after the
fact: set the time picker to the incident window and the hourly panels follow.

A world map of your sites colored by current temperature, then per site:
current temperature, hourly temperature, apparent temperature, wind gusts,
precipitation. Plus MeteoAlarm warnings per European country and US National
Weather Service alerts per state.

No API keys. Sites are dashboard variables, so the JSON carries no location
data of yours.

## Requirements

- Grafana 10.4 or newer (Grafana Cloud works).
- The [Infinity datasource](https://grafana.com/grafana/plugins/yesoreyeram-infinity-datasource/)
  plugin, with one Infinity datasource configured. No authentication needed.
  If the datasource has an allowed-hosts list, add `api.open-meteo.com`,
  `archive-api.open-meteo.com`, `feeds.meteoalarm.org` and `api.weather.gov`.

## Import

Dashboards > New > Import > upload `dashboard.json`, pick your Infinity
datasource when asked.

## Configure your sites

Dashboard settings > Variables:

- `site`: one entry per site, `Label : latitude=LAT&longitude=LON`, comma
  separated. A row of panels repeats per selected site. It is multi-select
  without an "All" option on purpose: the map takes its marker names from the
  selection. Two hidden variables, `lats` and `lons`, are derived from it for
  the map; leave them alone.
- `meteoalarm_country`: lowercase country names as used by MeteoAlarm feeds
  (`germany`, `france`, `italy`, ...). One warnings table per country.
- `nws_states`: US state codes (`CA`, `NY`, ...) for NWS active alerts.
- `api`: `forecast` covers the last 92 days and 16 days ahead; switch to
  `archive` (ERA5, back to 1940, a few days of lag) for older windows.

Save the dashboard after editing the variables so the values persist.

## Data and licences

- [Open-Meteo](https://open-meteo.com/) for conditions. Free for
  non-commercial use; commercial use needs an
  [Open-Meteo subscription](https://open-meteo.com/en/pricing). Each dashboard
  load makes about five calls per site.
- [MeteoAlarm](https://meteoalarm.org/) for European warnings, CC BY 4.0.
- [US National Weather Service API](https://www.weather.gov/documentation/services-web-api)
  for US alerts, public domain.

## Notes

- Warnings tables are "now", not bound to the time picker.
- A country or state with no active warnings shows an empty table.
- Coordinates only need to be close: weather is regional, so the nearest town
  is fine when the exact site is private or unknown.

## Licence

MIT, see `LICENSE`. The dashboard JSON is yours to adapt.
