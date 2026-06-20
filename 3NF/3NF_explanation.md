# Stage 3: Third Normal Form

## Which transitive dependencies did we find?

When we went through the tables from 2NF, most of them were already
fine and did not need any changes. The only table with a problem
was projects.csv. It had two transitive dependency chains sitting
in it that we had to sort out.

First one:
ProjectID → ClientName → ClientPhone, ClientEmail, ClientCity

Second one:
ProjectID → SupervisorName → SupervisorPhone

Both ClientName and SupervisorName are non-key attributes in the
projects table. But they were each determining other columns, which
is exactly the transitive dependency problem. The path to those
columns goes through a non-key attribute instead of coming directly
from the primary key.

## Which non-key attributes depended on other non-key attributes?

ClientPhone, ClientEmail and ClientCity all depended on ClientName.
ProjectID is the primary key of projects, not ClientName, so having
those three columns determined by ClientName was a transitive
dependency that violated 3NF.

SupervisorPhone had the same issue. It only tells you the phone of
the supervisor assigned to the project. Once you know who the
supervisor is you already know their number, meaning SupervisorPhone
was being determined by SupervisorName not by ProjectID directly.

For the other seven tables we carried over from 2NF - workers,
suppliers, project_workers, worker_skills, worker_certifications,
project_materials and project_equipment - none of them had a non-key
column determining another non-key column so they came into 3NF
unchanged.

## Which new tables did we create?

Two new tables were created:

clients.csv
- Primary key: ClientName
- Columns: ClientPhone, ClientEmail, ClientCity
- One row per client company

supervisors.csv
- Primary key: SupervisorName
- Columns: SupervisorPhone
- One row per supervisor

## How did we use foreign keys to preserve relationships?

projects.csv was updated to drop ClientPhone, ClientEmail, ClientCity
and SupervisorPhone. ClientName and SupervisorName stay in the table
but now act as foreign keys that point to the two new tables.

projects.csv
- Foreign key: ClientName references clients(ClientName)
- Foreign key: SupervisorName references supervisors(SupervisorName)

The link between a project and its client or supervisor is still
there, you just have to join the tables to get the full details.
Nothing was lost, it just lives in the right place now.

## Why is the design now in 3NF?

Every non-key column in every table now depends on the primary key
and only the primary key. There is no case left where a non-key
column tells you something about another non-key column in the same
table. The two transitive chains that existed in projects.csv were
broken by pulling the dependent attributes into their own tables and
leaving foreign keys behind. That satisfies 3NF across the whole
design.
