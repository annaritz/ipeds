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

Each dataset comes as an Access database along with an Excel file of metadata. The compressed Access database already includes the Excel file. Click on one of these Access databases and extract the folder. 

### Select Tables



### Convert Tables to CSV

