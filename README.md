# Flight Deal Search

A small Python tool that asks where you are and where you want to go, finds the cheapest
flight between the two on the Amadeus API, and emails you the deal.

## How it works

```
main.py
  ├─ FlightData.get_city_code("hail")   → resolves a city name to an IATA code (HAS)
  ├─ FlightData._get_auth_token()       → OAuth2 client-credentials bearer token
  ├─ FlightSearch(...)                  → GET /v2/shopping/flight-offers
  │    ├─ get_cheapest_flight()         → lowest grandTotal across all offers
  │    ├─ get_currency()                → currency of the winning offer
  │    └─ get_airline_name()            → carrier codes mapped to readable names
  └─ NotificationManager(...)           → sends the result over Gmail SMTP
```

| File | What it does |
|---|---|
| `main.py` | CLI entry point — prompts for cities, wires the pieces together |
| `flight_data.py` | Amadeus authentication and city-name → IATA-code lookup |
| `flight_search.py` | Queries flight offers, picks the cheapest, extracts carriers and currency |
| `notification_manager.py` | Formats the deal and emails it via Gmail SMTP |

## Usage

```bash
python main.py
```

```
where are you now?: riyadh
where would you like to go?: london
```

If an offer is found, the cheapest one is emailed to you:

> **Subject: Low Price Alert!!**
> Only 1840.50 SAR to fly from RUH to LON, on 2026-05-23 …, in these airlines: Saudia
