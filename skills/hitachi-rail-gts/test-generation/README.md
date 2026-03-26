# Test Generation Skill — Hitachi Rail GTS

This directory contains examples for the `/test-generation` prompt skill tailored to railway domain.

## Skill File

**Prompt file**: [`.github/prompts/hitachi-rail-gts/test-generation.prompt.md`](../../../.github/prompts/hitachi-rail-gts/test-generation.prompt.md)

## How to Use

1. Open the module you want tests for.
2. Open Copilot Chat and type `/test-generation`.
3. If relevant, mention the SIL level in the chat: "This is SIL 3 code."

## Coverage Targets by SIL

| SIL Level | Minimum Coverage Required |
|---|---|
| SIL 0–1 | 80% statement coverage |
| SIL 2–3 | 100% branch coverage (MC/DC recommended) |
| SIL 4 | 100% MC/DC (Modified Condition/Decision Coverage) |

## Example: ATP Speed Monitor Tests

### Source Module

```python
# atp_speed_monitor.py
# Implements: REQ-ATP-101 to REQ-ATP-105
# SIL Level: SIL 3

import math
from dataclasses import dataclass
from enum import Enum

class AlertState(Enum):
    NONE = "none"
    WARNING = "warning"       # >5 km/h over limit
    CRITICAL = "critical"     # >15 km/h over limit
    ACKNOWLEDGED = "acknowledged"

@dataclass
class SpeedAlert:
    state: AlertState
    overspeed_kmh: float
    time_to_brake_s: float | None

WARN_THRESHOLD_KMH = 5.0
BRAKE_THRESHOLD_KMH = 15.0
ACK_TIMEOUT_S = 5.0

def evaluate_speed(
    current_kmh: float,
    limit_kmh: float,
    ack_elapsed_s: float | None
) -> SpeedAlert:
    """
    Evaluate current train speed against limit and return alert state.

    Args:
        current_kmh: Current train speed in km/h. Must be >= 0.
        limit_kmh: Current section speed limit in km/h. Must be > 0.
        ack_elapsed_s: Seconds since alert was triggered and not acknowledged,
                       or None if alert was acknowledged.

    Returns:
        SpeedAlert with current state and overspeed amount.

    Raises:
        ValueError: If inputs are invalid (negative speed, zero/negative limit, NaN/Inf).
    """
    if math.isnan(current_kmh) or math.isinf(current_kmh):
        raise ValueError(f"Invalid current_kmh: {current_kmh}")
    if math.isnan(limit_kmh) or math.isinf(limit_kmh):
        raise ValueError(f"Invalid limit_kmh: {limit_kmh}")
    if current_kmh < 0:
        raise ValueError(f"current_kmh must be >= 0, got {current_kmh}")
    if limit_kmh <= 0:
        raise ValueError(f"limit_kmh must be > 0, got {limit_kmh}")

    overspeed = current_kmh - limit_kmh

    if overspeed <= WARN_THRESHOLD_KMH:
        return SpeedAlert(AlertState.NONE, max(overspeed, 0.0), None)

    if ack_elapsed_s is None:
        # Alert acknowledged
        return SpeedAlert(AlertState.ACKNOWLEDGED, overspeed, None)

    if overspeed > BRAKE_THRESHOLD_KMH:
        time_remaining = max(ACK_TIMEOUT_S - ack_elapsed_s, 0.0)
        if ack_elapsed_s >= ACK_TIMEOUT_S:
            return SpeedAlert(AlertState.CRITICAL, overspeed, 0.0)
        return SpeedAlert(AlertState.CRITICAL, overspeed, time_remaining)

    return SpeedAlert(AlertState.WARNING, overspeed, None)
```

### Generated Test Suite

```python
# test_atp_speed_monitor.py
import math
import pytest
from atp_speed_monitor import evaluate_speed, AlertState, SpeedAlert

class TestEvaluateSpeed:
    """
    Tests for evaluate_speed() — REQ-ATP-101 to REQ-ATP-105
    SIL Level: SIL 3 — 100% branch coverage target
    """

    # --- Happy Path Tests ---

    def test_evaluate_speed_within_limit_returns_none_alert(self):
        """
        Requirement: REQ-ATP-101
        Scenario: Train is within speed limit
        Expected: No alert state
        """
        # Arrange
        current_kmh, limit_kmh = 95.0, 100.0
        # Act
        result = evaluate_speed(current_kmh, limit_kmh, ack_elapsed_s=0.0)
        # Assert
        assert result.state == AlertState.NONE
        assert result.overspeed_kmh == 0.0

    def test_evaluate_speed_warning_threshold_triggers_warning(self):
        """
        Requirement: REQ-ATP-101, REQ-ATP-102
        Scenario: Train exceeds limit by exactly 5.1 km/h (just above warning threshold)
        Expected: WARNING alert state
        """
        result = evaluate_speed(105.1, 100.0, ack_elapsed_s=0.0)
        assert result.state == AlertState.WARNING
        assert abs(result.overspeed_kmh - 5.1) < 0.001

    def test_evaluate_speed_critical_no_ack_within_timeout_returns_brake_time(self):
        """
        Requirement: REQ-ATP-105
        Scenario: 16 km/h overspeed, 3 seconds elapsed, not acknowledged
        Expected: CRITICAL state with 2 seconds remaining to brake
        """
        result = evaluate_speed(116.0, 100.0, ack_elapsed_s=3.0)
        assert result.state == AlertState.CRITICAL
        assert result.time_to_brake_s == pytest.approx(2.0)

    def test_evaluate_speed_critical_timeout_expired_returns_zero_brake_time(self):
        """
        Requirement: REQ-ATP-105
        Scenario: 5+ seconds elapsed without acknowledgement — brake NOW
        Expected: CRITICAL state with 0 seconds remaining
        SIL Level: SIL 3
        """
        result = evaluate_speed(116.0, 100.0, ack_elapsed_s=5.0)
        assert result.state == AlertState.CRITICAL
        assert result.time_to_brake_s == 0.0

    def test_evaluate_speed_acknowledged_does_not_trigger_braking(self):
        """
        Requirement: REQ-ATP-104
        Scenario: Driver acknowledged alert (ack_elapsed_s is None)
        Expected: ACKNOWLEDGED state, no braking triggered
        """
        result = evaluate_speed(120.0, 100.0, ack_elapsed_s=None)
        assert result.state == AlertState.ACKNOWLEDGED
        assert result.time_to_brake_s is None

    # --- Boundary Value Tests ---

    def test_evaluate_speed_exactly_at_warn_threshold_no_alert(self):
        """Boundary: exactly 5.0 km/h over limit = no alert (threshold is EXCLUSIVE)."""
        result = evaluate_speed(105.0, 100.0, ack_elapsed_s=0.0)
        assert result.state == AlertState.NONE

    def test_evaluate_speed_zero_overspeed_no_alert(self):
        """Boundary: exactly at limit = no alert."""
        result = evaluate_speed(100.0, 100.0, ack_elapsed_s=0.0)
        assert result.state == AlertState.NONE

    def test_evaluate_speed_zero_current_speed_no_alert(self):
        """Boundary: stationary train = no alert."""
        result = evaluate_speed(0.0, 100.0, ack_elapsed_s=0.0)
        assert result.state == AlertState.NONE

    # --- Fault Injection Tests (SAFETY-CRITICAL) ---

    def test_evaluate_speed_nan_current_speed_raises_value_error(self):
        """
        Fault injection: NaN speed (sensor fault)
        Expected: ValueError — do NOT silently proceed
        SIL Level: SIL 3
        """
        with pytest.raises(ValueError, match="Invalid current_kmh"):
            evaluate_speed(math.nan, 100.0, ack_elapsed_s=0.0)

    def test_evaluate_speed_inf_current_speed_raises_value_error(self):
        """Fault injection: Inf speed (sensor overflow)."""
        with pytest.raises(ValueError, match="Invalid current_kmh"):
            evaluate_speed(math.inf, 100.0, ack_elapsed_s=0.0)

    def test_evaluate_speed_negative_current_speed_raises_value_error(self):
        """Fault injection: Negative speed (sensor or encoding fault)."""
        with pytest.raises(ValueError, match="current_kmh must be >= 0"):
            evaluate_speed(-1.0, 100.0, ack_elapsed_s=0.0)

    def test_evaluate_speed_zero_limit_raises_value_error(self):
        """Fault injection: Zero speed limit (configuration fault)."""
        with pytest.raises(ValueError, match="limit_kmh must be > 0"):
            evaluate_speed(50.0, 0.0, ack_elapsed_s=0.0)

    def test_evaluate_speed_nan_limit_raises_value_error(self):
        """Fault injection: NaN speed limit (data corruption)."""
        with pytest.raises(ValueError, match="Invalid limit_kmh"):
            evaluate_speed(50.0, math.nan, ack_elapsed_s=0.0)
```

## References

- [`skills/hitachi-rail-gts/safety-critical-code/`](../safety-critical-code/) — Safety code patterns
- [`skills/hitachi-rail-gts/requirements-analysis/`](../requirements-analysis/) — Where these requirements came from
- EN 50128 §6.11 — Software Testing
