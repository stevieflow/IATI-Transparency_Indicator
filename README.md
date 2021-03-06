IATI Transparency Tests
=======================
Licence
-------
This file is part of IATI Transparency Tests.

Copyright 2012 caprenter <caprenter@gmail.com>

IATI Transparency Tests is free software: you can redistribute it and/or modify
it under the terms of the GNU General Public License as published by
the Free Software Foundation, either version 3 of the License, or
(at your option) any later version.

IATI Transparency Tests is distributed in the hope that it will be useful,
but WITHOUT ANY WARRANTY; without even the implied warranty of
MERCHANTABILITY or FITNESS FOR A PARTICULAR PURPOSE.  See the
GNU General Public License for more details.

You should have received a copy of the GNU General Public License
along with IATI Transparency Tests.  If not, see <http://www.gnu.org/licenses/>.


`functions/pretty_jsopn.php` is taken from a comment on php.net so no claims about copyright are made.

Getting Started
===============


Checkout the scripts
--------------------

Get some data:  
You can fetch data from the IATI Registry (iatiregistry.org).   
Store each publishers data in it's own directory, which is itself inside a 'data' directory.  
e.g.  
`raw_IATI_xml_data/`  
`raw_IATI_xml_data/dfid`    
`raw_IATI_xml_data/publisher_name`          
and so on.

Settings
--------
Copy `example.settings.php` to `settings.php` and then edit the options.  
Create your `results` directory to store your results in.  
You will also need to create:  
* `csv` directory - to save your results in.
* `history` directory - to save partial results in.
* `helpers/CKAN_meta/tmp` directory - to store some temporary data
* `helpers/CKAN_meta/CKAN_data` directory - to store location information about files on the registry

Check your 'helper' files are up to date
----------------------------------------
The files under /helpers are additional external pre-calculated data files required to do things like:  
* map publisher id's to real names  
* map languages to countries  
* define the tests to be used  

You should check that they are up to date before you use them.

Run the scripts
---------------

Using `php_cli`  e.g.
`php transparency_tests.php`

Functional Notes
----------------
Run the tests in the following order:  
* `transparency_tests.php` - will generate a json file of data giving counts, scores and percentages, etc for a data publisher against their IATI data files.  
* `format_overview_results_Sig_Other_All.php` - generates some total information for all providers grouped in Signatories/Other/All. This data is required by:  
* `format_all_results_v2.php` - generates 2 csv files /csv/Signatories.csv and /csv/Other.csv  
* `transparency_tests_additional.sh` - generates the results for the 'top 4' tests that are a bit more complicated  

The final test is on the file updates based on the CKAN registry records.  
First you need to create some meta data about the records on the CKAN registry.  
`cd helpers/CKAN_meta`  
make sure you have two directories there:  
`tmp` - should be at `helpers/CKAN_meta/tmp`  
`CKAN_data` - should be at `helpers/CKAN_meta/CKAN_data`  

Then, in the `CKAN_meta` directory, run  
`ckan_records_metadata.php` - this grabs CKAN data about each file on the Registry and stores it for us. We need this data so we can then fetch the file change history on each one.  

Next:  
Back in the top level directory, make sure you have created a `./history` directory  
* `history_maker.php` grabs the data - this can take quite a while as it needs to pull every file from the CKAN webservice.  
 `history_assessment.php` makes the judgement on how often data is updated and saves it in a file called history.csv  
run:  
* `history_assesment.php > csv/Timeliness_Files_1.2.csv` - !NB the name of this output file is important!  


Finally run:  
* `Final_Results_Sheet.php` puts everything above into 2 csv files, one for Signatories, one for Others.  

/contrib
--------
This directory contains a number of one off scripts that can be run by `php_cli` and should be output to csv.  
Move the script into the root directory first.

Out dated formats:
------------------
* `format_results.php` - will generate a csv file of results only, grouped into data publisher types (e.g. signatory, NGO's etc)

 
