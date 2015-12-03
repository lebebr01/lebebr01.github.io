---
layout: slide
title: "Web Scraping to Item Response Theory: A College Football Adventure"
date: 2015-12-04
tags: [slides, R, rvest, irt]
---

<section>
    <h1 class="title">Web Scraping to Item Response Theory: A College Football Adventure</h1>
    <h2 class="author">Brandon LeBeau, Andrew Zieffler, and Kyle Nickodem</h2>
    <h3 class="date">University of Iowa &amp; University of Minnesota</h3>
</section>

<section data-markdown>
# Background
- Began after Tim Brewster was fired
- Wanted to try to predict next great coach
</section>

<section data-markdown>
# Data Available
- Data is avaiable at four levels
    1. Coach
    2. Team
    4. Game by Game
</section>

<section data-markdown>
# Coach
- Data
    - Overall record
    - Team history
- Not Available:
    - Coordinator history
</section>

<section data-markdown>
# Example Coach Data

```
##   Year Team Win Loss Tie     Pct  PF  PA Delta        coach
## 1 2010 Iowa   8    5   0 0.61538 376 221   155 Kirk Ferentz
## 2 2011 Iowa   7    6   0 0.53846 358 310    48 Kirk Ferentz
## 3 2012 Iowa   4    8   0 0.33333 232 275   -43 Kirk Ferentz
## 4 2013 Iowa   8    5   0 0.61538 342 246    96 Kirk Ferentz
## 5 2014 Iowa   7    6   0 0.53846 367 333    34 Kirk Ferentz
```
</section>
<section data-markdown>
# Game by Game
- Data
    - Final score of each game
    - Date played
    - Location
- Not Available
    - No information within a game
</section>
<section data-markdown>
# Example GBG Data

```
##    Team           Official Year       Date WL          Opponent PF PA
## 1  Iowa University of Iowa 2014  8/30/2014  W     Northern Iowa 31 23
## 2  Iowa University of Iowa 2014   9/6/2014  W     Ball St. (IN) 17 13
## 3  Iowa University of Iowa 2014  9/13/2014  L          Iowa St. 17 20
## 4  Iowa University of Iowa 2014  9/20/2014  W   Pittsburgh (PA) 24 20
## 5  Iowa University of Iowa 2014  9/27/2014  W       Purdue (IN) 24 10
## 6  Iowa University of Iowa 2014 10/11/2014  W           Indiana 45 29
## 7  Iowa University of Iowa 2014 10/18/2014  L          Maryland 31 38
## 8  Iowa University of Iowa 2014  11/1/2014  W Northwestern (IL) 48  7
## 9  Iowa University of Iowa 2014  11/8/2014  L         Minnesota 14 51
## 10 Iowa University of Iowa 2014 11/15/2014  W          Illinois 30 14
## 11 Iowa University of Iowa 2014 11/22/2014  L         Wisconsin 24 26
## 12 Iowa University of Iowa 2014 11/28/2014  L          Nebraska 34 37
## 13 Iowa University of Iowa 2014   1/2/2015  L         Tennessee 28 45
##              Location
## 1       Iowa City, IA
## 2       Iowa City, IA
## 3       Iowa City, IA
## 4      Pittsburgh, PA
## 5  West Lafayette, IN
## 6       Iowa City, IA
## 7    College Park, MD
## 8       Iowa City, IA
## 9     Minneapolis, MN
## 10      Champaign, IL
## 11      Iowa City, IA
## 12      Iowa City, IA
## 13   Jacksonville, FL
```
</section>

<section data-markdown>
# Team
- Data
    - Overall team record
    - Team statistics
    - Rankings
    - Conference Affiliation

</section>

<section data-markdown>
# Web Scraping
- Data were obtained from many sources
    - Much from <http://cfbdatawarehouse.com>
    - Also used wikipedia, ESPN, and rivals

</section>

<section data-markdown>
# Iowa Coaches Over Time
<img src="http://educate-r.org/figs/iowa.png" alt="" height = "500" width = "1200"/>

</section>
<section data-markdown>
# Iowa State Coaches Over Time
<img src="http://educate-r.org/figs/iowa_state.png" alt="" height = "500" width="1200"/>

</section>

<section data-markdown>   
# Strengths in web scraping
- Data is relatively easily obtained
- Structured process for obtaining data
- Can be easily updated

</section>

<section data-markdown>
# Challenges of web scraping
- At the mercy of the website
    - Many sites are old 
    - Not up to date on current design standards.
- Data validation can be difficult and time consuming
- Need some basic knowledge of html

</section>

<section data-markdown>
# When is Web Scraping Worthwhile?
- Best when scraping many pages
    - Particularly when web addresses are not structured
- Useful when data need to be updated
<hr>
- Not useful if only scraping a single page/table
</section>

<section data-markdown>
# HTML Basics
<ul>
<li> HTML is structured by start tags (e.g. `<table>`) and end tags (e.g. `</table>`) </li>
<li> Common tags 
</li>
</ul>
<div style="float: left; width: 75%;">
<ul>
<li><code>&lt;h1&gt;</code> - <code>&lt;h6&gt;</code></li>
<li><code>&lt;b&gt;</code> <code>&lt;i&gt;</code></li>
<li><code>&lt;a href=&quot;http://www.google.com&quot;&gt;</code></li>
<li><code>&lt;table&gt;</code></li>
</ul>
</div>
<div style="float: right; width: 25%;">
<ul>
<li><code>&lt;p&gt;</code></li>
<li><code>&lt;ul&gt;</code> &amp; <code>&lt;li&gt;</code></li>
<li><code>&lt;div&gt;</code></li>
<li><code>&lt;img&gt;</code></li>
</ul>
</div>
<hr>
<ul>
<li> Highly structured pages are the easiest to scrape. </li>
</ul>
</section>

<section data-markdown>
# HTML Code Example
<img src="http://educate-r.org/figs/ferentz_wikiside.png" alt="" height = "500" width="1200"/>

</section>
<section data-markdown>
# Tools for web scraping
- R
    - `rvest`: <http://blog.rstudio.org/2014/11/24/rvest-easy-web-scraping-with-r/>
    - `XML`: <http://www.omegahat.org/RSXML/>
- Python
    - `beautiful soup`: <http://www.crummy.com/software/BeautifulSoup/>
- Misc
    - `SelectorGadget`: <http://selectorgadget.com/>
</section>

<section data-markdown>    
# Basics of rvest
- `read_html` is the most basic function.
- `html_node` or `html_nodes`
    - These functions need css selectors or xpath
    - SelectorGadget is the easiest way to get this
</section>

<section data-markdown>
# SelectorGadget
- SelectorGadget is a Javascript addon for web browsers.
- Demo:
    - <https://en.wikipedia.org/wiki/Kirk_Ferentz>
</section>
<section data-markdown>
# Combine SelectorGadget with rvest

```r
library(rvest)
wiki_kirk <- read_html("https://en.wikipedia.org/wiki/Kirk_Ferentz")
wiki_kirk_extract <- wiki_kirk %>%
    html_nodes(".vcard td , .vcard th")
head(wiki_kirk_extract)
```

```
## {xml_nodeset (6)}
## [1] <td colspan="2" style="text-align:center"><a href="/wiki/File:Kirk_p ...
## [2] <th scope="row">Sport(s)</th>
## [3] <td class="category">\n  <a href="/wiki/American_football" title="Am ...
## [4] <th colspan="2" style="text-align:center;background-color: lightgray ...
## [5] <th scope="row">Title</th>
## [6] <td>\n  <a href="/wiki/Head_coach" title="Head coach">Head coach</a> ...
```
</section>
<section data-markdown>
# Extract text
- Use the `html_text` function


```r
wiki_kirk <- read_html("https://en.wikipedia.org/wiki/Kirk_Ferentz")
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_text()
head(wiki_kirk_extract)
```

```
## [1] "\nFerentz at the 2010 Orange Bowl\n"
## [2] "Sport(s)"                           
## [3] "Football"                           
## [4] "Current position"                   
## [5] "Title"                              
## [6] "Head coach"
```
</section>
<section data-markdown>
# Encoding problems
- Two solutions to fix encoding problems
- `guess_encoding`
- `repair_encoding`: fix encoding problems


```r
wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_text() %>%
  guess_encoding()
```

```
##       encoding language confidence
## 1        UTF-8                1.00
## 2 windows-1252       en       0.37
## 3 windows-1250       ro       0.18
## 4 windows-1254       tr       0.15
## 5     UTF-16BE                0.10
## 6     UTF-16LE                0.10
```
</section>
<section data-markdown>
# Fix Encoding Problems
- Best practice to reload page with correct encoding


```r
wiki_kirk <- read_html("https://en.wikipedia.org/wiki/Kirk_Ferentz", 
                       encoding = 'UTF-8')
```


```r
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_text() %>% 
  repair_encoding()
```
</section>
<section data-markdown>
# Extract html tags
- Use the `html_tags` function


```r
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_name()
head(wiki_kirk_extract)
```

```
## [1] "td" "th" "td" "th" "th" "td"
```
</section>
<section data-markdown>
# Extract html attributes
- Use the `html_attrs` function


```r
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_attrs()
head(wiki_kirk_extract)
```

```
## [[1]]
##             colspan               style 
##                 "2" "text-align:center" 
## 
## [[2]]
## scope 
## "row" 
## 
## [[3]]
##      class 
## "category" 
## 
## [[4]]
##                                          colspan 
##                                              "2" 
##                                            style 
## "text-align:center;background-color: lightgray;" 
## 
## [[5]]
## scope 
## "row" 
## 
## [[6]]
## named character(0)
```
</section>
<section data-markdown>
# Extract links
- Use the `html_attrs` function again


```r
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard a") %>%
  html_attr('href')
head(wiki_kirk_extract)
```

```
## [1] "/wiki/File:Kirk_pressconference_orangebowl2010.JPG"
## [2] "/wiki/American_football"                           
## [3] "/wiki/Head_coach"                                  
## [4] "/wiki/Iowa_Hawkeyes_football"                      
## [5] "/wiki/Big_Ten_Conference"                          
## [6] "/wiki/Royal_Oak,_Michigan"
```
</section>
<section data-markdown>
# Extract Tables

```r
record_kirk <- wiki_kirk %>%
  html_nodes(".wikitable") %>%
  .[[1]] %>%
  html_table(fill = TRUE)
head(record_kirk)
```

```
##                                                  Year  Team Overall
## 1 Maine Black Bears (Yankee Conference) (1990-1992)  <NA>    <NA>
## 2                                                1990 Maine   3-8
## 3                                                1991 Maine   3-8
## 4                                                1992 Maine   6-5
## 5                                              Maine:  <NA> 12-21
## 6 Iowa Hawkeyes (Big Ten Conference) (1999-present)  <NA>    <NA>
##
##   Conference Standing Bowl/playoffs Coaches# AP NA NA
## 1       <NA>     <NA>          <NA>       NA   NA NA NA
## 2      2-6                              NA   NA NA NA
## 3      2-6                              NA   NA NA NA
## 4      4-4                              NA   NA NA NA
## 5     8-16                   <NA>       NA   NA NA NA
## 6       <NA>     <NA>          <NA>       NA   NA NA NA
```
</section>
<section data-markdown>
# Caveats to Web Scraping
- Keep in mind when scraping we are using their bandwidth
    - Do not want to repeatedly do expensive bandwidth operations
    - Better to scrape once, then run only to update data
- Some websites are copyrighted (i.e. illegal to scrape)
</section>
<section data-markdown>
# Data Modeling
- Research Questions
    1. Who is the next great coach?
    2. What characteristics are in common for these coaches?
</section>
<section data-markdown>
# IRT modeling
- So far we have explored the win/loss records of teams in the BCS era with item response theory (IRT)
- IRT is commonly used to model assessment data.
</section>
<section data-markdown>
# Questions
brandon-lebeau@uiowa.edu

</section>

    
