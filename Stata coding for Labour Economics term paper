* This coding was done for a term paper for the Labour Economics course during MA in Economics degree

set more off
cd "C:\Users\HP\Desktop\2024\3. March\1. York\1. Labour\2. Data project - 40 Marks\Raw Data\2. Raw data"
use "cps_00007.dta", clear

**************************************************************************************************************************************************************************************
********************************************************************* Data cleaning and Merging **************************************************************************************
**************************************************************************************************************************************************************************************

* Data cleaning - Impose sample restrictions and adjust missing values
drop if age < 15 | age > 64
drop if empstat == 1
replace incwage=. if incwage== 99999999

* Merge in the CPI data that we downloaded from BLS
merge m:1 year month using cpi_2019

**************************************************************************************************************************************************************************************
********************************************************************* Generate variables and label values ****************************************************************************
**************************************************************************************************************************************************************************************

* Compute real weekly earnings
generate real_incwage=incwage/cpi_2019
* Generate log real earnings
generate log_real_earnings=ln(real_incwage)

* Generate an indicator variable for women
generate female=1 if sex==2
replace female=0 if sex==1
* label simplified gender variable with 2 categories:
label define gender_labels 1 "Female" 0 "Male", replace
label values female gender_labels
tab female [aw=asecwt]

* Simplified race variable with 3 categories:
generate race_simple=1 if race==100
replace race_simple=2 if race==200
replace race_simple=3 if race==651
replace race_simple=4 if race==300 | race==650 | race > 651
* label simplified race variable with 4 categories:
label define race_labels 1 "White" 2 "Black" 3 "Asian" 4 "Other races", replace
label values race_simple race_labels
tab race_simple [aw=asecwt]

* simplified health variable with 2 categories
generate health_simple=1 if health==1 | health==2 | health==3
replace health_simple=0 if health==4 | health==5
* label simplified race variable with 2 categories:
label define health_labels 0 "Suboptimal Health" 1 "Good Health", replace
label values health_simple health_labels
tab health_simple [aw=asecwt]

* Simplified education variable with 8 categories:
generate educ_simple=1 if educ<=22
replace educ_simple=2 if educ>=30 & educ<=32
replace educ_simple=3 if educ>= 40 & educ<=81
replace educ_simple=4 if educ>= 90 & educ<=111
replace educ_simple=5 if educ>=121 & educ<=125

* label simplified education variable with 5 categories:
label define educ_labels 1 "none or elementary" 2 "middle school" 3 "High school or High school diploma" 4 "Post-secondary" 5 "Post-graduate", replace
label values educ_simple educ_labels
* check new education variable
tab educ_simple [aw=asecwt]

* simplified industry varaible with 13 catergories 
generate ind_simple=0 if ind1990== 0
replace ind_simple=1 if ind1990>= 010 & ind1990<=032
replace ind_simple=2 if ind1990>= 040 & ind1990<=050
replace ind_simple=3 if ind1990==060
replace ind_simple=4 if ind1990>= 100 & ind1990<=392
replace ind_simple=5 if ind1990>= 400 & ind1990<=472
replace ind_simple=6 if ind1990>= 500 & ind1990<=571
replace ind_simple=7 if ind1990>= 580 & ind1990<=691
replace ind_simple=8 if ind1990>=700 & ind1990<=712
replace ind_simple=9 if ind1990>= 721 & ind1990<=760
replace ind_simple=10 if ind1990>=761 & ind1990<=791
replace ind_simple=11 if ind1990>= 800 & ind1990<=810
replace ind_simple=12 if ind1990>=812 & ind1990<=891
replace ind_simple=13 if ind1990>=892 & ind1990<=893
replace ind_simple=14 if ind1990>= 900 & ind1990<=932
replace ind_simple=15 if ind1990== 952
* label simplified industry variable with 13 categories:
label define ind_labels 0 " No industry" 1 "Agriculture" 2 "Mining" 3 "Construction" 4 "Manufacturing" 5 "Communication" 6 "Wholesale Trade" 7 "Retail Trade" 8 "Finance" 9 "Business and Repair" 10 "Personal Services" 11 "Recreation" 12 "Professionals" 13 "Management, public realtions, miscellaneous professional" 14 "Public Admin" 15 "armed force", replace
label values ind_simple ind_labels
tab ind_simple [aw=asecwt]

* simplified year variable with 3 categories (decades)
generate year_simple=1 if year>=2000 & year<=2009
replace year_simple=2 if year>=2010 & year<=2019
replace year_simple=3 if year>=2020 & year<=2023
* label simplified year variable with 2 categories:
label define year_labels 1 "2000s" 2 "2010s" 3 "2020-2023(Post-covid)", replace
label values year_simple year_labels

*** Employment, Unemployment, and Labour Force Participation Rates

generate lf_status = 1 if empstat==10 | empstat==12
replace lf_status = 2 if empstat>=20 & empstat<=22
replace lf_status = 3 if empstat>=30 & empstat<=36
* label
label define lf_labels 1 "Employed" 2 "Unemployed" 3 "Not in the Labor Force", replace
label values lf_status lf_labels
tab lf_status [aw=asecwt]

generate employed=1 if lf_status==1
replace employed=0 if lf_status==2 | lf_status==3
* label
label define employed_labels 1 "Employed" 0 "Unemployed or Not in the Labor Force", replace
label values employed employed_labels
tab employed [aw=asecwt]

generate labour_force=1 if lf_status==1 | lf_status==2
replace labour_force=0 if lf_status==3
* label
label define labor_labels 1 "Labor Force" 0 "Not in the Labor Force", replace
label values labour_force labor_labels
tab labour_force [aw=asecwt]

generate unemployed=1 if lf_status==2
replace unemployed=0 if lf_status==1
* label
label define unemployed_labels 1 "Unemployed" 0 "Employed", replace
label values unemployed unemployed_labels
tab unemployed [aw=asecwt]
** simplified age varaible with 3 categories
generate age_simple=0 if age >= 15 & age <=30
replace age_simple=1 if age >= 31 & age <=49
replace age_simple=2 if age >= 50 & age <=64
*label
label define age_labels 0 "15 - 30 years" 1 "31 - 49 years" 2 " 50 - 64 years", replace
label values age_simple age_labels
tab age_simple

** simplified earning variable 
generate income_simple=0 if incwage >= 0 & incwage <=30000 & lf_status== 1
replace income_simple=1 if incwage >= 30001 & incwage <=58020 & lf_status== 1 
replace income_simple=2 if incwage >= 58021 & incwage <=94000 & lf_status== 1
replace income_simple=3 if incwage >= 94001 & incwage <= 153000 & lf_status== 1
replace income_simple=4 if incwage >= 153000 & lf_status== 1
*label
label define income_labels 0 "lower class" 1 "lower middle class" 2 " middle class" 3 "Upper-middle class" 4 "Upper class", replace
label values income_simple income_labels
tab income_simple
**************************************************************************************************************************************************************************************
********************************************************************************* Label variables ************************************************************************************
**************************************************************************************************************************************************************************************

label variable cpi_2019 " CPI adjustment factor 2019 as base"
label variable _merge "Merged variable"
label variable real_incwage "Real annual income after adjustment"
label variable female "Female dummy"
label variable race_simple "Race categories"
label variable health_simple "Health dummy"
label variable educ_simple "Education categories"
label variable ind_simple  "Industry categories"
label variable lf_status "Labour force status"
label variable employed "Employment status dummy"
label variable labour_force "Labour force participation dummy"
label variable unemployed "Unemployment status dummy"
label variable age_simple "Age categories"
label variable income_simple "Income categories" 
label variable year_simple "Year categories"
label variable log_real_earnings "Natural log of real annual income"

**************************************************************************************************************************************************************************************
********************************************************************************* Analysis *******************************************************************************************
**************************************************************************************************************************************************************************************
** Figure 1 - Trends in Education Levels by Decades, (%) Have educational attainment increased overtime?
tab educ_simple year_simple [aw=asecwt], column

** Figure 2 - Trends in Good Health by decades ****************

** 2000 - 2009 Health
tab year_simple health_simple [aw=asecwt] if year_simple== 1, row
tab year_simple health_simple [aw=asecwt] if year_simple== 2, row
tab year_simple health_simple [aw=asecwt] if year_simple== 3, row

** Figure 3 - Health status by education level ****************
tab year
tab health_simple educ_simple [aw=asecwt], column

** Table 1 - Summary Statistics ****************

** Gender and Health
tab female health_simple [aw=asecwt], row

** Race and Healt
tab race_simple health_simple [aw=asecwt], row

** Industry and Health
tab ind_simple health_simple [aw=asecwt], row
set more off
** state and health
tab statefip health_simple [aw=asecwt], row
** age and health
tab age_simple health_simple [aw=asecwt], row

** employment and health
tab lf_status health_simple [aw=asecwt], row

** Mean earnings and health
sum real_incwage if health_simple== 1
sum real_incwage if health_simple== 0
** for employed
sum real_incwage if health_simple== 1 & lf_status == 1
sum real_incwage if health_simple== 0 & lf_status == 1

** Table 2 - health on education ****************
* Regression controlling for Year, State fixed effects and Demografics
set more off
estimates clear
eststo : areg health_simple i.educ_simple i.year i.statefip [aw=asecwt], r absorb(statefip)
eststo : areg health_simple i.educ_simple c.age##c.age i.female i.race_simple i.year i.statefip [aw=asecwt], r absorb(statefip)

esttab using table2, se nobaselevels rtf replace

** Table 3 - Health on education for employed
set more off
estimates clear
eststo : areg health_simple i.educ_simple c.age##c.age i.female i.race_simple i.employed i.year i.statefip [aw=asecwt], r absorb(statefip)
eststo : areg health_simple i.educ_simple c.age##c.age i.female i.race_simple i.employed i.ind_simple i.year i.statefip [aw=asecwt], r absorb(statefip)
eststo : areg health_simple i.educ_simple c.age##c.age i.female i.race_simple i.employed i.ind_simple i.year i.statefip [aw=asecwt] if log_real_earnings !=., r absorb(statefip)
eststo : areg health_simple i.educ_simple c.age##c.age i.female i.race_simple i.employed i.ind_simple i.year i.statefip log_real_earnings [aw=asecwt] if log_real_earnings !=., r absorb(statefip)

esttab using table3, se nobaselevels rtf replace

** Table 4 - regression results by decades ****************

* running all the cleaning codes again, but this time we don't drop the sample after 2019 and also this time we have a dummy for decades
* use the simplfied year variable

* check simplified year variable
tab year_simple [aw=asecwt]

tab year 

estimates clear
set more off
eststo : areg health_simple i.educ_simple c.age##c.age i.female i.race_simple i.employed i.ind_simple i.year i.statefip log_real_earnings [aw=asecwt] if log_real_earnings !=. & year_simple== 1, r absorb(statefip)
eststo : areg health_simple i.educ_simple c.age##c.age i.female i.race_simple i.employed i.ind_simple i.year i.statefip log_real_earnings [aw=asecwt] if log_real_earnings !=. & year_simple== 2, r absorb(statefip)
eststo : areg health_simple i.educ_simple c.age##c.age i.female i.race_simple i.employed i.ind_simple i.year i.statefip log_real_earnings [aw=asecwt] if log_real_earnings !=. & year_simple== 3, r absorb(statefip)

esttab using table4, se nobaselevels rtf replace
# Coding-for-term-paper-of-Labour-Economics
