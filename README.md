# pedigreemd
Throwing a few ideas around for implementing a markdown spec for pedigree notation

Taking a family tree (pedigree) is an important part of the Clinical Genetics consultation, although there are many other settings where drawing a family tree may be useful. It is also becoming increasingly common as people start researching their own genealogy and ancestry.

Here's a page I'm building based on **pedigreejs**: [**PEDIGREE EDITOR**](pedigreejs_demo.html)

However there is no standard form of notation when you're coding pedigrees "live", and the very nature of pedigrees makes it difficult to note them down - the biggest problem of course is that everyone has two parents, heading back pretty much ad infinitum, so we don't have one single lineage - we have *many*, and of course those lineages meet multiple common ancestors on their way back into the past. And sometimes there are gaps in our knowledge that may need to be filled in by new information.

Various standards have been developed over the years for drawing pedigrees in the Genetic Counselling scenario. The most commonly used standard derives from https://www.geneticcounselingtoolkit.com/cases/pedigree/Bennett%20JGC%202008%20-%20Standardized%20Human%20Pedigree%20Nomenclature%20-%20Update%20and%20Assessment%20of%20the%20Recommendations%20of%20the%20National%20Society%20of%20Genetic%20Counselors.pdf

There are several pedigree-drawing programs on the market, and many modern EHRs (electronic health records) incorporate at least some pedigree functionality. However for the Genetic Counsellor, Clinical Geneticist or Family History Researcher taking a family history from a patient or client, these can be clunky and paying too much attention to the computer can detract from the value of the consultation. When pedigrees are drawn by hand, transferring these to electronic format can be cumbersome and time-consuming, or the EHR may not have pedigree functionality built in as standard. Even when that functionality exists, outputting the pedigree to a format suitable for inclusion in clinical correspondence may be difficult or unsatisfactory.

## Statement of needs
So what do we want? We need a plain text format entry 

## Early days: ASCPED
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
While this is relatively easily readable in ASCII, it's a bit of a pain to type, and it really only works well for simple pedigrees where we're not trying to pull in too much information from different sides of the family. It also starts using up a lot of space characters, so can only align properly in fixed fonts. Proportional fonts will make it wibbly (that's a technical term). If we're going to make it easy to record the pedigrees *and* view them, we need to be a bit smarter, but we might have to make some sacrifices.

## Next iteration
One way might be to limit the number of cascading indents permitted, and also to do away with as many of the linking characters as possible, for example:
~~~
[\Brian Bloggs] deceased
&(Jenny Bloggs)
    [Benny Bloggs]
    (Lisa Bloggs)
    (Charlotte Tester) nee Bloggs
    &[Charlie Tester]
        (Penelope Tester)
        <P> Pregnant, sex unknown
        <4> siblings, sex unspecified
~~~

So that's a three generation pedigree, *single lineage*. You'll note I'm still using square brackets `[_]` for male, curved for female `(_)`, but this time instead of putting the names outside the brackets, I'm putting them inside; I'm joining spouses with an `&`. It's not as pretty or readable as the ASCPED example above, but maybe it doesn't have to be.

We could decide to "define" individuals as they become used in the file; potentially we could just list everyone at the top of the file, and then create the pedigree structure further down. Note there would be issues here if two people had the same name, or if you "re-used" someone in a context that didn't make sense in the pedigree. Remember that each individual can only have a pair of parents - in some cases we won't have info on one or other of them, so both parents are probably not strictly necessary

I'm still thinking however that indents might make this a bit tricky, especially for complex pedigrees. What about if we just made the decision that each sibship sits as its own block? We would then explicitly "reuse" individuals as we dealt with each sibship, particularly in multi-generation pedigrees.

~~~
(Charlotte Tester)
&[Charlie Tester]
-(Penelope Tester)
-<P>
-<4>
~~~

## Update 2022-12-22
Here are some of my [latest thoughts...](pedexamples.txt)

Ideally I'd like to be able to (in principle) take a pedigree in Notepad or Word (or the text editor of your choice), then paste/import it into the EHR or pedigree software, where it would read it and output a pretty pedigree in standard format.


This is very much a work in progress - suggestions welcome!

Interesting project: [pedigreejs](https://ccge-boadicea.github.io/pedigreejs/) - may be useful. Here's what the pedigreejs JSON file looks like:
~~~
[{"name":"m11","sex":"M","top_level":true},{"name":"f11","display_name":"Jane","sex":"F","status":1,"top_level":true,"breast_cancer":true,"ovarian_cancer":true},{"name":"m12","sex":"M","top_level":true},{"name":"f12","sex":"F","top_level":true,"breast_cancer":true},{"name":"m21","sex":"M","mother":"f11","father":"m11","age":56},{"name":"f21","display_name":"Joy","sex":"F","mother":"f12","father":"m12","breast_cancer":true,"breast_cancer2":true,"ovarian_cancer":true,"age":63},{"name":"ch1","display_name":"Ana","sex":"F","mother":"f21","father":"m21","proband":true,"age":25}]
~~~

So we can see to define an individual we need to know their **name** (which is the unique ID); their sex; their mother; their father. Other data attributes can be added as required.

## Further update 2024-12-30
One of the key things is this has to be usable "in the field" by Genetic doctors and counsellors etc, and I'm thinking the use of special characters is a bit of a barrier. The key thing is to start with an individual - a "PROBAND" - and work out from there, regardless of generation structure. So we only have to define the first degree relatives, and then any subsequent relatives can be nested/indented under them.
We're still keeping the format of one individual per line, and as long as relationships are defined, we can parse out the additional information from there. Here is a very basic example - it would be trivially easy to write some code to turn this into a "proper" pedigree, and then viewed in an appropriate viewer or imported into a designated database.
~~~
PROBAND: Charlene McFlynn
	MUM: Bertha
		MUM: Betty
		DAD: Henry
		SIS: Wendy Wilson
		BRO: Nigel Jones
			SPO: Gillian
			SON: Neil Jones
			DAU: Cindy Jones
			MISC: June 2013
	DAD: Jimmy McFlynn
~~~
