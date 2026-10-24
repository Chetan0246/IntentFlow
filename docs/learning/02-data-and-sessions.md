# Lesson 2 — RetailRocket Data and Sessions

## Learning goals

Understand the files and event fields before modeling. Learn to profile data, preserve timestamp order, define session boundaries, and document data quality.

## Dataset map

The RetailRocket release contains an event log, item properties, and a category tree. Read the [dataset card](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset) before downloading. The event log includes visitor ID, timestamp, event type, item ID, and an optional transaction ID. Item properties are time-dependent; many property values are hashed. Confirm the exact files and columns in the downloaded version—do not rely only on a secondary summary.

Treat the released files as raw input. Do not edit raw rows in place. Record source, retrieval date, file names, sizes, checksums if convenient, and license terms. Do not commit the raw dataset.

## Data profiling checklist

For each file, inspect:

- row count, columns, inferred types, null counts, and duplicate rows;
- timestamp minimum/maximum and conversion from milliseconds to UTC;
- unique visitors, items, event types, and events per visitor;
- event counts by type and time period;
- item/property coverage and repeated/change-log property values;
- missing or invalid identifiers, timestamp ties, and extreme users/sessions.

A number is not an explanation. Ask what it means for the model. For example, if purchase events are sparse, the team must choose carefully whether the target is a view or a purchase-related event.

## Sessionization

The dataset has visitor IDs, not ready-made session IDs. A sessionization rule can split one visitor's event stream after an inactivity gap. The gap threshold is a modeling choice, not a fact in the raw file.

Sort by timestamp and a deterministic tie-break rule. For consecutive events from one visitor, start a new session when the gap exceeds the chosen threshold. Record the threshold, timezone, tie policy, and whether event types/items repeat. Explore how session counts and lengths change under more than one threshold before selecting one.

A session sequence should preserve order: ([item_1, item_2, ...]). Deduplicating repeated views may remove useful behavior; retaining them may overweight repeated refreshes. Make this a documented experiment, not an unexamined cleanup step.

## Point-in-time item features

Item properties can change over time. At prediction time (t), use only property information available by (t). Joining the latest property snapshot to all historical events can leak future availability/category changes. Keep event-time joins explicit.

## Small exercises

1. Convert a timestamp such as 1439694000000 milliseconds to a UTC datetime using a library and verify the unit.
2. Draw a profile table with event count and distinct visitor/item counts per event type.
3. Hand-sessionize five events for one visitor with gaps of 2, 9, 20, and 3 minutes under a stated threshold.
4. Show how changing the inactivity threshold changes that visitor's session assignment.
5. Identify a future item-property value that must not be used for an earlier prediction.

## Build tickets

- **D1:** Create a data dictionary from actual file headers and observed values.
- **D2:** Write a read-only profile script/notebook; output summary tables rather than modifying raw data.
- **D3:** Choose a canonical event schema and validation checks.
- **D4:** Implement sessionization only after you have a hand-worked rule and tie policy.
- **D5:** Create a temporal split manifest and verify feature timestamps are <= prediction time.

## Study material

- [RetailRocket dataset card](https://www.kaggle.com/datasets/retailrocket/ecommerce-dataset)
- [pandas getting started](https://pandas.pydata.org/docs/getting_started/index.html)
- [DuckDB Python API](https://duckdb.org/docs/stable/clients/python/overview)
- The dataset card notes time-dependent item properties. Read that section carefully before joining item attributes.
