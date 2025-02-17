# Comprehensive Workout Log Parsing Instructions

## Objective
Transform raw workout logs into a standardized, machine-readable JSON format that captures maximum detail while maintaining clarity and consistency.

## Parsing Workflow

### 1. Input Preparation
- Remove any extraneous whitespace
- Normalize line breaks
- Ensure consistent date formatting (YYYY-MM-DD)

### 2. Parsing Rules

#### 2.1 Top-Level Structure
```json
{
  "workouts": [
    {
      "date": "YYYY-MM-DD",
      "exercises": [ ... ]
    }
  ]
}
```

#### 2.2 Exercise Parsing Guidelines

##### Naming Normalization
1. Standardize Exercise Names
   - Remove extra whitespaces
   - Correct capitalization
   - Use consistent anatomical terminology
   - Standardize abbreviations

Conversion Examples:
- "scull crushers" → "Skull Crushers"
- "step up" → "Step Ups"
- "Shoulders Lateral" → "Lateral Shoulder Raises"
- "Shoulders Anterior" → "Anterior Shoulder Raises"

##### Weight and Rep Notation Handling

###### 1. Standard Notation: `reps@weight`
- Example: `10@60`
- Parsing: Straightforward representation
```json
{
  "reps": 10,
  "weight": 60
}
```

###### 2. Symmetric Exercise Notation: `[reps,reps]@weight`
- Represents reps per side
- Example: `[25,25]@0`
```json
{
  "reps": 50,
  "weight": 0,
  "weightType": "bodyweight",
  "metadata": {
    "perSideReps": [25, 25]
  }
}
```

###### 3. Asymmetric Symmetric Notation
- Example: `[25,24]@35`
```json
{
  "reps": 49,
  "weight": 35,
  "weightType": "total",
  "metadata": {
    "perSideReps": [25, 24],
    "asymmetryNoted": true
  }
}
```

###### 4. Bodyweight/No Weight Exercises
- Reps without weight
- Example: `40, 40, 30`
```json
{
  "reps": 40,
  "weight": 0,
  "weightType": "bodyweight"
}
```

### 3. Detailed Parsing Steps

#### 3.1 Date Processing
- Validate date format
- Ensure chronological order
- Handle potential date inconsistencies

#### 3.2 Exercise Processing
1. Normalize exercise name
2. Parse each set
3. Capture all available information
4. Add metadata for complex exercises

#### 3.3 Special Handling
- Preserve incomplete workouts
- Flag partial sets
- Maintain original rep and weight information

### 4. Error Detection and Handling

#### Validation Checks
- Verify non-negative numerical values
- Detect unrealistic weights/reps
- Identify potential data entry errors

#### Confidence Levels
- Level 0: Parsing Failure
- Level 1: Major Uncertainties
- Level 2: Moderate Challenges
- Level 3: Minor Clarifications
- Level 4: High Confidence Parsing

### 5. Metadata Enrichment

#### Optional Metadata Fields
```json
{
  "metadata": {
    "perSideReps": [25, 25],
    "perSideWeight": 35,
    "asymmetryNoted": false,
    "incomplete": false,
    "notes": "Additional context if needed"
  }
}
```

## Parsing Philosophy
- Preserve original intent
- Maximize information capture
- Provide clear, consistent representation
- Allow for future analytical capabilities

## Recommended AI Workflow
1. Validate input format
2. Normalize data
3. Parse date and exercises
4. Apply naming standardization
5. Handle complex notations
6. Generate JSON output
7. Perform validation checks
8. Report any uncertainties

## Example Transformation

### Input
```
2025-01-05
* Bench press - 8@125, 7@125, 5@125, 5@125
* Incline Chest Barbells - 15@[25,25], 15@[25,25], 15@[25,25], 15@[25,25]
```

### Output
```json
{
  "workouts": [
    {
      "date": "2025-01-05",
      "exercises": [
        {
          "name": "Bench Press",
          "sets": [
            {"reps": 8, "weight": 125},
            {"reps": 7, "weight": 125},
            {"reps": 5, "weight": 125},
            {"reps": 5, "weight": 125}
          ]
        },
        {
          "name": "Incline Chest Barbells",
          "sets": [
            {
              "reps": 15,
              "weight": 50,
              "weightType": "total",
              "metadata": {
                "perSideReps": [15, 15],
                "perSideWeight": 25
              }
            }
            // ... additional sets ...
          ]
        }
      ]
    }
  ]
}
```

## Future Improvements
- Machine learning-based name normalization
- Enhanced error detection
- Predictive workout analysis
- Adaptive parsing capabilities
