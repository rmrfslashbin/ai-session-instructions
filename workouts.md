# Workout Log Parsing and Structuring Prompt

## Objective
Parse workout logs into a structured, consistent, and machine-readable format that captures all nuanced workout details with maximum flexibility and precision.

## Advanced Notation Parsing Guidelines

### Symmetric Exercise Notations
1. Bracket Notation `[reps,reps]`
   - Represents reps per side
   - Examples:
     * `[25,25]@0`: 25 reps per side, bodyweight
     * `[10,10]@25`: 10 reps per side, 25 lbs total

2. Handling Asymmetric Symmetric Sets
   - Preserve exact rep differences
   - Example: `[25,24]@35` 
     * Capture precise per-side rep count
     * Note variation in reps

### Weight and Rep Representation
- Support multiple weight representation formats:
  * Standard: `10@60`
  * Symmetric: `[10,10]@25`
  * Bodyweight: `@0`
  * No explicit weight: `40, 40, 30`

### Metadata Enrichment
```json
{
  "reps": 25,
  "weight": 35,
  "weightType": "total",
  "metadata": {
    "perSideReps": [25, 25],
    "perSideWeight": 35,
    "asymmetryNoted": false
  }
}
```

## Comprehensive Parsing Rules

### Exercise Naming Normalization
- Standardization Process:
  1. Remove extra whitespaces
  2. Correct capitalization
  3. Standardize anatomical terms
  4. Handle variations consistently

Naming Conversion Examples:
- "scull crushers" → "Skull Crushers"
- "Shoulders Lateral" → "Lateral Shoulder Raises"
- "step up" → "Step Ups"

### Incomplete Entry Handling
- Preserve partial workouts
- Flag incomplete sets
- Maintain chronological integrity
- Allow optional metadata for context

### Numerical Value Processing
- Handle various rep counting methods
  * Explicit reps with weight
  * Bodyweight exercises
  * Sets without explicit weight
- Validate numerical sanity
- Provide clear error/uncertainty markers

## Advanced Parsing Scenarios

### Bodyweight and No-Weight Exercises
```json
{
  "name": "Step Ups",
  "sets": [
    {
      "reps": 25,
      "weight": 0,
      "weightType": "bodyweight",
      "metadata": {
        "perSideReps": [25, 25]
      }
    }
  ]
}
```

### Asymmetric Symmetric Exercise
```json
{
  "name": "Obliques",
  "sets": [
    {
      "reps": 50,
      "weight": 35,
      "weightType": "total",
      "metadata": {
        "perSideReps": [25, 25],
        "asymmetryNoted": true,
        "actualPerSideReps": [25, 24]
      }
    }
  ]
}
```

## Error Detection and Uncertainty Handling

### Validation Strategies
- Detect and flag:
  * Extreme or unrealistic values
  * Incomplete exercise records
  * Naming inconsistencies
  * Potential data entry errors

### Confidence Levels
- Level 0: Parsing Failure
- Level 1: Major Uncertainties
- Level 2: Moderate Parsing Challenges
- Level 3: Minor Clarifications Needed
- Level 4: High-Confidence Parsing

## User Logging Recommendations
- Use consistent exercise naming
- Explicitly note symmetric exercise details
- Provide context for partial or unusual workouts
- Prefer clear, unambiguous notations

## Parsing Philosophy
Transform raw workout data into a precise, flexible, and informative representation that captures the nuanced details of physical training while maintaining the integrity of the original recording.

### Future Evolution
- Continuous improvement of parsing rules
- Adaptive learning from user input
- Enhanced exercise and movement recognition
- Predictive workout analysis capabilities

