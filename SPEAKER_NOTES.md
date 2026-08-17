# SE 223 — Database Design Project Presentation
## Topic 2: Integrated Healthcare and Hospital Network
### Complete 5-Person Speaker Script & Presentation Notes

> **Total Duration:** ~10 Minutes (~2 Minutes per Speaker)  
> **Course:** SE 223 — Database Design  
> **Institution:** Department of Software Engineering, Daffodil International University  
> **Interactive Slide File:** `v2-c-editorial-refined.html` (12 Slides Total)

---

## 📋 Speaker Allocation & Slide Distribution

| Speaker | Assigned Slides | Core Focus Areas | Est. Time |
| :--- | :--- | :--- | :--- |
| **Speaker 1** | **Slides 01 – 03** | Introduction, System Scope & Master Entities (`PATIENT`, `DOCTOR`, `HOSPITAL`) | ~2:00 min |
| **Speaker 2** | **Slides 04 – 05** | Hospital Infrastructure (`DEPARTMENT`, `WARD`) & Clinical Transactions (`APPOINTMENT`, `ADMISSION`, `SURGERY`) | ~2:00 min |
| **Speaker 3** | **Slide 06** | Attribute Theory, 1NF Normalization & Relational Mechanics (Key, Composite, Multivalued, Derived) | ~1:45 min |
| **Speaker 4** | **Slides 07 – 08** | Prescriptions M:N Bridge (`PRESCRIPTION_MEDICINE`) & Auxiliary Services (`INSURANCE`, `AMBULANCE_DISPATCH`) | ~2:00 min |
| **Speaker 5** | **Slides 09 – 12** | Conceptual ER Diagram, Relational DDL Schema, Business Constraints & Q&A Concluding Remarks | ~2:15 min |

---

## 🎙️ Detailed Slide-by-Slide Scripts

---

### 👤 SPEAKER 1 (Slides 01 – 03)
**Theme:** Project Kickoff, Architectural Scope & Persistent Master Data

---

#### 📌 Slide 01: Title & Presentation Intro
* **Visual on Screen:** Split slate-blue title panel, course meta, team roster, and faculty details.
* **Spoken Script:**
  > *"Good morning/afternoon, honorable faculty member Ms. Nusrat Jahan Tazin and dear classmates. We are Group 2, and today we are presenting our Database Design project for SE 223 on Topic 2: **Integrated Healthcare and Hospital Network**.*
  >
  > *In modern healthcare systems, patient medical histories, clinical records, and facility logistics are frequently siloed across independent hospitals, diagnostic labs, and pharmacies. Our objective in this project is to architect a robust, centralized relational database in PostgreSQL that eliminates record fragmentation while enforcing strict relational integrity."*

---

#### 📌 Slide 02: Multi-Facility Healthcare Domain Boundaries
* **Visual on Screen:** Scope stripes showing Facilities, Clinical, People, Services, and Organizational boundaries.
* **Key Points to Emphasize:**
  * Multi-hospital network scope (12+ physical centers).
  * Integration of outpatient, inpatient, lab diagnostics, and emergency fleets.
* **Spoken Script:**
  > *"Moving to Slide 2, let's understand the system boundaries of our enterprise network. A comprehensive healthcare organization doesn't just manage doctor appointments—it coordinates physical facilities like main hospitals, specialized clinics, diagnostic centers, and blood banks.*
  >
  > *Our relational model spans five core functional tiers:*
  > 1. ***Facilities:*** *Top-level hospitals hosting specialized clinical wings, laboratories, and inpatient wards.*
  > 2. ***Clinical Operations:*** *Real-time bed availability tracking, radiology services, and pharmacy inventories.*
  > 3. ***People:*** *Unified patient identities shared across facilities, licensed doctors, nursing staff, and blood donors.*
  > 4. ***Services:*** *OPD consultations, inpatient admissions, surgical logs, and emergency ambulance dispatches.*
  > 5. ***Administration:*** *Public health vaccination drives, donor registries, and automated insurance billing claims."*

---

#### 📌 Slide 03: Core Entity Catalogue with Live Sample Data
* **Visual on Screen:** Master tables for `PATIENT`, `DOCTOR`, and `HOSPITAL`. *(Tip: Press Down Arrow to highlight each card).*
* **Key Points to Emphasize:**
  * Master records are persistent and anchor the entire relational schema.
  * Primary Keys (`Patient_ID`, `Doctor_ID`, `Hospital_ID`).
* **Spoken Script:**
  > *"On Slide 3, we define our foundational Master Entities. These tables store persistent baseline records referenced by all subsequent transaction tables.*
  >
  > * *First, the **`PATIENT`** table anchors demographic records. Every patient receives an immutable `Patient_ID` as their Primary Key, along with date of birth, blood group, and gender.*
  > * *Second, the **`DOCTOR`** table registers licensed practitioners with their medical license number and medical specialization (such as Cardiology or Neurology).*
  > * *Third, the **`HOSPITAL`** table records physical branches across metropolitan locations.*
  >
  > *Notice that these tables do not store transient visit logs—they act as single sources of truth. I now hand over the presentation to Speaker 2 to discuss infrastructure and clinical services."*

---

### 👤 SPEAKER 2 (Slides 04 – 05)
**Theme:** Clinical Infrastructure, M:N Relationship Resolution & Patient Lifecycle

---

#### 📌 Slide 04: Hospital Departments & Doctor M:N Bridge
* **Visual on Screen:** `DEPARTMENT`, `DOCTOR_DEPARTMENT`, and `WARD` tables.
* **Key Points to Emphasize:**
  * Why `DOCTOR` to `DEPARTMENT` is Many-to-Many.
  * The role of the associative entity `DOCTOR_DEPARTMENT` with a Composite PK `(Doctor_ID, Dept_ID)`.
* **Spoken Script:**
  > *"Thank you, Speaker 1. On Slide 4, we model the internal infrastructure of our hospital network.*
  >
  > *A hospital contains multiple clinical departments like Cardiology, Oncology, or Critical Care. This is a standard One-to-Many relationship governed by `Hospital_ID` as a Foreign Key.*
  >
  > *However, a significant relational challenge arises with doctors: a specialist physician may hold appointments in multiple departments across branches, and a department employs multiple doctors. This Many-to-Many relationship cannot be directly linked with a single Foreign Key without creating redundancy.*
  >
  > *To resolve this, we introduced the **`DOCTOR_DEPARTMENT`** associative entity. It uses a Composite Primary Key combining `(Doctor_ID, Dept_ID)`—both acting as Foreign Keys. This junction table also tracks specific role attributes such as `Joining_Date` and doctor `Position`."*

---

#### 📌 Slide 05: Appointments, Admissions & Surgeries (1:N Transactions)
* **Visual on Screen:** `APPOINTMENT`, `ADMISSION`, and `SURGERY` tables.
* **Key Points to Emphasize:**
  * Transaction lifecycle: OPD Visit $\rightarrow$ Inpatient Admission $\rightarrow$ Operating Theatre.
* **Spoken Script:**
  > *"Slide 5 illustrates the primary clinical transactions representing the patient care lifecycle.*
  >
  > * *The **`APPOINTMENT`** table tracks outpatient consultations. It links a `Patient_ID`, a `Doctor_ID`, and a `Hospital_ID`, recording appointment date and status (`Scheduled`, `Completed`, or `Cancelled`).*
  > * *When a patient requires hospitalization, an **`ADMISSION`** record is created, referencing the specific `Ward_ID` bed allocated and tracking admission and discharge timestamps.*
  > * *If invasive care is required, the **`SURGERY`** table associates the patient with the lead surgeon and the assigned Operation Theatre room.*
  >
  > *Each of these operational tables maintains strict referential integrity through foreign keys. I will now pass the floor to Speaker 3 to present our database attribute theory and normalization decisions."*

---

### 👤 SPEAKER 3 (Slide 06)
**Theme:** Relational Attribute Taxonomy, Normalization & SQL Mechanics

---

#### 📌 Slide 06: ER Attribute Taxonomy & Relational Implementation
* **Visual on Screen:** Key, Composite, Multivalued, and Derived attribute breakdowns.
* **Key Points to Emphasize:**
  * 1NF compliance for Multivalued attributes.
  * Spatial query readiness for Composite attributes.
  * Zero redundancy for Derived attributes (computed on-the-fly).
* **Spoken Script:**
  > *"Thank you, Speaker 2. In compliance with the project specifications, Slide 6 demonstrates our comprehensive classification of ER attribute types and how each is translated into relational database structures.*
  >
  > 1. ***Key Attribute (`Patient_ID`):*** *Uniquely identifies every tuple in the relation. In PostgreSQL, this is enforced through a B-Tree unique index and an automatic `NOT NULL` constraint.*
  > 2. ***Composite Attribute (`Address`):*** *A patient's address consists of multiple distinct components. Rather than storing a single unformatted string, we decompose it into scalar sub-columns: `House_No`, `Street`, `City`, and `Zip_Code`. This allows precise indexing and location filtering.*
  > 3. ***Multivalued Attribute (`Phone_Number`):*** *In real life, patients frequently maintain multiple contact numbers. Storing comma-separated values inside one column directly violates First Normal Form (1NF). We resolved this by extracting contact numbers into a dedicated child table, `Patient_Phone`, where `(Patient_ID, Phone_Number)` forms a composite key.*
  > 4. ***Derived Attribute (`Age`):*** *Age continuously changes over time. Physically storing age in a static column creates data staleness and maintenance overhead. Instead, we store static `Date_of_Birth` and dynamically compute `Age` during SQL queries using the formula: `FLOOR(DATEDIFF(NOW(), Date_of_Birth) / 365)`.*
  >
  > *Next, Speaker 4 will explain medication management and auxiliary services."*

---

### 👤 SPEAKER 4 (Slides 07 – 08)
**Theme:** Pharmacy Line-Item Junctions, Insurance Coverage & Fleet Operations

---

#### 📌 Slide 07: Prescriptions & Medication Junction (M:N)
* **Visual on Screen:** `PRESCRIPTION`, `PRESCRIPTION_MEDICINE`, and `MEDICATION` tables.
* **Key Points to Emphasize:**
  * Prescriptions represent another crucial M:N junction.
  * Line-item attributes (`Dosage`, `Frequency`, `Duration`).
* **Spoken Script:**
  > *"Thank you, Speaker 3. Slide 7 showcases our medical pharmacy integration.*
  >
  > *When a doctor conducts a consultation, they issue a prescription header. However, a single prescription can prescribe multiple medicines (for example, an antibiotic along with a pain reliever), while the same medication is prescribed to thousands of patients.*
  >
  > *We modeled this Many-to-Many relationship using the **`PRESCRIPTION_MEDICINE`** associative table. It links `Rx_ID` and `Med_ID` as a Composite Primary Key, while storing critical line-item prescription parameters including specific `Dosage` (e.g., 500mg) and administration `Frequency` (e.g., twice daily for 7 days).*
  >
  > *This structure protects inventory integrity in our master `MEDICATION` catalog while maintaining complete prescription history."*

---

#### 📌 Slide 08: Insurance Policy Claims & Ambulance Dispatch
* **Visual on Screen:** `INSURANCE_POLICY`, `AMBULANCE`, and `AMBULANCE_DISPATCH` tables.
* **Key Points to Emphasize:**
  * Auxiliary hospital services seamlessly integrated into the relational core.
  * Real-time emergency vehicle tracking and billing claims.
* **Spoken Script:**
  > *"On Slide 8, we integrate essential auxiliary and financial services into the database.*
  >
  > * *The **`INSURANCE_POLICY`** table bridges patients to third-party coverage providers (such as Green Delta or Pragati Life), capturing policy numbers, validity periods, and financial coverage limits for automated billing claim clearance.*
  > * *For emergency operations, the **`AMBULANCE`** table manages the hospital vehicle fleet with registration IDs and availability states.*
  > * *Every emergency run is logged in the **`AMBULANCE_DISPATCH`** table, connecting the patient emergency call, the deployed vehicle, destination hospital, and pickup timestamps.*
  >
  > *I will now hand over to Speaker 5 to guide us through the complete ER Diagram, DDL schema specifications, and business rules."*

---

### 👤 SPEAKER 5 (Slides 09 – 12)
**Theme:** Conceptual ER Model, Relational Schema DDL, Integrity Constraints & Conclusion

---

#### 📌 Slide 09: Complete Entity-Relationship (ER) Model Diagram
* **Visual on Screen:** High-resolution interactive ER Diagram canvas. *(Tip: Move cursor across the diagram to dynamically zoom into specific sub-graphs).*
* **Key Points to Emphasize:**
  * Unified conceptual view of all 11+ entities.
  * Clear visual proof of cardinalities (1:1, 1:N, M:N).
* **Spoken Script:**
  > *"Thank you, Speaker 4. Slide 9 presents our complete conceptual Entity-Relationship Diagram.*
  >
  > *(Hovering mouse over diagram)* *As you can see on the interactive canvas:*
  > * *At the top, `HOSPITAL` anchors the organizational tree, branching into 1:N relationships with `DEPARTMENT`, `WARD`, and `AMBULANCE`.*
  > * *In the clinical core, `DOCTOR` and `PATIENT` are interconnected through three transaction hubs: `APPOINTMENT`, `ADMISSION`, and `SURGERY`.*
  > * *On the right, `PRESCRIPTION` decomposes into line-item medications, while auxiliary branches handle insurance policies and ambulance dispatch operations.*
  >
  > *Every entity is strictly annotated with its primary keys, foreign key references, and relationship cardinalities."*

---

#### 📌 Slide 10: ER Entities to Relational SQL Table Schemas
* **Visual on Screen:** DDL column cards with data types and constraints (`VARCHAR`, `DATE`, `ENUM`, `CHECK`).
* **Key Points to Emphasize:**
  * Exact conversion from Conceptual ER to Logical Relational Schema.
* **Spoken Script:**
  > *"Slide 10 details our DDL mapping from ER entities to concrete relational schemas.*
  >
  > *We enforce data integrity directly at the storage engine level:*
  > * *`VARCHAR` lengths are constrained to prevent memory bloating.*
  > * *Fields like `Date_of_Birth` and `Name` enforce `NOT NULL` constraints.*
  > * *Check constraints like `CHECK (Gender IN ('M', 'F', 'O'))` ensure valid domain inputs.*
  > * *Foreign key constraints are configured with appropriate cascading rules to prevent orphaned records."*

---

#### 📌 Slide 11: Enterprise Business Rules & Relational Constraints
* **Visual on Screen:** Numbered enterprise business rules with engine enforcement badges.
* **Key Points to Emphasize:**
  * Business logic enforced by database engine constraints, not just application code.
* **Spoken Script:**
  > *"On Slide 11, we summarize the six cardinal enterprise business rules governing the system:*
  > 1. *Each hospital contains multiple departments, enforced by `FOREIGN KEY (Hospital_ID)`.*
  > 2. *Doctors work across multiple departments, resolved by the `DOCTOR_DEPARTMENT` bridge.*
  > 3. *Patients schedule multiple consultations, tracked via `APPOINTMENT` Foreign Keys.*
  > 4. *Inpatient admissions are allocated to specific beds via `Ward_ID` Foreign Key.*
  > 5. *Prescriptions contain multiple drug line items via the `PRESC_MEDICINE` associative table.*
  > 6. *Ambulances log multiple dispatches over time via `Ambulance_ID` Foreign Key.*
  >
  > *These rules ensure zero data duplication and guaranteed ACID compliance across all transactions."*

---

#### 📌 Slide 12: Thank You & Q&A Handoff
* **Visual on Screen:** Concluding Thank You panel, team roster, and faculty contact card.
* **Spoken Script:**
  > *"In conclusion, our relational database design provides a scalable, normalized, and unified foundation for an integrated healthcare network.*
  >
  > *On behalf of Group 2—Md. Sourav Rana, Mahtabul Al Nahian, Mst. Afia Tasnim Esha, Meherin Ritu, and Zubayer Watsit Zoha—we thank our honorable lecturer Ms. Nusrat Jahan Tazin and the audience for your time and attention.*
  >
  > *We are now open to any questions or feedback. Thank you!"*

---

## 🎯 Faculty Q&A Preparation Cheat Sheet (Anticipated Questions & Model Answers)

### Q1: Why did you use associative entities for `Doctor_Department` and `Prescription_Medicine`?
* **Answer:** *"In relational databases, Many-to-Many relationships cannot be directly implemented with a single foreign key without introducing severe redundancy or multi-valued cells (which violates 1NF). An associative table resolves the M:N relationship into two 1:N relationships and allows us to store relationship-specific attributes, such as doctor joining dates and medication dosages."*

### Q2: How does your design ensure First Normal Form (1NF) for patient phone numbers?
* **Answer:** *"1NF requires all attribute values to be atomic. Since a patient can have multiple phone numbers, storing them in a single column violates 1NF. We extracted phone numbers into a separate child table `Patient_Phone` with composite primary key `(Patient_ID, Phone_Number)`, ensuring atomicity."*

### Q3: Why is Age not stored directly in the `PATIENT` table?
* **Answer:** *"Age is a derived attribute. Storing age statically causes data to become incorrect after a patient's birthday, requiring periodic batch update jobs. Storing immutable `Date_of_Birth` and deriving `Age` during queries guarantees 100% accuracy with zero storage redundancy."*

### Q4: What happens if a Hospital or Doctor record is deleted?
* **Answer:** *"In production healthcare systems, critical master records are protected by `ON DELETE RESTRICT` or soft-delete flags (e.g. `is_active = FALSE`) to preserve historical audit trails for prescriptions, surgeries, and billing."*

---
*End of Speaker Notes — Generated for SE 223 Presentation Group 2.*
