# Project CWP: Clinical Workflow Platform

## Project Plan

> **Legend:** Tasks marked **(Core)** are part of the minimum viable product. Complete these before expanding the project.

---

## Phase 1: Planning and Research

### Project Foundation

- [x] Define project scope and objectives
- [x] Select technology stack
- [x] Create GitHub repository
- [x] Identify Synthea as the primary data source
- [x] Draft README

### Clinical Research

- [ ] **(Core)** Research the AAP Bright Futures pediatric well-visit schedule
- [ ] Research HEDIS well-child visit measures and document relevant definitions
- [ ] Research pediatric preventive-care outreach and scheduling workflows
- [ ] Research clinical terminology used to identify well visits:
  - SNOMED CT
  - LOINC
  - CPT
  - ICD-10

### FHIR Research

- [ ] **(Core)** Research HL7 FHIR fundamentals:
  - Resources
  - References
  - Bundles
  - Coding elements
- [ ] Research the Patient resource
- [ ] Research the Practitioner resource
- [ ] Research the Encounter resource
- [ ] Research the Appointment resource
- [ ] Research the Schedule resource
- [ ] Research the Slot resource
- [ ] Research the Location resource

### Workflow Definition

- [ ] Write three to five questions for a practicing pediatrician
- [ ] Record the pediatrician’s answers in `docs/clinical-input.md`
- [ ] Define care-gap statuses:
  - Up to Date
  - Due Soon
  - Due
  - Overdue
- [ ] Define how a future appointment affects care-gap status
- [ ] Resolve the naming overlap between care-gap status and outreach status
- [ ] Define initial outreach statuses:
  - Not Contacted
  - Contacted
  - Scheduled
  - Declined
- [ ] Define provider-utilization metrics
- [ ] Define open-capacity metrics
- [ ] Draft the system architecture diagram

### Phase 1 Definition of Done

Phase 1 is complete when:

- Care-gap statuses are clearly defined
- Outreach statuses are clearly defined
- Provider-utilization and open-capacity metrics are documented
- The primary FHIR resources can be explained without notes
- The initial system architecture is documented
- Informal clinical input has been recorded without using real patient or clinic data

---

## Phase 2: Data Source and Interoperability

### Synthea Setup

- [ ] Install the required Java version
- [ ] Install or configure Synthea
- [ ] Generate a small synthetic population as a test
- [ ] Determine how to restrict or filter the population to pediatric patients
- [ ] **(Core)** Generate the working pediatric population
- [ ] Export the generated records as FHIR data

### Data Inspection

- [ ] Analyze Patient resources
- [ ] Analyze Practitioner resources
- [ ] **(Core)** Analyze Encounter resources
- [ ] **(Core)** Determine how pediatric well visits can be identified
- [ ] Check whether Appointment resources are generated
- [ ] Check whether Schedule resources are generated
- [ ] Check whether Slot resources are generated
- [ ] Determine how Encounters reference Practitioners
- [ ] Determine how Encounters reference Locations
- [ ] Identify the clinical codes associated with qualifying well visits

### Findings and Decisions

- [ ] Write `docs/synthea-findings.md`
- [ ] Document which FHIR fields are usable
- [ ] Document which resources or fields are missing
- [ ] Decide what must be simulated:
  - Provider schedules
  - Open appointment slots
  - Outreach history
  - Scheduling outcomes
- [ ] Re-estimate the project timeline based on the findings

### Phase 2 Definition of Done

Phase 2 is complete when:

- The contents of the Synthea dataset are understood
- Pediatric patients can be identified
- Qualifying well visits can be identified or a documented alternative exists
- All resources and fields used by the platform are known
- Simulated data is clearly separated from Synthea-generated data
- The project timeline has been updated based on actual findings

---

## Phase 3: Database and ETL Pipeline

### Database Design

- [ ] Install and configure PostgreSQL
- [ ] **(Core)** Design the relational database schema
- [ ] **(Core)** Draw an entity-relationship diagram
- [ ] Document FHIR-to-database field mappings in `docs/fhir-mapping.md`
- [ ] Create database schema scripts or migrations

### ETL Development

- [ ] **(Core)** Build the extraction process for selected FHIR resources
- [ ] Transform nested FHIR resources into relational records
- [ ] Validate required fields
- [ ] Handle missing or malformed fields
- [ ] Normalize coded values and references
- [ ] **(Core)** Load transformed data into PostgreSQL
- [ ] Compare database row counts with source-file counts
- [ ] Log rejected or incomplete records

### Simulated Workflow Data

- [ ] Build simulated provider schedules
- [ ] Build simulated open appointment slots
- [ ] Build simulated outreach history if included in Version 1
- [ ] Clearly label simulated records in the database

### Pipeline Automation

- [ ] Make the complete ETL pipeline rerunnable
- [ ] Add one command or script that rebuilds the database from source data
- [ ] Document how to run the pipeline

### Phase 3 Definition of Done

Phase 3 is complete when:

- A fresh PostgreSQL database can be rebuilt from the source data
- The ETL pipeline can be run from beginning to end with one documented command
- Source records and database records can be reconciled
- Invalid records are handled explicitly
- Simulated data is documented and distinguishable from generated FHIR data

---

## Phase 4: Care-Gap Engine

### Rules Configuration

- [ ] Select a rules format:
  - YAML
  - JSON
  - PostgreSQL configuration tables
- [ ] **(Core)** Encode the pediatric well-visit schedule as configuration
- [ ] Define the Due Soon window
- [ ] Define the Due period
- [ ] Define the Overdue grace period
- [ ] Define how future appointments affect patient status
- [ ] Write one complete worked example

### Test-First Development

- [ ] **(Core)** Write tests before implementing the complete engine
- [ ] Test a patient who is Up to Date
- [ ] Test a patient entering the Due Soon window
- [ ] Test a patient reaching the target due date
- [ ] Test a patient passing the overdue grace period
- [ ] Test a patient with a qualifying future appointment
- [ ] Test a patient with no well-visit history
- [ ] Test a visit completed slightly early
- [ ] Test a birthday near a visit threshold
- [ ] Test a patient aging out of the configured schedule
- [ ] Test ambiguous or unrelated encounter codes
- [ ] Test missing or incomplete fields
- [ ] Test duplicate encounters

### Care-Gap Classification

- [ ] **(Core)** Implement Up to Date status
- [ ] **(Core)** Implement Due Soon status
- [ ] **(Core)** Implement Due status
- [ ] **(Core)** Implement Overdue status
- [ ] Implement future-appointment handling
- [ ] Handle missing visit history explicitly
- [ ] Handle ambiguous clinical codes explicitly
- [ ] Handle patients outside the configured age range
- [ ] **(Core)** Produce a human-readable reason for every status

### Outreach Priority

- [ ] Define outreach-priority rules
- [ ] Document every input used by the priority rules
- [ ] Implement High, Medium, and Low priorities
- [ ] Display the reason behind each priority
- [ ] Run the engine against the complete synthetic population
- [ ] Review unexpected classifications

### Phase 4 Definition of Done

Phase 4 is complete when:

- All documented care-gap tests pass
- Every patient status includes a visible reason
- Future appointments are handled consistently
- Priority levels are based on documented rules
- Missing or ambiguous data does not result in silent classification errors

---

## Phase 5: API Layer

### FastAPI Setup

- [ ] **(Core)** Create the FastAPI project structure
- [ ] Configure the PostgreSQL connection
- [ ] Add environment-based configuration
- [ ] Create shared request and response models

### Core Endpoints

- [ ] **(Core)** Create a patient care-gap status endpoint
- [ ] **(Core)** Include the classification reason in the response
- [ ] **(Core)** Create a prioritized outreach-worklist endpoint
- [ ] Add filtering by:
  - Care-gap status
  - Priority
  - Age group
  - Outreach status
  - Provider
  - Location
- [ ] Create a provider-utilization endpoint
- [ ] Create an open-appointment-capacity endpoint
- [ ] Create an outreach-status update endpoint

### Validation and Testing

- [ ] Implement request validation
- [ ] Implement response validation
- [ ] Add clear API error responses
- [ ] Write tests for each endpoint
- [ ] Confirm API documentation is available through OpenAPI and Swagger

### Phase 5 Definition of Done

Phase 5 is complete when:

- The outreach worklist can be retrieved through the API
- The worklist can be filtered
- Individual patient statuses include human-readable reasons
- Outreach statuses can be updated
- Core endpoints have passing tests

---

## Phase 6: Dashboard

### React Setup

- [ ] Create the React project
- [ ] Configure routing
- [ ] Configure FastAPI communication
- [ ] Establish a simple and consistent visual design

### Core Workflow

- [ ] **(Core)** Build the outreach-worklist page
- [ ] Display care-gap status
- [ ] Display classification reason
- [ ] Display outreach priority
- [ ] Display outreach status
- [ ] Display suggested open appointment opportunities
- [ ] Add worklist filtering
- [ ] Add outreach-status update controls

### Analytics

- [ ] Build a care-gap summary view
- [ ] Display counts by care-gap status
- [ ] Display care-gap rates by age group
- [ ] Build provider-utilization charts
- [ ] Build open-capacity charts
- [ ] Add outreach activity metrics
- [ ] Add Plotly visualizations where they improve understanding

### Usability

- [ ] Add loading states
- [ ] Add empty states
- [ ] Add error states
- [ ] Keep the interface functional and simple
- [ ] Avoid unnecessary animations or custom design work

### Phase 6 Definition of Done

Phase 6 is complete when a user can:

- Open the outreach worklist
- Understand why each patient was flagged
- Filter and prioritize the worklist
- View suggested appointment opportunities
- Update outreach status
- Review basic care-gap and provider-capacity metrics

---

## Phase 7: Documentation and Release

### Documentation

- [ ] Update the README to describe only what was actually built
- [ ] Move detailed design information into `docs/`
- [ ] Document the final architecture
- [ ] Document the relational schema
- [ ] Document FHIR-to-table field mappings
- [ ] Document care-gap rules
- [ ] Document priority rules
- [ ] Document assumptions and data limitations
- [ ] Complete the design-decisions log

### Setup and Testing

- [ ] Write complete setup instructions
- [ ] Test setup instructions from a fresh folder
- [ ] Document how to generate or load the synthetic data
- [ ] Document how to rebuild the database
- [ ] Document how to run the API
- [ ] Document how to run the dashboard
- [ ] Document how to run tests

### Portfolio Materials

- [ ] Add dashboard screenshots
- [ ] Record a short demonstration GIF or video
- [ ] Add a license
- [ ] Write resume bullets using only completed and verifiable claims
- [ ] Prepare a short interview explanation
- [ ] Prepare a technical explanation of one important design decision

### Phase 7 Definition of Done

The initial release is complete when:

- A reviewer can set up and run the project
- Core tests pass
- The care-gap workflow functions from data ingestion through the dashboard
- Screenshots or a demonstration are available
- Documentation matches the implementation
- The project can be explained to both technical and nontechnical audiences

---

# Minimum Viable Product

The project is resume-ready when the following core slice works:

```text
Synthea FHIR Data
        |
        v
PostgreSQL Database
        |
        v
Rule-Based Care-Gap Engine
        |
        v
FastAPI Worklist Endpoint
        |
        v
React Outreach Worklist
```

The minimum viable product must:

- Import a pediatric synthetic population
- Identify qualifying well visits
- Classify patients as Up to Date, Due Soon, Due, or Overdue
- Explain each classification
- Store and query the results
- Serve a prioritized outreach worklist through FastAPI
- Display that worklist in React

Provider-capacity matching, outreach tracking, and additional dashboards can follow after this slice works.

---

# Stretch Goals

Complete these only after Phase 7:

- [ ] Improve appointment matching
- [ ] Generate reminder-message text without sending it
- [ ] Support additional preventive-care measures
- [ ] Experiment with outreach-response prediction
- [ ] Experiment with no-show prediction
- [ ] Add Docker configuration
- [ ] Connect to an external FHIR server
- [ ] Add authentication and role-based access control
- [ ] Deploy the application to the cloud

---

# Development Habits

- [ ] Commit at the end of every meaningful milestone
- [ ] Keep commits small and descriptive
- [ ] Maintain a short decisions log
- [ ] Record what was chosen and why
- [ ] Label generated, simulated, and application-created data clearly
- [ ] Write tests for clinical business rules
- [ ] Do not add a stretch feature until the current phase is complete
- [ ] Re-estimate the timeline after Phase 2
- [ ] Update checkboxes based on completed work, not planned work
## Project Constraints & Guardrails

### 1. This Is NOT an EHR

CWP is a scheduling and workflow optimization platform, not an Electronic Health Record system.

The project will not include:

- Medication management
- Clinical note authoring
- Allergy tracking
- Laboratory results
- Patient charting
- Medical imaging
- Prescription management

The primary resources and entities are:

- Patient
- Practitioner
- Appointment
- Encounter

The focus is scheduling intelligence, provider utilization, workflow analytics, and operational decision support.

---

### 2. Data Pipeline Before Machine Learning

FHIR resources contain deeply nested JSON structures that are not suitable for direct machine learning or relational database storage.

Before building any predictive models:

- Retrieve FHIR resources
- Transform them into a flattened format
- Validate data quality
- Load into PostgreSQL
- Perform exploratory data analysis

No Scikit-Learn or TensorFlow development should begin until the ETL pipeline is functional.

---

### 3. Backend Before Frontend

The primary value of CWP comes from:

- Data architecture
- FHIR integration
- Analytics
- Machine learning
- Deep learning
- Scheduling recommendations

Frontend development should remain simple and functional.

Avoid spending excessive time on:

- Complex UI animations
- Custom component libraries
- Pixel-perfect styling
- Advanced design systems

Focus on functionality over aesthetics.

---

### 4. Machine Learning Must Solve a Real Problem

Machine learning and deep learning should support scheduling optimization rather than exist as standalone features.

The central question is:

"How can scheduling recommendations improve provider utilization and operational efficiency?"

All predictive models must contribute toward answering this question.

---

### 5. MVP First

A working version of CWP consists of:

- PostgreSQL database
- FHIR integration
- FastAPI backend
- React dashboard
- Scheduling analytics
- ML recommendation engine

Any features beyond this baseline are considered future enhancements.

---

### 6. Finish Before Expanding

Before adding new capabilities:

- Complete the current phase
- Commit the work
- Document the work
- Verify functionality

Avoid expanding scope before the existing functionality is working.