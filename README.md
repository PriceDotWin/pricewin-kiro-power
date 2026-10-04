# PriceWin power for Kiro

Live hotel and flight prices compared across Booking.com, Agoda, Trip.com and
Traveloka, in USD, from inside Kiro. This power is in the Agent Plugins format:

- **`plugin.json`**: the manifest;
- **`mcp.json`**: the PriceWin MCP server, `https://mcp.price.win/mcp`
  (Streamable HTTP, no account, no API key);
- **`skills/pricewin-travel-search`**: how to run a search (results arrive in
  two steps), what to ask when the trip details are incomplete, and how to read
  prices and links.

## Install

In the Kiro IDE, open the Powers panel, choose **Add Custom Power**, then
**Import power from GitHub** and paste this repository's URL.

With Kiro CLI:

```bash
git clone https://github.com/PriceDotWin/pricewin-kiro-power
kiro-cli powers install ./pricewin-kiro-power
kiro-cli --v3
```

Powers activate from keywords in your prompt, so asking about hotel or flight
prices loads PriceWin's tools. Tested with Kiro CLI 2.27.1 on its V3 engine.

Then ask, for example:

> Compare hotel prices in Da Nang for 2 adults from November 10 to 12.

## Tools

| Tool | What it does |
|---|---|
| `search_hotels_live`, `poll_search_results` | Search a city's hotels across the OTAs and OpenTravel partner hotels; results arrive in two steps |
| `search_flights_live`, `poll_flight_results` | Search one-way or per-leg round-trip fares |
| `get_ota_hotel_detail` | Rooms, live prices, facilities and reviews for one named hotel (Booking.com) |
| `get_hotel_detail`, `get_hotel_info` | Rooms and prices, or facilities and policies, of an OpenTravel partner hotel |
| `get_cancellation_policy` | Refund terms for one rate |
| `request_booking`, `check_booking_status` | Send a booking request to a partner hotel and read its status |
| `request_cancel_token`, `cancel_booking` | Cancel such a booking, in two steps |

**Booking takes no money.** `request_booking` sends the guest's name, phone and
email to the hotel, which confirms the request; no room is held and the guest
pays at the property. Cancelling needs a single-use token that is emailed to the
address on the booking.

## What the power sends, and where

The power contains no code. It points Kiro at one remote MCP server,
`https://mcp.price.win/mcp`, and a skill that explains how to use it.

- **To `mcp.price.win`**: the arguments of each tool call (city, dates, party
  size, hotel name, price range, airports, cabin), a short excerpt of the
  request used only to pick the reply language, and, for a booking request, the
  guest's name, phone number and email.
- **Onward**: only to PriceWin's own backend and to OpenTravel's API
  (`api.travelopen.ai`), which the PriceWin developer also owns. A booking
  request's name, phone and email go to the partner hotel so it can confirm.
- **Where prices come from**: Booking.com, Agoda, Traveloka, Trip.com and Google
  Flights, read from their public pages at search time. PriceWin has no
  partnership with those sites; each result links to its source.

See the [privacy policy](https://www.price.win/en/privacy-policy) and
[terms of service](https://www.price.win/en/terms-of-service).

The same server and skill are packaged for Claude Code, Codex CLI, GitHub
Copilot CLI and others at
[pricewin-agent-plugin](https://github.com/PriceDotWin/pricewin-agent-plugin).

## Support

[mcp.price.win/support](https://mcp.price.win/support) · support@price.win ·
[tool reference](https://mcp.price.win/docs)

## License

MIT
