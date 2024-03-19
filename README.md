# load-mimic-iv-into-postgresql

## Requirements
This tutorial assumes the user has the following. Links are included to get them set up before moving to the next sections:

1. Windows 10/11 device.
2. Git installed (Tutorial here: [Click to learn how to install Git on windows](https://phoenixnap.com/kb/how-to-install-git-windows)).
3. Postgresql installed (Tutorial here: [Click to learn how to install postgresql.](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql/))
4. Access to [MIMIC-IV data.](https://physionet.org/content/mimiciv/2.2/) or [MIMIC-IV demo-data.](https://physionet.org/content/mimic-iv-demo/2.2/)
5. Recommended: WINRAR to unzip files. [Download WINRAR.](https://www.win-rar.com/download.html?&L=0)

>**NB: Access to MIMIC-IV Data requires becoming a credentialed user and completing a training. More information can be found online.**


## Instructions
1. Create a folder called **mimic-documents** in the documents folder. 
>NB: This can be stored in any folder but the documents folder is used for simplicity.

2. Right-click on the folder and click the option to open in terminal. 
![alt text](<Screenshot 2024-03-19 at 7.16.06 PM.png>)

3. Then type in the following and press enter: 

```bibtex
git clone https://github.com/MIT-LCP/mimic-code.git
```
![alt text](<Screenshot 2024-03-19 at 7.17.08 PM.png>)
Now the contents of the **mimic-documents** folder should look like this:
![alt text](<Screenshot 2024-03-19 at 7.18.29 PM.png>)

## Section-1: Prepare MIMIC-IV or MIMIC-IV demo

>**NB: The demo data would be used here as a sample but the process is essentially the same for both versions**

1. Download the mimic-demo file(links in requirements section):
![image of downloaded demo file](<Screenshot 2024-03-19 at 4.33.16 PM.png>)
***Downloaded demo file***

2. Using winRar, click on extract here. 
![alt text](<Screenshot 2024-03-19 at 6.17.10 PM.png>)


3. Copy the newly extracted folder  - **mimic-iv-clinical-database-demo-2.2** - and move it into the **mimic-documents** folder you created before.


4. Open the **mimic-iv-clinical-database-demo-2.2** that was just pasted in the **mimic-documents**  and it should lead to a page containing a "hosp" and an "icu" folder. 
![alt text](<Screenshot 2024-03-19 at 4.42.12 PM.png>)

5. Click on the hosp folder.
![alt text](<Screenshot 2024-03-19 at 5.07.01 PM.png>)the hosp folder and all its files

6. Select all files (Ctrl + a) and click on WinRAR, then click on "Extract here". This may take a while.
![alt text](<Screenshot 2024-03-19 at 5.13.20 PM.png>)
![alt text](<Screenshot 2024-03-19 at 5.13.31 PM.png>)

7. Do the same thing for the icu folder next to the hosp one. 


## Section-2: Getting Ready for Postgres

1. At this point, you should have unzipped contents of the "icu" and "hosp" folders in **mimic-iv-clinical-database-demo-2.2**  and the contents of **mimic-documents** should look like this:
![alt text](<Screenshot 2024-03-19 at 5.24.25 PM.png>)



2. Click the windows start button and type  **psql** and click on it.
![alt text](<Screenshot 2024-03-19 at 5.26.21 PM.png>)



3. The option "Server [localhost]" should show up first, click enter without typing anything. Do the same for "Database [postgres]",  "port[5432]" and "Username [postgres]". Next enter your password and click enter. 





4. Next, a "postgres=#" text should appear in the postgres console.
![alt text](<Screenshot 2024-03-19 at 5.29.31 PM.png>)






5. Next, we have to create a database for the mimic data in psql. And this is to be named **mimic** for simplicity purposes. 







6. Run the following code in the postgres console.
> NB: This would delete any previosuly made databases named "mimic"

```bibtex
DROP DATABASE IF EXISTS mimic;
```




7. After that, run:
```bibtex
CREATE DATABASE mimic OWNER postgres;
``` 
![alt text](<Screenshot 2024-03-19 at 5.35.42 PM.png>)





8. Now connect to the mimic database and created the schema:
```bibtex
\c mimic;
```
Next create the mimiciv schema.
```bibtex
CREATE SCHEMA mimiciv;
```





9. Now set the search_path:
```bibtex
set search_path to mimiciv;
```
![alt text](<Screenshot 2024-03-19 at 5.39.57 PM.png>)






## Section-3: Preparing Scripts
1. Click on "This PC" to go to the home directory and then click on the Local disk(C:) drive.
![alt text](<Screenshot 2024-03-19 at 5.45.51 PM.png>)

2. Then after clicking on it, create a folder called "postgres-scripts".
![alt text](<Screenshot 2024-03-19 at 5.46.56 PM.png>). 

3. Now return to the **mimic-documents** folder and follow the following steps: 
* Click on the **mimic-code** folder.
* Then click on the **mimic-iv** folder.
* Then click on the **buildmimic** folder.
* Then click on **postgres** folder, you should see a number of scripts that look like this
![alt text](<Screenshot 2024-03-19 at 5.48.53 PM.png>)


4. Select all the files, and paste them in the **postgres-scripts** folder  made in step 2.
![alt text](<Screenshot 2024-03-19 at 7.31.40 PM.png>)

5. Right click on the "create.sql" file and copy as path.
![alt text](<Screenshot 2024-03-19 at 5.59.39 PM.png>)

6. Return to the psql console and type in \i followed by the copied path.
```bibtex
\i 
```
Do not press enter yet, change the backslashes to forward-slashes and remove the quotation marks. Reference image.
![alt text](<Screenshot 2024-03-19 at 6.07.30 PM.png>)

7. Press enter and it should create a similar response.
![alt text](<Screenshot 2024-03-19 at 6.09.11 PM.png>)


8. Next, type in the following and press enter:
```bibtex
set ON_ERROR_STOP 1
```
![alt text](<Screenshot 2024-03-19 at 6.10.54 PM.png>)


## Section-4: Getting the file path of the mimic Files
Next, we need to get the path of the mimic-iv files to load into the database. This is also very similar for both the regular mimic and the demo.

1. Return to the **mimic-documents** folder and open the ***mimic-iv-clinical-database-demo-2.2***. Now copy the file path.
![alt text](<Screenshot 2024-03-19 at 6.22.57 PM.png>)




2. back in the psql terminal, type the following
```bibtex
\set mimic_data_dir **your file path**
```

* and next to it should be the copied file path with single quotation marks and the back slashes changed to forward slashes. Then click enter:
![alt text](<Screenshot 2024-03-19 at 7.51.01 PM.png>)


3. Next, we need to run the "load.sql" script. Repeat the steps used to run the "create.sql" or just run if the file paths are the same:
```bibtex
\i C:/postgres-scripts/load.sql
```
![alt text](<Screenshot 2024-03-19 at 7.51.38 PM.png>)
![alt text](<Screenshot 2024-03-19 at 7.51.38 PM-1.png>)

4. Next, if primary keys are needed for the databases, then run the following.
```bibtex
\i C:/postgres-scripts/constraint.sql
```
![alt text](<Screenshot 2024-03-19 at 7.52.46 PM.png>)
![alt text](<Screenshot 2024-03-19 at 7.53.23 PM.png>)

5. Next, run the script to create indexes for the database.
```bibtex
\i C:/postgres-scripts/index.sql
```


















21. Next run the vaildation script, if using the demo, you should do the same as in 18, but change "load.sql" to "validate_demo.sql" and if its the main MIMIC-IV data, chnage it to "validate.sql" instead.

22. Next check the pgadmin for the Data as needed.




