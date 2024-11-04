# Family Tree Database Structure - Version 2023-05-24

## Overview
This document outlines the structure for a comprehensive family tree database using Neo4j. It incorporates various features to support detailed genealogical research, including temporal data, confidence levels, custom fields, and historical context.

## Core Structure

### Person Node
- person_id (UUID, primary key)
- first_name, middle_name, last_name
- alt_01_first_name, alt_01_middle_name, alt_01_last_name
- alt_02_first_name, alt_02_middle_name, alt_02_last_name
- preferred_name
- name_variants (array)
- birth_date, death_date (RFC3339 format)
- occupation, education, religious_affiliation, military_service
- immigration_details
- certainty_level
- privacy_level
- notes
- data_quality_score
- date_created, last_modified, created_by, last_modified_by

### Relationship Types
- PARENT_OF
- CHILD_OF
- SPOUSE_OF
- SIBLING_OF
- STEP_PARENT_OF
- ADOPTED_PARENT_OF
- GUARDIAN_OF
- IN_LAW_OF

### Event Node
- event_id (UUID)
- event_type
- date
- description

### Place Node
- place_id (UUID)
- name
- type
- coordinates
- time_period

### Tag Node
- tag_id (UUID)
- name
- description

### Media Node
- media_id (UUID)
- type
- url
- description

### CustomField Node
- field_id (UUID)
- name
- value
- type

### Family Node
- family_id (UUID)
- name
- description

### HistoricalEvent Node
- event_id (UUID)
- name
- date
- description
- impact_level

### ResearchTask Node
- task_id (UUID)
- description
- status
- priority
- created_date
- due_date

### Source Node
- source_id (UUID)
- title
- author
- publication_date
- type
- url

## Key Features

1. UUID as primary key for all nodes
2. Flexible naming system with alternative names and variants
3. Temporal aspects for relationships (start_date, end_date)
4. Confidence levels for both nodes and relationships
5. Custom fields for person-specific information
6. Data quality scoring
7. Family grouping
8. Historical context integration
9. Research task tracking
10. Comprehensive event and place modeling
11. Media and source linking

## Data Validation

- Implement date range validations (e.g., birth before death)
- Age-based relationship validations
- Logical relationship validations
- Date format validation (RFC3339)
- Required field validation
- Certainty and privacy level validation

## Additional Considerations

- Bulk operations support
- Internationalization (multiple calendars, native language names)
- Occupation and education history tracking
- Health information (if appropriate)
- Property ownership tracking
- Expanded military service information
- Religion changes over time
- Migration path visualization
- Conflicting information management
- Specific relationship roles (e.g., godparent, mentor)
- Timeline generation functionality
- Collaborative features for multiple researchers
- Data export in common genealogical formats
- Potential AI integration for advanced features
- Tiered privacy system

## Next Steps

1. Implement the core structure in Neo4j
2. Develop data validation rules and procedures
3. Create API endpoints for CRUD operations, including bulk operations
4. Implement the data quality scoring system
5. Develop user interface for data entry and visualization
6. Test with sample data and refine as needed

This structure provides a solid foundation for a comprehensive family tree database. Future sessions can focus on implementing specific features, refining the data model, or addressing any challenges that arise during development.
