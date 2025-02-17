# Workout Data Normalization Instructions

You are tasked with normalizing workout data into a consistent YAML-like format. Follow these instructions carefully.

## Input Data Understanding

You will receive workout data in various formats including:
1. Split weights notation: `10@10+10` (indicating reps@weight+weight)
2. Array reps notation: `[15,15]@35` (indicating [left,right]@weight)
3. Standard notation: `10@50` (indicating reps@weight)
4. Bodyweight exercises: `15@0` (indicating reps with no weight)

## Core Normalization Rules

### Exercise Name Standardization
- Use Title Case consistently
- Remove any double spaces
- Standardize variations:
  - "Biceps Curls" → "Preacher Curl"
  - "Shoulders Press" → "Shoulder Press"
  - "Shoulder Raises Side" → "Lateral Raise"
  - "Shoulder Raises Front" → "Front Raise"
  - "Rows" → "Bench Row"
  - "Push Up" → "Push-Up"
  - "Pull Up" → "Pull-Up"

### Weight Format Normalization

For split weights (`a@b+b` format):
```
Input: 10@10+10
Process: Keep reps, sum weights
Output: 10@20
```

For array reps (`[a,b]@c` format):
```
Input: [15,15]@35
Process: Sum reps, keep weight
Output: 30@35
```

### Data Structure Requirements
- Use YAML-like hierarchy
- Date as top-level key (YYYY-MM-DD format)
- Exercise names as second-level keys
- Sets as arrays under exercises
- Trailing commas after complete sets should be ignored
- Remove any incomplete sets (sets missing weight or rep values)
- Maintain consistent indentation

## Output Format Specification

```yaml
YYYY-MM-DD:
  Exercise Name:
    - reps@weight
    - reps@weight
  Another Exercise:
    - reps@weight
    - reps@weight
```

## Example Transformations

### Input Example 1:
```
2024-07-15
* Shoulder Raises Front - 10@10+10, 10@10+10, 10@10+10,
* Bench Row - [15,15]@35, [15,15]@35, [15,15]@35
```

### Output Example 1:
```yaml
2024-07-15:
  Front Raise:
    - 10@20
    - 10@20
    - 10@20
  Bench Row:
    - 30@35
    - 30@35
    - 30@35
```

### Input Example 2:
```
2024-07-18
• ⁃ Preacher Biceps Curls - 10@60, 15@60, 15@60, 15@60,
• ⁃ Shoulder Raises Front - [10,10]@10, [15,15]@10, [12,12]@10,
```

### Output Example 2:
```yaml
2024-07-18:
  Preacher Curl:
    - 10@60
    - 15@60
    - 15@60
    - 15@60
  Front Raise:
    - 20@10
    - 30@10
    - 24@10
```

## Data Preservation and Validation Requirements

1. Date Validation:
   - Verify all dates are in YYYY-MM-DD format
   - Flag dates that appear outside expected range (e.g., past dates or far future)
   - Identify potential date typos (e.g., wrong year)
   - Request user verification for any suspicious dates
   - Never remove or modify dates without user confirmation

2. Exercise Validation:
   - Each exercise must have at least one complete set
   - Each set must have both reps and weight values
   - Remove sets missing either reps or weight values

3. Format Validation:
   - Consistent indentation throughout
   - No trailing spaces
   - Proper YAML structure
   - Note: Trailing commas after sets should be preserved - the set is considered complete and valid

## Processing Steps

1. Input Processing:
   - Read and parse the input format
   - Identify exercise names and set patterns
   - Extract reps and weights

2. Normalization:
   - Apply name standardization rules
   - Convert weight formats
   - Structure data hierarchically
   - Preserve sets with trailing commas

3. Validation:
   - Check all validation requirements
   - Remove sets missing rep or weight values
   - Verify YAML structure

4. Output Generation:
   - Format in YAML structure
   - Apply consistent indentation
   - Remove any extra whitespace
   - Keep trailing commas in the original data

## Error Handling and Data Preservation

When encountering ambiguous, unclear, or potentially invalid inputs:
1. Never discard or remove data without user confirmation
2. Flag suspicious or irregular data patterns for user review
3. Preserve original information in comments when normalizing
4. Document all assumptions and transformations made
5. For incomplete sets or entries:
   - Only remove if missing essential data (reps or weights)
   - Add comments explaining any removals
   - Request user guidance on how to handle them
6. For asymmetric exercises (e.g., uneven reps [25,24]@35):
   - Preserve the asymmetry in comments
   - Note the deviation from standard patterns
   - Ask for verification if pattern differs significantly

## Additional Notes

- Maintain data integrity at all times
- When in doubt about exercise names, preserve the original with a note
- Bodyweight exercises should always use @0 weight notation
- Multiple sets should be preserved in order
- For any data that appears invalid or incomplete:
  - Remove only if missing essential data (reps or weights)
  - Flag for user review
  - Add comments explaining the issue
  - Request specific guidance on handling
- Track all transformations applied to the data
- Create a separate validation report listing:
  - Suspicious dates
  - Sets missing rep or weight values
  - Asymmetric exercises
  - Unusual patterns
  - Any assumptions made during normalization
