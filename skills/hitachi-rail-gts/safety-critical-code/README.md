# Safety-Critical Code Skills — Hitachi Rail GTS

This directory contains examples for safety-critical code review using the `/safety-review` prompt.

## Skill File

**Prompt file**: [`.github/prompts/hitachi-rail-gts/safety-review.prompt.md`](../../../.github/prompts/hitachi-rail-gts/safety-review.prompt.md)

## Background

Safety-critical software in railway systems must comply with:
- **EN 50128 / IEC 62279** — Software for railway control and protection systems
- **SIL (Safety Integrity Level)** — SIL 0 (lowest) to SIL 4 (highest)

Higher SIL levels require stricter development, review, and testing practices.

## How to Use

1. Open a safety-critical code file.
2. Select the relevant function or module.
3. Open Copilot Chat and type `/safety-review`.
4. Review the output carefully — Copilot identifies potential issues, but **human review is mandatory** for SIL 2+ code.

## Example: Safe Speed Monitoring Function

### Code Under Review

```python
def check_speed_limit(current_speed_kmh: float, limit_kmh: float) -> bool:
    """
    Check whether a train's current speed is within the allowed limit.
    Implements: REQ-ATP-042
    SIL Level: SIL 3

    Args:
        current_speed_kmh: Current measured speed in km/h.
        limit_kmh: Maximum allowed speed in km/h.

    Returns:
        True if speed is within limit, False if overspeed detected.

    Safety note: A return value of False MUST trigger emergency braking.
    """
    if current_speed_kmh < 0:
        # SAFETY-CRITICAL: Negative speed is a sensor fault — treat as overspeed
        return False
    if limit_kmh <= 0:
        # SAFETY-CRITICAL: A zero or negative limit is a configuration fault
        return False
    return current_speed_kmh <= limit_kmh
```

### Example Safety Review Output

```
## Safety Review — check_speed_limit()

### Safety Summary
The function is logically correct and handles two critical fault conditions
(negative speed, invalid limit). It is simple, deterministic, and has no
side effects. Suitable for SIL 3 with the improvements noted below.

### Findings

| # | Severity | Location | Rule | Description | Recommendation |
|---|----------|----------|------|-------------|----------------|
| 1 | Major | line 1 | EN 50128 §6.8.4 | No type range enforcement — caller could pass NaN or Inf as float | Add: assert not (math.isnan(...) or math.isinf(...)), or use a validated numeric type |
| 2 | Minor | line 1 | Traceability | Return value semantics not documented in requirements | Add to REQ-ATP-042: "Returns False on any sensor or configuration fault" |
| 3 | Suggestion | — | Readability | Consider named constants for magic numbers if limits come from a config table | Define OVERSPEED_MARGIN_KMH = 0.0 or similar |

### Verdict: Conditional Pass
Address finding #1 (NaN/Inf protection) before SIL 3 qualification.
Finding #2 is a documentation issue to resolve with the safety team.
```

## Anti-Patterns to Avoid

The following patterns are **always flagged** in safety reviews:

```python
# ❌ NEVER ignore return values
result = safety_function()  # result not checked!

# ❌ NEVER use unbounded loops in real-time paths
while not sensor.is_ready():  # could loop forever!
    pass

# ❌ NEVER use dynamic allocation in real-time sections
data = []  # heap allocation
data.append(reading)  # unbounded growth

# ❌ NEVER rely on undefined behavior
speed = raw_bytes[0] << 24 | raw_bytes[1] << 16  # shift overflow risk in C
```

## Safe Patterns to Follow

```python
# ✅ Always check return values
status = safety_function()
if status != STATUS_OK:
    trigger_safe_state("safety_function failed")
    return

# ✅ Bounded loops with timeout
MAX_WAIT_CYCLES = 1000
for _ in range(MAX_WAIT_CYCLES):
    if sensor.is_ready():
        break
else:
    trigger_safe_state("sensor timeout")
    return

# ✅ Pre-allocated buffers
BUFFER_SIZE = 64
readings = [0.0] * BUFFER_SIZE  # fixed size, no heap growth

# ✅ Explicit range checks before arithmetic
if not (0 <= raw_value <= MAX_SENSOR_VALUE):
    trigger_safe_state("sensor out of range")
    return
speed = raw_value * SPEED_SCALE_FACTOR
```

## References

- EN 50128:2011 — Railway applications: Software for railway control and protection systems
- IEC 62279:2015 — Railway applications: Software for railway control and protection systems
- CENELEC EN 50657 — Railway applications: Rolling stock applications
