# How to deploy an Oracle database to Docker Desktop
## Step 1: Make an Docker image
To make an Docker image, you need to use the buildContainerImage.sh bash script.
With the following command:
```
./buildContainerImage.sh -v 11.2.0.2 -x
```
This will create an local image, which you can push to docker hub for easy access (optional).

## Step 2: Running the docker compose file
In the Docker compose file you can specify a password. The image will take several minutes to fully deploy and start. It is important to have a shm_size of at least 1G. Otherwise the image will fail to deploy.

## Step 3: Create a new table and user
Use the following SQL to add a new table and user for a clean workspace in the database:
```
CREATE USER CRCD_DB IDENTIFIED BY CRCD_DB;
GRANT UNLIMITED TABLESPACE TO CRCD_DB;
GRANT CREATE SESSION TO CRCD_DB;
GRANT CREATE TABLE TO CRCD_DB;
GRANT CREATE VIEW TO CRCD_DB;
GRANT CREATE TRIGGER TO CRCD_DB;
GRANT CREATE PROCEDURE TO CRCD_DB;
GRANT CREATE SEQUENCE TO CRCD_DB;
GRANT ALL PRIVILEGES TO CRCD_DB;

GRANT SELECT ON v_$session TO CRCD_DB;
```
You can run the script with your favorite SQL editor and change CRCD_DB to whatever you want.