---
title: 'Weekly Exercises #5'
author: "Ashley Vargas"
output: 
  html_document:
    keep_md: TRUE
    toc: TRUE
    toc_float: TRUE
    df_print: paged
    code_download: true
---





```r
library(tidyverse)     # for data cleaning and plotting
library(googlesheets4) # for reading googlesheet data
library(lubridate)     # for date manipulation
library(openintro)     # for the abbr2state() function
library(palmerpenguins)# for Palmer penguin data
library(maps)          # for map data
library(ggmap)         # for mapping points on maps
library(gplots)        # for col2hex() function
library(RColorBrewer)  # for color palettes
library(sf)            # for working with spatial data
library(leaflet)       # for highly customizable mapping
library(ggthemes)      # for more themes (including theme_map())
library(plotly)        # for the ggplotly() - basic interactivity
library(gganimate)     # for adding animation layers to ggplots
library(transformr)    # for "tweening" (gganimate)
library(shiny)         # for creating interactive apps
library(babynames)     # for using the babynames dataset
library(gifski)
library(ggimage)
gs4_deauth()           # To not have to authorize each time you knit.
theme_set(theme_minimal())
```



```r
# SNCF Train data
small_trains <- read_csv("https://raw.githubusercontent.com/rfordatascience/tidytuesday/master/data/2019/2019-02-26/small_trains.csv") 

# Lisa's garden data
garden_harvest <- read_sheet("https://docs.google.com/spreadsheets/d/1DekSazCzKqPS2jnGhKue7tLxRU3GVL1oxi-4bEM5IWw/edit?usp=sharing") %>% 
  mutate(date = ymd(date))

# Lisa's Mallorca cycling data
mallorca_bike_day7 <- read_csv("https://www.dropbox.com/s/zc6jan4ltmjtvy0/mallorca_bike_day7.csv?dl=1") %>% 
  select(1:4, speed)

# Heather Lendway's Ironman 70.3 Pan Am championships Panama data
panama_swim <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_swim_20160131.csv")

panama_bike <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_bike_20160131.csv")

panama_run <- read_csv("https://raw.githubusercontent.com/llendway/gps-data/master/data/panama_run_20160131.csv")

#COVID-19 data from the New York Times
covid19 <- read_csv("https://raw.githubusercontent.com/nytimes/covid-19-data/master/us-states.csv")
```

## Put your homework on GitHub!

Go [here](https://github.com/llendway/github_for_collaboration/blob/master/github_for_collaboration.md) or to previous homework to remind yourself how to get set up. 

Once your repository is created, you should always open your **project** rather than just opening an .Rmd file. You can do that by either clicking on the .Rproj file in your repository folder on your computer. Or, by going to the upper right hand corner in R Studio and clicking the arrow next to where it says Project: (None). You should see your project come up in that list if you've used it recently. You could also go to File --> Open Project and navigate to your .Rproj file. 

## Instructions

* Put your name at the top of the document. 

* **For ALL graphs, you should include appropriate labels.** 

* Feel free to change the default theme, which I currently have set to `theme_minimal()`. 

* Use good coding practice. Read the short sections on good code with [pipes](https://style.tidyverse.org/pipes.html) and [ggplot2](https://style.tidyverse.org/ggplot2.html). **This is part of your grade!**

* **NEW!!** With animated graphs, add `eval=FALSE` to the code chunk that creates the animation and saves it using `anim_save()`. Add another code chunk to reread the gif back into the file. See the [tutorial](https://animation-and-interactivity-in-r.netlify.app/) for help. 

* When you are finished with ALL the exercises, uncomment the options at the top so your document looks nicer. Don't do it before then, or else you might miss some important warnings and messages.

## Warm-up exercises from tutorial

  1. Choose 2 graphs you have created for ANY assignment in this class and add interactivity using the `ggplotly()` function.


```r
data_site <- 
  "https://www.macalester.edu/~dshuman1/data/112/2014-Q4-Trips-History-Data.rds" 
Trips <- readRDS(gzcon(url(data_site)))
```
  

```r
Bike_Rentals <- Trips %>% 
  mutate(day_of_week = wday(sdate, label = TRUE)) %>% 
  mutate(time_of_day = hour(sdate) + (minute(sdate)/60)) %>% 
  ggplot(aes(x = time_of_day, fill = client)) +
  geom_density(color=NA, alpha = 0.5, position = position_stack(),
               text = density) +
  facet_wrap(~ day_of_week) +
  labs(title = "Bike Rentals Over Time of Day",
      x= "Time of Day",  
      y = "Density") 


ggplotly(Bike_Rentals)
```

<!--html_preserve--><div id="htmlwidget-c1a7ecbdc985d3133c30" style="width:672px;height:480px;" class="plotly html-widget"></div>




```r
unique_babynames <- babynames %>% 
  group_by(year, sex) %>% 
  summarize(distinct_names = length(unique(name))) %>% 
  ggplot(aes(x=year, y=distinct_names, color = sex)) +
  geom_line() +
   labs(title = "Number of Distinct Names By Year", 
      y= "Number of Distinct Names", 
      x = "Year") 

ggplotly(unique_babynames, 
         tooltip = c("text", "x", "y"))
```

<!--html_preserve--><div id="htmlwidget-c5e2def33b1c267ad319" style="width:672px;height:480px;" class="plotly html-widget"></div>
<script type="application/json" data-for="htmlwidget-c5e2def33b1c267ad319">{"x":{"data":[{"x":[1880,1881,1882,1883,1884,1885,1886,1887,1888,1889,1890,1891,1892,1893,1894,1895,1896,1897,1898,1899,1900,1901,1902,1903,1904,1905,1906,1907,1908,1909,1910,1911,1912,1913,1914,1915,1916,1917,1918,1919,1920,1921,1922,1923,1924,1925,1926,1927,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[942,938,1028,1054,1172,1197,1282,1306,1474,1479,1534,1533,1661,1652,1702,1808,1825,1799,1975,1842,2224,1943,2042,2083,2165,2234,2220,2399,2434,2548,2790,2868,3445,3707,4206,4967,5162,5312,5586,5559,5765,5871,5789,5739,5899,5771,5621,5603,5436,5275,5248,4977,5100,4858,4973,4892,4856,4927,4994,4952,5025,5085,5380,5368,5245,5241,5686,6103,6040,6065,6111,6211,6391,6499,6616,6725,6885,7012,7022,7196,7331,7529,7583,7663,7803,7533,7616,7849,8194,8708,9350,9638,9661,9805,10239,10609,10900,11324,11470,11967,12157,12186,12327,12065,12171,12500,12823,13255,13877,14546,15235,15459,15611,15797,15751,15753,15891,16160,16598,16941,17653,17970,18081,18430,18826,19182,20050,20560,20457,20179,19811,19560,19498,19231,19181,19074,18817,18309],"text":["year: 1880<br />distinct_names:   942","year: 1881<br />distinct_names:   938","year: 1882<br />distinct_names:  1028","year: 1883<br />distinct_names:  1054","year: 1884<br />distinct_names:  1172","year: 1885<br />distinct_names:  1197","year: 1886<br />distinct_names:  1282","year: 1887<br />distinct_names:  1306","year: 1888<br />distinct_names:  1474","year: 1889<br />distinct_names:  1479","year: 1890<br />distinct_names:  1534","year: 1891<br />distinct_names:  1533","year: 1892<br />distinct_names:  1661","year: 1893<br />distinct_names:  1652","year: 1894<br />distinct_names:  1702","year: 1895<br />distinct_names:  1808","year: 1896<br />distinct_names:  1825","year: 1897<br />distinct_names:  1799","year: 1898<br />distinct_names:  1975","year: 1899<br />distinct_names:  1842","year: 1900<br />distinct_names:  2224","year: 1901<br />distinct_names:  1943","year: 1902<br />distinct_names:  2042","year: 1903<br />distinct_names:  2083","year: 1904<br />distinct_names:  2165","year: 1905<br />distinct_names:  2234","year: 1906<br />distinct_names:  2220","year: 1907<br />distinct_names:  2399","year: 1908<br />distinct_names:  2434","year: 1909<br />distinct_names:  2548","year: 1910<br />distinct_names:  2790","year: 1911<br />distinct_names:  2868","year: 1912<br />distinct_names:  3445","year: 1913<br />distinct_names:  3707","year: 1914<br />distinct_names:  4206","year: 1915<br />distinct_names:  4967","year: 1916<br />distinct_names:  5162","year: 1917<br />distinct_names:  5312","year: 1918<br />distinct_names:  5586","year: 1919<br />distinct_names:  5559","year: 1920<br />distinct_names:  5765","year: 1921<br />distinct_names:  5871","year: 1922<br />distinct_names:  5789","year: 1923<br />distinct_names:  5739","year: 1924<br />distinct_names:  5899","year: 1925<br />distinct_names:  5771","year: 1926<br />distinct_names:  5621","year: 1927<br />distinct_names:  5603","year: 1928<br />distinct_names:  5436","year: 1929<br />distinct_names:  5275","year: 1930<br />distinct_names:  5248","year: 1931<br />distinct_names:  4977","year: 1932<br />distinct_names:  5100","year: 1933<br />distinct_names:  4858","year: 1934<br />distinct_names:  4973","year: 1935<br />distinct_names:  4892","year: 1936<br />distinct_names:  4856","year: 1937<br />distinct_names:  4927","year: 1938<br />distinct_names:  4994","year: 1939<br />distinct_names:  4952","year: 1940<br />distinct_names:  5025","year: 1941<br />distinct_names:  5085","year: 1942<br />distinct_names:  5380","year: 1943<br />distinct_names:  5368","year: 1944<br />distinct_names:  5245","year: 1945<br />distinct_names:  5241","year: 1946<br />distinct_names:  5686","year: 1947<br />distinct_names:  6103","year: 1948<br />distinct_names:  6040","year: 1949<br />distinct_names:  6065","year: 1950<br />distinct_names:  6111","year: 1951<br />distinct_names:  6211","year: 1952<br />distinct_names:  6391","year: 1953<br />distinct_names:  6499","year: 1954<br />distinct_names:  6616","year: 1955<br />distinct_names:  6725","year: 1956<br />distinct_names:  6885","year: 1957<br />distinct_names:  7012","year: 1958<br />distinct_names:  7022","year: 1959<br />distinct_names:  7196","year: 1960<br />distinct_names:  7331","year: 1961<br />distinct_names:  7529","year: 1962<br />distinct_names:  7583","year: 1963<br />distinct_names:  7663","year: 1964<br />distinct_names:  7803","year: 1965<br />distinct_names:  7533","year: 1966<br />distinct_names:  7616","year: 1967<br />distinct_names:  7849","year: 1968<br />distinct_names:  8194","year: 1969<br />distinct_names:  8708","year: 1970<br />distinct_names:  9350","year: 1971<br />distinct_names:  9638","year: 1972<br />distinct_names:  9661","year: 1973<br />distinct_names:  9805","year: 1974<br />distinct_names: 10239","year: 1975<br />distinct_names: 10609","year: 1976<br />distinct_names: 10900","year: 1977<br />distinct_names: 11324","year: 1978<br />distinct_names: 11470","year: 1979<br />distinct_names: 11967","year: 1980<br />distinct_names: 12157","year: 1981<br />distinct_names: 12186","year: 1982<br />distinct_names: 12327","year: 1983<br />distinct_names: 12065","year: 1984<br />distinct_names: 12171","year: 1985<br />distinct_names: 12500","year: 1986<br />distinct_names: 12823","year: 1987<br />distinct_names: 13255","year: 1988<br />distinct_names: 13877","year: 1989<br />distinct_names: 14546","year: 1990<br />distinct_names: 15235","year: 1991<br />distinct_names: 15459","year: 1992<br />distinct_names: 15611","year: 1993<br />distinct_names: 15797","year: 1994<br />distinct_names: 15751","year: 1995<br />distinct_names: 15753","year: 1996<br />distinct_names: 15891","year: 1997<br />distinct_names: 16160","year: 1998<br />distinct_names: 16598","year: 1999<br />distinct_names: 16941","year: 2000<br />distinct_names: 17653","year: 2001<br />distinct_names: 17970","year: 2002<br />distinct_names: 18081","year: 2003<br />distinct_names: 18430","year: 2004<br />distinct_names: 18826","year: 2005<br />distinct_names: 19182","year: 2006<br />distinct_names: 20050","year: 2007<br />distinct_names: 20560","year: 2008<br />distinct_names: 20457","year: 2009<br />distinct_names: 20179","year: 2010<br />distinct_names: 19811","year: 2011<br />distinct_names: 19560","year: 2012<br />distinct_names: 19498","year: 2013<br />distinct_names: 19231","year: 2014<br />distinct_names: 19181","year: 2015<br />distinct_names: 19074","year: 2016<br />distinct_names: 18817","year: 2017<br />distinct_names: 18309"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(248,118,109,1)","dash":"solid"},"hoveron":"points","name":"F","legendgroup":"F","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null},{"x":[1880,1881,1882,1883,1884,1885,1886,1887,1888,1889,1890,1891,1892,1893,1894,1895,1896,1897,1898,1899,1900,1901,1902,1903,1904,1905,1906,1907,1908,1909,1910,1911,1912,1913,1914,1915,1916,1917,1918,1919,1920,1921,1922,1923,1924,1925,1926,1927,1928,1929,1930,1931,1932,1933,1934,1935,1936,1937,1938,1939,1940,1941,1942,1943,1944,1945,1946,1947,1948,1949,1950,1951,1952,1953,1954,1955,1956,1957,1958,1959,1960,1961,1962,1963,1964,1965,1966,1967,1968,1969,1970,1971,1972,1973,1974,1975,1976,1977,1978,1979,1980,1981,1982,1983,1984,1985,1986,1987,1988,1989,1990,1991,1992,1993,1994,1995,1996,1997,1998,1999,2000,2001,2002,2003,2004,2005,2006,2007,2008,2009,2010,2011,2012,2013,2014,2015,2016,2017],"y":[1058,997,1099,1030,1125,1097,1110,1067,1177,1111,1161,1127,1260,1179,1239,1241,1266,1229,1289,1200,1506,1210,1320,1306,1395,1421,1413,1549,1584,1679,1839,1999,2906,3261,3759,4390,4534,4602,4813,4810,4990,4986,4967,4904,4970,4867,4837,4803,4724,4543,4541,4320,4284,4154,4207,4145,4037,4019,4036,3967,3936,4000,4045,4040,3909,3783,4019,4268,4199,4203,4191,4251,4257,4339,4352,4388,4450,4552,4499,4573,4590,4652,4623,4619,4593,4420,4536,4550,4741,5040,5430,5655,5750,5876,6005,6335,6491,6851,6759,7071,7288,7286,7364,7338,7332,7581,7826,8148,8487,9227,9484,9646,9816,10169,10244,10327,10532,10811,11301,11609,12116,12299,12482,12753,13220,13364,14032,14390,14613,14523,14256,14343,14234,14038,14047,14024,14162,14160],"text":["year: 1880<br />distinct_names:  1058","year: 1881<br />distinct_names:   997","year: 1882<br />distinct_names:  1099","year: 1883<br />distinct_names:  1030","year: 1884<br />distinct_names:  1125","year: 1885<br />distinct_names:  1097","year: 1886<br />distinct_names:  1110","year: 1887<br />distinct_names:  1067","year: 1888<br />distinct_names:  1177","year: 1889<br />distinct_names:  1111","year: 1890<br />distinct_names:  1161","year: 1891<br />distinct_names:  1127","year: 1892<br />distinct_names:  1260","year: 1893<br />distinct_names:  1179","year: 1894<br />distinct_names:  1239","year: 1895<br />distinct_names:  1241","year: 1896<br />distinct_names:  1266","year: 1897<br />distinct_names:  1229","year: 1898<br />distinct_names:  1289","year: 1899<br />distinct_names:  1200","year: 1900<br />distinct_names:  1506","year: 1901<br />distinct_names:  1210","year: 1902<br />distinct_names:  1320","year: 1903<br />distinct_names:  1306","year: 1904<br />distinct_names:  1395","year: 1905<br />distinct_names:  1421","year: 1906<br />distinct_names:  1413","year: 1907<br />distinct_names:  1549","year: 1908<br />distinct_names:  1584","year: 1909<br />distinct_names:  1679","year: 1910<br />distinct_names:  1839","year: 1911<br />distinct_names:  1999","year: 1912<br />distinct_names:  2906","year: 1913<br />distinct_names:  3261","year: 1914<br />distinct_names:  3759","year: 1915<br />distinct_names:  4390","year: 1916<br />distinct_names:  4534","year: 1917<br />distinct_names:  4602","year: 1918<br />distinct_names:  4813","year: 1919<br />distinct_names:  4810","year: 1920<br />distinct_names:  4990","year: 1921<br />distinct_names:  4986","year: 1922<br />distinct_names:  4967","year: 1923<br />distinct_names:  4904","year: 1924<br />distinct_names:  4970","year: 1925<br />distinct_names:  4867","year: 1926<br />distinct_names:  4837","year: 1927<br />distinct_names:  4803","year: 1928<br />distinct_names:  4724","year: 1929<br />distinct_names:  4543","year: 1930<br />distinct_names:  4541","year: 1931<br />distinct_names:  4320","year: 1932<br />distinct_names:  4284","year: 1933<br />distinct_names:  4154","year: 1934<br />distinct_names:  4207","year: 1935<br />distinct_names:  4145","year: 1936<br />distinct_names:  4037","year: 1937<br />distinct_names:  4019","year: 1938<br />distinct_names:  4036","year: 1939<br />distinct_names:  3967","year: 1940<br />distinct_names:  3936","year: 1941<br />distinct_names:  4000","year: 1942<br />distinct_names:  4045","year: 1943<br />distinct_names:  4040","year: 1944<br />distinct_names:  3909","year: 1945<br />distinct_names:  3783","year: 1946<br />distinct_names:  4019","year: 1947<br />distinct_names:  4268","year: 1948<br />distinct_names:  4199","year: 1949<br />distinct_names:  4203","year: 1950<br />distinct_names:  4191","year: 1951<br />distinct_names:  4251","year: 1952<br />distinct_names:  4257","year: 1953<br />distinct_names:  4339","year: 1954<br />distinct_names:  4352","year: 1955<br />distinct_names:  4388","year: 1956<br />distinct_names:  4450","year: 1957<br />distinct_names:  4552","year: 1958<br />distinct_names:  4499","year: 1959<br />distinct_names:  4573","year: 1960<br />distinct_names:  4590","year: 1961<br />distinct_names:  4652","year: 1962<br />distinct_names:  4623","year: 1963<br />distinct_names:  4619","year: 1964<br />distinct_names:  4593","year: 1965<br />distinct_names:  4420","year: 1966<br />distinct_names:  4536","year: 1967<br />distinct_names:  4550","year: 1968<br />distinct_names:  4741","year: 1969<br />distinct_names:  5040","year: 1970<br />distinct_names:  5430","year: 1971<br />distinct_names:  5655","year: 1972<br />distinct_names:  5750","year: 1973<br />distinct_names:  5876","year: 1974<br />distinct_names:  6005","year: 1975<br />distinct_names:  6335","year: 1976<br />distinct_names:  6491","year: 1977<br />distinct_names:  6851","year: 1978<br />distinct_names:  6759","year: 1979<br />distinct_names:  7071","year: 1980<br />distinct_names:  7288","year: 1981<br />distinct_names:  7286","year: 1982<br />distinct_names:  7364","year: 1983<br />distinct_names:  7338","year: 1984<br />distinct_names:  7332","year: 1985<br />distinct_names:  7581","year: 1986<br />distinct_names:  7826","year: 1987<br />distinct_names:  8148","year: 1988<br />distinct_names:  8487","year: 1989<br />distinct_names:  9227","year: 1990<br />distinct_names:  9484","year: 1991<br />distinct_names:  9646","year: 1992<br />distinct_names:  9816","year: 1993<br />distinct_names: 10169","year: 1994<br />distinct_names: 10244","year: 1995<br />distinct_names: 10327","year: 1996<br />distinct_names: 10532","year: 1997<br />distinct_names: 10811","year: 1998<br />distinct_names: 11301","year: 1999<br />distinct_names: 11609","year: 2000<br />distinct_names: 12116","year: 2001<br />distinct_names: 12299","year: 2002<br />distinct_names: 12482","year: 2003<br />distinct_names: 12753","year: 2004<br />distinct_names: 13220","year: 2005<br />distinct_names: 13364","year: 2006<br />distinct_names: 14032","year: 2007<br />distinct_names: 14390","year: 2008<br />distinct_names: 14613","year: 2009<br />distinct_names: 14523","year: 2010<br />distinct_names: 14256","year: 2011<br />distinct_names: 14343","year: 2012<br />distinct_names: 14234","year: 2013<br />distinct_names: 14038","year: 2014<br />distinct_names: 14047","year: 2015<br />distinct_names: 14024","year: 2016<br />distinct_names: 14162","year: 2017<br />distinct_names: 14160"],"type":"scatter","mode":"lines","line":{"width":1.88976377952756,"color":"rgba(0,191,196,1)","dash":"solid"},"hoveron":"points","name":"M","legendgroup":"M","showlegend":true,"xaxis":"x","yaxis":"y","hoverinfo":"text","frame":null}],"layout":{"margin":{"t":43.7625570776256,"r":7.30593607305936,"b":40.1826484018265,"l":54.7945205479452},"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"title":{"text":"Number of Distinct Names By Year","font":{"color":"rgba(0,0,0,1)","family":"","size":17.5342465753425},"x":0,"xref":"paper"},"xaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[1873.15,2023.85],"tickmode":"array","ticktext":["1880","1920","1960","2000"],"tickvals":[1880,1920,1960,2000],"categoryorder":"array","categoryarray":["1880","1920","1960","2000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"y","title":{"text":"Year","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"yaxis":{"domain":[0,1],"automargin":true,"type":"linear","autorange":false,"range":[-43.1,21541.1],"tickmode":"array","ticktext":["0","5000","10000","15000","20000"],"tickvals":[-7.105427357601e-15,5000,10000,15000,20000],"categoryorder":"array","categoryarray":["0","5000","10000","15000","20000"],"nticks":null,"ticks":"","tickcolor":null,"ticklen":3.65296803652968,"tickwidth":0,"showticklabels":true,"tickfont":{"color":"rgba(77,77,77,1)","family":"","size":11.689497716895},"tickangle":-0,"showline":false,"linecolor":null,"linewidth":0,"showgrid":true,"gridcolor":"rgba(235,235,235,1)","gridwidth":0.66417600664176,"zeroline":false,"anchor":"x","title":{"text":"Number of Distinct Names","font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187}},"hoverformat":".2f"},"shapes":[{"type":"rect","fillcolor":null,"line":{"color":null,"width":0,"linetype":[]},"yref":"paper","xref":"paper","x0":0,"x1":1,"y0":0,"y1":1}],"showlegend":true,"legend":{"bgcolor":null,"bordercolor":null,"borderwidth":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":11.689497716895},"y":0.913385826771654},"annotations":[{"text":"sex","x":1.02,"y":1,"showarrow":false,"ax":0,"ay":0,"font":{"color":"rgba(0,0,0,1)","family":"","size":14.6118721461187},"xref":"paper","yref":"paper","textangle":-0,"xanchor":"left","yanchor":"bottom","legendTitle":true}],"hovermode":"closest","barmode":"relative"},"config":{"doubleClick":"reset","showSendToCloud":false},"source":"A","attrs":{"abb25a1f35de":{"x":{},"y":{},"colour":{},"type":"scatter"}},"cur_data":"abb25a1f35de","visdat":{"abb25a1f35de":["function (y) ","x"]},"highlight":{"on":"plotly_click","persistent":false,"dynamic":false,"selectize":false,"opacityDim":0.2,"selected":{"opacity":1},"debounce":0},"shinyEvents":["plotly_hover","plotly_click","plotly_selected","plotly_relayout","plotly_brushed","plotly_brushing","plotly_clickannotation","plotly_doubleclick","plotly_deselect","plotly_afterplot","plotly_sunburstclick"],"base_url":"https://plot.ly"},"evals":[],"jsHooks":[]}</script><!--/html_preserve-->
  
  2. Use animation to tell an interesting story with the `small_trains` dataset that contains data from the SNCF (National Society of French Railways). These are Tidy Tuesday data! Read more about it [here](https://github.com/rfordatascience/tidytuesday/tree/master/data/2019/2019-02-26).


```r
small_trains %>% 
  filter(service == "International") %>% 
  group_by(month) %>%
  summarise(meantrips = mean(total_num_trips))%>% 
  ggplot(aes(x=month, y=meantrips)) +
  geom_bar(stat="identity") +
  transition_states(month) +
  labs(title = "Trains",
       subtitle = "Month: {closest_state}")
```



```r
anim_save("trains1.gif")
```


```r
knitr::include_graphics("trains1.gif")
```




This graph looks at international trips and shows the number of trips across different months. We can see the number of trips is stable and similar across the year, except during November and December, where the number of trips decrease. 



## Garden data

  3. In this exercise, you will create a stacked area plot that reveals itself over time (see the `geom_area()` examples [here](https://ggplot2.tidyverse.org/reference/position_stack.html)). You will look at cumulative harvest of tomato varieties over time. You should do the following:
  * From the `garden_harvest` data, filter the data to the tomatoes and find the *daily* harvest in pounds for each variety.  
  * Then, for each variety, find the cumulative harvest in pounds.  
  * Use the data you just made to create a static cumulative harvest area plot, with the areas filled with different colors for each vegetable and arranged (HINT: `fct_reorder()`) from most to least harvested (most on the bottom).  
  * Add animation to reveal the plot over date. 
  
 
  

```r
garden_harvest %>% 
  filter(vegetable == "tomatoes") %>% 
  complete(variety, date = seq.Date(min(date), max(date), by = "day")) %>% 
  select(-c(vegetable, units)) %>% 
  mutate(weight = replace_na(weight, 0)) %>% 
  group_by(variety, date) %>% 
  summarize(daily_harvest_lb = sum(weight)*0.00220462) %>% 
  mutate(cum_harvest_lb = cumsum(daily_harvest_lb)) %>% 
  select(-daily_harvest_lb) %>% 
  ggplot() +
  geom_area(aes(x=date, 
                y = cum_harvest_lb, 
                fill = fct_reorder(variety, desc(cum_harvest_lb))),
               position = position_stack()) +
  transition_reveal(date) +
  labs(title = "Cumulative Harvest of Tomatoes Over Time in Pounds", 
       x = "",
       y = "",
       subtitle = "Moving to {frame_along}") +
  theme(legend.position = "right",
        legend.title = element_blank())
```


```r
anim_save("tomatoes.gif")
```


```r
knitr::include_graphics("tomatoes.gif")
```



## Maps, animation, and movement!

  4. Map my `mallorca_bike_day7` bike ride using animation! 
  Requirements:
  * Plot on a map using `ggmap`.  
  * Show "current" location with a red point. 
  * Show path up until the current point.  
  * Color the path according to elevation.  
  * Show the time in the subtitle.  
  * CHALLENGE: use the `ggimage` package and `geom_image` to add a bike image instead of a red point. You can use [this](https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png) image. See [here](https://goodekat.github.io/presentations/2019-isugg-gganimate-spooky/slides.html#35) for an example. 
  * Add something of your own! And comment on if you prefer this to the static map and why or why not.
  

```r
bike_pic <- "https://raw.githubusercontent.com/llendway/animation_and_interactivity/master/bike.png"  

mallorca_bike_day7 <- mallorca_bike_day7 %>% 
  mutate(image = bike_pic)


mallorca_map <- get_stamenmap(
  bbox = c(left = 2.2584, bottom = 39.4404, right = 3.0068, top = 39.7938), 
  maptype = "terrain",
  zoom = 11)

ggmap(mallorca_map) +
  geom_point(data = mallorca_bike_day7, 
             aes(x = lon, y = lat), 
             color = "red", size = .10) +
  geom_path(data = mallorca_bike_day7, 
            aes(x=lon, y=lat, color = ele), 
            size = 0.6) +
  geom_image(data = mallorca_bike_day7,
             aes(x = lon, y = lat, 
                 image = bike_pic),
             size = 0.06) +
  labs(title = "Mallorca Bike Route",
         subtitle = "Moving to {frame_along}") +
  transition_reveal(time) +
  shadow_wake(wake_length = .3) +
  theme_map()
```


```r
anim_save("bike.gif")
```


```r
knitr::include_graphics("bike.gif")
```

  
I prefer animated maps simply because they show a greater level of detail and are just more interesting to look at. A static map will not have shown us the different times at each point, but this animated map does. 
  
  5. In this exercise, you get to meet my sister, Heather! She is a proud Mac grad, currently works as a Data Scientist at 3M where she uses R everyday, and for a few years (while still holding a full-time job) she was a pro triathlete. You are going to map one of her races. The data from each discipline of the Ironman 70.3 Pan Am championships, Panama is in a separate file - `panama_swim`, `panama_bike`, and `panama_run`. Create a similar map to the one you created with my cycling data. You will need to make some small changes: 1. combine the files (HINT: `bind_rows()`, 2. make the leading dot a different color depending on the event (for an extra challenge, make it a different image using `geom_image()!), 3. CHALLENGE (optional): color by speed, which you will need to compute on your own from the data. You can read Heather's race report [here](https://heatherlendway.com/2016/02/10/ironman-70-3-pan-american-championships-panama-race-report/). She is also in the Macalester Athletics [Hall of Fame](https://athletics.macalester.edu/honors/hall-of-fame/heather-lendway/184) and still has records at the pool. 
  

```r
girl_emoji <- "https://raw.githubusercontent.com/ashley-vargas/Weekly-Assignment-5/main/Woman_Running_Emoji_Icon_ios10_large.png"  


triathlon <- panama_swim %>% 
  bind_rows(list(panama_bike, panama_run))
  

panama_map <- get_stamenmap(
  bbox = c(left = -79.6, bottom = 8.9, right = -79.4, top = 9.0),
  maptype = "terrain",
  zoom = 13)

ggmap(panama_map) +
  geom_point(data = triathlon, 
             aes(x = lon, y = lat), 
             color = "red", size = .10) +
  geom_path(data = triathlon, 
            aes(x=lon, y=lat, color = event), 
            size = 0.6) +
  geom_image(data = triathlon,
             aes(x = lon, y = lat, 
                 image = girl_emoji), 
             size = 0.06) +
  labs(title = "Panama Triathlon",
       subtitle = "Moving to {frame_along}") +
  transition_reveal(time) +
  theme_map()
```


```r
anim_save("heather_triathlon.gif")
```


```r
knitr::include_graphics("heather_triathlon.gif")
```
  
  
  
## COVID-19 data

  6. In this exercise, you are going to replicate many of the features in [this](https://aatishb.com/covidtrends/?region=US) visualization by Aitish Bhatia but include all US states. Requirements:
 * Create a new variable that computes the number of new cases in the past week (HINT: use the `lag()` function you've used in a previous set of exercises). Replace missing values with 0's using `replace_na()`.  
  * Filter the data to omit rows where the cumulative case counts are less than 20.  
  * Create a static plot with cumulative cases on the x-axis and new cases in the past 7 days on the x-axis. Connect the points for each state over time. HINTS: use `geom_path()` and add a `group` aesthetic.  Put the x and y axis on the log scale and make the tick labels look nice - `scales::comma` is one option. This plot will look pretty ugly as is.
  * Animate the plot to reveal the pattern by date. Display the date as the subtitle. Add a leading point to each state's line (`geom_point()`) and add the state name as a label (`geom_text()` - you should look at the `check_overlap` argument).  
  * Use the `animate()` function to have 200 frames in your animation and make it 30 seconds long. 
  * Comment on what you observe.
  

```r
covid1 <- covid19 %>% 
  group_by(state) %>% 
  mutate(lag7 = lag(cases, 7, order_by = date)) %>% 
  replace_na(list(lag7 = 0)) %>% 
  mutate(past7_cases = cases- lag7) %>% 
  filter(cases>20) %>% 
  ggplot(aes(x = cases,  y= past7_cases, group = state)) +
  geom_path(color = "thistle2") +
  geom_point() +
  geom_text(aes(label = state), check_overlap = TRUE) +
  labs(title = "COVID-19 cases in the past week",
       subtitle = "Date: {frame_along}",
       x= "Number of cases",  
      y = "New cases in the past week") +
  scale_x_log10(labels = scales::comma) +
  scale_y_log10(labels = scales::comma) +
  transition_reveal(date)

animate(covid1, duration = 30, nframes = 200)
```


```r
anim_save("covidpart1.gif")
```


```r
knitr::include_graphics("covidpart1.gif")
```
  
This map was very interesting because we were able to see the fall and rise of COVID-19 cases in each state across time. THis also allowed us to compare the states to one another. For instance, New York started off with the most cases, but has not been passed by other states, so it was interesting to see this trend in this animated map. 
  
  
  
 7. In this exercise you will animate a map of the US, showing how cumulative COVID-19 cases per 10,000 residents has changed over time. This is similar to exercises 11 & 12 from the previous exercises, with the added animation! So, in the end, you should have something like the static map you made there, but animated over all the days. Put date in the subtitle. Comment on what you see. The code below gives the population estimates for each state. Because there are so many dates, you are going to only do the animation for all Fridays. So, use `wday()` to create a day of week variable and filter to all the Fridays




```r
census_pop_est_2018 <-  read_csv("https://www.dropbox.com/s/6txwv3b4ng7pepe/us_census_2018_state_pop_est.csv?dl=1") %>% 
  separate(state, into = c("dot","state"), extra = "merge") %>% 
  select(-dot) %>% 
  mutate(state = str_to_lower(state))
```


```r
us_map <- map_data("state")

latest_covid <- covid19 %>% 
  mutate(state = str_to_lower(state)) %>% 
  left_join(census_pop_est_2018, covid19, 
            by = "state") %>% 
  group_by(state, date, est_pop_2018) %>% 
  # summarise(cum_cases = max(cases)) %>% 
  mutate(covid_per_10000 = cases/est_pop_2018*10000)

latest_covid %>% 
  mutate(state = str_to_lower(state), weekday = wday(date, label = TRUE)) %>% 
  filter(weekday == "Fri") %>% 
  ggplot() +
  geom_map(aes(map_id = state,
               fill = covid_per_10000, group = date),
            map = us_map) +
  expand_limits(x = us_map$long, y = us_map$lat) +
  theme(legend.position = "right") +
labs(title = "Most Recent Cumulative Number of COVID-19 Cases", 
    fill = "Case Count") +
    theme_map() +
  transition_states(date, transition_length = 0) +
  labs(subtitle = "Date: {closest_state}")
```

![](05_exercises_files/figure-html/unnamed-chunk-20-1.gif)<!-- -->


```r
anim_save("covidpart2.gif")
```


```r
knitr::include_graphics("covidpart2.gif")
```

![](covidpart2.gif)<!-- -->

This map was a improved visualization from last week's map. I am able to see the progression of COVID-19 cases in states much better in this map. The colors also allow to see the differences in casse across states. 



## GitHub link

  8. Below, provide a link to your GitHub page with this set of Weekly Exercises. Specifically, if the name of the file is 05_exercises.Rmd, provide a link to the 05_exercises.md file, which is the one that will be most readable on GitHub. If that file isn't very readable, then provide a link to your main GitHub page.


**DID YOU REMEMBER TO UNCOMMENT THE OPTIONS AT THE TOP?**