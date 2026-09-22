# Oracle PDB Assignment II

## Student Information

**Student Name:** Hirwa Kambali Hyacinthe
**Student ID:** 29364
**Course:** PL/SQL / Oracle Database

---

## 1. Project Overview

This project was completed as part of an Oracle Database and PL/SQL assignment. The main purpose of the assignment was to practice working with Oracle Pluggable Databases (PDBs) and basic database administration.

The work included creating a new PDB, configuring and verifying a database user, testing PDB operations, creating a temporary PDB, and completely removing the temporary PDB.

### Main PDB

`MU_PDB_27426`

### Coursework User

`MU_PLSQLAUCA_27426`

### Temporary PDB

`MU_TO_DELETE_PDB_27426`

---

## 2. Scope Summary

The scope of this assignment covers the creation, configuration, verification, and deletion of Oracle Pluggable Databases.

The main tasks included:

* Creating the required PDB.
* Configuring the required administrative user.
* Opening and verifying the PDB.
* Checking the PDB status using Oracle SQL commands.
* Saving the PDB state.
* Creating a temporary PDB for testing.
* Closing and deleting the temporary PDB.
* Verifying that the temporary PDB was completely removed.
* Documenting the work with screenshots.
* Recording challenges encountered during the implementation and how they were resolved.

---

## 3. Oracle Environment

The assignment was completed using the following environment:

| Item                    | Details                   |
| ----------------------- | ------------------------- |
| Database                | Oracle Database Free 26AI |
| Tool                    | Oracle SQL Developer      |
| Operating System        | Windows                   |
| Host                    | localhost                 |
| Port                    | 1521                      |
| Service Name            | FREEPDB1                  |
| Main PDB                | MU_PDB_27426              |
| Administrative Account  | SYS                       |
| User Created/Configured | MU_PLSQLAUCA_27426        |

Administrative commands were executed using the `SYS` account with the required administrative privileges.

---

# 4. Task 1 — Create the Main PDB

## Objective

The first task was to create a new Pluggable Database using the required naming convention.

The final PDB name was:

```text
MU_PDB_27426
```

The required database user was:

```text
MU_PLSQLAUCA_27426
```

## PDB Creation

The Oracle environment did not have Oracle Managed Files enabled, so the `FILE_NAME_CONVERT` clause was required when creating the PDB.

The PDB seed directory was:

```text
C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\PDBSEED\
```

The new PDB was created in its own directory.

Example command:

```sql
CREATE PLUGGABLE DATABASE MU_PDB_27426
ADMIN USER MU_PLSQLAUCA_27426
IDENTIFIED BY [password]
FILE_NAME_CONVERT = (
    'C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\PDBSEED\',
    'C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\MU_PDB_27426\'
);
```

The actual password is not included in this repository.

## Opening the PDB

After creation, the PDB was opened using:

```sql
ALTER PLUGGABLE DATABASE MU_PDB_27426 OPEN;
```

The PDB status was then checked with:

```sql
SHOW PDBS;
```

The final status showed:

```text
MU_PDB_27426    READ WRITE
```

## Saving the PDB State

To allow the PDB to retain its open state after database restart, the following command was executed:

```sql
ALTER PLUGGABLE DATABASE MU_PDB_27426 SAVE STATE;
```

### Screenshot Evidence

![Main PDB](screenshots/pdb_creation/01_main_pdb.png)

![PDB Status](screenshots/pdb_creation/02_pdb_status.png)

---

# 5. Task 2 — Create and Delete a Temporary PDB

## Objective

The second task was to create a temporary PDB and then remove it completely.

The temporary PDB was named:

```text
MU_TO_DELETE_PDB_27426
```

## Creating the Temporary PDB

The temporary PDB was created using the same PDB seed directory and a separate destination directory.

```sql
CREATE PLUGGABLE DATABASE MU_TO_DELETE_PDB_27426
ADMIN USER temp_admin
IDENTIFIED BY [password]
FILE_NAME_CONVERT = (
    'C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\PDBSEED\',
    'C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\MU_TO_DELETE_PDB_27426\'
);
```

The temporary PDB was then opened:

```sql
ALTER PLUGGABLE DATABASE MU_TO_DELETE_PDB_27426 OPEN;
```

Its existence was verified with:

```sql
SHOW PDBS;
```

## Closing the Temporary PDB

Before deleting the PDB, it had to be closed:

```sql
ALTER PLUGGABLE DATABASE MU_TO_DELETE_PDB_27426 CLOSE IMMEDIATE;
```

## Deleting the Temporary PDB

The temporary PDB was then permanently removed together with its associated datafiles:

```sql
DROP PLUGGABLE DATABASE MU_TO_DELETE_PDB_27426 INCLUDING DATAFILES;
```

The remaining PDBs were checked again:

```sql
SHOW PDBS;
```

The temporary PDB was no longer listed, confirming that the deletion was successful.

### Screenshot Evidence

![Temporary PDB](screenshots/pdb_deletion/01_temp_pdb.png)

![PDB Deletion](screenshots/pdb_deletion/02_deletion_verified.png)

---

# 6. Task 3 — PDB and User Verification

The PDB and coursework user were verified after completing the database operations.

## Checking the PDB

The following command was used:

```sql
SELECT NAME, OPEN_MODE, RESTRICTED
FROM V$PDBS
WHERE NAME = 'MU_PDB_27426';
```

The main PDB was confirmed to be:

```text
MU_PDB_27426
READ WRITE
```

## Checking the User

The required user was checked using:

```sql
SELECT USERNAME, ACCOUNT_STATUS
FROM DBA_USERS
WHERE USERNAME = 'MU_PLSQLAUCA_27426';
```

The query confirmed that the coursework user existed and was available in the database.

### Screenshot Evidence

![User Verification](screenshots/pdb_creation/03_user.png)

---

# 7. Important SQL Commands Used

The following commands were used throughout the assignment.

### Display PDBs

```sql
SHOW PDBS;
```

### Change to the Root Container

```sql
ALTER SESSION SET CONTAINER = CDB$ROOT;
```

### Change to the Main PDB

```sql
ALTER SESSION SET CONTAINER = MU_PDB_27426;
```

### Check Current Container

```sql
SHOW CON_NAME;
```

### Check PDB Information

```sql
SELECT NAME, OPEN_MODE, RESTRICTED
FROM V$PDBS;
```

### Save PDB State

```sql
ALTER PLUGGABLE DATABASE MU_PDB_27426 SAVE STATE;
```

### Check User

```sql
SELECT USERNAME, ACCOUNT_STATUS
FROM DBA_USERS
WHERE USERNAME = 'MU_PLSQLAUCA_27426';
```

---

# 8. Business Scenario

A university or organization can use Oracle Pluggable Databases to separate different applications, departments, projects, or development environments while keeping them within the same Oracle Container Database.

For this assignment, `MU_PDB_27426` represents a dedicated database environment for coursework.

The temporary PDB demonstrates how an isolated database environment can be created for testing and then removed when it is no longer required.

This approach can help with database organization, testing, administration, and separation of different workloads.

---

# 9. Challenges Encountered and Solutions

## Challenge 1 — ORA-65016

During the initial PDB creation, the following error was encountered:

```text
ORA-65016: FILE_NAME_CONVERT must be specified
```

### Solution

The Oracle environment was not using Oracle Managed Files for the PDB creation process. The PDB seed directory was identified and a destination directory was provided using `FILE_NAME_CONVERT`.

---

## Challenge 2 — ORA-01537

Another attempt to create a PDB resulted in:

```text
ORA-01537: cannot add file - file already part of database
```

### Solution

The existing PDB and its associated files were checked instead of attempting to create the same database again. The existing PDB was then used and renamed to the required assignment name.

---

## Challenge 3 — PDB Naming

The PDB initially had a different name and needed to match the assignment naming requirement.

### Solution

The PDB was renamed to:

```text
MU_PDB_27426
```

The final name was verified using:

```sql
SHOW PDBS;
```

---

## Challenge 4 — ORA-65025

When the temporary PDB was initially being deleted, Oracle returned:

```text
ORA-65025: Pluggable database MU_TO_DELETE_PDB_27426 is not closed on all instances.
```

### Solution

The PDB was first closed using:

```sql
ALTER PLUGGABLE DATABASE MU_TO_DELETE_PDB_27426 CLOSE IMMEDIATE;
```

It was then successfully deleted using:

```sql
DROP PLUGGABLE DATABASE MU_TO_DELETE_PDB_27426 INCLUDING DATAFILES;
```

---

# 10. Final Results

At the end of the assignment:

* The required PDB `MU_PDB_27426` was successfully configured.
* The PDB was opened in `READ WRITE` mode.
* The required coursework user `MU_PLSQLAUCA_27426` was verified.
* The PDB state was saved.
* The temporary PDB `MU_TO_DELETE_PDB_27426` was successfully deleted.
* The deletion was verified using Oracle database queries.
* Screenshots were collected as evidence of the completed tasks.

---

# 11. Screenshots

The screenshots for this assignment are organized into the following folders:

```text
screenshots/
│
├── pdb_creation/
│   ├── 01_main_pdb.png
│   ├── 02_pdb_status.png
│   └── 03_user.png
│
├── pdb_deletion/
│   ├── 01_temp_pdb.png
│   └── 02_deletion_verified.png
│
└── oem_dashboard/
```

The screenshots provide visual evidence of the PDB creation, verification, and deletion processes.

---

# 12. Repository Structure

The repository is organized as follows:

```text
oracle_pdb_ass_II_27426_[firstname]/
│
├── README.md
│
└── screenshots/
<img width="3840" height="2035" alt="pdb_creation" src="https://github.com/user-attachments/assets/94f5e221-69ad-46fa-8541-487aa23c339c" />
<img width="3840" height="2073" alt="2" src="https://github.com/user-attachments/assets/a46c957c-bca2-4f21-8634-d1d297de485d" />
<img width="3840" height="2055" alt="3" src="https://github.com/user-attachments/assets/9cbee6d8-7174-4ca9-a5c1-d86f32684f11" />
<img width="3814" height="2018" alt="4" src="https://github.com/user-attachments/assets/e9a58eae-3456-4ed4-8ee0-29ef2e9e88b7" />
<img width="3840" height="2062" alt="5" src="https://github.com/user-attachments/assets/2d3e00bb-6b52-4cd7-8d44-3b351719b34a" />


```

---


