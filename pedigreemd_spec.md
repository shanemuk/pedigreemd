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
Note that some RIs have synonyms to make it easier to record these in clinic. The characters are NOT necessarily hierarchical, although the final character sometimes adds context. It's intended to be as intuitive as possible. This format typically adheres to a somewhat "gonadal" view of sex and relationships. This is not intended in any way to make a statement in relation to social views of gender, but to provide the user with the flexibility to record family trees (pedigrees) as they find appropriate for genetic counselling purposes. PedigreeMD wishes to recognise and affirm our LGBTQIA+ friends, and comments are welcome.

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

## Pedigree of Napoleon Bonaparte
This example shows the use of PedigreeMD to construct a pedigree based on the use of Napoleon Bonaparte as its principal proband. The pedigree expands as first degree relatives are added. There are a few bits here I need to fix. Some of this data is from [Shannon Selin's site](https://shannonselin.com/extras/napoleons-family-tree/) - this is just a preliminary draft, but you can see how easy PedigreeMD is to construct.
~~~
// Pedigree: Napoleon Bonaparte
PRO: Napoleon Bonaparte
  MUM: Maria Letizia Bonaparte (nee Ramolino) (24/8/1750 – 2/2/1836)
    BRO: Joseph Fesch (3 January 1763 – 13 May 1839) – Roman Catholic cardinal, half-brother
  DAD: Carlo Maria Buonaparte
  BRO: Joseph Bonaparte (7 January 1768 – 28 July 1844)
    DAU: Charlotte (Lolotte) Napoléone Bonaparte (31 October 1802 – 2 March 1839)
  BRO: Lucien Bonaparte (21 May 1775 – 29 June 1840)
    DAU: Christine-Egypta Bonaparte (19 October 1798 – 19 May 1847)
  SIS: Elisa Bonaparte (3 January 1777 – 7 August 1820)
  BRO: Louis Bonaparte (2 September 1778 – 25 July 1846)
  SIS: Pauline Bonaparte (20 October 1780 – 9 June 1825)
  SIS: Caroline Bonaparte (25 March 1782 – 18 May 1839)
    SON: Achille Murat (21 January 1801 – 15 April 1847)
  BRO: Jérôme Bonaparte
    SPO: Elizabeth (Betsy) Patterson Bonaparte (6 February 1785 – 4 April 1879)
    SON: Jerome (Bo) Patterson Bonaparte (7 July 1805 – 17 June 1870)
  SON: Napoleon François Charles Joseph (Franz) Bonaparte (20 March 1811 – 22 July 1832) – King of Rome, Duke of Reichstadt
  ADOPTED: Eugène and Hortense de Beauharnais (3 September 1781 – 21 February 1824; 10 April 1783 – 5 October 1837)
  SON: Charles Léon Denuelle (13 December 1806 – 14 April 1881) – Napoleon’s illegitimate son
  SON: Alexandre Colonna Walewksi (4 May 1810 – 27 September 1868) – Napoleon’s illegitimate son
~~~
