# SE 223 — Database Design Project Presentation
## Topic 2: Integrated Healthcare and Hospital Network
### Complete 5-Person Speaker Script & Presentation Notes

> **Total Duration:** ~12–14 Minutes (~2.5 Minutes per Speaker)  
> **Course:** SE 223 — Database Design  
> **Institution:** Department of Software Engineering, Daffodil International University  
> **Interactive Slide File:** `index.html` (14 Slides Total)

---

## 📋 Speaker Allocation & Slide Distribution

| Speaker | Assigned Slides | Core Focus Areas | Est. Time |
| :--- | :--- | :--- | :--- |
| **Speaker 1** | **Slides 01 – 03** | Introduction, System Scope & Master Entities (`PATIENT`, `DOCTOR`, `HOSPITAL`) | ~2:30 min |
| **Speaker 2** | **Slides 04 – 06** | Hospital Infrastructure, M:N Resolution & Attribute Theory / Normalization | ~2:30 min |
| **Speaker 3** | **Slides 07 – 08** | Prescriptions M:N Bridge & Auxiliary Services (Insurance, Ambulance) | ~2:00 min |
| **Speaker 4** | **Slides 09 – 11** | Weak Entities, Relationships & Cardinality Table, and the ER Diagram | ~2:30 min |
| **Speaker 5** | **Slides 12 – 14** | Relational DDL Schema, Business Rules, Constraints & Concluding Remarks | ~2:30 min |

---

## 🎙️ Detailed Slide-by-Slide Scripts

---

### 👤 SPEAKER 1 (Slides 01 – 03)
**Theme:** Project Kickoff, Architectural Scope & Persistent Master Data

---

#### 📌 Slide 01: Title & Presentation Intro
* **Visual on Screen:** Split slate-blue title panel, course meta, team roster, and faculty details.
* **Spoken Script:**
  > *"Good morning / afternoon, honorable faculty member Ms. Nusrat Jahan Tazin and dear classmates. We are Group 2, and today we are presenting our Database Design project for SE 223 on Topic 2: **Integrated Healthcare and Hospital Network**.*
  >
  > *In modern healthcare systems, patient medical histories, clinical records, and facility logistics are frequently siloed across independent hospitals, diagnostic labs, and pharmacies. Our objective in this project is to architect a robust, centralized relational database that eliminates record fragmentation while enforcing strict relational integrity across a multi-facility network."*

---

#### 📌 Slide 02: Multi-Facility Healthcare Domain Boundaries
* **Visual on Screen:** Scope stripes showing Facilities, Clinical, People, Services, and Organizational boundaries. *(Tip: Hover over each stripe to highlight it.)*
* **Key Points to Emphasize:**
  * Multi-hospital network scope — 12+ physical centers integrated.
  * Five functional tiers from physical facilities down to automated administration.
* **Spoken Script:**
  > *"Moving to Slide 2, let's understand the system boundaries of our enterprise network. A comprehensive healthcare organization doesn't just manage doctor appointments — it coordinates physical facilities like main hospitals, specialized clinics, diagnostic centers, and blood banks.*
  >
  > *Our relational model spans five core functional tiers:*
  > 1. ***Facilities:*** *Top-level hospitals hosting specialized clinical wings, laboratories, and inpatient wards.*
  > 2. ***Clinical Operations:*** *Real-time bed availability tracking, radiology services, and pharmacy inventories.*
  > 3. ***People:*** *Unified patient identities shared across facilities, licensed doctors, nursing staff, and blood donors.*
  > 4. ***Services:*** *OPD consultations, inpatient admissions, surgical logs, and emergency ambulance dispatches.*
  > 5. ***Administration:*** *Public health vaccination drives, donor registries, and automated insurance billing claims."*

---

#### 📌 Slide 03: Core Entity Catalogue with Live Sample Data
* **Visual on Screen:** Master tables for `PATIENT`, `DOCTOR`, and `HOSPITAL`. *(Tip: Press Arrow keys to highlight each card individually.)*
* **Key Points to Emphasize:**
  * Master records are persistent and anchor the entire relational schema.
  * Primary Keys ( `Patient_ID`, `Doctor_ID`, `Hospital_ID` ) serve as immutable anchors.
* **Spoken Script:**
  > *"On Slide 3, we define our foundational Master Entities — tables that store persistent baseline records referenced by all subsequent transaction tables across the network.*
  >
  > * *The **`PATIENT`** table anchors demographic records. Every patient receives an immutable `Patient_ID` as their Primary Key, along with date of birth, blood group, and gender.*
  > * *The **`DOCTOR`** table registers licensed practitioners with their medical license number and specialization, such as Cardiology or Neurology.*
  > * *The **`HOSPITAL`** table records the physical branch facilities across metropolitan locations.*
  >
  > *Notice that these tables do not store transient visit logs — they act purely as single sources of truth. I now hand over to Speaker 2 to discuss infrastructure and attribute theory."*

---

### 👤 SPEAKER 2 (Slides 04 – 06)
**Theme:** Clinical Infrastructure, M:N Resolution & Attribute Theory

---

#### 📌 Slide 04: Hospital Departments & Doctor M:N Bridge
* **Visual on Screen:** `DEPARTMENT`, `DOCTOR_DEPARTMENT` (bridge), and `WARD` tables. *(Tip: Use Arrow keys to expand each card.)*
* **Key Points to Emphasize:**
  * Why `DOCTOR` to `DEPARTMENT` is a Many-to-Many relationship.
  * The `DOCTOR_DEPARTMENT` bridge table with Composite PK `(Doctor_ID, Dept_ID)`.
* **Spoken Script:**
  > *"Thank you, Speaker 1. On Slide 4, we model the internal infrastructure of our hospital network.*
  >
  > *A hospital contains multiple clinical departments like Cardiology, Oncology, or Critical Care. This is a standard One-to-Many relationship governed by `Hospital_ID` as a Foreign Key in the `DEPARTMENT` table.*
  >
  > *However, a significant relational challenge arises with doctors: a specialist physician may hold appointments in multiple departments across branches, and a department employs multiple doctors. This Many-to-Many relationship cannot be directly implemented with a single Foreign Key without introducing severe data redundancy.*
  >
  > *To resolve this, we introduced the **`DOCTOR_DEPARTMENT`** associative entity with a Composite Primary Key combining `(Doctor_ID, Dept_ID)` — both acting as Foreign Keys simultaneously. This junction table also records relationship-specific attributes such as `Joining_Date` and doctor `Position`."*

---

#### 📌 Slide 05: Appointments, Admissions & Surgeries (1:N Transactions)
* **Visual on Screen:** `APPOINTMENT`, `ADMISSION`, and `SURGERY` tables.
* **Key Points to Emphasize:**
  * Transaction lifecycle: OPD Visit → Inpatient Admission → Operating Theatre.
  * All three tables use Foreign Keys to maintain strict referential integrity.
* **Spoken Script:**
  > *"Slide 5 illustrates the primary clinical transactions that represent the patient care lifecycle.*
  >
  > * *The **`APPOINTMENT`** table tracks outpatient consultations. It links `Patient_ID` and `Doctor_ID` as Foreign Keys, recording appointment date and status — Scheduled, Completed, or Cancelled.*
  > * *When a patient requires hospitalization, an **`ADMISSION`** record is created, referencing the specific `Ward_ID` bed allocated and tracking both admission and discharge timestamps.*
  > * *If invasive care is required, the **`SURGERY`** table associates the patient with the lead surgeon and the assigned Operation Theatre room.*
  >
  > *Each of these operational tables maintains strict referential integrity through Foreign Keys. No appointment, admission, or surgery can exist without a valid patient and doctor record."*

---

#### 📌 Slide 06: ER Attribute Taxonomy & Relational Implementation
* **Visual on Screen:** Key, Composite, Multivalued, and Derived attribute breakdowns with SQL code.
* **Key Points to Emphasize:**
  * Why Multivalued attributes violate 1NF and how we resolve them.
  * Derived attributes are never stored statically.
* **Spoken Script:**
  > *"Slide 6 demonstrates our comprehensive classification of ER attribute types and how each is translated into relational database structures.*
  >
  > 1. ***Key Attribute ( `Patient_ID` ):*** *Uniquely identifies every tuple in the relation. Enforced in SQL via a B-Tree unique index and a `NOT NULL` constraint.*
  > 2. ***Composite Attribute ( `Address` ):*** *A patient's address is composed of multiple distinct sub-components. Rather than storing an unformatted string, we decompose it into `House_No`, `Street`, `City`, and `Zip_Code` — enabling precise indexing and filtering.*
  > 3. ***Multivalued Attribute ( `Phone_Number` ):*** *A patient can maintain multiple contact numbers. Storing comma-separated values in a single column violates First Normal Form. We resolve this by extracting phone numbers into a dedicated child table `Patient_Phone` where `(Patient_ID, Phone_Number)` forms a composite key.*
  > 4. ***Derived Attribute ( `Age` ):*** *Age changes continuously. Storing it statically creates stale data. Instead, we store `Date_of_Birth` and compute `Age` dynamically during queries using `FLOOR( DATEDIFF(NOW(), Date_of_Birth) / 365 )`.*
  >
  > *I now pass the floor to Speaker 3."*

---

### 👤 SPEAKER 3 (Slides 07 – 08)
**Theme:** Pharmacy Integration, Insurance Coverage & Emergency Fleet Operations

---

#### 📌 Slide 07: Prescriptions & Medication Junction (M:N)
* **Visual on Screen:** `PRESCRIPTION`, `PRESCRIPTION_MEDICINE` (junction), and `MEDICATION` tables.
* **Key Points to Emphasize:**
  * A prescription-to-medication relationship is Many-to-Many.
  * The junction table `PRESCRIPTION_MEDICINE` stores line-item attributes: `Dosage` and `Frequency`.
* **Spoken Script:**
  > *"Thank you, Speaker 2. Slide 7 showcases our medical pharmacy integration.*
  >
  > *When a doctor conducts a consultation, they issue a prescription header. However, a single prescription can contain multiple medications — for example, an antibiotic along with a pain reliever — and the same medication appears across thousands of prescriptions.*
  >
  > *We modeled this Many-to-Many relationship using the **`PRESCRIPTION_MEDICINE`** associative table. It links `Rx_ID` and `Med_ID` as a Composite Primary Key, while storing line-item parameters such as specific `Dosage` — for example, 500mg — and administration `Frequency` — for example, twice daily for 7 days.*
  >
  > *This structure protects the master `MEDICATION` catalog inventory while maintaining a complete, patient-specific prescription history per visit."*

---

#### 📌 Slide 08: Insurance Policy Claims & Ambulance Dispatch
* **Visual on Screen:** `INSURANCE_POLICY`, `AMBULANCE`, and `AMBULANCE_DISPATCH` tables.
* **Key Points to Emphasize:**
  * Auxiliary services are fully integrated into the relational schema.
  * `AMBULANCE_DISPATCH` is a dependent entity — it cannot exist without a valid ambulance.
* **Spoken Script:**
  > *"On Slide 8, we integrate essential auxiliary and financial services into the database.*
  >
  > * *The **`INSURANCE_POLICY`** table bridges patients to third-party coverage providers — such as Green Delta or Pragati Life — capturing policy numbers, coverage limits, and validity periods for automated billing claim clearance.*
  > * *For emergency operations, the **`AMBULANCE`** table manages each hospital's vehicle fleet with registration IDs and current availability status.*
  > * *Every emergency run is logged in the **`AMBULANCE_DISPATCH`** table, linking the patient request, the deployed vehicle, the pickup location, and a precise timestamp.*
  >
  > *I will now hand over to Speaker 4 to discuss our treatment of weak entities, the full relationships table, and the ER diagram."*

---

### 👤 SPEAKER 4 (Slides 09 – 11)
**Theme:** Weak Entities, Relationship Cardinality Reference & Conceptual ER Diagram

---

#### 📌 Slide 09: Weak Entities & Identifying Relationships
* **Visual on Screen:** Three-column stripe layout showing Owner Entity, Weak Entity with Partial Key, and dependency rules. *(Tip: Hover over each stripe to focus on it.)*
* **Key Points to Emphasize:**
  * Definition of a weak entity and what a partial key means.
  * Three weak / associative entities: `AMBULANCE_DISPATCH`, `PATIENT_PHONE`, `PRESCRIPTION_MEDICINE`.
  * ON DELETE CASCADE cascade rule.
* **Spoken Script:**
  > *"Thank you, Speaker 3. Slide 9 addresses one of the more technically precise aspects of ER modeling — weak entities.*
  >
  > *A weak entity is one that cannot be uniquely identified by its own attributes alone. It is only meaningful in the context of its owner — or strong entity — and requires an identifying relationship combined with a partial key.*
  >
  > *In our design, we have three such dependencies:*
  >
  > * *First, **`AMBULANCE_DISPATCH`** depends on **`AMBULANCE`**. A dispatch record cannot exist without a registered ambulance. The composite partial key is `(Amb_ID, Dispatch_ID)`. Deleting the ambulance record cascades and removes all its associated dispatch logs — enforced via `ON DELETE CASCADE`.*
  > * *Second, **`PATIENT_PHONE`** depends on **`PATIENT`**. Since a patient can have multiple contact numbers — a multivalued attribute — we extract these into a child table with the partial key `(Patient_ID, Phone)`. This simultaneously satisfies First Normal Form.*
  > * *Third, **`PRESCRIPTION_MEDICINE`** is an associative entity jointly dependent on both `PRESCRIPTION` and `MEDICATION`. Its Composite Primary Key `(Rx_ID, Med_ID)` references both parent tables, and both Foreign Keys enforce `NOT NULL`."*

---

#### 📌 Slide 10: Relationships & Cardinality Summary
* **Visual on Screen:** Full-height stripe list with cardinality badges (1:N in blue, M:N in amber), entity pairs in slate, verb phrases, and enforcement notes. *(Tip: Hover over each row to focus on it.)*
* **Key Points to Emphasize:**
  * 8 One-to-Many relationships and 2 Many-to-Many relationships in the system.
  * Every M:N is resolved through a junction or bridge table.
* **Spoken Script:**
  > *"Slide 10 provides a complete reference of all relationships in our schema.*
  >
  > *Looking across the stripes, the blue badges represent One-to-Many relationships, and the amber badges represent Many-to-Many relationships that required junction tables for resolution.*
  >
  > *Reading across each row: the entity pair, the verb phrase that describes the semantic action between them, the cardinality, and finally the specific Foreign Key or bridge table that enforces the relationship at the database engine level.*
  >
  > *Notice that every Many-to-Many relationship is fully resolved — `DOCTOR` to `DEPARTMENT` via the `DOCTOR_DEPARTMENT` bridge, and `PRESCRIPTION` to `MEDICATION` via the `PRESCRIPTION_MEDICINE` junction. No raw M:N relationship exists unresolved in the final relational schema."*

---

#### 📌 Slide 11: Complete Entity-Relationship (ER) Model Diagram
* **Visual on Screen:** High-resolution interactive ER Diagram canvas. *(Tip: Move cursor across the diagram to dynamically zoom into specific sub-graphs.)*
* **Key Points to Emphasize:**
  * Unified conceptual view of all entities, relationships, and cardinalities together.
  * All elements shown on the previous slides are visible here as a connected whole.
* **Spoken Script:**
  > *"Slide 11 presents our complete conceptual Entity-Relationship Diagram — the single unified view that brings together everything we have discussed.*
  >
  > *(Hovering mouse over diagram sections)* *As you can see on the interactive canvas:*
  >
  > * *At the top, `HOSPITAL` anchors the organizational tree, branching downward through 1:N relationships into `DEPARTMENT`, `WARD`, and the `AMBULANCE` fleet.*
  > * *In the clinical core, `DOCTOR` and `PATIENT` are interconnected through three transaction hubs: `APPOINTMENT`, `ADMISSION`, and `SURGERY`.*
  > * *On the pharmacy branch, `PRESCRIPTION` decomposes through the `PRESCRIPTION_MEDICINE` junction into the `MEDICATION` catalog.*
  > * *The auxiliary branches handle insurance policies and ambulance dispatch operations.*
  >
  > *Every entity is annotated with its primary keys, foreign key references, and relationship cardinalities — consistent with the Chen ER notation we studied in Lecture 6. I now hand over to Speaker 5."*

---

### 👤 SPEAKER 5 (Slides 12 – 14)
**Theme:** Relational DDL Schema, Integrity Constraints & Conclusion

---

#### 📌 Slide 12: ER Entities to Relational SQL Table Schemas
* **Visual on Screen:** DDL column cards with SQL data types and constraints ( `VARCHAR`, `DATE`, `ENUM`, `CHECK` ). *(Tip: Press Arrow keys to expand each schema card.)*
* **Key Points to Emphasize:**
  * Direct translation from conceptual ER model to implementable SQL schema.
  * Domain constraints enforced at the storage engine level, not in application code.
* **Spoken Script:**
  > *"Thank you, Speaker 4. Slide 12 details our DDL mapping — the translation from conceptual ER entities into concrete, implementable SQL table schemas.*
  >
  > *We enforce data integrity at the storage engine level through several mechanisms:*
  > * *`VARCHAR` column widths are constrained to prevent memory bloating — for example, `Name VARCHAR(100)` rather than an unbounded text type.*
  > * *Critical fields like `Date_of_Birth` and `Name` carry `NOT NULL` constraints to guarantee record completeness.*
  > * *Domain validation such as `CHECK (Gender IN ('M', 'F', 'O'))` ensures only valid enumerated values are accepted.*
  > * *Foreign key constraints are configured with appropriate cascading rules to prevent orphaned child records.*
  >
  > *This schema is normalized to Third Normal Form — eliminating all transitive and partial dependencies while preserving full relational integrity."*

---

#### 📌 Slide 13: Enterprise Business Rules & Relational Constraints
* **Visual on Screen:** Numbered enterprise business rules with cardinality badges and engine enforcement annotations.
* **Key Points to Emphasize:**
  * Rules are enforced at the database engine level, not left to application logic.
  * ACID compliance is guaranteed by the relational constraint system.
* **Spoken Script:**
  > *"On Slide 13, we summarize the six cardinal enterprise business rules that govern the system:*
  >
  > 1. *Each hospital contains multiple departments — enforced by `FOREIGN KEY (Hospital_ID)` in the `DEPARTMENT` table.*
  > 2. *Doctors work across multiple departments — resolved by the `DOCTOR_DEPARTMENT` bridge with a Composite Primary Key.*
  > 3. *Patients schedule multiple consultations — tracked via `Patient_ID` and `Doctor_ID` Foreign Keys in `APPOINTMENT`.*
  > 4. *Inpatient admissions are allocated to specific ward beds — enforced by `Ward_ID` Foreign Key in `ADMISSION`.*
  > 5. *One prescription contains multiple drug line items — resolved via the `PRESCRIPTION_MEDICINE` associative table.*
  > 6. *A hospital's ambulance fleet logs multiple dispatches over time — enforced by `Ambulance_ID` Foreign Key in `AMBULANCE_DISPATCH`.*
  >
  > *These six rules ensure zero data duplication, full referential integrity, and guaranteed ACID compliance across all system transactions."*

---

#### 📌 Slide 14: Thank You & Q&A Handoff
* **Visual on Screen:** Thank You panel with design summary stats ( 13 Entities · 3 Weak/Assoc. · 10+ Relationships · 3NF ), full team roster, and faculty contact card.
* **Spoken Script:**
  > *"In conclusion, our relational database design delivers a scalable, fully normalized, and centralized foundation for an integrated healthcare network.*
  >
  > *The design covers 13 entities — including 3 weak or associative entities — connected through more than 10 formally defined relationships, all normalized to Third Normal Form. Every relationship has a precisely chosen enforcement mechanism, from simple Foreign Keys to composite junction tables with cascading delete rules.*
  >
  > *On behalf of Group 2 — Md. Sourav Rana, Mahtabul Al Nahian, Mst. Afia Tasnim Esha, Meherin Ritu, and Zubayer Watsit Zoha — we sincerely thank our honorable lecturer Ms. Nusrat Jahan Tazin and everyone present for your time and attention.*
  >
  > *We are now open to any questions or feedback. Thank you!"*

---

## 🎯 Faculty Q&A Preparation Cheat Sheet

### Q1: Why did you use associative entities for `DOCTOR_DEPARTMENT` and `PRESCRIPTION_MEDICINE`?
* **Answer:** *"In relational databases, Many-to-Many relationships cannot be directly implemented with a single Foreign Key without introducing severe redundancy or multi-valued cells, which violates 1NF. An associative table resolves the M:N into two 1:N relationships and allows us to store relationship-specific attributes — such as doctor `Joining_Date` and medication `Dosage` — alongside the junction keys."*

### Q2: How does your design ensure First Normal Form (1NF) for patient phone numbers?
* **Answer:** *"1NF requires all attribute values to be atomic. Since a patient can have multiple phone numbers, storing them comma-separated in a single column violates 1NF. We extracted phone numbers into a separate child table `Patient_Phone` with composite primary key `(Patient_ID, Phone_Number)`, guaranteeing atomicity and enabling indexing on individual numbers."*

### Q3: Why is Age not stored directly in the `PATIENT` table?
* **Answer:** *"`Age` is a derived attribute. Storing it statically causes the value to become incorrect after every birthday, requiring costly periodic batch update jobs. Storing the immutable `Date_of_Birth` and deriving `Age` at query time via `FLOOR(DATEDIFF(NOW(), Date_of_Birth) / 365)` guarantees 100% accuracy with zero storage overhead."*

### Q4: What happens if a Hospital or Doctor record is deleted?
* **Answer:** *"In production healthcare systems, critical master records are protected by `ON DELETE RESTRICT` or a soft-delete mechanism — a flag such as `is_active = FALSE` — to preserve historical audit trails covering prescriptions, surgeries, and billing records. Weak entities like `AMBULANCE_DISPATCH` and `PATIENT_PHONE` use `ON DELETE CASCADE` since their existence is fully dependent on their owner."*

### Q5: What is the difference between a weak entity and an associative entity in your design?
* **Answer:** *"A weak entity ( such as `AMBULANCE_DISPATCH` or `PATIENT_PHONE` ) has no meaningful existence without its one owner entity and relies on the owner's key as part of its own partial key. An associative entity ( such as `PRESCRIPTION_MEDICINE` or `DOCTOR_DEPARTMENT` ) resolves a Many-to-Many relationship between two strong entities and uses a composite key formed from both parents' Primary Keys."*

### Q6: How is your schema normalized to 3NF?
* **Answer:** *"Third Normal Form requires that every non-key attribute depends only on the Primary Key and nothing else. We achieved this by: (1) removing all multi-valued attributes like `Phone_Number` into child tables to satisfy 1NF; (2) ensuring all partial dependencies — attributes depending on part of a composite key — were eliminated for 2NF; and (3) removing transitive dependencies, where one non-key attribute determines another, for 3NF."*

---
*End of Speaker Notes — SE 223 Presentation Group 2 · 14 Slides · Daffodil International University.*
