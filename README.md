# Clinical Workflow Platform (CWP)

A healthcare informatics platform designed to identify preventive care gaps, support patient outreach workflows, and improve appointment utilization using FHIR-based healthcare data.

**Status:** Planning and Architecture Phase

## Overview

Clinical Workflow Platform (CWP) is a healthcare informatics project focused on preventive care outreach, appointment scheduling workflows, provider utilization, and operational analytics.

The project was inspired by a challenge described by a practicing pediatrician: pediatric patients may become due or overdue for preventive well visits while providers simultaneously have unused appointment capacity. Identifying which patients should be contacted and connecting them with appropriate appointment opportunities can be a manual and time-consuming process.

CWP aims to support this workflow by identifying preventive care gaps, analyzing appointment capacity, and generating outreach worklists that help scheduling teams prioritize patient outreach.

The platform is not intended to function as an Electronic Health Record (EHR). Its focus is care-gap identification, operational reporting, healthcare interoperability, and scheduling workflow support.

## Who This Is For

### Outreach Coordinators and Scheduling Staff

The platform is intended to help outreach and scheduling staff determine:

- Which patients are due or overdue for preventive care
- Which patients should be contacted first
- Which providers have available appointment capacity
- Which patients have already been contacted
- Which outreach efforts resulted in scheduled appointments

### Practice Managers and Providers

The platform is intended to provide practice managers and providers with visibility into:

- Preventive care gap rates
- Provider utilization
- Open appointment capacity
- Scheduling trends
- Outreach activity
- The percentage of contacted patients who later schedule an appointment

## Problem Statement

Pediatric primary care practices may have patients who are due or overdue for preventive well visits while appointment openings remain unused.

As a result:

- Preventive care opportunities may be missed
- Patients may fall behind recommended visit schedules
- Providers may have unused appointment capacity
- Scheduling teams may lack actionable outreach lists
- Practice leaders may have limited visibility into preventive care trends

CWP explores how healthcare interoperability, operational analytics, and workflow support tools can help practices identify preventive care gaps and use available appointment capacity more effectively.

## Goals

- Import and transform FHIR-based synthetic healthcare data
- Identify pediatric patients who are due or overdue for preventive well visits
- Generate prioritized outreach worklists for scheduling staff
- Track outreach activity and scheduling outcomes
- Analyze provider utilization and open appointment capacity
- Create operational dashboards and reports
- Build the application using React, FastAPI, PostgreSQL, Python, and HL7 FHIR
- Document important architecture, terminology, data-mapping, and workflow decisions

## Scope

The initial release focuses on pediatric preventive care gap management and scheduling support.

### Included

- Preventive well-visit tracking
- Rule-based care-gap identification
- Configurable preventive care scheduling rules
- Prioritized outreach worklists
- Outreach status tracking
- Appointment analytics
- Provider utilization reporting
- Open-slot and scheduling-capacity analysis
- FHIR data transformation
- Operational dashboards and reports

### Out of Scope

- Complete Electronic Health Record functionality
- Clinical documentation
- Medication management
- Allergy tracking
- Billing and insurance workflows
- Laboratory systems
- Prescription management
- Medical imaging
- Real patient or clinic data
- Automated appointment booking
- Automated SMS or email delivery
- Authentication and role-based access control
- Multi-tenant support
- HIPAA compliance claims
- Production healthcare deployment
- Machine learning for the initial release

## Core Workflow

```text
Synthetic FHIR Data
        |
        v
Care-Gap Detection
        |
        v
Identify Due or Overdue Patients
        |
        v
Analyze Available Appointment Capacity
        |
        v
Generate a Prioritized Outreach Worklist
        |
        v
Track Outreach and Scheduling Status
```

Version 1 will not automatically create or modify appointments. It will identify possible appointment opportunities and allow a patient's outreach status to be marked as scheduled when appropriate.

## Care-Gap Logic

CWP will use configurable rules to determine whether a pediatric patient is up to date, approaching a recommended visit, currently due, or overdue for a preventive well visit.

The initial rule configuration will be based on a recognized pediatric preventive care schedule. The exact visit intervals and grace periods will be verified and documented before the rules are implemented.

The care-gap engine will evaluate:

- Patient date of birth and current age
- Recorded preventive well visits
- Date of the most recent qualifying well visit
- Recommended timing of the next well visit
- Configurable early and late grace periods
- Existing future appointments that may satisfy the identified care gap

Each patient will receive one of the following statuses:

- **Up to Date:** The patient's preventive care history satisfies the applicable scheduling rule.
- **Due Soon:** The patient is approaching the next recommended visit within the configured early-notification window.
- **Due:** The patient has reached the recommended visit date but has not exceeded the configured overdue grace period.
- **Overdue:** The patient has passed the recommended visit date and overdue grace period without a qualifying completed visit.
- **Scheduled:** The patient has an appropriate future appointment associated with the identified care gap.

Patients with no identifiable well-visit history, incomplete records, ambiguous visit codes, or ages outside the configured schedule will be handled explicitly rather than silently classified.

### Example

A future version of this section will include a worked example showing:

1. The patient's age
2. The target preventive visit
3. The most recent qualifying visit
4. The early-notification window
5. The due date
6. The overdue grace period
7. The resulting care-gap status

The final values will be added after the preventive care schedule and available synthetic data have been reviewed.

## Configurable Rules

Preventive care rules will be stored as configuration data rather than being permanently hard-coded into the application.

A rule may include:

- Target patient age
- Recommended visit type
- Early-notification window
- Overdue grace period
- Qualifying clinical codes
- Minimum or maximum patient age
- Whether an appropriate scheduled appointment changes the displayed status

This design will make the rules easier to test, document, update, and adapt to different practice policies.

## Outreach Worklist

The primary workflow output will be a prioritized outreach worklist.

| Patient | Care-Gap Status | Days Overdue | Priority | Outreach Status | Suggested Slot |
|---|---:|---:|---|---|---|
| Patient A | Overdue | 45 | High | Not Contacted | Available |
| Patient B | Due Soon | 0 | Medium | Contacted | Available |
| Patient C | Overdue | 14 | Medium | Scheduled | Assigned |

Possible outreach statuses include:

- Not Contacted
- Contacted
- Scheduled
- Declined

### Priority Rules

The initial worklist priority will be generated with documented, rule-based logic rather than an unexplained score.

Potential priority inputs include:

- Care-gap status
- Number of days overdue
- Patient age or applicable well-visit stage
- Whether the patient has an appropriate future appointment
- Whether the patient has already been contacted

The final priority rules will be defined after the preventive care schedule and synthetic data have been reviewed. The reasons behind each priority level will remain visible to the user.

## Data Source

CWP will use synthetic healthcare data generated by Synthea.

Synthea produces FHIR-compatible healthcare records that can be used to develop and demonstrate healthcare interoperability, analytics, and workflow applications without exposing real patient information.

No real patient, family, provider, or clinic data will be used.

## Data Limitations

Synthetic data does not fully reproduce real scheduling behavior, appointment adherence, provider availability, patient outreach, or clinical documentation practices.

The project may need to simulate:

- Open provider schedules
- Available appointment slots
- Outreach attempts
- Outreach outcomes
- Scheduling outcomes
- Some appointment adherence patterns

Any care-gap percentages, utilization measures, outreach results, or scheduling outcomes produced by the platform will demonstrate the application's method and workflow. They will not represent clinical findings or the performance of a real pediatric practice.

## FHIR Resources

| FHIR Resource | Planned Purpose |
|---|---|
| Patient | Patient demographics and care-gap tracking |
| Practitioner | Provider identification and utilization analysis |
| Encounter | Completed visit history and well-visit identification |
| Appointment | Existing and future appointment information |
| Location | Practice and clinic-level reporting |
| Schedule | Provider scheduling availability |
| Slot | Open appointment capacity |

Availability of these resources will be confirmed after generating and inspecting the Synthea output. Schedule and Slot data may be simulated if they are not generated directly.

Qualifying well visits will be identified using coded clinical information when available rather than relying only on text descriptions. The exact code systems and mappings will be documented after the generated resources have been inspected.

## Planned Technology Stack

### Frontend

- React
- JavaScript
- HTML/CSS
- Plotly

### Backend

- Python
- FastAPI

### Database

- PostgreSQL

### Healthcare Interoperability

- HL7 FHIR
- Synthea

### Data Processing

- Pandas
- NumPy

### Development Tools

- Git
- GitHub
- GitHub Copilot

Machine learning is not part of the initial release. Possible predictive experiments may be considered later only after the rule-based workflow, database, API, dashboard, and testing are complete.

## Planned Architecture

```text
Synthea FHIR Data
        |
        v
ETL and Validation Pipeline
        |
        v
PostgreSQL Database
        |
        v
FastAPI Services
        |
        v
React Dashboard
        |
        v
Care-Gap Reports and Outreach Worklists
```

The ETL pipeline will extract selected fields from FHIR resources, validate and transform the data, and load it into a relational PostgreSQL schema. FastAPI will provide access to care-gap, utilization, appointment-capacity, and outreach data. React will present the resulting information to scheduling staff, practice managers, and providers.

## Testing Priorities

Care-gap classification is one of the most important parts of the platform and will be tested independently from the user interface.

Planned test cases include:

- A patient who is up to date
- A patient entering the Due Soon window
- A patient reaching the target due date
- A patient exceeding the overdue grace period
- A patient with an appropriate future appointment
- A patient with no identifiable well-visit history
- A preventive visit completed slightly early
- A birthday occurring near a visit threshold
- A patient aging out of the configured schedule
- An encounter with an unrelated or ambiguous clinical code
- Missing or incomplete FHIR fields
- Duplicate encounters or appointments

## Success Criteria

The initial version of CWP will be considered successful if it can:

- Generate or import synthetic FHIR data
- Transform selected FHIR resources into a documented relational schema
- Store healthcare scheduling and encounter information in PostgreSQL
- Identify patients who are due or overdue for preventive well visits
- Explain why each patient received a particular care-gap status
- Generate a prioritized outreach worklist
- Track outreach status
- Calculate provider utilization and open-capacity metrics
- Suggest open appointment opportunities without automatically booking them
- Expose workflow and analytics data through FastAPI
- Display care-gap, outreach, and utilization information through React
- Pass documented tests for the care-gap rules
- Document architecture, field mappings, assumptions, limitations, and design decisions

## How This Could Work in Production

A production implementation would receive patient, encounter, appointment, provider, and scheduling information from authorized healthcare systems rather than from local Synthea files.

Depending on the healthcare environment, data could be obtained through approved interoperability interfaces, reports, APIs, or FHIR-based exchange. A production system would also require security controls, authentication, authorization, audit logging, consent management, privacy review, operational monitoring, and organizational approval.

Those production requirements are outside the scope of this portfolio project.

## Future Enhancements

Possible future enhancements include:

- Automated reminder message generation
- Integration with an approved messaging service
- Additional preventive care or population-health measures
- Real FHIR API integration
- More advanced appointment matching
- Outreach response prediction
- No-show risk prediction
- Authentication and role-based access control
- Cloud deployment

## Project Vision

CWP demonstrates how healthcare interoperability, clinical workflow analysis, configurable business rules, and operational reporting can help pediatric practices identify preventive care gaps, organize patient outreach, and use available appointment capacity more effectively.