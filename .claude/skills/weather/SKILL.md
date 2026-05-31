# Weather Skill

Use this skill to fetch and display current local weather information.

## When to use

Trigger when the user says: "clima", "weather", "qué tiempo hace", "cómo está el clima", "temperatura", "pronóstico", or invokes `/weather`.

## How it works

Use `wttr.in` — a free weather service that requires no API key. Fetch via:

```
https://wttr.in/{location}?format=j1
```

If no location is given, use `wttr.in/?format=j1` (auto-detects by IP).

## Steps

1. **Determine location** — use the location the user provides, or auto-detect via IP if none given.
2. **Fetch weather** — call `https://wttr.in/{location}?format=j1` using WebFetch. The response is JSON.
3. **Parse and present** — extract and display in a clean, readable format:
   - Current temperature (°C and °F)
   - Feels like temperature
   - Weather description
   - Humidity
   - Wind speed and direction
   - Visibility
   - UV index
   - 3-day forecast (date, max/min temp, description, rain chance)
4. **Language** — respond in the same language the user used (Spanish if asked in Spanish, English if in English).

## JSON field mapping

From `wttr.in?format=j1`:

```
current_condition[0]:
  temp_C, temp_F          → current temperature
  FeelsLikeC, FeelsLikeF  → feels like
  weatherDesc[0].value    → description
  humidity                → humidity %
  windspeedKmph           → wind speed
  winddir16Point          → wind direction
  visibility              → visibility km
  uvIndex                 → UV index

weather[] (3 days):
  date                    → date
  maxtempC, mintempC      → max/min temp
  hourly[4].weatherDesc   → midday description
  hourly[4].chanceofrain  → rain chance
```

## Example output (Spanish)

```
Clima actual en Ciudad de México
──────────────────────────────
Condición:     Parcialmente nublado
Temperatura:   24°C (75°F) — Sensación: 26°C
Humedad:       65%
Viento:        15 km/h del NW
Visibilidad:   10 km
Índice UV:     6

Pronóstico 3 días
──────────────────────────────
Hoy (Vie):   Max 26°C / Min 18°C | Parcialmente nublado | Lluvia: 20%
Mañana (Sáb): Max 24°C / Min 17°C | Lluvioso            | Lluvia: 75%
Dom:          Max 22°C / Min 16°C | Nublado              | Lluvia: 40%
```

## Error handling

- If WebFetch fails or returns non-JSON, show a friendly error and suggest the user try with an explicit city name.
- If the location is ambiguous, show the location name returned by wttr.in so the user can confirm.
