# Hot Water Control (Home Assistant Blueprint)

A Home Assistant automation blueprint that controls a hot water switch based on a temperature sensor, while only acting when you're home (e.g. according to a Tado presence sensor).

## What it does

- Turns hot water **ON** when the temperature drops below a minimum threshold, but only while in "Home" mode.
- Turns hot water **OFF** when the temperature rises above a maximum threshold, but only while in "Home" mode.
- Turns hot water **ON** automatically when you arrive home, *if* the temperature is currently below the maximum threshold (so it doesn't switch on if the water's already hot).
- Turns hot water **OFF** automatically when you leave home, regardless of temperature.

## Requirements

- A temperature sensor (e.g. a Shelly temperature probe).
- A switch entity controlling the hot water (immersion heater, boiler relay, etc.).
- A sensor reporting a Home/Away presence state (e.g. from the Tado integration).
- Two `input_number` helpers for the minimum and maximum temperature thresholds.

## Installation

1. Copy [`hot_water_control.yaml`](./hot_water_control.yaml) into your Home Assistant config folder at:
   ```
   config/blueprints/automation/<your_username>/hot_water_control.yaml
   ```
2. In Home Assistant, go to **Settings → Automations & Scenes → Blueprints**, and confirm the blueprint appears (or reload blueprints if it doesn't show up).
3. Click **Create Automation** from the blueprint.

Alternatively, if this repository is public, you can import it directly via **Settings → Automations & Scenes → Blueprints → Import Blueprint**, using the raw GitHub URL of `hot_water_control.yaml`.

## Configuration

| Input | Description |
|---|---|
| `temp_sensor` | Sensor reporting the current hot water temperature |
| `hot_water_switch` | Switch entity that controls the hot water |
| `mode_sensor` | Sensor reporting Home/Away presence mode |
| `home_state` | Exact state value representing "Home" (default: `Home`) |
| `away_state` | Exact state value representing "Away" (default: `Away`) |
| `hw_min_temp` | `input_number` helper holding the minimum temperature threshold |
| `hw_max_temp` | `input_number` helper holding the maximum temperature threshold |

## Notes

- The `home_state` and `away_state` values must match your sensor's actual state exactly (check under **Developer Tools → States**) — Tado integrations sometimes use lowercase `home`/`away` instead of `Home`/`Away`.
- On arrival home, the water turns on if the temperature is below `hw_max_temp` (not `hw_min_temp`) — meaning it'll turn on even if the temp is only "not yet maxed out," not strictly "too cold." Adjust this in the blueprint if you'd prefer stricter behavior.

## License

MIT (or update to your preferred license).
