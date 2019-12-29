********************************************************************
* GPRL Stat Assesmment - Adam Kurczewski: 12/28/19
********************************************************************

*****************
** Data Import **
*****************	
clear

cd "C:\Users\kurczew2\Desktop\GPRL\Data\Part 1"

/*
Still familiarizing myself with the application of Github
but if I was more comfrotable the current directory would be set
to a github repository with the necessary data sets in it.
Ideally all pathways would be non-specific, but for now it is
necessary to alter unique pathways/directory to match individual machines
*/

*New Variables import
import delim "New Variables.csv"

* use "C:\Users\kurczew2\Desktop\GPRL\Data\Part 1\Main Dataset.dta"

*join to retain all observations, including those with no data for new variables
joinby uniqueid using "Main Dataset.dta", unmatched(both)

* inspect new obs dataset for any potential merging issues
* use "C:\Users\kurczew2\Desktop\GPRL\Data\Part 1\New Observations.dta"

append using "New Observations.dta"

/*
Assumptions made to decide which variables to merge the datasets by, as the new variables have no HHID.
investigating the Main Dataset, I assume that the new variables dataset contains "baseline" information for 
many of the obs in the main dataset.  we can partially verify this by comparing the age and educations of
observations in both, using 'uniqueid' as the key.  age and educ make good comparison variables as they are
expected to only change by 1 each year (here we assume that time between baseline and endline is 1, however we can 
calculate time between baseline and endline by checking if the age and educ of ALL uniqueids in both datasets change 
by the same amount).  It appears that using uniqueid is a good key to merge on, as both education and age are consistent (or change
by 1) for each uniqueid.
*/

*******************
** Quality Check **
*******************

* summary statsics of survey length for completed surveys
* entire sample and by enumerator

sum surveytime if survey_complete == 1, d
 
 /*
 Median: 7165 seconds
 Mean: 7202 seconds
 Note: 
 101 surveys in sample 'not complete'
 */

/*
By preseting survey times for all surveys, we cannot get an idea of which enunerators are
administering the survey the fastest/slowest.  This information might be useful if we suspect
data quality is affected by time spent on the survey or if hitting sample size targets is becoming a concern.
*/

 bysort surveyor: summarize surveytime if survey_complete == 1
 
* make data browsing easier to check duplicate types by sorting by hhid
sort hhid

* list duplicates
duplicates l hhid

/*
3 hhids are duplicated (6 total)
Becuase so few duplicates exist it is possible to manually check
the observations to see if they are copies of the same observations or if they are incorrectly entered HHIDs.
Copies can be identified if infortmation recorded for each obs is identical or similar across enough key variables to 
warrent a determination by the analyst as the same observation entered twice (or on two separate occassions unintentionally).
To do so we can use identifiers such as age at base, age at end, education, occupation, address, or other identifiers
that can not be reasonably expected to change drastically within administrtion of the survey.  

Using this method, I was able to identify two of the 3 obs as copies.  The remaining duplicate is two seperate obs with a mismatched hhid.
usng age is not feasible here because of data entry differences (birthyear vs age).  If I new the survey year I could recode
but to avoid making any additional assumptions I will use education instead.  Assinging a unique HHID to one of these duplicates is
also an option but becomes less feasible with many duplicated obs.  For the sake of brevity and because it is one obs, it will be dropped.
*/

sort hhid educ sex
quietly by hhid educ sex : gen dup = cond(_N==1, 0, _n)
drop if dup>1


**************
** Cleaning **
**************

* most effiient way but retains names in labels ie fails privacy objective
* circle back if time allows
**** encode surveyor, g(surveyorid) ****

gen surveyorid = 0
replace surveyorid = 1 if surveyor == "Anna"
replace surveyorid = 2 if surveyor == "Benjamin"
replace surveyorid = 3 if surveyor == "Caroline"
replace surveyorid = 4 if surveyor == "David"
replace surveyorid = 5 if surveyor == "Grace"
replace surveyorid = 6 if surveyor == "Jane"
replace surveyorid = 7 if surveyor == "John"
replace surveyorid = 8 if surveyor == "Joseph"
replace surveyorid = 9 if surveyor == "Mary"
replace surveyorid = 10 if surveyor == "Peter"
replace surveyorid = 11 if surveyor == "Sam"


* replacing specific missing values

/*
label list yesno

* use existing label values to recode variables appropriately
replace burglaryyn = 99 if burglaryyn == -999
replace burglaryyn = 97 if burglaryyn == -997

replace vandalismyn = 99 if vandalismyn == -999
replace vandalismyn= 97 if vandalismyn == -997

replace trespassingyn = 99 if trespassingyn == -999
replace trespassingyn= 97 if trespassingyn== -997
*/

*replace with designated extending missing values
replace burglaryyn = .d if burglaryyn == -999
replace burglaryyn = .r if burglaryyn == -997

replace vandalismyn = .d if vandalismyn == -999
replace vandalismyn= .r if vandalismyn == -997

replace trespassingyn = .d if trespassingyn == -999
replace trespassingyn= .r if trespassingyn== -997

* Export
save stata.assessment1-output-AdamKurczewski.dta, replace


