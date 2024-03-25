# IPEDS Data Access
Anna Ritz, 3/25/2024

[IPEDS](https://nces.ed.gov/) is a public data repository for higher education institutions, collecting data on enrollment, completion, and other institutional research data. I accessed this data in preparation for the Clare Boothe Luce initial application. A preliminary analysis is in a private repo, but I pulled out the relevant bits here.

_Note:_ there seem to be a number of better-supported GitHub repos to analyze IPEDS data; consider using one or more of these:
- [jbryer/ipeds](https://github.com/jbryer/ipeds): R R package for accessing the Integrated Postsecondary Education Data System (IPEDS) from R.
- [UrbanInstitute/ipeds-scraper](https://github.com/UrbanInstitute/ipeds-scraper): Download IPEDS complete data files
- [huyouare/freebase-ipeds](https://github.com/huyouare/freebase-ipeds): College Data Reconciliation Project
- [adamrossnelson/StataIPEDSAll](https://github.com/adamrossnelson/StataIPEDSAll): Scripts to download and build panel data files for IPEDS.

[IPEDS Data](https://nces.ed.gov/ipeds/use-the-data) can be accessed in a number of ways. There is a data explorer to explore tables, charts, and publications, and a way to look up a single institution. These are build through an IPEDS data session (which can be saved and returned to), but I found it hard to use.

## Download IPEDS Data & Transform tables to CSV-formatted files

Here, I explain how to Download all IPEDS data. On their [Use the Data](https://nces.ed.gov/ipeds/use-the-data) webpage, click on "Access Database", which contains datasets from 2004/05 to present.

![Screenshot 2024-03-25 at 11 19 54 AM](https://github.com/annaritz/ipeds/assets/1457970/cc154d5b-a22f-4cbe-8f32-95f70faeb60d)

_Screenshot of [Use the Data](https://nces.ed.gov/ipeds/use-the-data) webpage_

Each dataset comes as an Access database along with an Excel file of metadata. The compressed Access database already includes the Excel file. Click on one of these Access databases and extract the folder. Note that the most recent data is provisional and might contain slightly different values than the final datasets. For this example, we will use the 2021-22 Access dataset, which extracts a folder called `IPEDS_2021-22_Final/`.

![Screenshot 2024-03-25 at 4 42 38 PM](https://github.com/annaritz/ipeds/assets/1457970/87feebdc-b579-4dd5-a956-ebe37daa89b3)

_Screenshot of [IPEDS Access Databases](https://nces.ed.gov/ipeds/use-the-data/download-access-database)_

### Select Tables

The Excel spreadsheet folder (`IPEDS202122TablesDoc.xlsx`) contains information about all tables, variables, and metadata definitions for the database. The `Tables21` spreadsheet lists all tables and table descriptions. I used `C2021_A` in my analysis, which lists "Awards/degrees conferred by program (6-digit CIP code), award level, race/ethnicity, and gender: July 1, 2020 to June 30, 2021."

![Screenshot 2024-03-25 at 4 46 43 PM](https://github.com/annaritz/ipeds/assets/1457970/e1ecf470-ec85-4bce-bb07-c096df7559aa)

_Screenshot of `Tables21` sheet in `IPEDS202122TablesDoc.xlsx`_

### Convert Tables to CSV Files

I don't have Microsoft Access or a way to get a license. I use `mdbtools` to extract tables from the Microsoft Access database into a .csv file for further processing. Information about `mdbtools` can be found on their [GitHub repo](https://github.com/mdbtools/mdbtools). After installing `mdbtools`, you can extract any tables based from the command line based on the Table Name. For example, if the Microsoft Access database is named `IPEDS_2021-22_Final/IPEDS202122.accdb`:

```
mdb-export IPEDS_2021-22_Final/IPEDS202122.accdb C2021_A > C2021_A.csv
```

You will now have a CSV of `C2021_A`:

```
UNITID,CIPCODE,MAJORNUM,AWLEVEL,CTOTALT,CTOTALM,CTOTALW,CAIANT,CAIANM,CAIANW,CASIAT,CASIAM,CASIAW,CBKAAT,CBKAAM,CBKAAW,CHISPT,CHISPM,CHISPW,CNHPIT,CNHPIM,CNHPIW,CWHITT,CWHITM,CWHITW,C2MORT,C2MORM,C2MORW,CUNKNT,CUNKNM,CUNKNW,CNRALT,CNRALM,CNRALW
101295,"01",1,1,10,7,3,0,0,0,0,0,0,0,0,0,1,1,0,0,0,0,9,6,3,0,0,0,0,0,0,0,0,0
101514,"01",1,1,3,3,0,0,0,0,0,0,0,1,1,0,0,0,0,0,0,0,2,2,0,0,0,0,0,0,0,0,0,0
102553,"01",1,1,10,2,8,0,0,0,0,0,0,0,0,0,1,0,1,0,0,0,7,2,5,1,0,1,1,0,1,0,0,0
102614,"01",1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
104151,"01",1,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
104160,"01",1,1,4,0,4,0,0,0,0,0,0,0,0,0,3,0,3,0,0,0,1,0,1,0,0,0,0,0,0,0,0,0
104179,"01",1,1,3,1,2,0,0,0,0,0,0,1,1,0,2,0,2,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0
104346,"01",1,1,1,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,1,0,0,0,0,0,0,0,0,0
104425,"01",1,1,1,0,1,0,0,0,0,0,0,0,0,0,0,0,0,0,0,0,1,0,1,0,0,0,0,0,0,0,0,0
...
```

## Interpret the Tables 

The Excel spreadsheet (`IPEDS202122TablesDoc.xlsx`) lists all the variables for `C2021_A` in the `vartable21` sheet, and all possible values that those variables may take on in the `valuesets21` sheet. For the `C2021_A` table:

- All institutions are coded by a `UNITID`: Reed's ID is `209922`
- `AWLEVEL` is the _type_ of degree: a Bachelor's is coded as `5`
- `MAJORNUM` determines the first or second majors; Reed has very few double majors so it's safe to set this to `1`

![Screenshot 2024-03-25 at 4 45 11 PM](https://github.com/annaritz/ipeds/assets/1457970/400f2365-ba2b-4119-b0ba-18e64a1ff7a8)

_Screenshot of `vartable21` sheet in `IPEDS202122TablesDoc.xlsx`_

### CIP Codes

The [Classification of Instructional Programs (CIP Codes)](https://nces.ed.gov/ipeds/cipcode/default.aspx?y=55) are used to define major names. There are two sets of CIP Codes, one from 2010 and one from 2020. You can download these lists from the [CIP Code Resources website](https://nces.ed.gov/ipeds/cipcode/resources.aspx?y=55). I've added plain-text lists of the CIP codes in `cipcodes/` directory. 

I shortened the MNS department major/program names to the following, after subsetting by Reed data and looking at the reported non-zero values:

CIP Code | Full Name | Shortened Name 
--- | --- | ---
26.0101 |Biology/Biological Sciences, General | Bio
40.0501 | Chemistry, General | Chem
40.0801 | Physics, General| Physics
11.0101 | Computer and Information Sciences, General| CS
26.0202 | Biochemistry| BMB
27.0101 | 27.0101	Mathematics, General | Math
30.0801 | Mathematics and Computer Science | MathCS
99 | Grand total | AllMajors

Note that some majors are missing (e.g., Physics/Chem), as well as ad-hoc intersciplinary majors whose two fields are within MNS.
