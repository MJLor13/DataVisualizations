Project 1 - `CO2 Emissions`
================
MJ Lorino
2025-03-20

### Introduction

As we know, CO2 emissions is one of the leading causes of global
warming, a greenhouse gas that traps heat from leaving the earth’s
atmosphere and in turn increasing the temperature of the climate. One
may be curious about our country’s emissions, seeing as our climate
situation is getting dire, and questions the nationwide impact that is
caused.

The data used in this visualization may be useful in pinpointing where
we need to focus clean energy efforts, creating a priority list and a
visual representation of the environmental impact of each state. A clear
lineup of which states have the most impact will lead us to further
question the reasons why the emissions are so high, which can lead to
answers and potentially solutions.

### Data Source

My data set comes from Kaggle, from a notebook titled “CO2 Emissions”.
This data set provided information about each state’s various carbon
emissions (in million tons) per year from 1970 to 2021. This data set is
focused on giving us the specific emission type, whether it’s derived
from coal, natural gas, petroleum, or all three. It also gives us
information about how they’re used, either in transportation,
commercial, residential, electrical use, etc.. Each variable gives us
these specifics so that we can see the difference between each state’s
cause for their carbon emissions, and which fuels they use, and with
that information we can start to come up with alternative, clean energy
replacements, that may be possible.

### Methods

Firstly, the a new dataframe was defined as ‘dfallf’, which includes the
following code:

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
df<-read.csv("emissions.csv")

dfallf<-df%>%
  filter(`fuel.name`=='All Fuels')%>%
  filter(`sector.name`=='Total carbon dioxide emissions from all sectors')%>%
  group_by(`state.name`,`year`)%>%
  filter(!`state.name`=="United States")
```

Essentially filtering our initial df to give us our desired inputs,
which gives us a clear list of the total carbon emissions, all from the
collective fuels used each year for each state. Afterwards, I created a
“state list”, consisting of an arranged list of the states from the
final year 2021. With this list I found the top 5 states who had the
highest carbon emissions and started my code for the visualization. I
used a line plot, the principles used being position and color to show
the viewers which states are emitting how much CO2 throughout the years.
Here is the code:

``` r
dfallf%>%
  filter(`state.name` %in% c("Ohio", "Pennsylvania","Florida",
  "California", "Texas"))%>%
ggplot()+
  geom_line(aes(x=`year`,color=`state.name`, y=`value`), linewidth=1.3)+
  labs(title="Carbon Emissions from All Fuels", x="Year", y="Carbon Emissions (million tons)",
       color="States")+
  theme_classic()
```

![](Project-1---%60CO2-Emissions%60_files/figure-gfm/unnamed-chunk-2-1.png)<!-- -->

The data was filtered so that the only states that would appear would be
the top 5 in the state list, allowing us to compare these states more
clearly. Then, the line plot was coded above, defining the line plot,
giving appropriate labels to each axis, legend, and the title, and using
the classic theme to help with visual clarity.

### Results

The top 5 states causing the most CO2 emissions from all fuels used from
all sectors (Transportation, commercial, residential, etc.) of fuel use
is shown above. We can see some general trends, like the slight rise and
fall around the 1980s, a similar but bigger trend taking place in the
2000s, falling at around the 2010s, and lastly a small dip in 2020,
which we all can assume is from the Covid-19 pandemic, and the decrease
of productivity outside as we quarantined inside.

### Conclusions

As the world’s population continues to grow, so do our needs, and with
increased needs comes an increase in cost. Texas remains uncontested in
its insanely massive amounts of carbon emissions even compared to the
states only second to fifth in the standing. We can infer that the
states of Ohio and Pennsylvania may have very similar needs for these
fuels, which may explain their very close totals throughout the years,
as well as Florida’s as its total rose and matches their lines after the
year 2000.

Though this graph shows only the totals of everything, the data set has
much more data to showcase more specific cases, focusing on either
different fuels used or the sectors that fuel is spent on, or both.
Going in depth and creating more specific visualizations may allow us to
more easily investigate what is causing all of these carbon emissions,
and potentially find a way to curb it with what we can do that’s within
our reach.
