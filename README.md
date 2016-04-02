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
sqlite> .once /path/to/uhsdictionary-data/data/pali_dict.sqlite.csv
sqlite> select * from PALI_MYANMAR_DICTIONARY;
sqlite> ^D
```
