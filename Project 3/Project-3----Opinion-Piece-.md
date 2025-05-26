Project 3 - ‘Opinion Piece’
================
MJ Lorino
2025-05-05

### Introduction

For this project, I chose an opinion piece from The New York Times
entitled “I Have Never Been More Afraid for My Country’s Future”. This
opinion piece highlights current events and the various issues the
author has about how the country is handling major issues. Though the
piece does not directly have graphs or any other visualizations present,
it does reference the data being shown, and luckily it was easy to find
the sources for those. I will be recreating these visualizations below
accompanied with a chunk of the author’s writings explaining the data.

NYT Link:
<https://www.nytimes.com/2025/04/15/opinion/trump-administration-china.html>

### Summary

Thomas L. Friedman writes about his opinions, observations, fears, and
questions about the current state of the US, its presidency, and the
public opinion of not only the American people, but of other countries
as well. He highlights the fact that there is a lot of important
information being missed and overlooked through the noise being created
by the chaos of the Trump administration, including Trump’s executive
orders bolstering the return of coal mining while trying to remove green
energy jobs from the budget, launching trade wars using tariffs, and the
dwindling trust from America’s allies. Friedman lays out the
consequences, and also compares the state of the country to China in
multiple facets including environmental and economical choices.
Ultimately, Friedman expresses his concerns about the choices that our
President is making, our reputation as a country, about what is
happening to the country now, and its future.

### Visualizations

#### Visual \#1:

“…coal miners in hard hats, members of a work force that has declined to
about 40,000 from 70,000 over the last decade, according to Reuters.”

``` r
library(tidyverse)
```

    ## ── Attaching core tidyverse packages ──────────────────────── tidyverse 2.0.0 ──
    ## ✔ dplyr     1.1.4     ✔ readr     2.1.5
    ## ✔ forcats   1.0.0     ✔ stringr   1.5.1
    ## ✔ ggplot2   3.5.1     ✔ tibble    3.2.1
    ## ✔ lubridate 1.9.4     ✔ tidyr     1.3.1
    ## ✔ purrr     1.0.4     
    ## ── Conflicts ────────────────────────────────────────── tidyverse_conflicts() ──
    ## ✖ dplyr::filter() masks stats::filter()
    ## ✖ dplyr::lag()    masks stats::lag()
    ## ℹ Use the conflicted package (<http://conflicted.r-lib.org/>) to force all conflicts to become errors

``` r
library(readr)

coal <- read_csv("Coal Mining Jobs.csv")
```

    ## Rows: 483 Columns: 2
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## dbl  (1): CES1021210001
    ## date (1): observation_date
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
coal$observation_date<-as.Date(coal$observation_date, '%Y-%d-%m')

ggplot(coal)+
  geom_line(aes(x=observation_date, y=CES1021210001), color="blue", linewidth=1)+
  theme(text=element_text(color = "black", size = 14, family="serif"),
        rect = element_rect(color = "lightblue", fill = "lightblue"),
        panel.border = element_rect(color="darkgray",fill=NA,linewidth = 1.5),
        panel.background = element_rect(fill="white"),
        panel.grid = element_line(color="gray"),
        panel.grid.minor.x = element_blank(),
        panel.grid.major.x = element_blank(),
        panel.grid.minor.y = element_blank())+
  labs(x="",y="Thousands of persons", title= "All Employees, Coal Mining",
       caption = "Shaded areas indicate US Recessions")+
  scale_y_continuous(breaks=c(20,40,60,80,100,120,140,160,180))+
  annotate("rect",xmin=as.Date("1990-07-01"),xmax=as.Date("1991-03-01"),ymin=-Inf, ymax=Inf,color="darkgray", fill="gray",alpha=0.3)+
  annotate("rect",xmin=as.Date("2001-03-01"),xmax=as.Date("2001-11-01"),ymin=-Inf, ymax=Inf,color="darkgray", fill="gray",alpha=0.3)+
  annotate("rect",xmin=as.Date("2007-12-01"),xmax=as.Date("2009-06-01"),ymin=-Inf, ymax=Inf,color="darkgray", fill="gray",alpha=0.3)+
  annotate("rect",xmin=as.Date("2020-02-01"),xmax=as.Date("2020-04-01"),ymin=-Inf, ymax=Inf,color="darkgray", fill="gray",alpha=0.3)
```

![](Project-3----Opinion-Piece-_files/figure-gfm/unnamed-chunk-1-1.png)<!-- -->

#### Visual \#2:

“The most recent Nature Index shows that China has become “the leading
country globally for research output in the database in chemistry, earth
and environmental sciences and physical sciences, and is second for
biological sciences and health sciences.””

``` r
impact<-read_csv("Impact Report.csv")
```

    ## Rows: 5 Columns: 5
    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr (2): Country, Total
    ## dbl (3): Top 1%, Top 2%, Top 10%
    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

``` r
view(impact)

newimpact<-pivot_longer(impact, cols=c("Top 1%","Top 2%","Top 10%"),
                        names_to = "Percentage Value", 
                        values_to = "Number of Highly Cited Publications")

ggplot(newimpact)+
  geom_col(aes(y=`Number of Highly Cited Publications`, x=`Percentage Value`,
               fill=`Percentage Value`), stat="identity")+
  geom_text(aes(y=`Number of Highly Cited Publications`/50, 
                x=`Percentage Value`, label=`Number of Highly Cited Publications`),
            position=position_identity())+
  coord_radial(theta="y", start=-pi/2, end=pi/2, inner.radius = 0.3)+
  facet_wrap(~`Country`)+
  scale_color_brewer(palette="Set2")+
  scale_y_log10()+
  theme_void()
```

    ## Warning in geom_col(aes(y = `Number of Highly Cited Publications`, x =
    ## `Percentage Value`, : Ignoring unknown parameters: `stat`

![](Project-3----Opinion-Piece-_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

#### Visual \#3:

“Trump is triggering a serious loss of global confidence in America.”

``` r
library(haven)

pew <- read_sav("Pew.sav")

newpew<-pew%>%
  select(c("BIDEN_PERSONALITY_QUALIFIED","BIDEN_PERSONALITY_STRONG",
  "BIDEN_PERSONALITY_DANGEROUS","BIDEN_PERSONALITY_ARROGANT","SEX","AGE",
  "ID",))%>%
  pivot_longer(cols=c("BIDEN_PERSONALITY_QUALIFIED",
                              "BIDEN_PERSONALITY_STRONG",
                              "BIDEN_PERSONALITY_DANGEROUS",
                              "BIDEN_PERSONALITY_ARROGANT"),
                        names_to = "Questions", 
                        values_to = "Answers")%>%
  filter(Answers<=3)
  

Qs <- c(`BIDEN_PERSONALITY_QUALIFIED` = "Well-Qulified",
        `BIDEN_PERSONALITY_STRONG` = "A Strong Leader",
        `BIDEN_PERSONALITY_DANGEROUS` = "Dangerous",
        `BIDEN_PERSONALITY_ARROGANT` = "Arrogant")

ggplot(newpew)+
  geom_bar(aes(y=`Answers`, x=`Answers`, fill=`Questions`), stat="identity",
           show.legend = FALSE)+
  facet_grid(~`Questions`, labeller = as_labeller(Qs))+
  theme(text=element_text(color = "black", size = 11),
        panel.border = element_rect(color=NA,fill=NA,linewidth = 3),
        panel.background = element_rect(fill="white"))+
  labs(x="",y="", 
       title="Biden Gets Higher Ratings than Trump on Personal, Leadership Traits")+
  scale_x_continuous(breaks=c(1,2), labels=c("Biden","Trump"))+
  scale_color_brewer(palette = "Set1")
```

![](Project-3----Opinion-Piece-_files/figure-gfm/unnamed-chunk-3-1.png)<!-- -->
