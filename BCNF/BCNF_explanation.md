# Stage 4: Boyce-Codd Normal Form (BCNF)

## Which functional dependencies did we check?

We examined every table carried over from 3NF and listed all functional
dependencies present in each one:

- projects: ProjectID → ProjectName, ProjectType, StartDate, EndDate,
  SiteAddress, SiteCity, SiteState, ClientName, SupervisorName
- workers: WorkerName → WorkerPhone, WorkerHourlyRate
- clients: ClientName → ClientPhone, ClientEmail, ClientCity
- supervisors: SupervisorName → SupervisorPhone
- project_workers: (ProjectID, WorkerName) — no additional attributes
- worker_skills: (WorkerName, WorkerSkill) — no additional attributes
- worker_certifications: (WorkerName, WorkerCertification) — no additional attributes
- project_materials: (ProjectID, SupplierName, MaterialSupplied) → MaterialUnitCost
- project_equipment: (ProjectID, EquipmentUsed) → EquipmentRentalCost
- suppliers: (SupplierName, SupplierPhone) is the composite key,
  but SupplierName → SupplierCity also holds

## Did we find any determinant that was not a candidate key?

Yes, in suppliers.csv.

In the 3NF version of suppliers, two suppliers (BuildPro Supplies and
SteelWorks Inc) each have two phone numbers, producing multiple rows per
supplier name. That means SupplierName alone cannot uniquely identify a
row, so the actual candidate key is the composite (SupplierName, SupplierPhone).

Despite that, SupplierName → SupplierCity holds: BuildPro Supplies is
always in Boston and SteelWorks Inc is always in Chicago. SupplierName
determines SupplierCity, but SupplierName is not a candidate key. That
is a BCNF violation.

Every other table passed the BCNF check because every determinant in
those tables is a candidate key.

## Which table violated BCNF?

Only suppliers.csv violated BCNF.

The violation:
  SupplierName → SupplierCity
  SupplierName is NOT a candidate key (it is only part of the composite
  key (SupplierName, SupplierPhone))

## How did we decompose the table?

We split the original suppliers table into two tables:

suppliers.csv
- Columns: SupplierName, SupplierCity
- Primary key: SupplierName
- One row per supplier, no phone numbers here

supplier_phones.csv
- Columns: SupplierName, SupplierPhone
- Primary key: (SupplierName, SupplierPhone)
- Foreign key: SupplierName references suppliers(SupplierName)
- One row per phone number a supplier has

project_materials.csv was not changed. It still references SupplierName
as a foreign key, which now points to the suppliers table where
SupplierName is the primary key.

## What candidate keys exist after decomposition?

suppliers.csv
- Candidate key: SupplierName

supplier_phones.csv
- Candidate key: (SupplierName, SupplierPhone)

All other tables retain the same candidate keys they had in 3NF:
- projects: ProjectID
- workers: WorkerName
- clients: ClientName
- supervisors: SupervisorName
- project_workers: (ProjectID, WorkerName)
- worker_skills: (WorkerName, WorkerSkill)
- worker_certifications: (WorkerName, WorkerCertification)
- project_materials: (ProjectID, SupplierName, MaterialSupplied)
- project_equipment: (ProjectID, EquipmentUsed)

## Why is the design now in BCNF?

In suppliers.csv the only functional dependency is
SupplierName → SupplierCity, and SupplierName is now the primary key,
so it is a candidate key. The violation is gone.

In supplier_phones.csv the only non-trivial dependency is
(SupplierName, SupplierPhone) → SupplierPhone (trivial) and the whole
composite key determines each row. There are no non-trivial FDs whose
left side is not a candidate key.

In every other table, all functional dependencies have a candidate key
on the left side, which was already true in 3NF and remains true now.

Every functional dependency across the entire design now has a candidate
key as its determinant. That satisfies BCNF throughout.
