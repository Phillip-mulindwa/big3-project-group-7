# Stage 1: First Normal Form 

## Which columns violated 1NF?

Seven columns in the raw CSV violated 1NF by storing multiple values
in a single cell using a pipe character (|) as a separator. These columns were:

- WorkerSkills (example: "Carpentry|Framing")
- WorkerCertifications (example: "OSHA|First Aid")
- SupplierPhones (example: "617-555-9000|617-555-9001")
- MaterialSupplied (example: "Concrete|Steel")
- MaterialUnitCost (example: "120|300")
- EquipmentUsed (example: "Crane|Bulldozer")
- EquipmentRentalCost (example: "5000|3000")

Each of these cells contained more than one value, which breaks the
atomicity rule of 1NF. A table is in 1NF only when every cell holds
exactly one value.

## How did we make the values atomic?

I split every pipe-separated value by creating new rows. For each
individual skill, certification, phone number, material, and equipment
value, a separate row was produced. All other columns were repeated
on each new row so no information was lost.

For example, the original row for Mike Ross on Project P001 had:
- WorkerSkills = "Carpentry|Framing"
- WorkerCertifications = "OSHA|First Aid"

After applying 1NF this became four rows, one for each unique
combination of skill and certification.

## Did we create new rows, new tables, or both?

New rows only. All data at this stage remains in one flat table:
1nf_construction_projects.csv. No new tables were created. The
approach was purely to expand multi-valued cells horizontally into
individual atomic rows.

## What key identifies each row?

Each row is uniquely identified by the following composite key:

(ProjectID, WorkerName, WorkerSkill, WorkerCertification,
SupplierName, SupplierPhone, MaterialSupplied, EquipmentUsed)

Together these eight columns produce a unique combination for every
single row in the 1NF table.
