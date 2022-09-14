# Oracle lookup filter plugin for Embulk

A filter plugin for Embulk to lookup MySql table data

## Configuration

- **mysql_lookup**: Required attribute for this filter plugin
    - **host**: database host (example `localhost`) (required)
    - **port**: database port (example port for oracle `1521`) (required)
    - **database**: database name (required)
    - **tablename**: table name of your database (required)
    - **username**: username for your database (required)
    - **password**: password for database (required)
    - **mapping_from**: (Name of column to be matched with input file 1) (required)
        - **Name of column-1**: column name-1 from input file
        - **Name of column-2**: column name-2 from input file etc ...
    - **mapping_to**:   (Name of column to be matched with input file 2) (required)
        - **Name of column-1**: column name-1 from input file
        - **Name of column-2**: column name-2 from input file
    - **New Columns**:   (Newly generated column name) (required)
        - **Name-1,Type-1**: Any Name, Type of the name (name: country_name, type: string)
        - **Name-2,Type-2**: Any Name, Type of the name (name: country_address, type: string) etc ...
## Example - columns

Say input.csv is as follows:

```
dim_calendar_key,year_number,quarter_number,attr_1
-1,0,0
19900101,1990,1,FirstIndia
19900102,1990,2,SecondUSA
19900103,1990,,1000.56
19900104,1990,4,40_China
19900105,1990,5,50_Ukraine
19900106,1990,2,10_USA
19900107,1990,3,30_UK
19900109,1990,6,
```

```yaml
 - type: oracle_lookup
     lookup_destination: mysql
     host: localhost
     port: 1521
     database: XE
     tablename: country
     username: sys as sysdba
     password: root
     mapping_from:
       - quarter_number
       - attr_1
     mapping_to:
       - id
       - country_address
     new_columns:
       - { name: country_name, type: string }
       - { name: country_address, type: string }
```

Notes:
1. mapping_from attribute should be in same order as mentioned in input file.

## Development

Run example:

```
$ ./gradlew package
$ embulk run -I ./lib seed.yml
```

Deployment Steps:

```
Install ruby in your machine
$ gem install gemcutter (For windows OS)

$ ./gradlew gemPush
$ gem build NameOfYourPlugins (example: embulk-filter-postgress_lookup)
$ gem push embulk-filter-postgress_lookup-0.1.0.gem (You will get this name after running above command)
```


Release gem:

```
$ ./gradlew gemPush
```