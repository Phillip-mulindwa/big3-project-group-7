# Stage 5: Fourth Normal Form (4NF)

## Which multi-valued dependencies did we identify?

A multi-valued dependency (MVD) X →→ Y means that for a given value of X,
the set of Y values is completely independent of any other attributes in
the same table.

We went through every BCNF table and looked for cases where one entity
controls two or more independent lists of facts stored together.

We found the following MVDs in the design:

WorkerName →→ WorkerSkill
WorkerName →→ WorkerCertification

SupplierName →→ SupplierPhone

EquipmentUsed →→ TrainingModule
EquipmentUsed →→ InspectionItem

## Which independent lists were mixed together?

The first two pairs (worker skills/certifications and supplier phones) were
already separated into their own two-column tables during earlier
normalization stages. Each of those tables holds only one multi-valued
fact per entity, so they contain only trivial MVDs and already satisfy 4NF.

The only pair that was not yet separated was equipment data. A piece of
equipment independently has a list of required training modules and a list
of required inspection items. These two lists have no connection to each
other — knowing which training modules a crane needs tells you nothing
about which inspection items it needs and vice versa.

If we had stored this in one table:

  equipment_requirements(EquipmentUsed, TrainingModule, InspectionItem)

it would violate 4NF. Because EquipmentUsed →→ TrainingModule and
EquipmentUsed →→ InspectionItem are both non-trivial MVDs in the same
table, every training module would have to be paired with every inspection
item for the same equipment type to keep the data consistent. That creates
redundant rows that represent no real fact.

## How did we separate them?

We created three new tables:

equipment_types.csv
- Columns: EquipmentUsed
- Primary key: EquipmentUsed
- One row per distinct equipment type used across all projects

equipment_training.csv
- Columns: EquipmentUsed, TrainingModule
- Primary key: (EquipmentUsed, TrainingModule)
- Foreign key: EquipmentUsed references equipment_types(EquipmentUsed)
- Each row records one training module required for one equipment type

equipment_inspections.csv
- Columns: EquipmentUsed, InspectionItem
- Primary key: (EquipmentUsed, InspectionItem)
- Foreign key: EquipmentUsed references equipment_types(EquipmentUsed)
- Each row records one inspection item required for one equipment type

project_equipment.csv also gains a foreign key in this stage:
- Foreign key: EquipmentUsed references equipment_types(EquipmentUsed)

Each table carries exactly one independent multi-valued fact per equipment
type, so neither table has a non-trivial MVD.

All other BCNF tables (projects, workers, clients, supervisors, suppliers,
supplier_phones, project_workers, worker_skills, worker_certifications,
project_materials, project_equipment) are carried into 4NF unchanged
because they were already free of non-trivial MVDs.

## What are the primary keys of the new tables?

equipment_types:
  Primary key: EquipmentUsed

equipment_training:
  Primary key: (EquipmentUsed, TrainingModule)
  Foreign key: EquipmentUsed → equipment_types(EquipmentUsed)

equipment_inspections:
  Primary key: (EquipmentUsed, InspectionItem)
  Foreign key: EquipmentUsed → equipment_types(EquipmentUsed)

project_equipment:
  Primary key: (ProjectID, EquipmentUsed)
  Foreign keys: ProjectID → projects(ProjectID)
                EquipmentUsed → equipment_types(EquipmentUsed)

## Why is the final design in 4NF?

Every table in the design now holds at most one independent multi-valued
fact for each entity. Specifically:

- worker_skills holds only WorkerName →→ WorkerSkill. No second MVD.
- worker_certifications holds only WorkerName →→ WorkerCertification. No second MVD.
- supplier_phones holds only SupplierName →→ SupplierPhone. No second MVD.
- equipment_training holds only EquipmentUsed →→ TrainingModule. No second MVD.
- equipment_inspections holds only EquipmentUsed →→ InspectionItem. No second MVD.

All remaining tables have single-valued attributes for every key, so they
contain no non-trivial MVDs at all.

Because every table is in BCNF and no table contains two or more
independent multi-valued dependencies, the entire design satisfies 4NF.
