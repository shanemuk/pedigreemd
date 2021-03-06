# pedigreemd
Throwing a few ideas around for implementing a markdown spec for pedigree notation

Taking a family tree (pedigree) is an important part of the Clinical Genetics consultation, although there are many other settings where drawing a family tree may be useful. It is also becoming increasingly common as people start researching their own genealogy and ancestry.

However there is no standard form of notation, and the very nature of pedigrees makes it difficult to note them down - the biggest problem of course is that everyone has two parents, heading back pretty much ad infinitum, so we don't have one single lineage - we have *many*, and of course those lineages meet multiple common ancestors on their way back into the past. And sometimes there are gaps in our knowledge that may need to be filled in by new information.

Various standards have been developed over the years for drawing pedigrees in the Genetic Counselling scenario. The most commonly used standard derives from https://www.geneticcounselingtoolkit.com/cases/pedigree/Bennett%20JGC%202008%20-%20Standardized%20Human%20Pedigree%20Nomenclature%20-%20Update%20and%20Assessment%20of%20the%20Recommendations%20of%20the%20National%20Society%20of%20Genetic%20Counselors.pdf

There are several pedigree-drawing programs on the market, and many modern EHRs (electronic health records) incorporate at least some pedigree functionality. However for the Genetic Counsellor, Clinical Geneticist or Family History Researcher taking a family history from a patient or client, these can be clunky and paying too much attention to the computer can detract from the value of the consultation. When pedigrees are drawn by hand, transferring these to electronic format can be cumbersome and time-consuming, or the EHR may not have pedigree functionality built in as standard. Even when that functionality exists, outputting the pedigree to a format suitable for inclusion in clinical correspondence may be difficult or unsatisfactory.

One very early effort to address this was ASCPED, which sought to largely replicate pedigree output using ASCII characters. An example is shown below.
~~~
[\] Brian Bloggs (deceased)
 |----|
(_)   |  Jenny Bloggs
      |--[_] Benny Bloggs
      |--(_) Lisa Bloggs
      |--(_) Charlotte Tester (nee Bloggs)
          |----|
         [_]   |  Charlie Tester
               |--(_) Penelope Tester
               |--<P> Currently pregnant, sex unknown/unspecified
               |--<4> 4 other siblings, sex unspecified
~~~


So this is very much a work in progress - suggestions welcome!
