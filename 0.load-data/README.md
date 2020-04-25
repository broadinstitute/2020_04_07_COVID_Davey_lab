# Loading data


Install `cytominer-database`

```
pip install cytominer-database
```


Got to the parent directory of the folder `Output` shown below. The output contains the CellProfiler output:

```
Output
├── MyExpt_Experiment.csv
└── Simple_set
    ├── MyExpt_Image.csv
    ├── MyExpt_Infected.csv
    └── MyExpt_Nuclei.csv
```


On my computer, `Output` is in `~/Downloads`

```
cd ~/Downloads
```


Create a file named `config.ini` in `Downloads` with the content as below

```
[filenames]
image = MyExpt_Image.csv
experiment = MyExpt_Experiment.csv
```

Now run `cytominer-database` to ingest all the CellProfiler output into a SQLite backend

```
cytominer-database ingest -c config.ini Output sqlite:///backend.sqlite
```

Here are the tables now available in `backend.sqlite`:

```
sqlite> .tables
Myexpt_image     Myexpt_infected  Myexpt_nuclei
```

Here are the schemas `backend.sqlite`:

```
sqlite> .schema Myexpt_nuclei
CREATE TABLE IF NOT EXISTS "Myexpt_nuclei" (
	"TableNumber" BIGINT,
	"ImageNumber" BIGINT,
	"ObjectNumber" BIGINT,
	"Myexpt_nuclei_Location_Center_X" FLOAT,
	"Myexpt_nuclei_Location_Center_Y" FLOAT,
	"Myexpt_nuclei_Location_Center_Z" BIGINT,
	"Myexpt_nuclei_Number_Object_Number" BIGINT
);
CREATE INDEX "ix_Myexpt_nuclei_TableNumber" ON "Myexpt_nuclei" ("TableNumber");

sqlite> .schema Myexpt_Infected
CREATE TABLE IF NOT EXISTS "Myexpt_infected" (
	"TableNumber" BIGINT,
	"ImageNumber" BIGINT,
	"ObjectNumber" BIGINT,
	"Myexpt_infected_Location_Center_X" FLOAT,
	"Myexpt_infected_Location_Center_Y" FLOAT,
	"Myexpt_infected_Location_Center_Z" BIGINT,
	"Myexpt_infected_Number_Object_Number" BIGINT
);
CREATE INDEX "ix_Myexpt_infected_TableNumber" ON "Myexpt_infected" ("TableNumber");

sqlite> .schema Myexpt_Image
CREATE TABLE IF NOT EXISTS "Myexpt_image" (
	"TableNumber" BIGINT,
	"Channel_Blue" BIGINT,
	"Channel_Green" BIGINT,
	"Count_Infected" FLOAT,
	"Count_Nuclei" FLOAT,
	"ExecutionTime_01Images" BIGINT,
...
```

An example of working with this data is shown in `0.inspect-backend.md`