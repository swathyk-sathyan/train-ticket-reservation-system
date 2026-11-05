# Train Ticket Reservation System — Test Cases

## Scope and assumptions

These are system-level functional test cases derived from the MVP storyboard. Replace bracketed/configurable values with the product's actual routes, passenger limits, date rules, fare policy, payment integration, and booking lookup policy. Use a payment sandbox for payment scenarios; never use real payment credentials in test data.

## Test data

- **Route A:** a configured route with at least one scheduled train.
- **Route B:** a configured valid route with no service on the chosen date (or a controlled no-results fixture).
- **Travel dates:** one permitted future date, one past date, and one date at the configured booking boundary.
- **Passenger counts:** 1, configured maximum, and maximum + 1.
- **Payment outcomes:** sandbox success, decline, cancel, timeout, and retry.
- **Passenger data:** valid fixture plus missing/invalid field variants; use synthetic personal information only.

## Test cases

| ID | Priority | Test scenario | Preconditions / steps | Expected result |
|---|---|---|---|---|
| TC-001 | High | Search with valid route and future date | Open search; enter Route A, permitted future date, and 1 passenger; submit. | Search completes and only matching journeys for that route/date are shown with schedule, class/availability, and fare. |
| TC-002 | High | Required search fields are missing | Leave origin, destination, or date empty one at a time; submit. | Search is not performed; the missing field is identified with understandable validation guidance. |
| TC-003 | High | Origin equals destination | Set origin and destination to the same location; submit. | Search is rejected and the user is told that the locations must differ. |
| TC-004 | High | Past travel date | Enter a past date and submit. | The date is rejected according to product policy; no booking search is started. |
| TC-005 | Medium | Booking date boundary | Search at the earliest/latest date allowed by the configured booking window and just outside that boundary. | Boundary dates behave according to policy; out-of-window dates are blocked with guidance. |
| TC-006 | High | Passenger count validation | Search with 1 passenger, the configured maximum, and maximum + 1. | Valid counts are accepted; a count outside the allowed range is blocked and explained. |
| TC-007 | Medium | No matching trains | Search Route B on the controlled no-service date. | A clear no-results state appears, with an option to revise search; no stale results are shown as matches. |
| TC-008 | High | Select an available result | Search Route A and select an available train/class. | The selected train, route, date, class, and price are carried forward accurately. |
| TC-009 | High | Unavailable result cannot be booked | Display or fixture an unavailable class; attempt to select/book it. | The unavailable option cannot proceed to payment; the user gets a clear availability message and can choose another option. |
| TC-010 | High | Availability changes after selection | Select a journey, then make the test fixture report no availability before review/payment. | The system rechecks availability, prevents an invalid reservation, and asks the user to select another option. |
| TC-011 | High | Fare changes after selection | Select an option, then make the test fixture return a changed fare before confirmation. | Updated fare is disclosed; payment does not proceed at the changed price without the required user confirmation. |
| TC-012 | High | Valid passenger details | Select an available journey; enter valid required passenger/contact details; continue. | Details pass validation and the correct journey and passenger count remain associated with the flow. |
| TC-013 | High | Missing or malformed passenger details | Submit with a required field missing and with invalid formats where applicable. | Continuation is blocked; actionable field-level errors appear and valid entries are retained. |
| TC-014 | Medium | Multiple passengers | Select more than one passenger within the allowed limit and enter required details for each. | The system validates each traveler, associates the correct count/details, and calculates the fare consistently with policy. |
| TC-015 | High | Review summary and total | Complete valid search, selection, and passenger details; inspect review. | Route, train, date, class, passenger count, fare breakdown, and total match the selections; currency and applicable fees are clear. |
| TC-016 | High | Edit details before payment | From review, return to change passenger details or journey selection. | User can make changes; the updated summary is shown and stale values are not used at payment. |
| TC-017 | Critical | Successful payment creates booking | Use sandbox success for a valid booking and complete payment. | Exactly one booking is created with confirmed status and unique reference; confirmation displays matching journey details and payment result. |
| TC-018 | Critical | Declined payment | Use sandbox decline for a valid booking. | No confirmed booking is presented; user sees a clear failure and safe retry/back option; payment/booking state is not falsely successful. |
| TC-019 | High | Passenger cancels payment | Start payment and cancel from the sandbox/redirect flow. | User returns to a clear non-confirmed state; no success confirmation is shown and the booking remains unconfirmed or is released according to policy. |
| TC-020 | High | Payment timeout | Simulate a payment timeout or delayed provider response. | UI communicates pending/failure state without falsely confirming; subsequent status check/retry does not create a duplicate booking or charge. |
| TC-021 | Critical | Duplicate submit / idempotent retry | Submit payment, then repeat the same request (e.g., double-click or controlled retry) using the same operation. | At most one successful charge and one booking are created; user sees a single consistent outcome. |
| TC-022 | High | Retrieve booking with valid verification | Complete a booking; use its reference and the required valid verification information. | Only the matching booking is returned with correct status and journey/ticket details. |
| TC-023 | High | Retrieve booking with invalid verification | Try an invalid reference and a valid reference with incorrect verification. | Access is denied without exposing personal or booking data; response does not unnecessarily disclose whether another passenger's reference exists. |
| TC-024 | High | Cancel/change eligible booking (if in scope) | Retrieve an eligible booking; inspect terms; confirm a supported change/cancellation. | Eligibility and consequences are shown first; confirmed action updates status and refund/change result is communicated. |
| TC-025 | Medium | Attempt ineligible cancellation/change (if in scope) | Retrieve a booking outside the permitted change/cancellation policy and attempt the action. | Action is blocked; policy reason is shown and booking remains unchanged. |
| TC-026 | High | Personal data and payment data handling | Exercise search, passenger, review, payment, error, and booking lookup flows; inspect visible URLs and logs available to testers. | Sensitive passenger/payment data is not exposed in URLs or unintended UI/error output; payment data handling follows the chosen provider and project security requirements. |
| TC-027 | Medium | Refresh/back navigation during booking | Navigate through the flow, then use browser back/refresh at review and payment-result stages. | State remains coherent; no unintended duplicate payment/booking occurs; expired selections require a safe recheck. |
| TC-028 | Medium | Keyboard and validation usability | Complete core search and booking forms using keyboard only; trigger validation errors. | Controls are keyboard-operable, focus moves understandably, and errors are perceivable and associated with the relevant inputs. |

## Execution record template

| Run date | Build / commit | Tester | Test case ID | Result (Pass/Fail/Blocked) | Evidence / issue link | Notes |
|---|---|---|---|---|---|---|
| YYYY-MM-DD |  |  |  |  |  |  |

## Exit criteria (suggested)

- All Critical and High priority cases pass, or have an agreed and documented disposition.
- No unresolved defect can create a false booking confirmation, duplicate successful charge, or expose another passenger's booking.
- Failures and blocked cases include reproducible evidence and are tracked for follow-up.
