# Workout Log Parser

## Purpose
Convert raw workout log text into a standardized JSON format.

## Input Format
Raw text with workouts in the following format:
```
YYYY-MM-DD
* Exercise Name - set1@weight, set2@weight, ...
* Another Exercise - set1@weight, set2@weight, ...
```

## Output Format
JSON object with the following structure:
```json
{
  "workouts": [
    {
      "date": "YYYY-MM-DD",
      "exercises": [
        {
          "name": "Exercise Name",
          "sets": [
            {
              "reps": number,
              "weight": number,
              "weightType": string (optional),
              "metadata": object (optional)
            }
          ],
          "metadata": object (optional)
        }
      ]
    }
  ]
}
```

## Parsing Rules

### Exercise Names
- Normalize capitalization
- Standardize terminology
- Examples:
  - "scull crushers" → "Skull Crushers"
  - "Shoulders Lateral" → "Lateral Shoulder Raises"

### Set Notation Types

1. Standard: `reps@weight`
   ```
   10@60 → {"reps": 10, "weight": 60}
   ```

2. Per-Side Weights: `reps@weight+weight`
   ```
   10@35+35 → {
     "reps": 10,
     "weight": 70,
     "metadata": {"perSideWeight": 35}
   }
   ```

3. Per-Side Reps: `leftReps+rightReps@weight`
   ```
   5+5@0 → {
     "reps": 10,
     "weight": 0,
     "weightType": "bodyweight",
     "metadata": {"perSideReps": [5, 5]}
   }
   ```

4. Bodyweight/No Weight: `reps@0`
   ```
   15@0 → {
     "reps": 15,
     "weight": 0,
     "weightType": "bodyweight"
   }
   ```

### Special Cases

1. Incomplete Sets
   - When entry ends with comma
   - Assume the workout is complete

2. Missing Data
   - Skip invalid entries
   - Document in parsing report

## Example

Input:
```
2024-07-15
* Bench Press - 8@115, 8@115, 6@115
* Shoulder Raises Side - 10@10+10, 10@10+10,
```

Output:
```json
{
  "workouts": [
    {
      "date": "2024-07-15",
      "exercises": [
        {
          "name": "Bench Press",
          "sets": [
            {"reps": 8, "weight": 115},
            {"reps": 8, "weight": 115},
            {"reps": 6, "weight": 115}
          ]
        },
        {
          "name": "Lateral Shoulder Raises",
          "sets": [
            {"reps": 10, "weight": 20, "metadata": {"perSideWeight": 10}},
            {"reps": 10, "weight": 20, "metadata": {"perSideWeight": 10}}
          ],
          "metadata": {"incomplete": true}
        }
      ]
    }
  ]
}
```
