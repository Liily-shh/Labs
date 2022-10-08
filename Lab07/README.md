Lab07
================
Lily
2022-10-07

``` r
knitr::opts_chunk$set(echo = TRUE)
```

``` r
library(tidytext)
library(tidyverse)
```

    ## ── Attaching packages ─────────────────────────────────────── tidyverse 1.3.2 ──
    ## ✔ ggplot2 3.3.6     ✔ purrr   0.3.4
    ## ✔ tibble  3.1.8     ✔ dplyr   1.0.9
    ## ✔ tidyr   1.2.0     ✔ stringr 1.4.1
    ## ✔ readr   2.1.2     ✔ forcats 0.5.2
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()

``` r
library(dplyr)
library(ggplot2)
library(forcats)
library(stringr)
library(rvest)
```

    ## 
    ## Attaching package: 'rvest'
    ## 
    ## The following object is masked from 'package:readr':
    ## 
    ##     guess_encoding

``` r
library(httr)
query_ids <- GET(
  url   = "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/esearch.fcgi",
  query = list(
    db      = "pubmed",
    term    = "covid19 hawaii",
    retmax  = 1000
  ), 
)
ids <- httr::content(query_ids)
```

``` r
ids <- as.character(ids)

ids <- stringr::str_remove_all(ids, "</?Id>")
head(ids)
```

    ## [1] "<?xml version=\"1.0\" encoding=\"UTF-8\"?>\n<!DOCTYPE eSearchResult PUBLIC \"-//NLM//DTD esearch 20060628//EN\" \"https://eutils.ncbi.nlm.nih.gov/eutils/dtd/20060628/esearch.dtd\">\n<eSearchResult>\n  <Count>286</Count>\n  <RetMax>286</RetMax>\n  <RetStart>0</RetStart>\n  <IdList>\n    36186620\n    36173983\n    36146513\n    36128780\n    36126272\n    36108254\n    36091468\n    36081413\n    36065487\n    36051401\n    36043851\n    36018589\n    35969382\n    35954726\n    35951232\n    35947596\n    35942481\n    35923385\n    35922730\n    35921109\n    35916603\n    35899278\n    35873301\n    35873076\n    35833192\n    35821668\n    35818366\n    35801366\n    35775002\n    35746577\n    35742804\n    35716737\n    35710669\n    35700997\n    35678989\n    35673364\n    35661139\n    35636901\n    35632529\n    35627656\n    35605700\n    35605371\n    35600422\n    35594688\n    35584085\n    35570918\n    35528753\n    35528751\n    35503921\n    35502304\n    35495071\n    35455823\n    35415617\n    35405638\n    35399704\n    35382257\n    35382073\n    35379351\n    35370509\n    35358349\n    35320122\n    35311911\n    35296505\n    35273936\n    35263081\n    35252592\n    35182433\n    35179422\n    35156056\n    35146208\n    35142835\n    35141345\n    35123100\n    35120186\n    35111349\n    35100194\n    35089919\n    35076582\n    35059075\n    35047039\n    35043124\n    35028589\n    35027873\n    35023402\n    34967845\n    34946336\n    34936687\n    34909439\n    34901299\n    34897806\n    34896104\n    34890263\n    34877361\n    34855156\n    34825050\n    34815815\n    34786570\n    34778744\n    34765988\n    34765987\n    34757265\n    34744296\n    34739906\n    34736373\n    34736125\n    34734403\n    34732841\n    34704071\n    34704070\n    34704069\n    34704068\n    34704067\n    34704066\n    34704065\n    34704064\n    34704063\n    34704061\n    34687321\n    34662333\n    34661133\n    34661132\n    34661131\n    34661130\n    34661129\n    34661128\n    34661127\n    34661126\n    34661125\n    34661124\n    34661123\n    34661122\n    34661121\n    34621978\n    34562997\n    34559481\n    34545941\n    34536350\n    34532685\n    34529634\n    34491990\n    34481278\n    34473201\n    34448649\n    34417121\n    34406840\n    34391908\n    34367726\n    34355196\n    34352507\n    34334985\n    34331541\n    34314211\n    34308400\n    34308322\n    34291832\n    34287651\n    34287159\n    34283939\n    34254888\n    34228774\n    34226774\n    34210370\n    34195618\n    34189029\n    34183789\n    34183411\n    34183191\n    34180390\n    34140009\n    34125658\n    34117878\n    34108898\n    34102878\n    34091576\n    34062806\n    33990619\n    33982008\n    33980567\n    33973241\n    33971389\n    33966879\n    33938253\n    33929934\n    33926498\n    33900192\n    33897904\n    33894385\n    33889849\n    33889848\n    33859192\n    33856881\n    33851191\n    33826985\n    33789080\n    33781762\n    33781585\n    33775167\n    33770003\n    33769536\n    33746047\n    33728687\n    33718878\n    33717793\n    33706209\n    33661861\n    33661727\n    33657176\n    33655229\n    33607081\n    33606666\n    33606656\n    33587873\n    33495523\n    33482708\n    33471778\n    33464637\n    33442699\n    33422679\n    33422626\n    33417334\n    33407957\n    35412644\n    33331197\n    33316097\n    33308888\n    33301024\n    33276110\n    33270782\n    33251328\n    33244071\n    33236896\n    33229999\n    33216726\n    33193454\n    33186704\n    33176077\n    33139866\n    33098971\n    33096099\n    33087192\n    33083826\n    33043445\n    33027604\n    32984015\n    32969950\n    32921878\n    32914097\n    32914093\n    32912595\n    32907823\n    32907673\n    32891785\n    32888905\n    32881116\n    32837709\n    32837535\n    32763956\n    32763350\n    32745072\n    32742897\n    32692706\n    32690354\n    32680824\n    32666058\n    32649272\n    32596689\n    32592394\n    32584245\n    32501143\n    32486844\n    32462545\n    32432219\n    32432218\n    32432217\n    32427288\n    32420720\n    32386898\n    32371624\n    32371551\n    32361738\n    32326959\n    32323016\n    32314954\n    32300051\n    32259247\n    32151778\n  </IdList>\n  <TranslationSet>\n    <Translation>\n      <From>covid19</From>\n      <To>\"covid-19\"[MeSH Terms] OR \"covid-19\"[All Fields] OR \"covid19\"[All Fields]</To>\n    </Translation>\n    <Translation>\n      <From>hawaii</From>\n      <To>\"hawaii\"[MeSH Terms] OR \"hawaii\"[All Fields]</To>\n    </Translation>\n  </TranslationSet>\n  <TranslationStack>\n    <TermSet>\n      <Term>\"covid-19\"[MeSH Terms]</Term>\n      <Field>MeSH Terms</Field>\n      <Count>188418</Count>\n      <Explode>Y</Explode>\n    </TermSet>\n    <TermSet>\n      <Term>\"covid-19\"[All Fields]</Term>\n      <Field>All Fields</Field>\n      <Count>287482</Count>\n      <Explode>N</Explode>\n    </TermSet>\n    <OP>OR</OP>\n    <TermSet>\n      <Term>\"covid19\"[All Fields]</Term>\n      <Field>All Fields</Field>\n      <Count>3406</Count>\n      <Explode>N</Explode>\n    </TermSet>\n    <OP>OR</OP>\n    <OP>GROUP</OP>\n    <TermSet>\n      <Term>\"hawaii\"[MeSH Terms]</Term>\n      <Field>MeSH Terms</Field>\n      <Count>8340</Count>\n      <Explode>Y</Explode>\n    </TermSet>\n    <TermSet>\n      <Term>\"hawaii\"[All Fields]</Term>\n      <Field>All Fields</Field>\n      <Count>30738</Count>\n      <Explode>N</Explode>\n    </TermSet>\n    <OP>OR</OP>\n    <OP>GROUP</OP>\n    <OP>AND</OP>\n    <OP>GROUP</OP>\n  </TranslationStack>\n  <QueryTranslation>(\"covid-19\"[MeSH Terms] OR \"covid-19\"[All Fields] OR \"covid19\"[All Fields]) AND (\"hawaii\"[MeSH Terms] OR \"hawaii\"[All Fields])</QueryTranslation>\n</eSearchResult>\n"

``` r
publications <- GET(
  url   = "https://eutils.ncbi.nlm.nih.gov/entrez/eutils/efetch.fcgi",
  query = list(
    db  = "pubmed",
    id  = paste(ids,collapse = ","),
  retmax = 1000,
  rettype = "abstract"))
publications <- httr::content(publications)
```

``` r
pub_char_list <- xml2::xml_children(publications)
pub_char_list <- sapply(pub_char_list, as.character)
abstracts <- str_extract(pub_char_list, "<Abstract>[[:print:][:space:]]+</Abstract>")
abstracts[[1]]
```

    ## [1] NA

``` r
abstracts <- str_remove_all(abstracts, "</?[[:alnum:]- =\"]+>") 
abstracts[[1]]
```

    ## [1] NA

``` r
abstracts <- str_replace_all(abstracts, "[[:space:]]+"," ")
abstracts[[1]]
```

    ## [1] NA

``` r
titles <- str_extract(pub_char_list, "<ArticleTitle>[[:print:][:space:]]+</ArticleTitle>")
titles[[1]]
```

    ## [1] NA

``` r
titles <- str_remove_all(titles, "</?[[:alnum:]- =\"]+>")
titles[[1]]
```

    ## [1] NA

``` r
database <- data.frame(
  PubMedId = ids,
  Title    = titles,
  Abstract = abstracts
)
knitr::kable(database[1:8,], caption = "Some papers about Covid19 and Hawaii")
```

|     | PubMedId                               | Title | Abstract |
|:----|:---------------------------------------|:------|:---------|
| 1   | <?xml version="1.0" encoding="UTF-8"?> |       |          |

Some papers about Covid19 and Hawaii

\<!DOCTYPE eSearchResult PUBLIC “-//NLM//DTD esearch 20060628//EN”
“<https://eutils.ncbi.nlm.nih.gov/eutils/dtd/20060628/esearch.dtd>”\>
<eSearchResult> <Count>286</Count> <RetMax>286</RetMax>
<RetStart>0</RetStart> <IdList> 36186620 36173983 36146513 36128780
36126272 36108254 36091468 36081413 36065487 36051401 36043851 36018589
35969382 35954726 35951232 35947596 35942481 35923385 35922730 35921109
35916603 35899278 35873301 35873076 35833192 35821668 35818366 35801366
35775002 35746577 35742804 35716737 35710669 35700997 35678989 35673364
35661139 35636901 35632529 35627656 35605700 35605371 35600422 35594688
35584085 35570918 35528753 35528751 35503921 35502304 35495071 35455823
35415617 35405638 35399704 35382257 35382073 35379351 35370509 35358349
35320122 35311911 35296505 35273936 35263081 35252592 35182433 35179422
35156056 35146208 35142835 35141345 35123100 35120186 35111349 35100194
35089919 35076582 35059075 35047039 35043124 35028589 35027873 35023402
34967845 34946336 34936687 34909439 34901299 34897806 34896104 34890263
34877361 34855156 34825050 34815815 34786570 34778744 34765988 34765987
34757265 34744296 34739906 34736373 34736125 34734403 34732841 34704071
34704070 34704069 34704068 34704067 34704066 34704065 34704064 34704063
34704061 34687321 34662333 34661133 34661132 34661131 34661130 34661129
34661128 34661127 34661126 34661125 34661124 34661123 34661122 34661121
34621978 34562997 34559481 34545941 34536350 34532685 34529634 34491990
34481278 34473201 34448649 34417121 34406840 34391908 34367726 34355196
34352507 34334985 34331541 34314211 34308400 34308322 34291832 34287651
34287159 34283939 34254888 34228774 34226774 34210370 34195618 34189029
34183789 34183411 34183191 34180390 34140009 34125658 34117878 34108898
34102878 34091576 34062806 33990619 33982008 33980567 33973241 33971389
33966879 33938253 33929934 33926498 33900192 33897904 33894385 33889849
33889848 33859192 33856881 33851191 33826985 33789080 33781762 33781585
33775167 33770003 33769536 33746047 33728687 33718878 33717793 33706209
33661861 33661727 33657176 33655229 33607081 33606666 33606656 33587873
33495523 33482708 33471778 33464637 33442699 33422679 33422626 33417334
33407957 35412644 33331197 33316097 33308888 33301024 33276110 33270782
33251328 33244071 33236896 33229999 33216726 33193454 33186704 33176077
33139866 33098971 33096099 33087192 33083826 33043445 33027604 32984015
32969950 32921878 32914097 32914093 32912595 32907823 32907673 32891785
32888905 32881116 32837709 32837535 32763956 32763350 32745072 32742897
32692706 32690354 32680824 32666058 32649272 32596689 32592394 32584245
32501143 32486844 32462545 32432219 32432218 32432217 32427288 32420720
32386898 32371624 32371551 32361738 32326959 32323016 32314954 32300051
32259247 32151778 </IdList> <TranslationSet> <Translation>
<From>covid19</From> <To>“covid-19”\[MeSH Terms\] OR “covid-19”\[All
Fields\] OR “covid19”\[All Fields\]</To> </Translation> <Translation>
<From>hawaii</From> <To>“hawaii”\[MeSH Terms\] OR “hawaii”\[All
Fields\]</To> </Translation> </TranslationSet> <TranslationStack>
<TermSet> <Term>“covid-19”\[MeSH Terms\]</Term> <Field>MeSH
Terms</Field> <Count>188418</Count> <Explode>Y</Explode> </TermSet>
<TermSet> <Term>“covid-19”\[All Fields\]</Term> <Field>All
Fields</Field> <Count>287482</Count> <Explode>N</Explode> </TermSet>
<OP>OR</OP> <TermSet> <Term>“covid19”\[All Fields\]</Term> <Field>All
Fields</Field> <Count>3406</Count> <Explode>N</Explode> </TermSet>
<OP>OR</OP> <OP>GROUP</OP> <TermSet> <Term>“hawaii”\[MeSH Terms\]</Term>
<Field>MeSH Terms</Field> <Count>8340</Count> <Explode>Y</Explode>
</TermSet> <TermSet> <Term>“hawaii”\[All Fields\]</Term> <Field>All
Fields</Field> <Count>30738</Count> <Explode>N</Explode> </TermSet>
<OP>OR</OP> <OP>GROUP</OP> <OP>AND</OP> <OP>GROUP</OP>
</TranslationStack> <QueryTranslation>(“covid-19”\[MeSH Terms\] OR
“covid-19”\[All Fields\] OR “covid19”\[All Fields\]) AND (“hawaii”\[MeSH
Terms\] OR “hawaii”\[All Fields\])</QueryTranslation> </eSearchResult>
\|NA \|NA \| \|2 \|<?xml version="1.0" encoding="UTF-8"?> \<!DOCTYPE
eSearchResult PUBLIC “-//NLM//DTD esearch 20060628//EN”
“<https://eutils.ncbi.nlm.nih.gov/eutils/dtd/20060628/esearch.dtd>”\>
<eSearchResult> <Count>286</Count> <RetMax>286</RetMax>
<RetStart>0</RetStart> <IdList> 36186620 36173983 36146513 36128780
36126272 36108254 36091468 36081413 36065487 36051401 36043851 36018589
35969382 35954726 35951232 35947596 35942481 35923385 35922730 35921109
35916603 35899278 35873301 35873076 35833192 35821668 35818366 35801366
35775002 35746577 35742804 35716737 35710669 35700997 35678989 35673364
35661139 35636901 35632529 35627656 35605700 35605371 35600422 35594688
35584085 35570918 35528753 35528751 35503921 35502304 35495071 35455823
35415617 35405638 35399704 35382257 35382073 35379351 35370509 35358349
35320122 35311911 35296505 35273936 35263081 35252592 35182433 35179422
35156056 35146208 35142835 35141345 35123100 35120186 35111349 35100194
35089919 35076582 35059075 35047039 35043124 35028589 35027873 35023402
34967845 34946336 34936687 34909439 34901299 34897806 34896104 34890263
34877361 34855156 34825050 34815815 34786570 34778744 34765988 34765987
34757265 34744296 34739906 34736373 34736125 34734403 34732841 34704071
34704070 34704069 34704068 34704067 34704066 34704065 34704064 34704063
34704061 34687321 34662333 34661133 34661132 34661131 34661130 34661129
34661128 34661127 34661126 34661125 34661124 34661123 34661122 34661121
34621978 34562997 34559481 34545941 34536350 34532685 34529634 34491990
34481278 34473201 34448649 34417121 34406840 34391908 34367726 34355196
34352507 34334985 34331541 34314211 34308400 34308322 34291832 34287651
34287159 34283939 34254888 34228774 34226774 34210370 34195618 34189029
34183789 34183411 34183191 34180390 34140009 34125658 34117878 34108898
34102878 34091576 34062806 33990619 33982008 33980567 33973241 33971389
33966879 33938253 33929934 33926498 33900192 33897904 33894385 33889849
33889848 33859192 33856881 33851191 33826985 33789080 33781762 33781585
33775167 33770003 33769536 33746047 33728687 33718878 33717793 33706209
33661861 33661727 33657176 33655229 33607081 33606666 33606656 33587873
33495523 33482708 33471778 33464637 33442699 33422679 33422626 33417334
33407957 35412644 33331197 33316097 33308888 33301024 33276110 33270782
33251328 33244071 33236896 33229999 33216726 33193454 33186704 33176077
33139866 33098971 33096099 33087192 33083826 33043445 33027604 32984015
32969950 32921878 32914097 32914093 32912595 32907823 32907673 32891785
32888905 32881116 32837709 32837535 32763956 32763350 32745072 32742897
32692706 32690354 32680824 32666058 32649272 32596689 32592394 32584245
32501143 32486844 32462545 32432219 32432218 32432217 32427288 32420720
32386898 32371624 32371551 32361738 32326959 32323016 32314954 32300051
32259247 32151778 </IdList> <TranslationSet> <Translation>
<From>covid19</From> <To>“covid-19”\[MeSH Terms\] OR “covid-19”\[All
Fields\] OR “covid19”\[All Fields\]</To> </Translation> <Translation>
<From>hawaii</From> <To>“hawaii”\[MeSH Terms\] OR “hawaii”\[All
Fields\]</To> </Translation> </TranslationSet> <TranslationStack>
<TermSet> <Term>“covid-19”\[MeSH Terms\]</Term> <Field>MeSH
Terms</Field> <Count>188418</Count> <Explode>Y</Explode> </TermSet>
<TermSet> <Term>“covid-19”\[All Fields\]</Term> <Field>All
Fields</Field> <Count>287482</Count> <Explode>N</Explode> </TermSet>
<OP>OR</OP> <TermSet> <Term>“covid19”\[All Fields\]</Term> <Field>All
Fields</Field> <Count>3406</Count> <Explode>N</Explode> </TermSet>
<OP>OR</OP> <OP>GROUP</OP> <TermSet> <Term>“hawaii”\[MeSH Terms\]</Term>
<Field>MeSH Terms</Field> <Count>8340</Count> <Explode>Y</Explode>
</TermSet> <TermSet> <Term>“hawaii”\[All Fields\]</Term> <Field>All
Fields</Field> <Count>30738</Count> <Explode>N</Explode> </TermSet>
<OP>OR</OP> <OP>GROUP</OP> <OP>AND</OP> <OP>GROUP</OP>
</TranslationStack> <QueryTranslation>(“covid-19”\[MeSH Terms\] OR
“covid-19”\[All Fields\] OR “covid19”\[All Fields\]) AND (“hawaii”\[MeSH
Terms\] OR “hawaii”\[All Fields\])</QueryTranslation> </eSearchResult>
\|NA \|NA \| \|NA \|NA \|NA \|NA \| \|NA.1 \|NA \|NA \|NA \| \|NA.2 \|NA
\|NA \|NA \| \|NA.3 \|NA \|NA \|NA \| \|NA.4 \|NA \|NA \|NA \| \|NA.5
\|NA \|NA \|NA \|
