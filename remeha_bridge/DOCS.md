# Remeha Bridge

Reads your boiler and thermostat from the Remeha Home cloud every minute and
publishes them to Home Assistant via MQTT discovery, as one device
"Remeha". Read-only: the app never sends a command to the boiler.

## Configuration

- **email**, **password:** your Remeha Home account.
- **poll_interval_s:** seconds between polls, 30 to 3600 (default 60).
- **timezone:** your time zone; decides which day "today" is for the
  consumption and boiler log sensors (default `Europe/Amsterdam`).
- **model_name:** the model shown on the device (default `Tzerra Ace`).
- **log_level:** `error`, `warn`, `info` or `debug` (default `info`).
- **mqtt:** your broker; the defaults fit the Mosquitto app. Fill in the
  username and password of an MQTT user.
- **api_client_id**, **api_subscription_key:** public values of the Remeha
  Home app, built in. Hidden; only needed if a new Remeha app version
  changes them: turn on "Show unused optional configuration options" and
  fill in the new value. Empty means the built-in value.

A wrong or missing value stops the app at startup; the log names the option.

## When something is wrong

- **login status** is not `ok`: with `login_failed` the log says why. If
  Remeha refused the e-mail address or password, correct them and restart
  the app – it never retries a refused password. `cloud_unreachable`
  resolves itself.
- **cloud connected** is off: Remeha did not answer for 5 minutes. The
  boiler entities show "unavailable" until it answers again; nothing to do.
- **Consumption today** shows 0 or unknown between midnight and 01:00
  (02:00 in summer): Remeha counts per UTC date.
- **Heat demand / hot water time and starts today** are too low after the
  app restarted: they start again at 0 and show today's value only from
  midnight on. The daily totals in Home Assistant's statistics stay right.

## Your data

The app keeps one file: its login token in `/data/token.json`. Deleting it
(by reinstalling the app) only causes one new login. The options and the
token are part of your Home Assistant backups: encrypt your backups and use
a Remeha password you use nowhere else.
