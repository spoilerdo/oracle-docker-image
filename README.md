# How to deploy an Oracle database to Docker Desktop
## Step 0: Add a .rpm folder
You need to have a version of the Oracle Linux .rpm file that will be used to make the docker image.
Some older versions cannot be downloaded from the official site but can be downloaded from the following links:
After downloading and extracting put them in the corresponding version folder under a dockerfiles folder.
- [11.2.0.2](https://drive.google.com/file/d/1ttGZ3xW4iHlsNR6ubWqqgFxFk9xSWzxx/view?usp=sharing)
## Step 1: Make an Docker image
To make an Docker image, you need to use the buildContainerImage.sh bash script.
With the following command:
```
./buildContainerImage.sh -v 11.2.0.2 -x
```
This will create an local image, which you can push to docker hub for easy access (optional).

## Step 2: Running the docker compose file
In the Docker compose file you can specify a password. The image will take several minutes to fully deploy and start. It is important to have a shm_size of at least 1G. Otherwise the image will fail to deploy.

## Step 3: Connect locally to the database
To connect locally with your favorite database manager you can use the following credentials:
- Host: 127.0.0.1
- Port: 1521
- Database: XE (SID)
- Username: system
- Password: the one you chose
- Client: XE

To connect with it using an application you need a connection string with the following template:
```Data Source=(DESCRIPTION=(ADDRESS=(PROTOCOL=TCP)(HOST=...)(PORT=1521))(CONNECT_DATA=(SERVICE_NAME=XE)));User Id=...;Password=...;```.
You can find the host to look into the following file inside the docker image:
```cat ./u01/app/oracle/product/11.2.0/xe/network/admin/tnsnames.ora```
But for Docker desktop it usually is **localhost**

## Step 4: Create a new table and user
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

### Disclaimer
This repo has been forked and modified from https://github.com/oracle/docker-images