# Day 74 : Jenkins Database Backup Job

## Introduction
In this task, we configure a Jenkins job that performs automated MySQL database backups every 10 minutes using SSH-based authentication...

## Concepts to Know Before Starting
1. SSH Key-Based Authentication
2. SSH Agent Plugin
3. Jenkins Credentials
4. mysqldump
5. Cron Schedule
6. Parameterized Jobs

## Steps to Complete the Task

### Step 1: Login to Jenkins
- Login using username `admin` and password `Adm!n321`.

### Step 2: Install Required Plugin
- Install the SSH Agent plugin from Manage Jenkins → Plugins.

### Step 3: Generate SSH Keys
```bash
ssh-keygen -t ed25519
ls -lah .ssh/
ssh-copy-id peter@stdb01
ssh-copy-id clint@stbkp01
ssh clint@stbkp01
ls -lah .ssh/
cat .ssh/id_ed25519
```

### Step 4: Add Jenkins Credentials
- Add SSH Username with Private Key.
- ID: `db-backup`
- Username: `peter`

### Step 5: Create Job
- Create freestyle project `database-backup0`.

### Step 6: Add Build Parameters
| Parameter | Value |
|----------|--------|
| DB_NAME | `kodekloud_db01` |
| DB_USER | `kodekloud_roy` |
| DB_PASS | `asdfgdsd` |

### Step 7: Set Trigger
```
*/10 * * * *
```

### Step 8: Enable SSH Agent
Select credential `db-backup`.

### Step 9: Build Step Shell Script
```bash
FILE_PATH="/tmp/db_$(date +%F).sql"

ssh -o StrictHostKeyChecking=no -t peter@stdb01 "mysqldump -u ${DB_USER} -p${DB_PASS} ${DB_NAME} > ${FILE_PATH}"

scp -o StrictHostKeyChecking=no peter@stdb01:${FILE_PATH} clint@stbkp01:/home/clint/db_backups/
```

### Step 10: Run the Job
- Build with parameters.

## Summary of Commands
```bash
ssh-keygen -t ed25519
ls -lah .ssh/
ssh-copy-id peter@stdb01
ssh-copy-id clint@stbkp01
cat .ssh/id_ed25519
```
