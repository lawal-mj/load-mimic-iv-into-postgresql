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
>NB: This can be done in any folder but documents folder is used for simplicity.

2. Right-click on the folder and click the option to open in terminal. Then type in the following and press enter: 

```bibtex
git clone https://github.com/MIT-LCP/mimic-code.git
```
Now the contents of the **mimic-documents** folder should look like this:
![image of new folder created](<Screenshot 2024-03-19 at 4.29.01 PM.png>)

## Prepare MIMIC-IV or MIMIC-IV demo

>**NB: The demo data would be used here as a sample but the process is essentially the same for both versions**

1. Download the mimic-demo file:
![image of downloaded demo file](<Screenshot 2024-03-19 at 4.33.16 PM.png>)
***Downloaded demo file***

2. Copy the folder **mimic-iv-clinical-database-demo-2.2** and move it into the **mimic-documents** folder you created before.


2. Unzip the file contents:
![demo file after being unizipped](<Screenshot 2024-03-19 at 4.33.16 PM-1.png>)
***demo file after being unzipped***


3. Open the **mimic-iv-clinical-database-demo-2.2** and it should lead to a page containing a "hosp" and an "icu" folder. If it leads to another file with the same name, click on that and now you should see the "hosp" and "icu" like this.
![alt text](<Screenshot 2024-03-19 at 4.42.12 PM.png>)

4. Click on the hosp folder.
![alt text](<Screenshot 2024-03-19 at 5.07.01 PM.png>)the hosp folder ad all its files

5. Select all files (Ctrl + a) and click on WinRAR, then click on "Extract here". This may take a while.
![alt text](<Screenshot 2024-03-19 at 5.13.20 PM.png>)
![alt text](<Screenshot 2024-03-19 at 5.13.31 PM.png>)

6. Do the same thing for the icu folder next to the hosp one. 


## Getting Ready for Postgres






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





10. Back in the **mimic-documents**, create a third folder named **scripts**.
![alt text](<Screenshot 2024-03-19 at 5.41.05 PM.png>)



## Preparing Scripts
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


12. Select all the files, and paste them in the **postgres-scripts** made in step in **mimic-documents** that was created in step 10.

13. Now right click on the "create.sql" file and copy its path: 
Paste it in a notepad and it should look similar to this

> "C:\Users\your-pc-name\Documents\mimic-documents\create.sql"

Change all the backslashes to forward-slashed and remove the quotation marks. You should now have something that looks like this.

> C:/Users/your-pc-name/Documents/mimic-documents/create.sql

Copy this new path and go back to the psql(postgres) terminal.

14. In the psql terminal, type in "\i" and paste the new file path to look like this.
> **mimic=#** \i C:/Users/your-pc-name/Documents/mimic-documents/create.sql


15. Next, type in:
```bibtex
set ON_ERROR_STOP 1
```


16. Next, we need to get the path of the mimic-iv files to load into the database. This is a bit different for both the regular mimic and the demo.

### MIMIC-IV 
* click on ***mimic-iv-clinical-database-demo-2.2*** folder, there would be a second folder with the same name, click on that also till it shows a page like this. 
* Copy the path of this folder and it should look something like this
> C:\Users\mujeeb\Documents\mimic-documents\mimic-iv-clinical-database-demo-2.2\mimic-iv-clinical-database-demo-2.2


remove any quotation marks if present and change the back-slshes to forward slashes once again to make it look similar to this

>C:/Users/mujeeb/Documents/mimic-documents/mimic-iv-clinical-database-demo-2.2/mimic-iv-clinical-database-demo-2.2

FInally, add single quotation marks to make it like this:
>'C:/Users/mujeeb/Documents/mimic-documents/mimic-iv-clinical-database-demo-2.2/mimic-iv-clinical-database-demo-2.2'


17. return to the psql terminal and run the following(paste last file path as ):
> set mimic_data-dir last_file_path_from_step_16


18. Next, similar to step 14, do the same thing except this time its for the "load.sql". It should look like this

> **mimic=#**  \i C:/Users/your-pc-name/Documents/mimic-documents/load.sql

19. Here, if primark keys are needed for the database, run the same code. and change just to the "load.sql" to **constraint.sql**. SKip this step if you dont need primary keys for the database.

> **mimic=#**  \i C:/Users/your-pc-name/Documents/mimic-documents/constraint.sql


20. do the same thing as in 18, just change the load.sql to index.sql. Ths creates indexes for the tables in the database.


21. Next run the vaildation script, if using the demo, you should do the same as in 18, but change "load.sql" to "validate_demo.sql" and if its the main MIMIC-IV data, chnage it to "validate.sql" instead.

22. Next check the pgadmin for the Data as needed.




