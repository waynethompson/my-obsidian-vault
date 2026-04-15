# Team Capability Assessment - Requirements Specification

**Feature Name:** Team Capability Assessment  
**Version:** 1.0  
**Last Updated:** October 5, 2025  
**Status:** Planning Phase  
**Part of:** Teaminate Platform

---

## Table of Contents
1. [Executive Summary](#executive-summary)
2. [Feature Overview](#feature-overview)
3. [User Stories](#user-stories)
4. [Domain Model](#domain-model)
5. [API Specifications](#api-specifications)
6. [Business Logic](#business-logic)
7. [UI/UX Requirements](#uiux-requirements)
8. [Technical Requirements](#technical-requirements)
9. [Integration Points](#integration-points)
10. [Success Metrics](#success-metrics)

---

## Executive Summary

The Team Capability Assessment feature enables team managers and leaders to evaluate their team's practices against the 24 capabilities of high-performing teams identified in the book "Accelerate" by Nicole Forsgren, Jez Humble, and Gene Kim. The feature provides a wizard-style assessment interface, generates traffic-light scored reports, and delivers actionable recommendations for improvement.

### Key Value Propositions
- **Standardized Assessment**: Evaluate teams against research-backed DevOps and organizational capabilities
- **Actionable Insights**: Receive specific, prioritized recommendations for capability improvements
- **Progress Tracking**: Monitor team capability improvements over time
- **Benchmarking**: Compare team performance across capability categories

---

## Feature Overview

### Purpose
To provide teams with a structured, repeatable way to assess their current state across critical DevOps and organizational capabilities and identify specific areas for improvement.

### Key Capabilities
1. Guided wizard-style assessment with 24 capability questions
2. Traffic-light scoring system (Red/Yellow/Green)
3. Category-based capability grouping (5 categories)
4. Detailed assessment reports with visualizations
5. Prioritized recommendations based on current state
6. Historical assessment tracking and comparison
7. Team and organizational-level reporting

### Target Users
- **Primary**: Engineering Managers, Team Leads, DevOps Managers
- **Secondary**: Directors of Engineering, CTOs, Organizational Development Teams
- **Tertiary**: Individual Contributors (for self-assessment)

---

## User Stories

### Epic 1: Assessment Execution
**US-CAP-001**: As a team manager, I want to complete a capability assessment for my team so that I can understand our current state across all 24 capabilities.

**US-CAP-002**: As an assessment taker, I want to see my progress through the assessment so that I know how much remains to complete.

**US-CAP-003**: As an assessment taker, I want to be able to go back and change previous answers so that I can correct mistakes.

**US-CAP-004**: As an assessment taker, I want questions grouped by capability category so that I can understand the context of what I'm evaluating.

### Epic 2: Results and Reporting
**US-CAP-005**: As a team manager, I want to see a visual dashboard of my team's capability scores so that I can quickly understand our strengths and weaknesses.

**US-CAP-006**: As a team manager, I want to receive prioritized recommendations based on our assessment so that I know what to focus on first.

**US-CAP-007**: As a team manager, I want to export assessment results as a PDF so that I can share findings with leadership.

**US-CAP-008**: As a team manager, I want to see detailed explanations for each capability so that I can understand what we're being measured against.

### Epic 3: Historical Tracking
**US-CAP-009**: As a team manager, I want to view previous assessment results so that I can track our improvement over time.

**US-CAP-010**: As a team manager, I want to compare two assessments side-by-side so that I can see specific areas where we've improved or regressed.

**US-CAP-011**: As a team manager, I want to see trend lines for individual capabilities so that I can monitor progress on specific improvement areas.

### Epic 4: Organization-Level Features
**US-CAP-012**: As a director, I want to see aggregated capability scores across all my teams so that I can identify organizational patterns.

**US-CAP-013**: As a director, I want to compare teams within my organization so that I can identify high-performing teams and share best practices.

---

## Domain Model

### Core Entities

#### 1. CapabilityAssessment
Represents a complete assessment instance for a team.

```csharp
public class CapabilityAssessment
{
    // Identity
    public Guid Id { get; set; }
    public string PartitionKey { get; set; } // Format: "ORG#{OrganizationId}"
    
    // Relationships
    public Guid TeamId { get; set; }
    public Guid OrganizationId { get; set; }
    public Guid CreatedByUserId { get; set; }
    
    // Assessment Metadata
    public string AssessmentName { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime? CompletedAt { get; set; }
    public AssessmentStatus Status { get; set; }
    
    // Assessment Data
    public List<CapabilityResponse> Responses { get; set; }
    
    // Scoring Results (computed)
    public AssessmentScore OverallScore { get; set; }
    public Dictionary<string, CategoryScore> CategoryScores { get; set; }
    
    // Metadata
    public int TotalQuestions { get; set; }
    public int CompletedQuestions { get; set; }
    public int Version { get; set; } // Schema version for capability definitions
    
    // Audit
    public DateTime LastModifiedAt { get; set; }
    public string LastModifiedBy { get; set; }
}

public enum AssessmentStatus
{
    Draft,
    InProgress,
    Completed,
    Archived
}
```

#### 2. CapabilityResponse
Represents a single answer to a capability question.

```csharp
public class CapabilityResponse
{
    public Guid Id { get; set; }
    public Guid CapabilityDefinitionId { get; set; }
    public string CapabilityName { get; set; }
    public string Category { get; set; }
    
    // Response Data
    public int SelectedOptionIndex { get; set; }
    public int Score { get; set; } // 0, 1, or 2
    public ScoreLevel Level { get; set; } // Red, Yellow, Green
    
    // Additional Context
    public string Notes { get; set; } // Optional notes from assessor
    public DateTime AnsweredAt { get; set; }
    
    // Question snapshot (for historical accuracy)
    public string QuestionText { get; set; }
    public List<ResponseOption> AvailableOptions { get; set; }
}

public enum ScoreLevel
{
    Red = 0,      // Not Started
    Yellow = 1,   // Working Towards
    Green = 2     // Achieved
}

public class ResponseOption
{
    public string Label { get; set; }
    public int Value { get; set; }
}
```

#### 3. CapabilityDefinition
Represents the master definition of a capability from the Accelerate framework.

```csharp
public class CapabilityDefinition
{
    public Guid Id { get; set; }
    public string PartitionKey { get; set; } // "CAPABILITY_DEFINITION"
    
    // Capability Identity
    public string Name { get; set; }
    public string Code { get; set; } // e.g., "CD-001", "ARCH-001"
    public CapabilityCategory Category { get; set; }
    public int DisplayOrder { get; set; }
    
    // Assessment Content
    public string Question { get; set; }
    public List<AssessmentOption> Options { get; set; }
    public string Description { get; set; }
    public string Recommendation { get; set; }
    
    // References
    public string AccelerateChapterReference { get; set; }
    public List<string> RelatedCapabilities { get; set; }
    public List<string> Tags { get; set; }
    
    // Version Control
    public int Version { get; set; }
    public DateTime CreatedAt { get; set; }
    public DateTime? DeprecatedAt { get; set; }
    public bool IsActive { get; set; }
}

public enum CapabilityCategory
{
    ContinuousDelivery,
    Architecture,
    ProductAndProcess,
    LeanManagementAndMonitoring,
    Cultural
}

public class AssessmentOption
{
    public string Label { get; set; }
    public int Value { get; set; } // 0, 1, or 2
    public string Description { get; set; } // Optional detailed description
}
```

#### 4. AssessmentScore
Represents computed scoring results for an assessment.

```csharp
public class AssessmentScore
{
    public int TotalPoints { get; set; }
    public int MaxPossiblePoints { get; set; }
    public decimal PercentageScore { get; set; }
    public OverallPerformanceLevel PerformanceLevel { get; set; }
    
    public int GreenCount { get; set; }
    public int YellowCount { get; set; }
    public int RedCount { get; set; }
    
    public DateTime CalculatedAt { get; set; }
}

public enum OverallPerformanceLevel
{
    NeedsImprovement,  // < 40%
    Developing,        // 40-60%
    Competent,         // 60-80%
    HighPerforming     // > 80%
}
```

#### 5. CategoryScore
Represents scoring for a specific capability category.

```csharp
public class CategoryScore
{
    public CapabilityCategory Category { get; set; }
    public string CategoryName { get; set; }
    
    public int TotalPoints { get; set; }
    public int MaxPossiblePoints { get; set; }
    public decimal AverageScore { get; set; }
    public ScoreLevel Level { get; set; } // Red/Yellow/Green
    
    public int CapabilityCount { get; set; }
    public int GreenCount { get; set; }
    public int YellowCount { get; set; }
    public int RedCount { get; set; }
    
    public List<CapabilityScore> Capabilities { get; set; }
}

public class CapabilityScore
{
    public string CapabilityName { get; set; }
    public int Score { get; set; }
    public ScoreLevel Level { get; set; }
}
```

#### 6. AssessmentReport
Represents a generated report with recommendations.

```csharp
public class AssessmentReport
{
    public Guid Id { get; set; }
    public string PartitionKey { get; set; } // Format: "ORG#{OrganizationId}"
    
    public Guid AssessmentId { get; set; }
    public Guid TeamId { get; set; }
    public DateTime GeneratedAt { get; set; }
    
    // Report Content
    public AssessmentScore OverallScore { get; set; }
    public Dictionary<string, CategoryScore> CategoryScores { get; set; }
    public List<PriorityRecommendation> Recommendations { get; set; }
    public List<StrengthHighlight> Strengths { get; set; }
    
    // Report Metadata
    public string ReportType { get; set; } // "Standard", "Executive", "Detailed"
    public string Format { get; set; } // "HTML", "PDF", "JSON"
}

public class PriorityRecommendation
{
    public int Priority { get; set; } // 1 = highest
    public string CapabilityName { get; set; }
    public string Category { get; set; }
    public ScoreLevel CurrentLevel { get; set; }
    public string RecommendationText { get; set; }
    public List<string> ActionItems { get; set; }
    public string ExpectedImpact { get; set; }
    public string Difficulty { get; set; } // "Low", "Medium", "High"
}

public class StrengthHighlight
{
    public string CapabilityName { get; set; }
    public string Category { get; set; }
    public string StrengthDescription { get; set; }
}
```

#### 7. AssessmentComparison
Represents a comparison between two assessments.

```csharp
public class AssessmentComparison
{
    public Guid Id { get; set; }
    public Guid BaselineAssessmentId { get; set; }
    public Guid CurrentAssessmentId { get; set; }
    public Guid TeamId { get; set; }
    public DateTime CreatedAt { get; set; }
    
    // Comparison Results
    public ScoreComparison OverallComparison { get; set; }
    public Dictionary<string, CategoryComparison> CategoryComparisons { get; set; }
    public List<CapabilityChange> Changes { get; set; }
    
    // Summary
    public int ImprovedCount { get; set; }
    public int DeclinedCount { get; set; }
    public int UnchangedCount { get; set; }
}

public class ScoreComparison
{
    public decimal BaselineScore { get; set; }
    public decimal CurrentScore { get; set; }
    public decimal Change { get; set; }
    public decimal PercentageChange { get; set; }
    public TrendDirection Trend { get; set; }
}

public class CategoryComparison
{
    public CapabilityCategory Category { get; set; }
    public ScoreComparison Comparison { get; set; }
    public List<CapabilityChange> CapabilityChanges { get; set; }
}

public class CapabilityChange
{
    public string CapabilityName { get; set; }
    public string Category { get; set; }
    public ScoreLevel PreviousLevel { get; set; }
    public ScoreLevel CurrentLevel { get; set; }
    public ChangeType ChangeType { get; set; }
    public int ScoreChange { get; set; }
}

public enum TrendDirection
{
    Improved,
    Declined,
    Unchanged
}

public enum ChangeType
{
    Improved,
    Declined,
    Unchanged
}
```

#### 8. AssessmentTemplate
Represents a customizable assessment template.

```csharp
public class AssessmentTemplate
{
    public Guid Id { get; set; }
    public string PartitionKey { get; set; } // Format: "ORG#{OrganizationId}"
    
    public Guid OrganizationId { get; set; }
    public string Name { get; set; }
    public string Description { get; set; }
    
    // Template Configuration
    public List<Guid> IncludedCapabilityIds { get; set; }
    public Dictionary<string, string> CustomQuestions { get; set; }
    public List<string> DisabledCategories { get; set; }
    
    // Settings
    public bool IsDefault { get; set; }
    public bool IsPublic { get; set; }
    public DateTime CreatedAt { get; set; }
    public Guid CreatedBy { get; set; }
}
```

### Supporting Entities

#### 9. AssessmentSchedule
For recurring assessments.

```csharp
public class AssessmentSchedule
{
    public Guid Id { get; set; }
    public string PartitionKey { get; set; }
    
    public Guid TeamId { get; set; }
    public Guid OrganizationId { get; set; }
    
    public string ScheduleName { get; set; }
    public RecurrencePattern Recurrence { get; set; }
    public DateTime NextScheduledDate { get; set; }
    public DateTime? LastExecutedDate { get; set; }
    
    public bool IsActive { get; set; }
    public List<Guid> NotificationUserIds { get; set; }
    public Guid? TemplateId { get; set; }
}

public enum RecurrencePattern
{
    Weekly,
    Biweekly,
    Monthly,
    Quarterly,
    Annually
}
```

---

## API Specifications

### REST Endpoints

#### Assessment Management

```
POST   /api/v1/assessments
GET    /api/v1/assessments/{assessmentId}
PUT    /api/v1/assessments/{assessmentId}
DELETE /api/v1/assessments/{assessmentId}
GET    /api/v1/assessments/team/{teamId}
POST   /api/v1/assessments/{assessmentId}/complete
```

#### Assessment Responses

```
POST   /api/v1/assessments/{assessmentId}/responses
PUT    /api/v1/assessments/{assessmentId}/responses/{responseId}
GET    /api/v1/assessments/{assessmentId}/responses
```

#### Reporting

```
GET    /api/v1/assessments/{assessmentId}/report
POST   /api/v1/assessments/{assessmentId}/report/generate
GET    /api/v1/assessments/{assessmentId}/report/pdf
GET    /api/v1/assessments/compare?baseline={id}&current={id}
```

#### Capability Definitions

```
GET    /api/v1/capabilities
GET    /api/v1/capabilities/{capabilityId}
GET    /api/v1/capabilities/categories
```

#### Organization-Level

```
GET    /api/v1/organizations/{orgId}/assessments
GET    /api/v1/organizations/{orgId}/assessments/summary
GET    /api/v1/organizations/{orgId}/teams/comparison
```

### Request/Response Examples

#### POST /api/v1/assessments
**Request:**
```json
{
  "teamId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "assessmentName": "Q1 2025 Capability Assessment",
  "templateId": "3fa85f64-5717-4562-b3fc-2c963f66afa6" // optional
}
```

**Response:**
```json
{
  "id": "7fa85f64-5717-4562-b3fc-2c963f66afa6",
  "teamId": "3fa85f64-5717-4562-b3fc-2c963f66afa6",
  "assessmentName": "Q1 2025 Capability Assessment",
  "status": "Draft",
  "createdAt": "2025-01-15T10:00:00Z",
  "totalQuestions": 24,
  "completedQuestions": 0,
  "version": 1
}
```

#### POST /api/v1/assessments/{assessmentId}/responses
**Request:**
```json
{
  "capabilityDefinitionId": "1fa85f64-5717-4562-b3fc-2c963f66afa6",
  "selectedOptionIndex": 2,
  "notes": "We have full CI/CD pipeline with automated tests"
}
```

**Response:**
```json
{
  "id": "9fa85f64-5717-4562-b3fc-2c963f66afa6",
  "capabilityName": "Continuous Integration",
  "category": "ContinuousDelivery",
  "score": 2,
  "level": "Green",
  "answeredAt": "2025-01-15T10:15:00Z"
}
```

#### GET /api/v1/assessments/{assessmentId}/report
**Response:**
```json
{
  "assessmentId": "7fa85f64-5717-4562-b3fc-2c963f66afa6",
  "teamName": "Platform Engineering",
  "completedAt": "2025-01-15T11:00:00Z",
  "overallScore": {
    "totalPoints": 32,
    "maxPossiblePoints": 48,
    "percentageScore": 66.7,
    "performanceLevel": "Competent",
    "greenCount": 12,
    "yellowCount": 8,
    "redCount": 4
  },
  "categoryScores": {
    "ContinuousDelivery": {
      "averageScore": 1.75,
      "level": "Yellow",
      "capabilities": [...]
    }
  },
  "recommendations": [
    {
      "priority": 1,
      "capabilityName": "Version Control",
      "currentLevel": "Red",
      "recommendationText": "Implement version control for all artifacts...",
      "actionItems": [
        "Audit all production artifacts",
        "Set up Git repository structure",
        "Create migration plan"
      ],
      "difficulty": "Low"
    }
  ]
}
```

---

## Business Logic

### Scoring Algorithm

#### Individual Capability Scoring
```
Score Range:
- 2 points = Green (Achieved)
- 1 point  = Yellow (Working Towards)
- 0 points = Red (Not Started)
```

#### Category Scoring
```
Category Average = Sum(Capability Scores) / Number of Capabilities in Category

Category Level:
- Green  (Achieved):         Average >= 1.5
- Yellow (Working Towards):  Average >= 0.8 and < 1.5
- Red    (Not Started):      Average < 0.8
```

#### Overall Assessment Scoring
```
Total Points = Sum of all capability scores
Max Possible Points = Number of Capabilities × 2
Percentage Score = (Total Points / Max Possible Points) × 100

Performance Level:
- High Performing:    Percentage >= 80%
- Competent:          Percentage >= 60% and < 80%
- Developing:         Percentage >= 40% and < 60%
- Needs Improvement:  Percentage < 40%
```

### Recommendation Prioritization

Recommendations are prioritized using the following algorithm:

1. **Priority 1 (Critical)**: Red capabilities in Continuous Delivery or Architecture categories
2. **Priority 2 (High)**: All other Red capabilities
3. **Priority 3 (Medium)**: Yellow capabilities in Continuous Delivery or Architecture categories
4. **Priority 4 (Low)**: All other Yellow capabilities

Within each priority level, sort by:
- Category importance (based on research impact)
- Ease of implementation (Low difficulty first)

### Business Rules

#### BR-001: Assessment Completion
- An assessment is considered "Completed" when all 24 capability questions have responses
- Assessments can be saved as "Draft" or "InProgress" at any time
- Once marked "Completed", assessments cannot be edited (must create new assessment)

#### BR-002: Historical Comparison
- Comparisons can only be made between assessments of the same team
- Baseline assessment must be older than current assessment
- Both assessments must be in "Completed" status

#### BR-003: Organization-Level Access
- Organization admins can view all team assessments within their organization
- Team-level users can only view assessments for their assigned teams
- Assessment results are private by default unless explicitly shared

#### BR-004: Recommendation Generation
- Recommendations are automatically generated when assessment is marked complete
- Minimum 3 recommendations, maximum 10 recommendations
- Focus on capabilities marked as "Red" or "Yellow"

#### BR-005: Assessment Archival
- Assessments older than 2 years are automatically archived
- Archived assessments remain viewable but cannot be used as comparison baselines
- Archived assessments do not appear in default lists

---

## UI/UX Requirements

### Wizard Interface

#### Layout
- Single question per screen
- Progress indicator showing completion (X of 24)
- Category badge showing current capability category
- Navigation buttons: Back, Next, Save Draft

#### Question Display
- Large, readable question text
- 3 radio button options per question
- Clear visual feedback for selected option
- Option descriptions displayed on hover (optional)

#### Progress Tracking
- Visual progress bar at top of screen
- Percentage complete indicator
- Ability to see which questions have been answered
- Jump to specific question functionality

### Report Display

#### Summary Dashboard
- Hero section with overall score and performance level
- 5 category cards with color-coded status
- Quick stats: Green/Yellow/Red count

#### Detailed Scores
- Expandable sections per category
- List of capabilities with traffic light indicators
- Capability descriptions on click/expand

#### Recommendations Section
- Prioritized list with visual priority indicators
- Expandable recommendation cards
- Action item checklists
- Related capabilities links

#### Historical View
- Timeline visualization of past assessments
- Line charts showing capability trends
- Side-by-side comparison view

### Responsive Design
- Mobile-optimized wizard interface
- Touch-friendly radio buttons and navigation
- Swipe gestures for navigation
- Collapsible sections on mobile for reports

### Accessibility
- WCAG 2.1 AA compliant
- Keyboard navigation support
- Screen reader compatible
- High contrast mode support
- Color-blind friendly color palette

---

## Technical Requirements

### Frontend

#### Technology Stack
- React 18+ with TypeScript
- State Management: React Context or Redux Toolkit
- UI Framework: Tailwind CSS
- Icons: Lucide React
- Charts: Recharts or Chart.js
- PDF Generation: jsPDF or react-pdf

#### Performance Requirements
- Initial load time < 3 seconds
- Question navigation < 100ms
- Report generation < 2 seconds
- Support for 1000+ historical assessments per team

#### Browser Support
- Chrome 90+
- Firefox 88+
- Safari 14+
- Edge 90+

### Backend (.NET)

#### Technology Stack
- .NET 8+ (Azure Functions - Isolated)
- Azure Cosmos DB (NoSQL database)
- Azure Blob Storage (PDF reports)
- Azure Application Insights (monitoring)

#### API Requirements
- RESTful API design
- JWT authentication
- Rate limiting: 100 requests/minute per user
- Response time: < 500ms for 95th percentile

#### Data Storage
- Cosmos DB containers:
  - Assessments (partition key: organizationId)
  - CapabilityDefinitions (partition key: "CAPABILITY_DEFINITION")
  - AssessmentReports (partition key: organizationId)

### Security

#### Authentication & Authorization
- JWT-based authentication
- Role-based access control (RBAC)
- Roles: OrgAdmin, TeamManager, TeamMember

#### Data Protection
- Encryption at rest (Cosmos DB)
- Encryption in transit (TLS 1.3)
- PII data masking in logs
- GDPR compliance

#### API Security
- API key required for all requests
- Rate limiting per user/organization
- Input validation and sanitization
- SQL injection protection (parameterized queries)

---

## Integration Points

### Internal Integrations

#### Teams Module
- Fetch team information for assessment context
- Team member list for notifications
- Team hierarchy for org-level reporting

#### User Management
- User authentication and authorization
- User profile data for assessment creator
- Notification preferences

#### OKRs Module (Future)
- Link capability improvements to OKRs
- Show related OKRs in recommendations
- Track capability goals as key results

### External Integrations (Future)

#### Slack/Teams Integration
- Post assessment completion notifications
- Share report summaries
- Reminder notifications for scheduled assessments

#### Jira/Linear Integration
- Create tickets from recommendations
- Track action item completion
- Link capability improvements to sprints

#### Calendar Integration
- Schedule recurring assessments
- Add assessment deadlines to calendar
- Send reminder notifications

---

## Success Metrics

### Usage Metrics
- Number of assessments completed per month
- Average time to complete assessment
- Assessment completion rate (started vs. completed)
- Repeat assessment frequency

### Engagement Metrics
- Report view rate
- PDF export rate
- Comparison feature usage
- Recommendation implementation tracking

### Outcome Metrics
- Average capability score improvement over time
- Percentage of teams showing improvement
- Most improved capabilities
- Time to improvement (Yellow to Green)

### Technical Metrics
- API response time (p95, p99)
- Error rate < 0.1%
- System uptime > 99.9%
- Report generation success rate > 99%

---

## Future Enhancements

### Phase 2 Features
- Custom capability definitions
- Team capability benchmarking
- Industry comparison data
- AI-powered recommendation refinement
- Capability maturity roadmaps

### Phase 3 Features
- Real-time collaborative assessments
- Video explanations for each capability
- Gamification (badges, achievements)
- Capability certification programs
- Integration with CI/CD metrics

---

## Appendix

### Capability Reference

#### Continuous Delivery Capabilities (8)
1. Version Control
2. Deployment Automation
3. Continuous Integration
4. Trunk-Based Development
5. Test Automation
6. Test Data Management
7. Shift Left on Security
8. Continuous Delivery

#### Architecture Capabilities (2)
9. Loosely Coupled Architecture
10. Empowered Teams

#### Product and Process Capabilities (4)
11. Customer Feedback
12. Flow Visibility
13. Small Batches
14. Team Experimentation

#### Lean Management and Monitoring Capabilities (5)
15. Lightweight Change Approval
16. Monitoring for Business Decisions
17. Proactive System Health
18. WIP Limits
19. Visual Work Management

#### Cultural Capabilities (5)
20. Generative Culture
21. Learning Culture
22. Team Collaboration
23. Meaningful Work
24. Transformational Leadership

### Glossary

**Capability**: A specific practice or organizational characteristic identified in the Accelerate research as contributing to high performance.

**Traffic Light Scoring**: A visual scoring system using Red (Not Started), Yellow (Working Towards), and Green (Achieved) to indicate capability maturity.

**Baseline Assessment**: The first or reference assessment used for comparison purposes.

**Capability Category**: A grouping of related capabilities (5 categories total).

**Assessment Template**: A pre-configured set of capabilities that can be reused for assessments.

---

## Document History

| Version | Date | Author | Changes |
|---------|------|--------|---------|
| 1.0 | 2025-10-05 | Initial | Initial requirements specification created |

