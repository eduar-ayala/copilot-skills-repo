# Testing Skill — General

This directory contains examples for the `/testing` prompt skill.

## Skill File

**Prompt file**: [`.github/prompts/testing.prompt.md`](../../../.github/prompts/testing.prompt.md)

## How to Use

1. Open the source file you want tests for.
2. Open Copilot Chat (`Ctrl+Alt+I` / `Cmd+Option+I`).
3. Type `/testing` and press Enter.

## What It Generates

- Happy path tests
- Boundary value tests
- Equivalence class tests
- Error/exception tests
- Edge case tests
- Properly mocked external dependencies

## Example Input

```python
def calculate_fare(distance_km: float, passenger_type: str) -> float:
    """Calculate train fare based on distance and passenger type."""
    BASE_RATE = 0.15  # £ per km
    if distance_km <= 0:
        raise ValueError("Distance must be positive")
    if passenger_type not in ("adult", "child", "senior"):
        raise ValueError(f"Unknown passenger type: {passenger_type}")

    fare = distance_km * BASE_RATE
    if passenger_type == "child":
        fare *= 0.5
    elif passenger_type == "senior":
        fare *= 0.75
    return round(fare, 2)
```

## Example Output (Generated Tests)

```python
import pytest
from fare_calculator import calculate_fare


class TestCalculateFare:

    # --- Happy Path Tests ---

    def test_calculate_fare_adult_short_distance_returns_correct_fare(self):
        """Adult passenger, 10 km trip at £0.15/km = £1.50."""
        result = calculate_fare(10.0, "adult")
        assert result == 1.50

    def test_calculate_fare_child_applies_50_percent_discount(self):
        """Child passengers pay 50% of adult fare."""
        result = calculate_fare(10.0, "child")
        assert result == 0.75

    def test_calculate_fare_senior_applies_25_percent_discount(self):
        """Senior passengers pay 75% of adult fare."""
        result = calculate_fare(10.0, "senior")
        assert result == 1.13  # round(1.50 * 0.75, 2)

    # --- Boundary Value Tests ---

    def test_calculate_fare_minimum_valid_distance_returns_fare(self):
        """Smallest positive float distance is valid."""
        result = calculate_fare(0.01, "adult")
        assert result == 0.0  # rounds to £0.00 for very short trips

    def test_calculate_fare_zero_distance_raises_value_error(self):
        """Distance of exactly 0 is invalid."""
        with pytest.raises(ValueError, match="Distance must be positive"):
            calculate_fare(0.0, "adult")

    def test_calculate_fare_large_distance_returns_correct_fare(self):
        """Very large distance (cross-country) should work correctly."""
        result = calculate_fare(1000.0, "adult")
        assert result == 150.0

    # --- Equivalence Class Tests ---

    def test_calculate_fare_negative_distance_raises_value_error(self):
        """Negative distances are invalid (invalid partition)."""
        with pytest.raises(ValueError, match="Distance must be positive"):
            calculate_fare(-5.0, "adult")

    def test_calculate_fare_unknown_passenger_type_raises_value_error(self):
        """Unrecognized passenger types are invalid."""
        with pytest.raises(ValueError, match="Unknown passenger type: student"):
            calculate_fare(10.0, "student")

    # --- Edge Case Tests ---

    def test_calculate_fare_fare_is_rounded_to_two_decimal_places(self):
        """Fares are always rounded to 2 decimal places."""
        result = calculate_fare(7.0, "senior")  # 7 * 0.15 * 0.75 = 0.7875 → 0.79
        assert result == 0.79

    def test_calculate_fare_empty_string_passenger_type_raises_value_error(self):
        """Empty string is not a valid passenger type."""
        with pytest.raises(ValueError):
            calculate_fare(10.0, "")
```

## Customization Tips

- To target a specific test framework: add "Use JUnit 5 / Jest / Mocha" to the chat.
- To increase coverage: add "Include parameterized tests for all passenger types and 5 distance values."
- For safety-critical code: use the `/test-generation` skill instead, which adds SIL-specific requirements.
