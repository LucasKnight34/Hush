# Task and trigger type inventory

**Status:** Draft. Every example below is a suggestion. Lucas and Sarah need to confirm each one against our real house, or replace it.

This inventory lists the kinds of task HUSH must support and which trigger type fits each one. The data model (HUSH-77) and the recurrence engine (HUSH-95) build on it.

## Trigger types

These are my working definitions of the six trigger types named in HUSH-31. Change them if you read them differently.

| Trigger type | Meaning |
|---|---|
| Fixed schedule | Due on set calendar dates, regardless of when it was last done (every April 1, every first of the month) |
| Relative to completion | Due a set time after the last time it was done (90 days after the filter was changed) |
| Anchored with lead time | Due a set time before a date that is fixed by something else, such as an expiry or a birthday |
| Usage-based | Due after a set amount of use, such as miles driven |
| Profile-derived | The interval is worked out from a profile, such as a pet's breed and activity |
| Condition-based | Due when something outside the app happens, such as the first freeze of the year |

## Home

| Task | Trigger type | Notes |
|---|---|---|
| Change the HVAC filter | Relative to completion | Interval depends on filter size; 90 days is typical |
| Clean the dryer vent | Relative to completion | Once a year |
| Flush the water heater | Relative to completion | Once a year |
| Clean the gutters | Fixed schedule | Late fall; could also be condition-based on leaf drop |

## Pets

| Task | Trigger type | Notes |
|---|---|---|
| Groom the dog | Profile-derived | Interval comes from breed and activity level |
| Flea and tick prevention | Relative to completion | Monthly, from the last dose |
| Vet checkup and vaccines | Anchored with lead time | The vet sets the next date; remind ahead of it |
| Trim nails | Profile-derived | Depends on breed and how active the pet is |

## Car

| Task | Trigger type | Notes |
|---|---|---|
| Oil change | Usage-based | Every set number of miles; see flagged item 1 |
| Rotate the tires | Usage-based | Every set number of miles; see flagged item 1 |
| Registration renewal | Anchored with lead time | The expiry date is fixed by the state |
| Annual inspection | Anchored with lead time | Due by a set date each year |

## Health and admin

| Task | Trigger type | Notes |
|---|---|---|
| Renew the passport | Anchored with lead time | Expiry date is on the document; long lead time |
| Renew the driver's license | Anchored with lead time | Expiry date is on the license |
| Dentist cleaning | Relative to completion | Every six months from the last visit |
| Refill a prescription | Relative to completion | From the last fill; see flagged item 3 |
| Review insurance policies | Fixed schedule | Once a year, before renewal |

## People

| Task | Trigger type | Notes |
|---|---|---|
| Buy a birthday gift | Anchored with lead time | The birthday is fixed; the lead time covers shopping and shipping |
| Anniversary plans | Anchored with lead time | Reservations may need weeks of notice |
| Send holiday cards | Anchored with lead time | Lead time covers ordering and mailing |
| Check in with a relative | Relative to completion | A call every few weeks, from the last one |

## Safety

| Task | Trigger type | Notes |
|---|---|---|
| Test smoke and CO detectors | Relative to completion | Monthly or twice a year |
| Replace detector batteries | Fixed schedule | Often tied to a date such as a time change |
| Replace the fire extinguisher | Anchored with lead time | The expiry is printed on the unit |
| Check emergency kit supplies | Anchored with lead time | Supplies have expiry dates |

## Seasonal

| Task | Trigger type | Notes |
|---|---|---|
| Winterize outdoor faucets | Condition-based | When the first freeze is forecast |
| Service the AC before summer | Fixed schedule | Every spring |
| Switch smoke detector batteries at the time change | Fixed schedule | Two dates a year |
| Check the sump pump before storm season | Condition-based | Before heavy rain, or at a set time each year |

## Flagged for discussion

These do not fit one trigger type cleanly.

1. **Whichever comes first.** Oil changes and tire rotations are often "every 5,000 miles or 6 months, whichever comes first". That combines usage-based and relative to completion. Do we support combined triggers, or pick one?
2. **Event-driven tasks.** "Replace a detector when it chirps" or "act on a product recall" have no schedule at all. They are really one-off tasks created by an event. Are they in scope?
3. **Variable amounts.** A prescription refill depends on how many pills were dispensed, which the app does not know. We may need a per-task interval the user sets.
4. **Condition-based with a fallback.** Winterizing faucets depends on a forecast, but the weather data may be missing. We need a fixed date to fall back on.
5. **Dates the user sets by hand.** Vet visits and the dentist's next appointment are often booked rather than calculated. Anchored with lead time may be right, or it may just be a one-off date.

## To finish this ticket

- Replace or confirm each example above so that every category has at least 3 real examples from our house.
- Decide on the flagged items, or move them into HUSH-94 (recurrence model).
