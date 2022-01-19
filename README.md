# check sha256 disposition in CTR from a sha256 list

The python script named **1-ctr_get_sha_dispositions_from_a_file.py**  reads the text file named **SH256.txt** located in the same folder, and check the disposition of every SHA256 that it contains.

This text file must contains several lines one after the other and must contain one or more observable per line.  The line can contain strings before and after the observables to check.

example :

```
any string before the observable ....:"50.60.57.20"... and any string after the sha. The line must contain one observable to check
any string before the observable ....:"internetbadguy.com"... and any string after the sha. The line must contain one observable to check
any string before the observable ....:"5.188.206.150"... and any string after the sha. The line must contain one observable to check
any string before the observable ....:"3372c1edab46837f1e973164fa2d726c5c5e17bcb888828ccd7c4dfcc234a370"... and any string after the sha. The line must contain one observable to check
any string before the observable ....:"https://catatonic-judge.000webhostapp.com/"... and any string after the sha. The line must contain one observable to check
```

Every observables will be extracted from the line and a disposition query will be send to SecureX Threat Response in order to identify if any Integrated Modules knows them. And if Yes what are their dispositions ( Malicious, Suspicious or something else )

A resulting file named **result.txt** is created with the results of the queries :

For example :

```
https://catatonic-judge.000webhostapp.com/;Talos Intelligence     ;2    ;Malicious  
```

If the observable doesn't any verdict from any module then the result will contain : ;No Verdict;Good News; at the end of the line.

Example :

```
any string before the observable ....:"https://catatonic-judge.000webhostapp.com/"... and any string after the sha. The line must contain one observable to check;No Verdict;Good News
```

All that means is that you can copy and paste into the **SHA256.txt** file, the whole content of any log file, from any security sensors as far the log are a readable text , and then you will be able to identify all malicious observable that it contains. The log file can be large. The script had been tested with logs that contains 10 000 observables.
It takes 2h30 to check 10 000 observables.

Don't hesitate to modify the code in order to directly read to log file you want to analyse. 

At the same time the script create a file named **sha256_observables.txt** that will contain only sha256s.

At this point **result.txt** output all known malicious observables.

And then you can move forward on investigation by running the **2-threatgrid_get_sha_submission_from_a_file.py** file. This script will read the **sha256_observables.txt** created in the previous step and will query ThreatGrid for every sha256 in the file in order to check if there already are submissions for these sha256. If yes the result of the query is saved into the **resultat_TG.txt** file.

And lastly you can move forward on investigation again by running the **3-amp_get_event_for_sha_from_a_flie.py**. This script check in your Cisco Secure Endpoint environment ( AMP for Endpoint ) if there are infected hosts by malicious sha256 contained into the **sha256_observables.txt** file. If yes the script output hostnames of infected machines.

## Installation

Installing these script is pretty straight forward . You can just copy / and paste them into you python environment but a good practice is to run them into a python virtual environment.

### Install a Python virtual environment

	For Linux/Mac 

	python3 -m venv venv
	source bin activate

	For Windows 

	virtualenv env 
	\env\Scripts\activate.bat 
	
	or
	
	python -m venv venv 
	venv\Scripts\activate

### clone the scripts

	git clone https://github.com/pcardotatgit/check_sha256_disposition_in_CTR_from_a_sha256_list.git
	cd check_sha256_disposition_in_CTR_from_a_sha256_list/

### Install required modules

    pip install -r requirements.txt

## Running the scripts


**1 - First** of all you have to open the **environment_api_keys.py** and put into it the value of API credentials at least for CTR_, and AMP_ plus THREATGRID if you use these security backends.

**2- Second** run the scripts one after the other

    1- 1-ctr_get_sha_dispositions_from_a_file.py
    2- 2-threatgrid_get_sha_submission_from_a_file.py
    3- 3-amp_get_event_for_sha_from_a_file.py

 

