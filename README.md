## UHS(U Hoke Sein) Dictionary Data

Convert ဦးဟုတ်စိန် အဘိဓာန် data from Zawgyi to Unicode encoding.

* https://my.wikipedia.org/wiki/ဟုတ်​စိန်၊ ဦး(အဘိဓာန်​ဆရာ​ကြီး)

## Notes

Extracted UHS_DB.sqlite and pali_dict.sqlite from:
* https://play.google.com/store/apps/details?id=uhs.tayar.myanmar.pali.dictionary.reference
* https://play.google.com/store/apps/details?id=tayar.myanmar.pali.dictionary.reference

zg12uni51.js from:
* https://github.com/ngwestar/parabaik/blob/master/scripts/zg12uni51.js

### Export pali_dict.sqlite db to csv file

```
$ sqlite3 pali_dict.sqlite
SQLite version 3.8.10.2 2015-05-20 18:17:19
Enter ".help" for usage hints.
sqlite> .header on
sqlite> .mode csv
sqlite> .once /path/to/data/pali_dict.sqlite.csv
sqlite> select * from PALI_MYANMAR_DICTIONARY;
sqlite> ^D
```

See: https://www.sqlite.org/cli.html

#### Convert to unicode using `z2u` script

```
$ ./z2u
Convert Zawgyi to Unicode.
Usage: ../../../.nvm/versions/node/v5.5.0/bin/node ./z2u

Options:
  -f, --file  Zawgyi encoded file  [required]

Missing required arguments: f
```

```
$ ./z2u -f ./data/pali_dict.sqlite.csv > ./data/pali_dict.sqlite.unicode.csv
```

#### Create `pali_dict.unicode.sqlite` database file

Get `pali_dict.sqlite` schema:
```
$ sqlite3 pali_dict.sqlite
SQLite version 3.8.10.2 2015-05-20 18:17:19
Enter ".help" for usage hints.
sqlite> .schema
CREATE TABLE "PALI_MYANMAR_DICTIONARY" ("pali" VARCHAR, "myanmar" VARCHAR);
CREATE INDEX "pali_col_indez" ON "PALI_MYANMAR_DICTIONARY" ("pali" ASC, "myanmar" ASC);
CREATE TABLE android_metadata (locale TEXT);
sqlite> select count(*) from pali_myanmar_dictionary;
115356
sqlite> ^D
```

Create `pali_dict.unicode.sqlite` and import unicode csv file:
```
$ sqlite3 pali_dict.unicode.sqlite
SQLite version 3.8.10.2 2015-05-20 18:17:19
Enter ".help" for usage hints.
sqlite> CREATE TABLE "PALI_MYANMAR_DICTIONARY" ("pali" VARCHAR, "myanmar" VARCHAR);
sqlite> CREATE INDEX "pali_col_indez" ON "PALI_MYANMAR_DICTIONARY" ("pali" ASC, "myanmar" ASC);
sqlite> .mode csv
sqlite> .import /path/to/data/pali_dict.sqlite.unicode.csv PALI_MYANMAR_DICTIONARY
sqlite> select count(*) from pali_myanmar_dictionary;
115357
```

### Convert cvs file to json

```
$ cd data/

# `tail -n +2` removes first line
$ cat pali_dict.sqlite.unicode.csv | tail -n +2 | sed 's/","/": "/g; s/"$/",/g' > pali_dict.sqlite.unicode.json

# Remove last comma
$ sed -i '$ s/",$/"/' pali_dict.sqlite.unicode.json

# Write '{' into fist line of file
$ sed -i '1 i\{' pali_dict.sqlite.unicode.json

# Write '}' into end of file
$ echo '}' >> pali_dict.sqlite.unicode.json
```

Use `cvs2json` script instead of method above as that doesn't produce valid json:
```
$ ./cvs2json -f ./data/pali_dict.sqlite.unicode.csv > ./data/pali_dict.sqlite.unicode.json
```
