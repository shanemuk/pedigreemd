# Elements of a pedigreeMD line
1. EXAMPLE
2. sex: display sex for purposes of pedigree: ```[_]``` for male, ```(_)``` for female, ```<_>``` for sex not specified
    - note: need for other symbols
4. deceased status, a slash indicating deceased, e.g. ```[/]``` would be a deceased male
5. disease attributes enclosed within the brackets

If a person is "reused" within a pedigree, they need to be given an ID for use *within* the pedigree.

Information enclosed by the pedigree symbol "brackets" represents "flags", e.g.:
~~~
[/] male deceased (reserved flag)
(#) female affected
<?> sex unspecified, query affected
~~~
You can add as many flags as you like - beware - some may be contradictory, and unrecognised flags will be... unrecognised!
If the bracket isn't closed in the same line, the system will assume there are no flags.
In the description you can use hashtags for diseases, e.g. #breastcancer #epilepsy

## Keys
It's good practice for your pedigrees to have a key, e.g.
~~~
# affected
% unaffected gene carrier
? query affected
SB stillbirth

~~~
- Usually each individual will have a single line. exceptions are  for relationships & comments.
- Comments start with // a bit like in some programming languages. Everything after // for that line is ignored

# Examples
~~~
[/] Billy Bloggs d65y #LungCancer:65y @m11
& // this is a relationship indicator. You can use && for consanguinity; &/ for divorced/separated 
(_) Jenny Bloggs #MultipleSclerosis:59y b1960-12-11
           // this is a comment - this line will be ignored.
--[#] Tarquin Bloggs
--(_) Jemima Smythers
  &[_] Stanley Smythers
  --(_) Darlene Smythers
--[_] Jordan Bloggs #diabetes:5y #leukaemia:10y
--(/) Lottie Bloggs d3y #medulloblastoma:3y

[@m11] //indicates REUSE of a symbol for relationship purposes, in this case
~~~
