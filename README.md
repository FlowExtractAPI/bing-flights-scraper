**[Bing Flights Scraper](https://apify.com/dz_omar/bing-flights-scraper?fpr=smcx63)** gets flight data from **Bing Flights** the way you would search it yourself: paste **Bing Flights links**, or fill in one search like Bing's search bar. You get every itinerary Bing prices — **price**, **airlines**, **stops**, **times** and **every flight leg** — plus each flight's **booking options** (every seller with its price and booking link) and Bing's **"book now or wait" price advice**, in **23 country markets**.

Perfect for **travel-deal sites and newsletters**, **fare-tracking and price-monitoring tools**, **travel agencies and corporate travel teams** and **analysts** who need Bing's flight prices as data without a Bing account or a key.

## What does Bing Flights Scraper do?

It mirrors Bing Flights: the same search bar, the same filters and sort tabs, the same booking page.

- **Paste Bing links** — any number of links copied from bing.com/travel. Each link is one search, read exactly as Bing reads it (route, dates, one way or round trip, cabin, travellers). A link to a flight you chose on Bing returns that flight with all its booking options.
- **Or fill in one search** — One way or Round trip (Bing has no multi-city), From, To, dates from a calendar or relative ("30 days" from today, ideal for schedules), cabin and travellers within Bing's own rules: at least 1 adult, at most 8 travellers, no more infants than adults.
- **Every itinerary** — total price for all travellers, airlines, stops and stop airports, travel time, local departure and arrival times, and each leg with flight number, operating airline, aircraft, airports and layover.
- **Bing's filters and sorts** — Stops (Non-stop, 1, 2+), Airlines, Departs and Arrives (early morning, morning, afternoon, night) for the outbound and the return, and a price limit, with Bing's own logic; Best, Cheapest, Quickest, and the four "Sort by time" orders.
- **Booking options** — the sellers on a flight's Bing booking page, with price, rating, booking link, change rule and fare details (cancellation, cabin and checked bags).
- **Price advice** — Bing's "Book now / Wait" advice for the route, with its confidence.
- **Explore** — leave To empty (or write Anywhere) for Bing's Explore: where you can fly from your airport, cheapest, quickest or most popular first.
- **Price calendar** — optionally, the lowest one-way price for each of the next ~80 days on every searched route.
- **23 markets** — each prices in its own currency; every label is in English whatever the market.

### What data can you extract from Bing Flights?

| Result type | Fields |
|---|---|
| Flight (`flight`) | `price`, `currency`, `priceSource`, `tripType`, `origin`, `destination`, `departureDate`, `returnDate`, `cabin`, `travellers`, `market`, `outbound` and `inbound` (`stops`, `durationMinutes`, `departureTime`, `arrivalTime`, `airlines`, `carrierCodes`, `stopAirports`, `legs`), `bookingOptions`, `priceAdvice`, `priceTrend`, `priceAdviceConfidence`, `bingRecommendedScore`, `itinerariesFound`, `searchComplete`, `inputUrl`, `source_url` |
| Flight leg (inside `legs`) | `flightNumber`, `carrierCode`, `carrierName`, `operatedByName`, `aircraft`, `durationMinutes`, `departure` and `arrival` (`airportCode`, `airportName`, `city`, `country`, `time`), `layoverMinutesAfter`, `layoverNote` |
| Booking option (inside `bookingOptions`) | `seller`, `sellerId`, `price`, `currency`, `sellerRating`, `bookingUrl`, `changeRule`, `fare` (`advanceChange`, `cancellation`, `seatPreReservation`, `upgradeEligibility`, `cabinBaggage`, `checkedBaggage`) |
| Explore destination (`destination`) | `destination`, `destinationCity`, `destinationAirport`, coordinates, `departureDate`, `returnDate`, `price`, `currency`, `priceLabel`, `stops`, `durationMinutes`, `airline`, `popularityScore`, `destinationCategories`, `lists` |
| Price calendar day (`price_day`) | `date`, `price`, `currency`, `priceLevel`, `lastUpdated`, `origin`, `destination` |

The price is Bing's total for **all the travellers**, and for a round trip it is the total for both directions. Values are delivered as Bing gives them — nothing is converted or rounded — and only display duplicates, logos and tracking fields are left out. Every row carries `source_url` (the same search or flight on Bing), `inputUrl` (the link it came from), `scrapedAt` and `_source`.

### Why use Bing Flights Scraper on Apify?

Running it on Apify gives you **API access**, **scheduling**, **monitoring**, **integrations** (Make, Zapier, n8n, webhooks) and **proxy rotation** without managing infrastructure. A one-way search takes about 10-30 seconds and a round trip about 20-45 seconds; several searches run at once, and booking options add a few seconds per flight.

## How to scrape Bing Flights

1. Search on [Bing Flights](https://www.bing.com/travel/flights) and copy the address of the results page (or of a flight's booking page) into **Bing Flights links** — or fill in **From**, **To** and the **Departure date** in Option B.
2. Choose the **Market** (country and currency).
3. Optionally tick Bing's filters and choose **Sort by**.
4. Start small: **Max results per search** `10`. Each link is one search and Option B is one more, so 4 links + Option B with 10 returns up to 50 rows.
5. Click **Start**, then download the results as JSON, CSV or Excel, or fetch them over the API.

## How much does it cost to scrape Bing Flights?

You pay for what you receive, in two parts. Each **search** is charged once, and only when Bing returns something for it; each **row** you receive (a flight, an Explore destination or a calendar day) is charged as a result. Booking options and price advice are included in the flight row. Rows removed by your filters are never charged.

| Event | FREE | BRONZE | SILVER | GOLD |
|---|---|---|---|---|
| Search completed (one link, or the Option B search) | $0.0050 | $0.0040 | $0.0030 | $0.0025 |
| Result returned (each flight, destination or calendar day) | $0.0020 | $0.0015 | $0.0012 | $0.0010 |

- One search with 20 flights costs about **$0.045** at the highest tier price and **$0.022** on GOLD.
- A price check of one flight per link costs about **$0.007** at the highest tier price and **$0.004** on GOLD.
- A search that returns nothing, an unknown airport or place, and every notice row are **never charged**.
- Test with a low **Max results per search** first, and set a maximum cost per run in Apify: the Actor sizes the run to fit it.

## Input and output examples

### Input

```json
{
    "searchUrls": [
        { "url": "https://www.bing.com/travel/flight-search?q=flights&src=LHR&des=JFK&ddate=2027-01-12&isr=0&cls=0&adult=1&child=0&infant=0" }
    ],
    "tripType": "round_trip",
    "origin": "London",
    "destination": "Paris",
    "departureDate": "30 days",
    "returnDate": "35 days",
    "adults": 1,
    "cabin": "economy",
    "market": "gb",
    "stops": ["nonstop"],
    "departs": ["morning", "afternoon"],
    "sortBy": "cheapest",
    "maxResults": 20,
    "bookingOptionsTop": 3
}
```

| Input value | What it does |
|---|---|
| `searchUrls` | Bing Flights links, one search each. A link with `sel=` (a flight you chose) returns that flight with its booking options. |
| `tripType`, `origin`, `destination`, `departureDate`, `returnDate` | One more search, like Bing's search bar. Dates are `YYYY-MM-DD` or a time from today (`30 days`, `2 weeks`). Empty `destination` = Explore. |
| `adults`, `children`, `infants`, `cabin` | Travellers (children 2-12, infants under 2; at most 8; no more infants than adults) and cabin, for the Option B search. Links carry their own. |
| `market` | Country and currency for Option B and for links that do not name one. |
| `stops`, `airlines`, `departs`, `arrives`, `returnDeparts`, `returnArrives`, `maxPrice` | Bing's filters. Several choices in one filter match any of them; different filters all have to match. |
| `sortBy`, `maxResults` | Bing's sorts and the rows kept per search (`0` = everything). |
| `bookingOptionsTop`, `priceCalendar` | Booking options for the first N flights of each search; Bing's price calendar for each route. |

### Output

A real result from a pasted link (London to New York, 2 adults, booking options on): the cheapest seller is a little above the search price because Bing re-checks prices on the booking page. Only the booking link and the list of sellers are shortened.

```json
{
    "resultType": "flight",
    "position": 1,
    "market": "us",
    "tripType": "OneWay",
    "origin": "LHR",
    "originName": "London Heathrow Airport",
    "destination": "JFK",
    "destinationName": "John F. Kennedy International Airport",
    "departureDate": "2027-01-12",
    "returnDate": null,
    "cabin": "economy",
    "travellers": {
        "adults": 2,
        "children": 0,
        "infants": 0
    },
    "price": 886.46,
    "currency": "USD",
    "priceSource": "SkyScanner_US",
    "outbound": {
        "stops": 0,
        "durationMinutes": 505,
        "departureTime": "2027-01-12T07:25:00",
        "arrivalTime": "2027-01-12T10:50:00",
        "departureAirport": "LHR",
        "arrivalAirport": "JFK",
        "airlines": [
            "JetBlue Airways"
        ],
        "carrierCodes": [
            "B6"
        ],
        "stopAirports": [],
        "legs": [
            {
                "flightNumber": "B6 2220",
                "carrierCode": "B6",
                "carrierName": "JetBlue Airways",
                "operatedByCode": "B6",
                "operatedByName": "jetBlue",
                "aircraft": null,
                "durationMinutes": 505,
                "departure": {
                    "airportCode": "LHR",
                    "airportName": "London Heathrow Airport",
                    "city": "London",
                    "country": "United Kingdom",
                    "time": "2027-01-12T07:25:00"
                },
                "arrival": {
                    "airportCode": "JFK",
                    "airportName": "John F. Kennedy International Airport",
                    "city": "New York",
                    "country": "United States",
                    "time": "2027-01-12T10:50:00"
                },
                "layoverMinutesAfter": 0
            }
        ],
        "resultId": "LHR,JFK,07:25:00,B6,2220,0|"
    },
    "inbound": null,
    "bookingOptions": [
        {
            "seller": "Super.com",
            "sellerId": "spus",
            "price": 891.56,
            "currency": "USD",
            "sellerRating": 4.11,
            "bookingUrl": "https://agw.skyscnr.com/v1/redirect?pageUrl=...",
            "changeRule": "Unchangeable",
            "fare": {
                "advanceChange": "ASSESSMENT_UNAVAILABLE",
                "cancellation": "ASSESSMENT_UNAVAILABLE",
                "seatPreReservation": "ASSESSMENT_UNKNOWN",
                "upgradeEligibility": "ASSESSMENT_UNKNOWN",
                "cabinBaggage": {
                    "assessment": "ASSESSMENT_UNAVAILABLE",
                    "pieces": null,
                    "weight": null,
                    "priceAmount": 0.0,
                    "priceUnit": "PRICE_UNIT_MILLI"
                },
                "checkedBaggage": {
                    "assessment": "ASSESSMENT_UNAVAILABLE",
                    "pieces": null,
                    "weight": null,
                    "priceAmount": 0.0,
                    "priceUnit": "PRICE_UNIT_MILLI"
                }
            }
        }
    ],
    "priceAdvice": "Buy",
    "priceTrend": "Constant",
    "priceAdviceConfidence": 66.0,
    "itinerariesFound": 484,
    "searchComplete": true,
    "inputUrl": "https://www.bing.com/travel/flight-search?q=flights&src=LHR&des=JFK&ddate=2027-01-12&isr=0&rdate=&cls=0&adult=2&child=0&infant=0",
    "source_url": "https://www.bing.com/travel/flight-search?q=flights&src=LHR&des=JFK&ddate=2027-01-12&isr=0&rdate=&cls=0&adult=2&child=0&infant=0&cc=us&setmkt=en-US&setlang=en-US",
    "scrapedAt": "2026-11-09T11:38:15Z"
}
```

An Explore row, a price-calendar row and a "no flights" notice look like this:

```json
{ "resultType": "destination", "destination": "Dublin, Ireland", "destinationAirport": "DUB", "origin": "LHR", "departureDate": "2026-11-12", "returnDate": "2026-11-20", "price": 78, "currency": "GBP", "stops": "Non-stop", "durationMinutes": 75, "airline": "British Airways", "lists": ["popular", "cheapest", "quickest"] }
{ "resultType": "price_day", "origin": "LHR", "destination": "JFK", "date": "2026-12-15", "price": 354.0, "currency": "USD", "priceLevel": "Low", "lastUpdated": "2026-11-09T00:31:00" }
{ "resultType": "notice", "status": "no_results", "origin": "oag", "destination": "mqn", "message": "Bing found no flights for OAG to MQN 2026-12-14 [us]. It suggests a nearby airport instead: Bodo (BOO), 136 miles away.", "alternatives": [{ "destination": { "code": "BOO", "name": "Bodo Airport", "city": "Bodo", "country": "Norway" }, "distanceFromDestination": "136 miles" }] }
```

## Other Actors you might need

| Actor | What it adds |
|---|---|
| **[Google Flights Scraper](https://apify.com/dz_omar/google-flights-scraper?fpr=smcx63)** | The same kind of search on Google Flights: prices, airlines, stops and booking options. |
| **[Bing Images Scraper](https://apify.com/dz_omar/bing-images-scraper?fpr=smcx63)** | Start from a keyword: thousands of Bing images per search, with filters. |
| **[Bing Reverse Image Search](https://apify.com/dz_omar/bing-reverse-image-search?fpr=smcx63)** | Start from a picture: where it appears and what looks like it. |

## Frequently asked questions

### Is it legal to scrape Bing Flights?

The Actor collects publicly available flight search results, the same ones anyone sees in a browser, and respects the source's limits. Prices and availability belong to the airlines and booking partners, so check their terms and your local rules before reusing them commercially. This is not legal advice.

### Do I need a Bing or Microsoft account or an API key?

No. It works from links or airports and dates alone — no Bing or Microsoft account, no key and no API subscription.

### Which Bing links can I paste?

Any results page from bing.com/travel/flight-search (after you search a route on Bing), the booking page of a flight you chose (its address has `sel=`), or the Flights home page with an origin (Explore). The search text in the link can be in any language. A round-trip link must have both dates; Bing treats a link with `isr=0` as one-way even if it has a return date, and so does the Actor.

### How many rows will a run return?

Up to **Max results per search** for each search: each link is one search and Option B is one more. 4 links + Option B with 11 returns up to 55 rows (fewer when Bing has fewer flights or your filters remove some).

### Is the price per person?

No. It is Bing's **total for all travellers**, and for a round trip the total for both directions — the same number Bing shows.

### What does "Best" mean?

Bing's own recommendation, the default tab on its page: a balance of price, duration and stops that Bing scores itself. Choose **Cheapest** or **Quickest** for those orders, or one of the time orders.

### Which airlines can I filter by?

The airlines Bing lists for the route, by the name Bing shows (British Airways, easyJet, JetBlue Airways) or by their two-character code (BA, U2, B6). A flight is kept when any of its airlines is listed, so flights that mix airlines are kept too.

### Why did Explore return nothing for my airport?

Bing keeps Explore destinations ready for some airports in some markets only; a market without them gives a notice that says so. Try another market (for example the United States or the United Kingdom) or a larger nearby airport.

### Why does the price calendar not follow my cabin or travellers?

Bing publishes calendar prices for **one adult in Economy** only, for roughly the next 80 days.

### Can I use Bing Flights Scraper in Make, Zapier, n8n or my own code?

Yes. Use Apify's integrations, webhooks, the API or the client libraries for JavaScript and Python.

### What happens if a run is interrupted?

The Actor saves its progress and picks up where it left off, without repeating or re-charging rows it has already delivered.

### Why did I get fewer results than I asked for, or a notice instead?

| Situation | What you see | What to do |
|---|---|---|
| Bing found no flights for the route | One notice row (`no_results`, not charged), with Bing's nearby-airport suggestions in `alternatives` when it has some | Try the suggested airport, or other dates |
| The flight in your link is no longer offered | A notice with `flight_not_found` (not charged) | Choose the flight again on Bing and copy the new link |
| A place name matched no airport | A notice with `place_not_found` (not charged) | Use an airport code or a more specific name |
| Your filters removed everything | A notice with `no_results` (not charged) | Loosen the filters |
| Bing was still adding results when the search ended | The rows carry `searchComplete: false` | Run again; Bing may know more flights |
| The input cannot be used (a link without a route, a date in the past, more infants than adults) | One guidance row (`bad_input`) saying what to change | Fix that field |
| Bing did not answer | A notice with `bing_unavailable` (not charged); the search is kept for a resume | Run it again later |
| Your maximum cost per run was reached | The run stops after the last row it charged for | Raise the maximum cost per run |

## Support

- 🌐 **Website:** [flowextractapi.com](https://flowextractapi.com)
- 📧 **Email:** [flowextractapi@outlook.com](mailto:flowextractapi@outlook.com)
- 💬 **GitHub:** [FlowExtractAPI](https://github.com/FlowExtractAPI)
- 💼 **LinkedIn:** [flowextract-api](https://www.linkedin.com/in/flowextract-api/)
- 🐦 **X (Twitter):** [@FlowExtractAPI](https://x.com/FlowExtractAPI)
- 📱 **Facebook:** [flowextractapi](https://www.facebook.com/flowextractapi)
- 🎵 **TikTok:** [@flowextractapi](https://www.tiktok.com/@flowextractapi)

## Legal & compliance

- **Public data only** — processes publicly available data.
- **Rate limiting** — respects source-service limits and terms of use.
- **Data protection** — no storage of personal information or unauthorized data collection.
- **Commercial use** — suitable for business intelligence and research.
- No affiliation with or endorsement by Microsoft or Bing is implied.

---

*Bing Flights Scraper — by FlowExtract API. Turn any website into structured data.*
