---
layout: slide
title: Web Scraping to Item Response Theory - A College Football Adventure
tags: [R, slides, rvest, cfb]
---

<section>
    <h1 class="title">Web Scraping to Item Response Theory: A College Football Adventure</h1>
    <h2 class="author">Brandon LeBeau, Andrew Zieffler, and Kyle Nickodem</h2>
    <h3 class="date">University of Iowa &amp; University of Minnesota</h3>
</section>


<section>
# Background
- Began after Tim Brewster was fired
- Wanted to try to predict next great coach

</section>

<section>
# Data Available
- Data is available at three levels
    1. Coach
    2. Game by Game
    3. Team

</section>

<section>    
# Coach
- Data
    - Overall record
    - Team history
- Not Available
    - Coordinator history

</section>

<section>    
# Game by Game
- Data
    - Final score of each game
    - Date played
    - Location
- Not Available
    - No information within a game

</section>

<section>
# Team
- Data
    - Overall team record
    - Team statistics
    - Rankings
    - Conference Affiliation
- Data is very similar to that of the coach level

</section>

<section>
# Web Scraping
- Data were obtained from many sources
    - Much from <http://cfbdatawarehouse.com>
    - Also used wikipedia, ESPN, and rivals

</section>

<section>
# Iowa Coaches Over Time
<img src="http://educate-r.org/figs/iowa.png" alt="" height = "500" width = "1200"/>

</section>

<section>
# Iowa State Coaches Over Time
<img src="http://educate-r.org/figs/iowa_state.png" alt="" height = "500" width="1200"/>

</section>

<section>
# Strengths in web scraping
- Data is relatively easily obtained
- Structured process for obtaining data
- Can be easily updated

</section>

<section>
# Challenges of web scraping
- At the mercy of the website
    - Many sites are old 
    - Not up to date on current design standards
- Data validation can be difficult and time consuming
- Need some basic knowledge of html

</section>

<section>
# When is Web Scraping Worthwhile?
- Best when scraping many pages
    - Particularly when web addresses are not structured
- Useful when data need to be updated
<hr>
- Not useful if only scraping a single page/table

</section>

<section>
# HTML Basics
<ul>
<li> HTML is structured by start tags (e.g. <code>&lt;table&gt;</code>) and end tags (e.g. <code>&lt;&frasl;table&gt;</code>) </li>
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
<li> Highly structured pages are the easiest to scrape </li>
</ul>
</section>

<section>
# HTML Code Example
<img src="http://educate-r.org/figs/ferentz_wikiside.png" alt="" height = "500" width="1200"/>
</section>

<section>
# Tools for web scraping
- R
    - `rvest`: <http://blog.rstudio.org/2014/11/24/rvest-easy-web-scraping-with-r/>
    - `XML`: <http://www.omegahat.org/RSXML/>
- Python
    - `beautiful soup`: <http://www.crummy.com/software/BeautifulSoup/>
- Misc
    - `SelectorGadget`: <http://selectorgadget.com/>

</section>

<section>    
# Basics of rvest
- `read_html` is the most basic function
- `html_node` or `html_nodes`
    - These functions need css selectors or xpath
    - SelectorGadget is the easiest way to get this

</section>

<section>
# SelectorGadget
- SelectorGadget is a Javascript addon for web browsers
- Can quickly identify a css selector or xpath to select correct portion of web page
- Demo:
    - <https://en.wikipedia.org/wiki/Kirk_Ferentz>

</section>

<section>
# Combine SelectorGadget with rvest

```r
library(rvest)
wiki_kirk <- read_html("https://en.wikipedia.org/wiki/Kirk_Ferentz")
wiki_kirk_extract <- wiki_kirk %>%
    html_nodes(".vcard td , .vcard th")
head(wiki_kirk_extract)
```


</section>

<section>
# Extract text
- Use the `html_text` function


```r
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_text()
head(wiki_kirk_extract)
```


</section>

<section>
# Encoding problems
- Two solutions to fix encoding problems
    - `guess_encoding`

    - `repair_encoding` to fix encoding problems

```r
wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_text() %>%
  guess_encoding()
```


</section>

<section>
# Fix Encoding Problems
- Best practice to reload page with correct encoding


```r
wiki_kirk <- read_html("https://en.wikipedia.org/wiki/Kirk_Ferentz", 
                       encoding = 'UTF-8')
```

- Can also repair encoding after the fact


```r
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_text() %>% 
  repair_encoding()
```
</section>

<section>
# Extract html tags
- Use the `html_tags` function


```r
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_name()
head(wiki_kirk_extract)
```


</section>

<section>
# Extract html attributes
- Use the `html_attrs` function


```r
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard td , .vcard th") %>%
  html_attrs()
head(wiki_kirk_extract)
```


</section>

<section>
# Extract links
- Use the `html_attrs` function again


```r
wiki_kirk_extract <- wiki_kirk %>%
  html_nodes(".vcard a") %>%
  html_attr('href')
head(wiki_kirk_extract)
```


</section>

<section>
# Valid Links
- The `paste0` function is helpful for this


```r
valid_links <- paste0('https://www.wikipedia.org', wiki_kirk_extract)
head(valid_links)
```


</section>

<section>
# Extract Tables
- The `html_table` function is useful to scrape well formatted tables


```r
record_kirk <- wiki_kirk %>%
  html_nodes(".wikitable") %>%
  .[[1]] %>%
  html_table(fill = TRUE)
```
</section>

<section>
# Caveats to Web Scraping
- Keep in mind when scraping we are using their bandwidth
    - Do not want to repeatedly do expensive bandwidth operations
    - Better to scrape once, then run only to update data
- Some websites are copyrighted (i.e. illegal to scrape)

</section>

<section>
# Data Modeling
- Research Questions
    1. Who is the next great coach?
    2. What characteristics are in common for these coaches?

</section>

<section>
# IRT modeling
- So far we have explored the win/loss records of teams in the BCS era with item response theory (IRT)
- IRT is commonly used to model assessment data to estimate item parameters and person 'ability'
- We recode the Win/Loss/Tie game by game results
    - 1 = Win 
    - 0 = Otherwise

</section>

<section>    
# Example code with lme4
- A 1 parameter multilevel IRT model can be fitted using `glmer` in the `lme4` package


```r
library(lme4)
fm1a <- glmer(wingbg ~ 0 + (1|coach) + (1|Team), 
              data = yby_coach, family = binomial)
```
</section>

<section>
# Plot Showing Team Ability
<img src="http://educate-r.org/figs/team_ability.png" alt="" height = "500" width="1200"/>


</section>

<section>


# Connect
- e-mail: brandon-lebeau (at) uiowa.edu
- Twitter: @blebeau11; <https://twitter.com/blebeau11>
- Linkedin: <https://www.linkedin.com/in/lebeaubr>
- Website: <http://educate-r.org>
    - <http://educate-r.org/2015/12/04/centraliowaruser/>

</section>
