# load-mimic-iv-into-postgresql

## Requirements
This tutorial assumes user has the following. Links are included to get them set up before moving to the next sections:

1. Windows 10/11 device.
2. Git installed (Tutorial here: [Click to learn how to install Git on windows](https://phoenixnap.com/kb/how-to-install-git-windows)).
3. Postgresql installed (Tutorial here: [Click to learn how to install postgresql](https://www.postgresqltutorial.com/postgresql-getting-started/install-postgresql/))
4. Access to [MIMIC-IV data](https://physionet.org/content/mimiciv/2.2/) or [MIMIC-IV demo-data](https://physionet.org/content/mimic-iv-demo/2.2/)

>**NB: Access to MIMIC-IV Data requires becoming a credentialed user and completing a training. More information can be found online.**


## Instructions
1. Create a folder called **mimic-documents** in the documents folder. 
>NB: This can be done in any folder but documents folder is used for simplicity.

2. Right-click on the folder and click the option to open in terminal. Then type in the following and press enter: 

..... insert image here 

```bibtex
git clone https://github.com/MIT-LCP/mimic-code.git
```
Now the contents of the **mimic-documents** folder should look like this:

....insert image here  
3. Download and unzip all the files in either the MIMIC_IV data or the MIMIC_IV demo data (links in the requirements section). The main mimic data also has zipped filled in the icu and hospital module. Unzip those also

>**NB: The demo data would be used here as a sample but the process is essentially the same for both versions**

4. Move the mimic data you've decided to use into the **mimic-documents** folder and the contents of the folder should now look like:
.. insert image here

5. on the windows page, search and click on the **psql**. To simplify things, most things would be left at their default values. The option "Server [localhost]" should show up first, click enter without typing anything. Do the same for "Database [postgres]",  "port[5432]" and "Username [postgres]". Next enter your password and click enter. 


6. Next, a "postgres=#" text should appear in the postgres console. That means we're set.
.....insert image here 


Now we need to create a database for the mimic data in psql. Tha nem if the database we wan to create is **mimic** and this is for simplicity purposes. If you already have a databse called mimic, skip this step and create another database using your chosen name.

7. Run the following code in the postgres console.
```bibtex
DROP DATABASE IF EXISTS mimic;
```
After that, run :
```bibtex
CREATE DATABASE mimic OWNER postgres;
```
you should have something like this .... 
..insert image 


8. Now connect to the mimic database created using:
```bibtex
\c mimic;
```
Next type in:
```bibtex
CREATE SCHEMA mimiciv;
```

9. Now type in:
```bibtex
set search_path to mimiciv;
```

10. Back in the **mimic-documents**, create a third folder named **scripts**.

11. While in the **mimic-documents** folder, follow the following steps:
* Click on the **mimic-code** folder.
* Then click on the **mimic-iv** folder.
* Then click on the **buildmimic** folder.
* Then click on **postgres** folder, you should see a number of scripts that look like this.

...insert image;

12. Copy all the files, and paste them in the **scripts** in **mimic-documents** that was created in step 10.

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




