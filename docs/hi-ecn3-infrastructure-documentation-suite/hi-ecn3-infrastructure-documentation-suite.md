# Technical Coordination Document: HI-ECN3 Infrastructure Project
## 1. Introduction
The transition of the ECN3 experimental area from the NA62 experiment to the High-Intensity Beam Dump Facility (HI-ECN3 / BDF) represents a complex, multi-disciplinary upgrade involving beamline reconfiguration, civil engineering works, and high-radiation environment management.
This dossier demonstrates a structured approach to technical coordination aligned with CERN’s Engineering and Equipment Data Management System (EDMS). It reflects core coordination competencies including:
* **Engineering change control and traceability.**
* **Cross-functional impact analysis.**
* **Lifecycle documentation management.**
* **Integration of safety and scheduling constraints.**

The objective is to define a documentation framework that ensures alignment between **engineering changes, installation sequencing, and safety validation within LS3 constraints**.

---
# 2. Engineering Change Request (ECR) Framework
## 2.1 Role of ECR in HI-ECN3
Engineering Change Requests (ECRs) are used to manage modifications after a system baseline has been established. Within HI-ECN3, they serve as the primary mechanism to:
* Control technical deviations from the NA62 baseline.
* Coordinate multi-disciplinary validation.
* Maintain full traceability across the project lifecycle.

---
## 2.2 Standardized ECR Structure
A consistent structure ensures clarity and enforceable coordination:
### 1. Header Information
* ECR Reference ID
* Submission Date
* Project Leader
* Initiator / Responsible Engineer
### 2. Description of Change
* Current State (“As-Is”).
* Proposed State (“To-Be”).
* Associated layout or system references.
### 3. Justification
* Performance requirements.
* Safety compliance (ALARA principles).
* Operational or cost efficiency.
### 4. Impact Analysis (Core Coordination Layer)
* **Schedule Impact**  Alignment with LS3 milestones and shutdown windows.
* **Spatial Constraints**  Co-activity management within ECN3 cavern and adjacent civil works.
* **Safety Considerations**  Radiation exposure, activation handling, and mitigation strategy.
* **System Dependencies**  Mechanical, vacuum, cryogenic, and electrical interactions.
### 5. Decision and Approval Workflow
* Preliminary validation: Mechanical & Vacuum Groups.
* Safety clearance: Radiation Protection (mandatory before dismantling).
* Final approval: Project coordination board aligned with LS3 schedule freeze.
---
## 2.3 Case Application: XTAX Replacement
**ECR Reference**: HI-ECN3-B-EC-0001
**Functional Area**: SPS North Area – ECN3 Cavern
### Description
The existing motorized XTAX modules used in NA62 will be decommissioned. They will be replaced with a fixed absorber integrated into a vacuum chamber to eliminate beam interaction with air.
The absorber baseline design utilizes high-Z materials such as **Tungsten (W) or Molybdenum (Mo)** with **Tantalum (Ta) cladding**.
### Justification
This modification is required to meet high-intensity beam conditions and reduce background interference for SHiP experimental requirements.
### Impact Analysis
* **Coordination & Planning**  This activity lies on the critical path of LS3 and is dependent on complete removal of the NA62 K12 beamline.
* **Space Reservation**  The absorber assembly must comply with ECN3 spatial constraints to avoid co-activity conflicts with PA855 shaft construction.
* **Safety & ALARA**  Removal of activated XTAX modules requires remote handling strategies, shielding, and strict radiation protection procedures.
* **System Dependencies**  Potential interaction with nearby cryogenic and vacuum systems during installation.
---
# 3. Technical Design Report (TDR) Framework
## 3.1 Role of TDR in Coordination
The Technical Design Report (TDR) defines the functional and engineering baseline of the facility. For coordination, it serves as:
* A **reference for validation of all changes (ECR linkage)**.
* A **blueprint for integration and installation**.
* A **control document for scope and system boundaries**.
---
## 3.2 Standardized TDR Structure
### 1. Executive Summary
Project objectives and expected performance outcomes.
### 2. Functional Objectives
Scientific purpose and system-level requirements.
### 3. Infrastructure Systems
* Civil engineering works.
* Cryogenic and vacuum systems.
* Power and cooling infrastructure.
### 4. Component Design
* Beamline elements.
* Muon shielding systems.
* Decay volumes and detector interfaces.
### 5. Integration and Assembly
* Layout validation.
* Installation sequencing.
* Heavy handling constraints.
### 6. Project Management Layer
* Work Package (WP) structure.
* Timeline and milestone tracking.
* Resource coordination.
---
## 3.3 Case Application: SHiP Decay Volume Integration
**Project**: HI-ECN3 / SHiP Hidden Sector Decay Spectrometer (HSDS).
### Objective
Integration of the decay spectrometer within ECN3 infrastructure while maintaining alignment with shielding systems and beamline configuration.
### Technical Characteristics
* **Geometry**: Pyramidal frustum, total length of **50 m**.
* **Dimensions**: Expanding from **1.0 × 2.7 m²** to **4 × 6 m²**.
* **Volume**: ~2040 m³.
* **Operating Condition**: Internal pressure of approximately **1 mbar**.
### Integration and Coordination Requirements
* A **staged installation plan** is required to coordinate crane allocation, floor space zoning, and sequential assembly of structural segments.
* **Parallel execution** with muon shield installation must be maintained to meet LS3 deadlines.
* **Detector synchronization** is required to integrate background taggers within the vacuum vessel during assembly.
---
# 4. Documentation and Workflow Management (EDMS)
## 4.1 Role of EDMS
The Engineering and Equipment Data Management System (EDMS) ensures documentation integrity across the project lifecycle by providing:
* Centralized document control.
* Version tracking and approval workflows.
* Traceability between design, manufacturing, and installation.
---
## 4.2 Coordination Practices
### 1. Document Lifecycle Control
* Draft → Review → Approval → Release.
* Defined checker and approver roles across departments.
### 2. Naming and Classification
* Compliance with CERN Naming Portal conventions.
* Structured tagging for integration with layout databases.
### 3. Traceability Enforcement
* Mandatory linkage between ECR IDs and affected TDR sections.
* Integration with Manufacturing and Inspection Plans (MIP).
---
## 4.3 Risk in Documentation Misalignment
* **Risk**: Inconsistency between ECR revisions and TDR baseline.
* **Impact**: Installation errors, rework, and schedule delays.
* **Mitigation**: Controlled version synchronization and cross-referencing protocols within EDMS.
---
# 5. Key Coordination Risks and Mitigation
| **Risk** | **Impact** | **Mitigation Strategy** |
| --------------------------------- | --------------------- | --------------------------------------------------------- |
| Delay in XTAX removal             | LS3 schedule slippage | Pre-approved dismantling sequence and dependency tracking. |
| Radiation exposure during removal | Safety hazard | Remote handling systems and shielding protocols.           |
| Spatial conflict with PA855 works | Installation delays| 3D layout validation and co-activity scheduling. |
| Misalignment between ECR and TDR  | Execution errors | Mandatory EDMS traceability and version control.           |

---
# 6. Conclusion
This dossier presents a structured coordination approach integrating:
* **Engineering change control (ECR)**.
* **Baseline system definition (TDR)**.
* **Centralized documentation workflows (EDMS)**.

The framework ensures that complex infrastructure transitions such as HI-ECN3 are executed with:
* Controlled decision-making.
* Full traceability.
* Cross-functional alignment.
* Integrated safety and scheduling awareness.
---
