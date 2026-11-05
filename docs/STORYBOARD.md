# Train Ticket Reservation System — Storyboard

## Purpose

This storyboard describes the main passenger journey for an online train ticket reservation system. It is an MVP-oriented product specification; exact routes, fare rules, payment provider, and ticketing policies should be confirmed by the project team.

## Primary actor

**Passenger:** searches for a train, selects a journey, enters traveler details, pays, and retrieves a booking confirmation.

## Storyboard: booking a train journey

| Scene | Passenger action | System response | Outcome / acceptance criteria |
|---|---|---|---|
| 1. Start a search | Opens the booking page and enters origin, destination, travel date, and passenger count. | Validates required fields and date, then offers a way to search. | Search is unavailable until required inputs are valid. Origin and destination must differ; passenger count must be within the configured limit. |
| 2. Review trains | Submits the search. | Shows matching trains with departure/arrival times, duration, available classes, availability, and fare. Provides a useful empty state if no trains match. | Results match the requested route/date and are presented consistently. An empty result does not look like a system error. |
| 3. Choose an option | Selects a train and class. | Displays the selected journey, current availability, fare breakdown, and any applicable terms. | Passenger can verify the journey and total before proceeding. The selected option is retained for the booking flow. |
| 4. Enter passenger details | Provides the required passenger/contact information. | Validates required fields and format, and explains any errors without discarding valid entries. | Invalid or missing details prevent continuation; valid details are retained for review. Only necessary personal data is requested. |
| 5. Review booking | Checks journey, passenger count/details, and total fare. | Presents a final summary and an explicit action to proceed to payment. | Summary agrees with the selected train, class, traveler count, and calculated price. Changes can be made before payment. |
| 6. Pay | Chooses an available payment method and completes payment. | Processes payment safely and reports success or failure. Prevents duplicate booking/payment when a request is retried. | A confirmed booking is created only after successful payment (or the configured payment policy). Failure leaves a clear retry/back path and does not falsely confirm a booking. |
| 7. Receive confirmation | Views the result and optionally retrieves the ticket. | Shows a unique booking reference, journey details, payment/booking status, and ticket retrieval or delivery instructions. | Passenger can identify and retrieve the confirmed booking. Confirmation is not shown for an unconfirmed transaction. |
| 8. Manage booking (follow-up) | Looks up a booking using the supported verification process and requests cancellation or changes, if supported. | Shows current booking status and applicable options, fees, and policy before applying an action. | Only an authorized passenger can view/manage the booking. State and any refund/change result are clearly communicated. |

## User stories and acceptance criteria

### Search journeys

**US-01 — Search by route and date**  
As a passenger, I want to search trains by origin, destination, and travel date so that I can find a suitable journey.

- Given valid route and date inputs, when I search, then matching journeys are shown.
- Given missing, malformed, past, or otherwise disallowed inputs, when I submit, then field-level guidance is shown and no invalid search is performed.
- Given origin and destination are the same, then the search is rejected with a clear message.
- Given no services match, then I see an empty state and can adjust my search.

### Compare availability and fares

**US-02 — Compare train options**  
As a passenger, I want to see schedules, class availability, and fares so that I can choose a journey that suits me.

- Each result identifies the train/journey, departure and arrival, duration, class, availability, and fare.
- Results correspond to the searched route and date.
- Unavailable options cannot be selected as bookable options.

### Select a journey

**US-03 — Select train and class**  
As a passenger, I want to select an available train and class so that I can reserve the option I chose.

- The selected journey and class are carried into passenger details and review.
- Before confirmation, availability and price are checked again or the user is told when a selection has expired/changed.
- A changed fare or unavailable option is disclosed and requires the passenger's confirmation or a new selection.

### Provide passenger details

**US-04 — Enter traveler information**  
As a passenger, I want to provide the required traveler and contact details so that the reservation can be issued.

- Required fields, formats, and passenger-count limits are validated.
- Errors identify the affected field and preserve other valid entries.
- Personal details are not exposed in URLs, public pages, or another passenger's booking.

### Review total

**US-05 — Review before payment**  
As a passenger, I want to review the journey and full price before paying so that I can catch mistakes.

- The final summary matches the selected itinerary and passenger details.
- The displayed total includes all configured charges and currency is explicit.
- The passenger can return to correct information before payment.

### Complete payment and booking

**US-06 — Pay and confirm**  
As a passenger, I want to pay and receive a booking reference so that I know my reservation is confirmed.

- A successful payment produces one booking with a unique reference and a confirmed status.
- A declined, canceled, or timed-out payment is not presented as confirmed.
- Retrying a payment request does not create duplicate charges or bookings.
- Confirmation includes the booking reference and selected journey details.

### Retrieve/manage a booking

**US-07 — Retrieve a booking**  
As a passenger, I want to retrieve my booking using the supported verification method so that I can view my ticket details.

- A valid booking reference and required verification retrieve the matching booking.
- Invalid credentials do not reveal whether another person's booking exists or expose its details.
- Booking status is visible and reflects the latest known state.

**US-08 — Cancel or change a booking (if in scope)**  
As a passenger, I want to see eligible change/cancellation options and consequences before confirming so that I can manage my trip.

- Eligibility, fees, and refund/change consequences are shown before the action is committed.
- An ineligible action is blocked with an explanation.
- A successful action updates the booking status and communicates the outcome.

## Suggested backlog / sprint grouping

| Iteration | Suggested scope | Stories |
|---|---|---|
| Sprint 1 — Search and discovery | Search form, input validation, results, empty state | US-01, US-02 |
| Sprint 2 — Reservation details | Selection, availability/price recheck, passenger details, review | US-03, US-04, US-05 |
| Sprint 3 — Payment and confirmation | Payment outcomes, idempotent retry, booking reference, confirmation | US-06 |
| Sprint 4 — Booking management | Secure retrieval; cancellation/change only if included in product scope | US-07, US-08 |

## Assumptions to confirm

- Supported origins, destinations, routes, dates, passenger limits, and ticket classes.
- Whether accounts are required or booking is available to guests.
- Required passenger fields and applicable age/identity rules.
- Payment methods, currency, taxes/fees, timeout behavior, and refund policy.
- Whether cancellation and modification are in scope for the first release.
- Ticket delivery/retrieval channel and accessibility/localization requirements.
