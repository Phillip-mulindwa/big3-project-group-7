## Overview

The 1NF table contained atomic values,but it still stored information about projects, workers, suppliers, materials, equipment, skills, and certifications in a single relation.because the table effectively used a composite key formed from project, worker, supplier, material, and equipment information, many attributes depended on only part of that key. These are partial dependencies and violate Second Normal Form.

## Composite Keys Identified

Several relationships in the 1NF table required composite keys, including:

(ProjectID, WorkerName)
(WorkerName, WorkerSkill)
(WorkerName, WorkerCertification)
(ProjectID, SupplierName, MaterialSupplied)
(ProjectID, EquipmentUsed)
Partial Dependencies Found

The following attributes depended on only part of the composite key:

## Project Attributes

ProjectName, ProjectType, StartDate, EndDate, SiteAddress, SiteCity, and SiteState depend only on ProjectID.

## Client Attributes

ClientName, ClientPhone, ClientEmail, and ClientCity are associated with a project and do not depend on worker, supplier, material, or equipment information.

## Supervisor Attributes

SupervisorName and SupervisorPhone depend on the supervisor rather than the entire composite key.

## Worker Attributes

WorkerPhone and WorkerHourlyRate depend only on WorkerName.

## Supplier Attributes

SupplierCity and SupplierPhone depend only on SupplierName/SupplierPhone and not on projects, workers, materials, or equipment.

## Material Attributes

MaterialUnitCost depends on the supplied material and supplier relationship rather than the entire composite key.

## Equipment Attributes

EquipmentRentalCost depends only on the equipment used within a project.

## Tables Created

To remove partial dependencies, the 1NF table was decomposed into the following relations:

Projects
Workers
ProjectWorkers
WorkerSkills
WorkerCertifications
Suppliers
ProjectMaterials
ProjectEquipment
Primary Keys and Foreign Keys
Projects

## Primary Key:

ProjectID
Workers

## Primary Key:

WorkerName
ProjectWorkers

## Primary Key:

(ProjectID, WorkerName)

## Foreign Keys:

ProjectID → Projects
WorkerName → Workers
WorkerSkills

## Primary Key:

(WorkerName, WorkerSkill)

Foreign Key:

WorkerName → Workers
WorkerCertifications

## Primary Key:

(WorkerName, WorkerCertification)

Foreign Key:

WorkerName → Workers
Suppliers

## Primary Key:

(SupplierName, SupplierPhone)
ProjectMaterials

## Primary Key:

(ProjectID, SupplierName, MaterialSupplied)

## Foreign Keys:

ProjectID → Projects
SupplierName → Suppliers
ProjectEquipment

## Primary Key:

(ProjectID, EquipmentUsed)

## Foreign Key:

ProjectID → Projects
Why the Design is in 2NF

The decomposition removes all partial dependencies.every non-key attribute now depends on the entire primary key of its relation and not on only part of a composite key. As a result, the design satisfies the requirements of Second Normal Form (2NF)