## Summary

Allow historical drone detections within a newly created alarm zone to be backfilled into that zone, with an option to skip backfilling when it is not relevant.

## Problem Statement

When an organization creates a new alarm zone, previously detected drones from that geographical area do not appear in the zone. The alarm zone therefore starts without any historical context, even when relevant detection data already exists.

However, automatic backfilling may be unnecessary or distracting in some situations, such as when a sensor has been moved or is used while roaming. Organizations should therefore be able to decide whether historical detections are relevant when creating the zone.

## Proposed Solution

Add an option to the alarm-zone creation flow that determines whether existing historical detections within the new zone should be backfilled.

Suggested user flow:

1. The user defines the new alarm zone.
2. The user chooses whether to include previously detected drones from that area.
3. The user creates the alarm zone.
4. If backfilling is enabled, matching historical detections are associated with and displayed for the new zone.
5. If backfilling is disabled, the zone only includes detections occurring after its creation.

The option’s default value should be decided during implementation based on the most common use case.

## Acceptance Criteria

- The alarm-zone creation flow includes an option to backfill historical drone detections.
- When backfilling is enabled, available historical detections located within the alarm-zone boundaries are displayed for that zone.
- Backfilled detections retain their original detection timestamps and data.
- Only detection data belonging to or accessible by the organization can be backfilled.
- Backfilling does not create duplicate entries within the same alarm zone.
- When backfilling is disabled, detections from before the alarm zone was created are not added to the zone.
- Future detections continue to be handled normally regardless of the selected backfill option.
- Users can create a zone without backfilling when a sensor has been moved or is roaming.

## Out of Scope

- Backfilling alarm zones that already exist.
- Changing the retention period for historical detection data.
- Changing how future detections trigger alarms.
- Sending new real-time notifications for backfilled historical detections.

## Technical Notes

- Use the existing retained detection history as the source for backfilling.
- A spatial query may be required to find detections whose recorded positions fall within the new alarm-zone geometry.
- The available backfill period will depend on the platform’s existing data-retention rules.
- Historical detections should be associated with the new zone without being processed as newly received live detections.