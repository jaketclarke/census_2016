# 2016 Census DataPacks to database

This project contains a few bits and pieces to get 2016 Census DataPacks into a database.

The first set of functions are database agnostic, and make .sql files to define the table structure you want from a csv file.

This makes use of csvkit.

The second is python code to execute mysql .sql files

The third uses pandas to write to mysql - this is also database agnostic, and could be modified to write to a postgres or other database

### To Do

* Currently uses pymysql to execute the file and mysqldb to do the insert, this is silly, should normalise at some stage

* Make the build-the-tables bit also be database agnostic, probably by using the connection string definition option of csvkit

* csvsql outputs decimal columns in a create statement in the form: `SA4_CODE_2016 DECIMAL NOT NULL`

	* Mysql interprets `DECIMAL` as `DECIMAL(10,0)`, thus this breaks the Average column on table 02 of the Census.

	* This probably requires a patch to csvsql which I might do at some stage

	* For the moment, this is why `monkey_patch_averages()` is there

* the 'append' in Pandas to_sql method, combined with no primary key on the table, means if you run the insert several times, it will insert
	* in future will add a 'truncate table' step

### Requires

Jupyter and Python3

Needs the libraries:

```
csvkit
pandas
pymysql
mysql-python
```

These should all be installable by pip, i.e: `pip install csvkit` or `pip3 install csvkit`

On Windows, mysql-python needs the Visual C++ build tools, 2015 or greater, to compile

## Authors

* **Jake Clarke** - [jaketclarke](https://github.com/jaketclarke) - jake@theredfox.group

## Acknowledgments

* Thanks to Zach A for so many things, but specifically for reminding me the Census was out