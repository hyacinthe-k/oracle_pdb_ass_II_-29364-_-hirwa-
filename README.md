# Oracle PDB Assignment II

## Student Information

**Student Name:** Hirwa Kambali Hyacinthe
**Student ID:** 29364
**Course:** PL/SQL / Oracle Database

---

## 1. Project Overview

This project was completed as part of an Oracle Database and PL/SQL assignment. The purpose of the assignment was to practice creating, managing, verifying, and deleting Oracle Pluggable Databases (PDBs).

The assignment involved creating a main PDB using the required naming format, configuring the required database user, verifying the PDB and user, and creating and deleting a temporary PDB.

### Main PDB

`HI_PDB_29364`

### Coursework User

`HIRWA_PLSQLAUCA_29364`

### Temporary PDB

`HI_TO_DELETE_PDB_29364`

---

## 2. Scope Summary

The scope of this assignment covers basic Oracle PDB administration and database management.

The main activities included:

* Creating the required Pluggable Database.
* Configuring the required database user.
* Opening and verifying the PDB.
* Checking PDB status using SQL commands.
* Saving the PDB state.
* Creating a temporary PDB.
* Closing the temporary PDB.
* Permanently deleting the temporary PDB.
* Verifying that the temporary PDB was removed.
* Documenting the work using screenshots.
* Recording challenges encountered and their solutions.

---

## 3. Oracle Environment

The assignment was completed using the following environment:

| Item                   | Details                   |
| ---------------------- | ------------------------- |
| Database               | Oracle Database Free 26AI |
| Database Tool          | Oracle SQL Developer      |
| Operating System       | Windows                   |
| Host                   | localhost                 |
| Port                   | 1521                      |
| Service Name           | FREEPDB1                  |
| Main PDB               | HI_PDB_29364              |
| Administrative Account | SYS                       |
| Coursework User        | HIRWA_PLSQLAUCA_29364     |

Administrative operations were performed using the `SYS` account with the required privileges.

---

# 4. Task 1 — Create a New PDB

## Objective

The first task was to create a new Oracle Pluggable Database using the required naming convention.

The PDB created for the assignment was:

```text
HI_PDB_29364
```

The required coursework user was:

```text
HIRWA_PLSQLAUCA_29364
```

## PDB Creation

The Oracle environment required the use of `FILE_NAME_CONVERT` because Oracle Managed Files were not enabled for the PDB creation process.

The PDB seed directory was identified as:

```text
C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\PDBSEED\
```

The PDB was created using a separate destination directory.

```sql
CREATE PLUGGABLE DATABASE HI_PDB_29364
ADMIN USER HIRWA_PLSQLAUCA_29364
IDENTIFIED BY [password]
FILE_NAME_CONVERT = (
    'C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\PDBSEED\',
    'C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\HI_PDB_29364\'
);
```

The actual password is not included in this repository.

## Opening the PDB

After creation, the PDB was opened using:

```sql
ALTER PLUGGABLE DATABASE HI_PDB_29364 OPEN;
```

The PDB status was checked with:

```sql
SHOW PDBS;
```

The expected result was:

```text
HI_PDB_29364    READ WRITE
```

## Saving the PDB State

The PDB state was saved using:

```sql
ALTER PLUGGABLE DATABASE HI_PDB_29364 SAVE STATE;
```

This allows the PDB to retain its open state after a database restart.

### Screenshot Evidence

![PDB Creation](screenshots/pdb_creation/01_main_pdb.png)

![PDB Status](screenshots/pdb_creation/02_pdb_status.png)

---

# 5. Task 2 — Create and Delete a Temporary PDB

## Objective

The second task was to create a temporary PDB and then completely remove it from the database.

The temporary PDB was named:

```text
HI_TO_DELETE_PDB_29364
```

## Creating the Temporary PDB

The temporary PDB was created from the PDB seed:

```sql
CREATE PLUGGABLE DATABASE HI_TO_DELETE_PDB_29364
ADMIN USER temp_admin
IDENTIFIED BY [password]
FILE_NAME_CONVERT = (
    'C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\PDBSEED\',
    'C:\APP\KALIS\PRODUCT\26AI\ORADATA\FREE\HI_TO_DELETE_PDB_29364\'
);
```

The temporary PDB was then opened:

```sql
ALTER PLUGGABLE DATABASE HI_TO_DELETE_PDB_29364 OPEN;
```

Its existence was verified using:

```sql
SHOW PDBS;
```

## Closing the Temporary PDB

Before the temporary PDB could be deleted, it was closed:

```sql
ALTER PLUGGABLE DATABASE HI_TO_DELETE_PDB_29364 CLOSE IMMEDIATE;
```

## Deleting the Temporary PDB

The temporary PDB was permanently deleted together with its datafiles:

```sql
DROP PLUGGABLE DATABASE HI_TO_DELETE_PDB_29364 INCLUDING DATAFILES;
```

The remaining PDBs were then checked:

```sql
SHOW PDBS;
```

The temporary PDB was no longer listed, confirming that the deletion was successful.

### Screenshot Evidence

![Temporary PDB](screenshots/pdb_deletion/01_temp_pdb.png)

![Deletion Verification](screenshots/pdb_deletion/02_deletion_verified.png)

---

# 6. Task 3 — PDB and User Verification

The main PDB and coursework user were verified after completing the database operations.

## Checking the PDB

The following query was used:

```sql
SELECT NAME, OPEN_MODE, RESTRICTED
FROM V$PDBS
WHERE NAME = 'HI_PDB_29364';
```

The main PDB was expected to show:

```text
HI_PDB_29364
READ WRITE
```

## Checking the User

The required user was checked using:

```sql
SELECT USERNAME, ACCOUNT_STATUS
FROM DBA_USERS
WHERE USERNAME = 'HIRWA_PLSQLAUCA_29364';
```

This query was used to confirm that the required coursework user existed and to check its account status.

### Screenshot Evidence

![User Verification](screenshots/pdb_creation/03_user.png)

---

# 7. SQL Commands Used

The following commands were used during the assignment.

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
ALTER SESSION SET CONTAINER = HI_PDB_29364;
```

### Check the Current Container

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
ALTER PLUGGABLE DATABASE HI_PDB_29364 SAVE STATE;
```

### Check the Coursework User

```sql
SELECT USERNAME, ACCOUNT_STATUS
FROM DBA_USERS
WHERE USERNAME = 'HIRWA_PLSQLAUCA_29364';
```

---

# 8. Business Scenario

A university or organization can use Oracle Pluggable Databases to separate different applications, departments, projects, or testing environments while keeping them within a single Oracle Container Database.

For this assignment, `HI_PDB_29364` represents a dedicated database environment for student coursework.

The temporary PDB demonstrates how an isolated database environment can be created for testing and then removed when it is no longer needed.

This approach provides a practical way to organize database environments and manage separate workloads within an Oracle database system.

---

# 9. Challenges Encountered and Solutions

## Challenge 1 — ORA-65016

During PDB creation, the following error may occur when Oracle Managed Files are not enabled:

```text
ORA-65016: FILE_NAME_CONVERT must be specified
```

### Solution

The PDB seed directory was identified and a destination directory was specified using the `FILE_NAME_CONVERT` clause.

---

## Challenge 2 — Existing Database Files

An attempt to create a PDB using files that already existed could result in:

```text
ORA-01537: cannot add file - file already part of database
```

### Solution

The existing PDB and its files were checked before attempting another creation. This prevented duplicate database files from being created.

---

## Challenge 3 — PDB Must Be Closed Before Deletion

When deleting a PDB that was still open, Oracle could return:

```text
ORA-65025: Pluggable database is not closed on all instances.
```

### Solution

The temporary PDB was first closed:

```sql
ALTER PLUGGABLE DATABASE HI_TO_DELETE_PDB_29364 CLOSE IMMEDIATE;
```

It was then deleted:

```sql
DROP PLUGGABLE DATABASE HI_TO_DELETE_PDB_29364 INCLUDING DATAFILES;
```

---

# 10. Final Results

At the end of the assignment:

* The required PDB `HI_PDB_29364` was created and configured.
* The PDB was opened in `READ WRITE` mode.
* The coursework user `HIRWA_PLSQLAUCA_29364` was verified.
* The PDB state was saved.
* The temporary PDB `HI_TO_DELETE_PDB_29364` was created for testing.
* The temporary PDB was closed and permanently deleted.
* The deletion was verified using Oracle SQL commands.
* Screenshots were collected as evidence of the completed work.

---

# 11. Screenshots

The screenshots are organized into the following folders:

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

```text
oracle_pdb_ass_II_29364_hirwa/
│
├── README.md
│
└── screenshots/
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

---

# 13. Integrity Statement

I confirm that the work presented in this repository was completed in my own Oracle Database environment. The SQL commands were executed and tested as part of the assignment, and the screenshots represent the results from my database setup.

Passwords and other sensitive credentials have not been included in this repository.

---

# 14. Submission Details

**Repository Name:** `oracle_pdb_ass_II_29364_hirwa`

**Repository Link:** [Paste GitHub repository link here]

**Student Name:** Hirwa Kambali Hyacinthe

**Student ID:** 29364

**PDB Name Created:** `HI_PDB_29364`

**Temporary PDB:** `HI_TO_DELETE_PDB_29364`

**Issues Encountered:** Yes

**Issues Resolved:** Yes
