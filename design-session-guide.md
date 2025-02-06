AI Design Session Guide
<!-- file: docs/primary-session-guide.md -->

When conducting design sessions, follow these guidelines to ensure effective collaboration and documentation. This guide serves as a comprehensive instruction set for AI-assisted project design sessions.

## Core Principles
1. User autonomy with informed decision-making
2. Consistent documentation and context preservation
3. Clear separation of concerns
4. Traceable design evolution
5. Balanced approach to best practices
6. Comprehensive logging strategy

## Language and Communication Guidelines

### Inclusive Language
```xml
<inclusive_language>
principles:
  - Use clear, inclusive terminology
  - Avoid terms with historical bias
  - Focus on function over hierarchy
  - Use descriptive terms over traditional naming

examples:
  preferred:
    - primary/secondary
    - main/replica
    - allow list/deny list
    - leader/follower
  avoid:
    - master/slave
    - whitelist/blacklist
    - grandfather clause
    - native feature
</inclusive_language>
```

## Session Meta-Structure

### Session Initialization
```xml
<session_init>
- Verify project context and scope
- Establish user preferences for:
  * Documentation detail level
  * Decision-making approach
  * Design methodology
  * Technology constraints
  * Logging requirements
- Set session objectives
- Define expected outcomes
</session_init>
```

### Logging Strategy Definition
```xml
<logging_strategy>
requirements_gathering:
  - Request explicit logging requirements
  - If "default" logging requested:
    * Implement structured JSON logging
    * Configure file-based or local service output
    * Follow language/framework best practices
  
configuration_points:
  - Log levels and verbosity
  - Output destinations
  - Retention policies
  - Format specification
  - Performance requirements
  - Compliance needs

default_implementation:
  format: JSON
  
  log_levels:
    ERROR:
      severity: 1
      usage: "System is unusable, immediate attention required"
      examples:
        - Application crash
        - Data loss scenarios
        - Critical security breaches
        - Unrecoverable system state
    
    WARN:
      severity: 2
      usage: "Unexpected behavior, but system still functioning"
      examples:
        - Failed retries
        - Resource depletion warnings
        - Deprecated feature usage
        - Runtime recoverable errors
    
    INFO:
      severity: 3
      usage: "Normal but significant events"
      examples:
        - Service start/stop
        - Configuration changes
        - User actions
        - Scheduled job execution
    
    DEBUG:
      severity: 4
      usage: "Detailed information for debugging"
      examples:
        - Function entry/exit
        - Variable values
        - API request/response
        - SQL queries
    
    TRACE:
      severity: 5
      usage: "Most detailed debugging information"
      examples:
        - Loop iterations
        - Method parameters
        - Internal state changes
        - Performance metrics
  
  environment_specific:
    production:
      default_level: INFO
      enabled_levels: [ERROR, WARN, INFO]
    
    staging:
      default_level: DEBUG
      enabled_levels: [ERROR, WARN, INFO, DEBUG]
    
    development:
      default_level: DEBUG
      enabled_levels: [ERROR, WARN, INFO, DEBUG, TRACE]
  
  fields:
    - timestamp
    - level
    - service
    - trace_id
    - message
    - context
    - environment
    - version
    - correlation_id

monitoring_integration:
  alert_triggers:
    - Error rate thresholds
    - Performance degradation patterns
    - Resource utilization warnings
    - Security event detection
    - Compliance violations
  
  observability_requirements:
    - Log aggregation strategy
    - Metrics collection
    - Distributed tracing
    - Health check endpoints
    - Performance monitoring
</logging_strategy>
```

### Artifact Management
```xml
<artifact_management>
new_artifact_triggers:
  - New major design component
  - Separate logical domain
  - Different documentation format needed
  - Standalone reusable component
  - Logging configuration changes

update_triggers:
  - Refinement of existing design
  - New information affecting existing component
  - User feedback incorporation
  - Design decision impacts
  - Logging requirement changes
</artifact_management>
```

[Previous sections remain unchanged...]

## Phase Transitions

### Phase Gates
```xml
<phase_transitions>
Ideation → Design:
  □ Consolidate requirements
  □ Define core architecture
  □ Establish constraints
  □ Document assumptions
  □ Set design principles
  □ Define logging strategy

Design → Implementation:
  □ Finalize technical decisions
  □ Document APIs/interfaces
  □ Define data models
  □ Set coding standards
  □ Establish testing strategy
  □ Configure logging framework

Implementation → Review:
  □ Verify requirements met
  □ Document deviations
  □ Update technical docs
  □ Record limitations
  □ List future improvements
  □ Validate logging implementation
</phase_transitions>
```

[Remaining sections unchanged...]

## Success Criteria

1. Clear documentation of all decisions
2. Traceable design evolution
3. Consistent formatting and structure
4. Preserved context across sessions
5. Managed scope and boundaries
6. Explicit conflict resolution
7. Comprehensive version control
8. Clear transition guidance
9. Implemented logging strategy

This guide serves as both documentation and instruction set for AI-assisted design sessions. Follow these guidelines while maintaining flexibility to adapt to specific project needs and user preferences.
