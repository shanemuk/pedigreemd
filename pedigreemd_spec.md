# Schema for PedigreeMD
## Objective
To define a text based format for rapidly, easily, intuitively and legibly recording pedigree information in a clinic using a standard text editor (txt files).

## Basics
* A PedigreeMD file contains a number of lines of text. In the very basic version each line has a max of 255 characters
* Each line normally defines an individual.
* first degree relatives of this individual are defined by indented lines - lines are indented using the TAB character.
* There can be ambiguity in how this gets defined - it is the job of the parser program to clean this up.
## Example
Here is a small example pedigree. Note the pedigree is **anchored** on the proband, Jimmy Bloggs, and his relatives are specified by **relationship indicators**.
~~~
PRO: Jimmy Bloggs // this is the proband
  SPO: Jenny Bloggs // this is Jimmy's spouse
  SON: Nigel Bloggs // Jimmy & Jenny's son
  DAU: Hilda Bloggs // Jimmy & Jenny's daughter
    DAU: Nellie Bloggs // Hilda's daughter - note we have not defined Nellie's dad here
    MIS: // Miscarriage
~~~
Is very easy to note this down in a clinic situation where you're talking with a family member. You basically start with the proband (or whatever individual you're interested in) and work outwards. The job of the Parser will be to turn this into a more "standard" graphical format.
## Relationship indicators (RI)
A "Relationship indicator" is typically a 3-letter prefix to a line which tells you how the subject of the line is related to the next level up. This is typically called the "parent" record, but very often will not be the parent in a biological sense. Here is a list of RIs:
~~~
  PRO - proband
  BRO - brother (full)
  SIS - sister (full)
  PHM - paternal half brother
  PHF - paternal half sister
  MHM - maternal half brother
  MHF - maternal half sister
  SIB - sibling sex unspecified
  DAD - father
  MUM - mother
  SON - son
  DAU - daughter
  CHX - child, sex unspecified
  MZM - monozygotic sibling MALE
  MZF - monozygotic sibling FEMALE
  MZX - monozygotic sibling sex unspecified
  DZM - dizygotic twin brother
  DZF - dizygotic twin sister
  DZX - dizygotic sibling sex unspecified
  SPO - spouse (in this context, "spouse" means a reproductive partner)
  SPC - spouse, consaunguineous
  SPD - spouse divorced
~~~  
Note that some RIs have synonyms to make it easier to record these in clinic. The characters are NOT necessarily hierarchical, although the final character adds context. This format is intended where possible to adhere to a somewhat "gonadal" view of sex and relationships. This is not intended in any way to make a statement in relation to more flexible understandings, but to provide the user with the flexibility to record family trees (pedigrees) as they find appropriate for genetic counselling purposes. PedigreeMD wishes to recognise and affirm our LGBTQIA+ friends, and comments are welcome.

~~~
SIB Erica Bloggs affected HD, deceased
~~~
The parser should be smart enough to not need the colon after "SIB" and to read the name, as well as work out the term "affected" means that what follows is the name of the disease. Terms like "deceased" will be interpreted appropriately. The parser will have quite loose syntactical rules, and make its best effort at working out what the user means, before rendering it in standardised format.

## Reserved terms
Certain terms and abbreviations, in addition to those above, are "reserved" so that the parser can make sense of the text. 
~~~
carr, carrier: carrier of the conditions specified 
aff, affected: affected with the list of diseases that follows
dec, deceased: deceased
male, female: override relationship indicator sex specification
adin, adout: Adopted in/out
pos, positive: tested positive for whatever genetic condition is being specified
neg, negative: tested negative
// anything that follows this is a comment and will not be parsed further 
~~~
## DISEASES section
The file can have an optional section for DISEASES, and the first line of this section should say just that: "DISEASES". eg:
~~~
DISEASES
1. Huntington's Disease
2. Cystic Fibrosis
3. Lynch syndrome
~~~
This allows flexibility and re-use, e.g. 
~~~
affected 1,2. 
~~~
