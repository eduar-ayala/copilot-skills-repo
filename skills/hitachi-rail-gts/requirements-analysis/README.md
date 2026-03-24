# Requirements Analysis Skill — Hitachi Rail GTS

This directory contains examples for the `/requirements-analysis` prompt skill.

## Skill File

**Prompt file**: [`.github/prompts/hitachi-rail-gts/requirements-analysis.prompt.md`](../../../.github/prompts/hitachi-rail-gts/requirements-analysis.prompt.md)

## How to Use

1. Open Copilot Chat (`Ctrl+Alt+I` / `Cmd+Option+I`).
2. Type `/requirements-analysis`.
3. Paste the requirements text, user story, or system description into the chat.

## Example Input — User Story

```
As a train driver, I want the Automatic Train Protection system to alert me
when the train exceeds the speed limit for the current track section,
so that I can apply the brakes before an automatic emergency brake is triggered.

The alert should:
- Be visual (display on the cab panel) and audible (alarm sound)
- Activate when the train speed exceeds the limit by more than 5 km/h
- Remain active until the speed drops back below the limit
- Be suppressible by the driver with a single button press (alert acknowledged)
- Trigger automatic emergency braking if speed exceeds limit by more than 15 km/h
  and the driver has not acknowledged the alert within 5 seconds
```

## Example Output

```markdown
## Requirements Decomposition

### REQ-ATP-101
**Statement:** The system SHALL display a visual alert on the cab panel when the train speed exceeds the current track section speed limit by more than 5 km/h.
**Priority:** Must Have
**SIL Level:** SIL 2
**Source:** US-ATP-001
**Rationale:** Visual alerts are the primary notification channel for drivers.

### REQ-ATP-102
**Statement:** The system SHALL activate an audible alarm when the train speed exceeds the current track section speed limit by more than 5 km/h.
**Priority:** Must Have
**SIL Level:** SIL 2
**Source:** US-ATP-001
**Rationale:** Audible alerts supplement visual alerts, especially under low-visibility conditions.

### REQ-ATP-103
**Statement:** The visual alert and audible alarm SHALL remain active until the train speed drops below the current track section speed limit.
**Priority:** Must Have
**SIL Level:** SIL 2
**Source:** US-ATP-001
**Rationale:** Continuous alerting ensures the driver is aware the overspeed condition persists.

### REQ-ATP-104
**Statement:** The driver SHALL be able to acknowledge the overspeed alert by pressing a designated button, which suppresses the alarm for the current overspeed event.
**Priority:** Must Have
**SIL Level:** SIL 1
**Source:** US-ATP-001
**Rationale:** Acknowledgement prevents alarm fatigue while maintaining driver awareness.

### REQ-ATP-105
**Statement:** The system SHALL trigger automatic emergency braking if the train speed exceeds the current track section speed limit by more than 15 km/h AND the driver has not acknowledged the alert within 5 seconds.
**Priority:** Must Have
**SIL Level:** SIL 3
**Source:** US-ATP-001
**Rationale:** Automatic intervention is required when the driver fails to respond to a critical overspeed condition.

---

## Ambiguity & Gap Analysis

| # | Issue | Action Required |
|---|-------|----------------|
| 1 | "current track section speed limit" — Is this retrieved from a trackside beacon, on-board map, or both? | Clarify data source and fallback behavior |
| 2 | "suppress" — Does acknowledging the alarm disable it permanently for the journey, or only until speed drops below limit? | Clarify acknowledgement scope |
| 3 | What happens if the alert button fails? | Define failure mode and fallback |
| 4 | Is the 5-second countdown from first overspeed or from when acknowledgement window opens? | Clarify timing reference |

---

## Safety Hazard Identification

| Requirement | Failure Mode | Severity | Mitigation |
|---|---|---|---|
| REQ-ATP-105 | Emergency braking not triggered when required | Catastrophic | Redundant speed monitoring, watchdog on brake command |
| REQ-ATP-101 | Visual alert not displayed (display failure) | Critical | Dual-channel display, audible backup |
| REQ-ATP-104 | Acknowledgement button stuck/unresponsive | Critical | Timeout fallback (REQ-ATP-105) |

---

## Acceptance Criteria

### REQ-ATP-105
```
Given the train is travelling at 120 km/h
And the current speed limit is 100 km/h (overspeed = 20 km/h > 15 km/h threshold)
And the driver has NOT pressed the acknowledgement button
When 5 seconds have elapsed since the alert was triggered
Then the system SHALL activate the emergency braking command
And the emergency brake SHALL reduce speed below 100 km/h within the braking distance

Given the driver presses the acknowledgement button within 4 seconds of the alert
And speed exceeds the limit by 20 km/h
Then the system SHALL NOT trigger automatic emergency braking
```
```

## Related Resources

- [IEC 62279 / EN 50128](https://www.cenelec.eu) — Railway software safety standard
- [`skills/hitachi-rail-gts/safety-critical-code/`](../safety-critical-code/) — Safety code review examples
- [`skills/hitachi-rail-gts/test-generation/`](../test-generation/) — Test generation from requirements
