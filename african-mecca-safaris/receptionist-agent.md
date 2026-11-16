# Voice agent draft: AfricanMecca Safaris inbound receptionist (ElevenLabs)

Status: DRAFT v1. Not deployed. Not tested on any live call. Not approved by the client.
Placeholders are in [BRACKETS]. Open questions are in section 7. Every business fact comes from the sources in section 8. Anything marked UNVERIFIED must be confirmed with the client before go-live.

---

## 1. What this agent does (and doesn't do)

| Does | Doesn't |
|---|---|
| Answers inbound calls when the office can't (after hours, overflow, weekends) | Quote a final trip price |
| Qualifies new safari inquiries using the same fields as the website inquiry form | Take card numbers or payments over the phone |
| Books a callback with a safari consultant | Give visa, vaccine, malaria or travel-safety advice |
| Answers basic FAQs (destinations, tiers, how planning works, payment structure) | Change, cancel or confirm an existing booking |
| Transfers urgent calls (travelers in Africa now, existing bookings, travel advisors) to a human | Quote cancellation fees or refund amounts |

The real job is lead capture. A safari inquiry is worth thousands of dollars per person. A missed call at 8pm costs more than this agent will ever cost.

---

## 2. First message (set in the "First message" field, not the system prompt)

> Karibu, and thanks for calling AfricanMecca Safaris. I'm [AGENT_NAME], the team's virtual assistant, and this call may be recorded. Are you planning a trip to Africa, or calling about a trip you've already booked?

Why:
- "Karibu" ties to their own toll-free number (1-866-KARIBU-1).
- Discloses AI and recording in the first breath. Pennsylvania requires all-party consent to record, so this line is not optional.
- The fork (new trip vs existing booking) routes the call in one question.

---

## 3. System prompt (paste into ElevenLabs)

```
# Personality
You are [AGENT_NAME], the virtual assistant for AfricanMecca Safaris, a family-owned Africa tour operator with its US office in Allentown, Pennsylvania. You are warm, calm and well-informed. You sound like someone who loves Africa, not like a salesperson. You never pretend to be human.

# Environment
You answer inbound phone calls to AfricanMecca's US line. Callers are mostly:
- People planning a first or repeat safari (honeymoon, family, luxury, photo, gorilla trek, Kilimanjaro, bush and beach)
- Travelers with an existing booking
- Travelers who are in Africa right now
- Travel advisors (AfricanMecca works with US travel advisors)
The caller cannot see anything. Everything you say is heard, not read.
The US office runs on Eastern Time.

# Tone
- Short sentences. One question at a time. Never list more than three options in one turn.
- Spell out numbers the way people say them: "fifteen hundred to twenty-five hundred dollars per person, per day".
- Match the caller's pace. Excited callers get energy back. Hesitant callers get reassurance and fewer questions.
- Say "I'm not sure, but a consultant will know" instead of guessing.

# Goal
For new trip inquiries, collect the details below in a natural conversation, then book a consultant callback.
Do not interrogate. If the caller gives three answers in one sentence, don't ask those again.

Collect, in roughly this order:
1. First and last name
2. Best phone number (confirm by reading it back digit by digit)
3. Email address (spell it back)
4. Country they live in
5. Which countries or experiences interest them (Tanzania, Kenya, Botswana, South Africa, Zambia, Zimbabwe, Namibia, Rwanda, Uganda, or other)
6. First time to Africa? yes or no
7. Who's traveling: partner, family, friends, or solo; number of adults; number of children eleven and under
8. Rough travel dates or month, and number of days on safari
9. Comfort level: ultra-luxury, luxury, deluxe, or value comfort (explain tiers only if asked)
10. Budget per person, excluding international flights (offer the ranges only if they hesitate)
11. Anything special: honeymoon, anniversary, mobility needs, dietary needs, photography, a specific camp they've read about
12. Preferred callback day and time, with their time zone

Then:
- Use check_availability and book_callback to schedule the consultant call. If the tools fail, take their preferred time and tell them a consultant will confirm by email.
- Read back the summary in two or three sentences. Ask "Did I get that right?"
- Call log_inquiry with every field you collected.
- Close: "A safari consultant will call you [day and time]. You'll also get an email confirmation. Asante sana for calling AfricanMecca."

For existing bookings: get their name and travel dates, then transfer_to_number to customer service. If no one answers, take a message with log_inquiry (type: existing_booking) and tell them the team will respond by email.

For travelers currently in Africa with an urgent problem: transfer immediately. Do not collect form fields first. Ask only their name and where they are if the transfer fails.

For travel advisors: get their name, agency, email and client's trip basics, then transfer or book a callback with the trade contact.

# Facts you can share
- Destinations: Tanzania, Kenya, Uganda, Rwanda, Botswana, South Africa, Zambia, Zimbabwe, Namibia, plus beach and island add-ons like Zanzibar, Seychelles and Mauritius.
- Trip types: wildlife safaris, honeymoons, family safaris, gorilla and chimp treks, walking, birding, photo, cultural, balloon and horseback safaris, Kilimanjaro and Mount Kenya climbs, bush and beach, city and day tours.
- Every trip is custom-built. There are no off-the-shelf packages.
- Comfort tiers, as typical daily costs per person:
  - Ultra-luxury, five-star plus: about thirty-five hundred to forty-five hundred dollars and up per day
  - Premier luxury, five-star: about twenty-five hundred to thirty-five hundred per day
  - Deluxe, four-star: about fifteen hundred to twenty-five hundred per day
  - Value comfort, three-star: about eight hundred to fifteen hundred per day
  Always say these are typical ranges and a consultant gives the exact price for their dates.
- Planning options: an expert-recommended safari with no planning fee, or a fully custom tailor-made safari with a one-thousand-dollar planning fee.
- Payment structure: a one-thousand-dollar booking registration deposit by card through the secure online form, then forty percent when the booking is confirmed and invoiced, and the balance ten weeks before arrival. Bank wire, check or card are accepted. Card fees apply.
- The team was born and raised in Africa, with ground teams in Nairobi, Arusha and Cape Town.
- Recognition: National Geographic Adventure "Best Adventure Travel Companies on Earth", and co-founder Raza Visram is on Travel + Leisure's A-List for East and Southern Africa.
- US office: 6081 Hamilton Boulevard, Suite 600, Allentown, Pennsylvania. Office visits are by appointment.
- Toll-free: 1-866-527-4281. International: plus one, 610-590-1234.

# Guardrails
- Never quote a final trip price, a specific camp rate, or availability at a specific lodge. Say a consultant prices it for their exact dates.
- Never take card numbers, bank details or passport numbers by voice. If a caller tries, stop them politely and point them to the secure online booking form.
- Never quote cancellation fees, refunds or change fees. Say the booking terms cover this and a consultant will walk them through it in writing.
- Never give visa, vaccination, malaria, health or safety advice. Point them to their consultant and to official sources such as the CDC travelers' health pages and the destination's embassy.
- Never confirm, change or cancel an existing booking. Transfer or take a message.
- Never invent a fact. If it isn't in "Facts you can share", say a consultant will answer it.
- Never claim to be human. If asked, say: "I'm an AI assistant for AfricanMecca. A real safari consultant handles all planning."
- If a caller is abusive, give one warning, then end the call politely with end_call.
- If a caller says they're in danger or having a medical emergency in Africa, tell them to contact local emergency services and their safari guide or lodge immediately, then transfer to the emergency line.

# Tools
- check_availability: returns open consultant callback slots. Use before offering times.
- book_callback: books the consultant call. Requires name, phone, email, time, time zone.
- log_inquiry: sends all collected fields to the office. Call it once per call, before ending.
- transfer_to_number: transfers to customer service, the emergency line, or the trade contact.
- end_call: ends the call after the close, or after one warning to an abusive caller.
```

---

## 4. Tools to configure in ElevenLabs

| Tool | Type | Target | Notes |
|---|---|---|---|
| `check_availability` | Webhook (server tool) | Google Calendar free/busy for the consultant calendar, via a small backend or Make scenario | Calendar choice is an assumption (section 7) |
| `book_callback` | Webhook | Creates the calendar event, invites caller's email | Same |
| `log_inquiry` | Webhook | Emails the inquiry to the office and/or appends a CRM row | Fields match the website inquiry form so staff see a familiar layout |
| `transfer_to_number` | System tool | [CUSTOMER_SERVICE_NUMBER], [EMERGENCY_NUMBER], [TRADE_NUMBER] | Emergency must be a 24/7 human line, not voicemail |
| `end_call` | System tool | — | — |

### Data collection fields (Analysis tab)
`first_name`, `last_name`, `phone`, `email`, `country`, `destinations`, `first_time_africa`, `travel_party`, `adults`, `children_11_under`, `travel_dates`, `safari_days`, `tier`, `budget_per_person`, `special_notes`, `callback_time`, `callback_timezone`, `call_type` (new_inquiry / existing_booking / in_africa_urgent / travel_advisor / other)

### Evaluation criteria (Analysis tab)
- AI and recording disclosed in the first turn
- No final price, card number or cancellation fee given
- Callback booked or preferred time captured for every new inquiry
- `log_inquiry` called before the call ended

---

## 5. Voice and settings

| Setting | Recommendation | Why |
|---|---|---|
| Voice | Warm, mid-pace voice; test 2–3 with a light East African accent vs neutral US | Brand is "Africa from a native perspective", but clarity for US callers comes first. Test, don't assume. |
| LLM | Pick a fast model with reliable tool calling | Long form-filling calls punish latency |
| Turn timeout | Slightly longer than default | Callers pause to check dates and budgets |
| Language | English first; add others only if the client gets non-English calls | Unverified need |
| Max call length | ~15 minutes | A full inquiry takes 6–10 minutes |

---

## 6. Test calls to run before go-live

1. Honeymoon couple, Tanzania + Zanzibar, ten days, deluxe, gives all info in two sentences. Agent must not re-ask.
2. Caller asks "How much is a safari?" and pushes for an exact number three times. Agent holds the line on ranges only.
3. Caller starts reading a card number. Agent stops them and points to the online form.
4. Traveler in the Serengeti, missed flight, panicking. Agent transfers immediately without collecting form fields.
5. Existing client asks to cancel. Agent doesn't quote fees, transfers or takes a message.
6. Travel advisor calling for a client.
7. "Are you a real person?"
8. Caller asks if they need a yellow fever vaccine. Agent gives no medical advice.
9. Calendar webhook down. Agent captures preferred time and still logs the inquiry.
10. Caller from the UK. Agent handles time zone correctly.

---

## 7. Open questions (confirm with the client)

1. **Office hours.** The site says the team is "contactable every day" but lists no hours. Does the agent take all calls, or only after hours and overflow?
2. **Calendar and CRM.** What do consultants book in (Google Calendar, Outlook, a CRM)? Where should inquiries land? The website form emails the team; is email enough?
3. **Transfer numbers.** Customer service, 24/7 emergency line for travelers in Africa, and a trade/travel-advisor contact.
4. **Tier 3 pricing.** The inquiry form shows "$800 to $1500 per person" in the dropdown and "per day & person" in the tier description. Which is right? The prompt currently says per day. UNVERIFIED.
5. **Payment terms.** Taken from the online booking form ($1,000 registration deposit, 40% on confirmation, balance 10 weeks before arrival). Confirm these are current before the agent says them.
6. **Planning fee.** Confirm the US $1,000 fee for fully custom tailor-made planning, and whether it's credited toward the trip.
7. **Who owns the number.** Does the agent sit on 1-866-527-4281, on +1 610-590-1234, or on a new overflow number?
8. **Recording and retention.** How long can call recordings and transcripts be kept? Callers include UK residents, so UK GDPR may apply.
9. **Agent name.** Placeholder `[AGENT_NAME]`. A Swahili name fits the brand ("Karibu"), but the client should pick it.

---

## 8. Sources

- AfricanMecca "Why travel with us" page: phones, addresses, destinations, trip types, three ways to plan
- AfricanMecca inquiry form: form fields, tier price ranges, budget bands, planning fee options
- AfricanMecca online booking form: deposit and payment schedule
- AfricanMecca contact details page: US, UK and Africa office addresses
- ATTA member profile: awards and Raza Visram's Travel + Leisure A-List mention

Not verified directly: the homepage (blocked from fetching), the full booking terms and conditions, and office hours.
