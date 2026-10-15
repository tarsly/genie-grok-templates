---
name: bizops-travel-plan
description: >-
  Plan trips — flights, hotel recommendations, daily itinerary, ground transportation, restaurant suggestions, packing checklist, pre-trip timeline, conference travel, business trips, road trips, or needs to plan flights, hotel options, a daily itinerary, or a packing checklist for an upcoming trip.
---

# Travel Planner

## Overview

Full trip logistics planned like a $180K/year executive assistant handles it. The user provides a destination, dates, and purpose — the AI builds everything: flight search criteria with optimal timing and airline recommendations, hotel recommendations organized by neighborhood with pros/cons for the trip's purpose, ground transportation plan (rideshare, rental car, public transit), a day-by-day itinerary with time blocks, restaurant suggestions near meeting locations and the hotel, a packing checklist customized to the trip, and a pre-trip preparation timeline so nothing gets forgotten. The output is a single trip brief document you can pull up on your phone the morning of departure and have everything you need.

## When This Skill Applies

- User says "I'm traveling to {city}" or "plan my trip to {destination}"
- User mentions an upcoming conference, event, or business meeting in another city
- User asks for hotel recommendations or flight search help
- User wants a trip itinerary or daily schedule
- User asks for a packing list or travel checklist
- User says "I have a trip to {place} on {date}" and wants logistics planned
- User asks "what do I need to prepare for my trip?"
- User wants restaurant recommendations near a specific location
- User asks for ground transportation options in a city
- User mentions a road trip and wants route/stop planning

## Pre-Flight — Travel Preferences

1. Search Cloud Brain: `search_notes` with query `"bizops travel preferences"` (folder: `brain/preferences`)
2. **If found:** Display as a table, confirm, then proceed
3. **If not found:** Ask in ONE message:
   - Home city / departure city (used for all flight searches)
   - Seat preference (aisle / window / no preference)
   - Hotel style (boutique / chain / budget / no preference)
   - Rental car preference (yes usually / only if needed / never)
   - Dietary notes for restaurant recommendations (if any)
   - Save to Cloud Brain: `write_note` → title: `bizops-travel-preferences`, folder: `brain/preferences`
4. Apply throughout: use departure city for flight searches, seat/hotel/car preferences throughout the plan
5. Show banner at top of every output:
   ```
   ✈️ Travel Planner | Departure: {City} | Preferences: ✓ loaded
   ```
   Add: *"Say 'update my travel preferences' to change settings."*

## How It Works

### Step 0: Gather Trip Details

Parse what the user provided. Required inputs (ask for anything missing):

| Input | Required | Example |
|-------|----------|---------|
| Destination | Yes | "Los Angeles", "NYC", "Austin, TX" |
| Dates | Yes | "March 4-8", "next Tuesday through Friday", "3 nights" |
| Purpose | Yes | "Business — {YOUR_EVENT} conference", "visiting a property", "vacation" |
| Departure city | Needed for flights | "Phoenix", infer from user profile if available |
| Budget range | Helpful | "mid-range", "$200/night max for hotel", "no budget constraint" |
| Specific meetings/events | Helpful | "Meeting at 123 Main St at 2pm Tuesday", "Conference at Convention Center all day Wednesday" |
| Travel preferences | Helpful | "I prefer non-stop flights", "need a rental car", "boutique hotels over chains" |
| Number of travelers | Helpful | "just me", "me and a partner", "team of 4" |

If the user gives minimal info (just destination and dates), proceed with reasonable defaults and note assumptions.

### Step 1: Research the Destination

Use WebSearch to gather:

**Search queries to run:**
- `"{destination}" weather {month} {year}` — packing and wardrobe decisions
- `"{destination}" best neighborhoods to stay {purpose}` — hotel area recommendations
- `"{destination}" {purpose-related keyword} {year}` — event/conference venue info
- `"{destination}" airport transportation options` — ground transit intel
- `"{destination}" best restaurants near {meeting area or downtown}` — dining recs
- `"flights to {destination} from {departure city}" {month} {year}` — flight intel

### Step 2: Build Flight Search Criteria

```markdown
## Flight Search

### Recommended Search Parameters
| Parameter | Recommendation |
|-----------|---------------|
| Route | {departure} → {destination airport code} |
| Outbound | {date} — depart {morning/afternoon/evening} |
| Return | {date} — depart {morning/afternoon/evening} |
| Airlines to check | {top 3 airlines for this route with reasoning} |
| Class | {economy/business based on trip duration and purpose} |
| Booking platforms | Google Flights, Kayak, direct airline sites |

### Flight Tips for This Route
- {Best day to book, typical price range}
- {Direct flight availability or connection info}
- {Airport terminal/gate tips if relevant}
- {TSA PreCheck / CLEAR availability}

### Alternative Options
- {If driving is viable: drive time, cost comparison}
- {If train is viable: Amtrak/regional options}
```

If the user's departure city is unknown, skip the flight section and note: "I don't know your departure city. Tell me where you're flying from and I'll add flight recommendations."

### Step 3: Hotel Recommendations

Recommend 3 hotels in different categories, organized by relevance to the trip's purpose:

```markdown
## Hotel Recommendations

### Where to Stay
**Best area for your trip:** {neighborhood/area} — {why: close to venue, walkable, safe, good restaurants}

### Option 1: Best for {primary criterion — location/value/experience}
- **Name:** {hotel name}
- **Area:** {neighborhood}
- **Why:** {specific reason this fits the trip}
- **Price Range:** ${X}/night (estimated)
- **Distance to {venue/meetings}:** {X min by car/walk}
- **Standout Feature:** {what makes it worth it}

### Option 2: Best for {second criterion}
- (same format)

### Option 3: Best for {third criterion — budget/luxury/convenience}
- (same format)

### Booking Tips
- {Best platform to book for this hotel/area}
- {Check for corporate/event rates if business trip}
- {Cancellation policy advice}
```

### Step 4: Ground Transportation Plan

```markdown
## Ground Transportation

### Airport to Hotel
| Option | Cost | Time | Best For |
|--------|------|------|---------|
| Rideshare (Uber/Lyft) | ${X} est. | {X} min | Convenience |
| Taxi | ${X} est. | {X} min | Availability |
| Rental car | ${X}/day | {X} min | Freedom/multiple stops |
| Hotel shuttle | {Free/$X} | {X} min | Budget |
| Public transit | ${X} | {X} min | Cost savings |

### Getting Around During the Trip
**Recommended:** {rideshare / rental car / public transit / walking — with reasoning}

{If rental car is recommended:}
- **Pick-up:** {airport or off-site location}
- **Best rental companies for this market:** {names}
- **Parking at hotel:** ${X}/night — {included or extra}
- **Parking at venue/meetings:** {availability and cost}

{If public transit is viable:}
- **Transit system:** {name, e.g., LA Metro, NYC Subway}
- **Relevant lines/routes:** {specific routes from hotel to venues}
- **Day pass cost:** ${X}
```

### Step 5: Daily Itinerary

Build a day-by-day schedule with time blocks:

```markdown
## Daily Itinerary

### Day 1 — {Date} ({Day of Week}) — Travel Day
| Time | Activity | Location | Notes |
|------|----------|----------|-------|
| {time} | Depart for airport | Home | {leave by X for flight at Y} |
| {time} | Flight departs | {airport} | {flight details if known} |
| {time} | Arrive {destination} | {airport} | {terminal info} |
| {time} | Ground transport to hotel | | {ride option, est. cost} |
| {time} | Check in | {hotel} | {check-in time, early check-in tips} |
| {time} | Dinner | {restaurant recommendation} | {walking distance from hotel, cuisine, price range} |

### Day 2 — {Date} ({Day of Week}) — {Theme: e.g., "Conference Day 1"}
| Time | Activity | Location | Notes |
|------|----------|----------|-------|
| {time} | Wake up / gym | {hotel or nearby gym} | |
| {time} | Breakfast | {restaurant near hotel} | |
| {time} | {Meeting/event} | {venue address} | {transport: X min by car} |
| {time} | Lunch | {restaurant near venue} | {cuisine, price range} |
| {time} | {Afternoon activity} | {location} | |
| {time} | Free time / networking | | |
| {time} | Dinner | {restaurant} | {recommendation and why} |

### Day 3 — {Date} ({Day of Week})...

### Day {N} — {Date} ({Day of Week}) — Departure Day
| Time | Activity | Location | Notes |
|------|----------|----------|-------|
| {time} | Check out | {hotel} | {late checkout request tip} |
| {time} | {Morning activity if time allows} | | |
| {time} | Head to airport | | {leave by X for Y flight} |
| {time} | Flight departs | {airport} | |
```

For business trips: block meeting times first, fill around them.
For conferences: include session recommendations, networking events, after-parties.
For vacation: balance scheduled activities with free time.

### Step 6: Restaurant Suggestions

```markdown
## Restaurant Suggestions

### Near Hotel ({area})
| Restaurant | Cuisine | Price | Best For | Distance |
|-----------|---------|-------|---------|----------|
| {name} | {type} | {$/$$/$$$/$$$$} | {breakfast/lunch/dinner/drinks} | {walk/drive time} |

### Near Venue/Meetings ({area})
| Restaurant | Cuisine | Price | Best For | Distance |
|-----------|---------|-------|---------|----------|

### Special Occasion / Impress a Client
| Restaurant | Cuisine | Price | Why | Reservation Needed? |
|-----------|---------|-------|-----|-------------------|

### Quick Bites / Grab-and-Go
| Spot | What to Get | Price | Near |
|------|-----------|-------|------|
```

### Step 7: Packing Checklist

Customized to destination weather, trip purpose, and duration:

```markdown
## Packing Checklist

### Essentials
- [ ] Passport / ID
- [ ] Phone + charger
- [ ] Laptop + charger (if business)
- [ ] Wallet / cash / cards
- [ ] Travel insurance docs (if applicable)
- [ ] Boarding pass (digital or printed)
- [ ] Hotel confirmation

### Clothing ({weather}: {temp range}, {conditions})
- [ ] {X} days of {business casual / formal / casual} outfits
- [ ] {Outerwear recommendation based on weather}
- [ ] {Shoes recommendation: walking shoes, dress shoes, etc.}
- [ ] {Weather-specific: umbrella, sunscreen, sunglasses, layers}
- [ ] Gym clothes (if user has fitness routine)
- [ ] {Event-specific: suit for gala, swimsuit for pool, etc.}

### Tech
- [ ] Portable battery pack
- [ ] Headphones / AirPods
- [ ] Adapter / international converter (if needed)
- [ ] Hotspot (if unreliable venue WiFi expected)

### Business (if applicable)
- [ ] Business cards
- [ ] Presentation materials / USB backup
- [ ] Notebook / pen
- [ ] {Event-specific materials: badge, tickets, printed agenda}

### Personal
- [ ] Toiletries (or plan to buy at destination)
- [ ] Medications
- [ ] Snacks for travel day
- [ ] {Anything specific to the user's preferences from their brain profile}
```

### Step 8: Pre-Trip Preparation Timeline

```markdown
## Pre-Trip Preparation

### 1 Week Before ({date})
- [ ] Book flights (if not booked)
- [ ] Book hotel (if not booked)
- [ ] Reserve rental car (if needed)
- [ ] Confirm meeting times and locations
- [ ] Research attendees / prepare networking intel (run `/networking-intel` if applicable)
- [ ] Set out-of-office / auto-reply
- [ ] Share itinerary with {assistant / partner / team}

### 3 Days Before ({date})
- [ ] Check flight status (delays, gate changes)
- [ ] Download offline maps for {destination}
- [ ] Charge all devices
- [ ] Confirm hotel reservation
- [ ] Check weather forecast and adjust packing
- [ ] Notify bank of travel (avoid card blocks)

### Day Before ({date})
- [ ] Pack (use checklist above)
- [ ] Print or save boarding pass
- [ ] Set alarm for departure
- [ ] Confirm ground transportation
- [ ] Check in online (if airline allows 24hr check-in)

### Day Of
- [ ] Grab bag, ID, phone, charger
- [ ] Leave for airport by {time} (allow {X} hours for security + buffer)
- [ ] Text arrival update to {relevant person}
```

### Step 9: Compile and Save the Trip Brief

Assemble everything into a single document:

```markdown
# Trip Brief: {Destination} — {Dates}

> **Purpose:** {purpose}
> **Traveler:** {name, from user profile if available}
> **Created:** {today's date}

---

{All sections from Steps 2-8 compiled in order}

---

## Emergency Info
- **Hotel address:** {full address}
- **Hotel phone:** {number}
- **Local emergency:** 911 (US) / {local number if international}
- **Nearest hospital:** {name and address near hotel}
- **Embassy/consulate:** {if international travel}

---

*Trip brief generated {date} by the provider Platform — Travel Planner.*
```

Save to Cloud Brain using `write_note` (project: "brain", folder: "research", title: "travel-{destination-slug}-{YYYY-MM-DD}")

## Example Usage

**User:** "I'm going to LA March 4-8 for the {YOUR_EVENT} conference"

**AI:** Researches LA weather for March, the {YOUR_EVENT} venue and schedule, hotel options near the event, ground transportation from the nearest airport. Builds a full trip brief: flight search criteria from the user's home city, 3 hotel options ranked by proximity to the venue, daily itinerary blocking conference sessions and networking events, restaurant picks near the venue and hotel, LA-appropriate packing list, and a pre-trip timeline starting one week out. Saves to Cloud Brain (project: "brain", folder: "research", title: "travel-los-angeles-2026-03-03").

**User:** "/travel-plan Austin March 15-17 meeting with investors"

**AI:** Business trip plan. Hotels near the meeting location (asks for address if not provided). Restaurants suitable for client dinners. Professional packing list. Itinerary blocks meeting times and builds around them with arrival buffer and decompression time.

**User:** "I need to plan a road trip from Phoenix to San Diego this weekend"

**AI:** Skips flight section. Builds a driving plan: optimal route, drive time, gas stops, rest stops. Hotel recommendations in San Diego for the weekend. Daily itinerary with a mix of the user's purpose and recommended activities. Packing checklist for a driving trip.

**User:** "What should I pack for NYC in January?"

**AI:** Focused response on just the packing checklist. Researches NYC weather for January (cold, possible snow). Builds a detailed packing list with layers, waterproof boots, thermal base layers, and tech items for cold-weather travel.

**User:** "Where should I eat near the Convention Center in Austin?"

**AI:** Focused response on restaurant recommendations only. Researches restaurants within walking distance and a short drive of the Austin Convention Center. Categorizes by meal type and price range.

## Error Handling

- **If destination is not provided:** Ask: "Where are you headed? Give me the city and I'll plan the whole trip."
- **If dates are not provided:** Ask: "When are you going? I need dates (or at least the month) to check weather, pricing, and availability."
- **If purpose is not provided:** Assume a general business/personal trip and note: "I'm planning this as a general trip. If you have meetings, events, or a specific purpose, tell me and I'll customize the itinerary around it."
- **If WebSearch returns limited data for the destination:** Build what you can from general knowledge and note: "Limited current data available for {destination}. I've used general knowledge for this plan. Verify hotel pricing and restaurant hours closer to your trip."
- **If the destination is international:** Add visa/entry requirements check, currency info, outlet adapter type, time zone difference, and language tips to the brief. Flag: "International travel — check passport expiration (must be valid 6+ months past return date for most countries), visa requirements, and travel advisories."
- **If the trip is same-day or next-day (short notice):** Skip the 1-week and 3-day prep timeline. Focus on what can still be done: packing, transportation, key logistics. Note: "Short notice trip — I've trimmed the prep timeline to what's actionable right now."
- **If the user has conflicting preferences (e.g., budget hotel but luxury restaurants):** Note the mismatch and provide options at both levels. Don't judge.
- **If a trip brief already exists for this destination/date range in Cloud Brain:** Ask: "A trip brief for {destination} already exists from {date}. Should I update it with new information or create a fresh one?"
