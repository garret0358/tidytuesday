#tidytuesday Week 51
================
Garret Pittsenbarger

## Taking a Look at the Spice Girls

First I load the libraries I plan to use.

``` r
library(tidyverse)
library(skimr)
library(ggplot2)
library(Hmisc)
library(patchwork)
library(ggtext)
```

Then I load the data from tidytuesday.

``` r
studio_album_tracks <- readr::read_csv("https://github.com/jacquietran/spice_girls_data/raw/main/data/studio_album_tracks.csv")
```

    ## Rows: 31 Columns: 25

    ## ── Column specification ────────────────────────────────────────────────────────
    ## Delimiter: ","
    ## chr   (9): artist_name, artist_id, album_id, track_id, track_name, album_nam...
    ## dbl  (15): album_release_year, danceability, energy, key, loudness, mode, sp...
    ## date  (1): album_release_date

    ## 
    ## ℹ Use `spec()` to retrieve the full column specification for this data.
    ## ℹ Specify the column types or set `show_col_types = FALSE` to quiet this message.

### First look

First I take a look at the first few rows of the table…

``` r
head(studio_album_tracks)
```

    ## # A tibble: 6 × 25
    ##   artist_name artist_id  album_id album_release_d… album_release_y… danceability
    ##   <chr>       <chr>      <chr>    <date>                      <dbl>        <dbl>
    ## 1 Spice Girls 0uq5PttqE… 4jbWZmf… 2000-01-01                   2000        0.769
    ## 2 Spice Girls 0uq5PttqE… 4jbWZmf… 2000-01-01                   2000        0.829
    ## 3 Spice Girls 0uq5PttqE… 4jbWZmf… 2000-01-01                   2000        0.614
    ## 4 Spice Girls 0uq5PttqE… 4jbWZmf… 2000-01-01                   2000        0.736
    ## 5 Spice Girls 0uq5PttqE… 4jbWZmf… 2000-01-01                   2000        0.86 
    ## 6 Spice Girls 0uq5PttqE… 4jbWZmf… 2000-01-01                   2000        0.798
    ## # … with 19 more variables: energy <dbl>, key <dbl>, loudness <dbl>,
    ## #   mode <dbl>, speechiness <dbl>, acousticness <dbl>, instrumentalness <dbl>,
    ## #   liveness <dbl>, valence <dbl>, tempo <dbl>, track_id <chr>,
    ## #   time_signature <dbl>, duration_ms <dbl>, track_name <chr>,
    ## #   track_number <dbl>, album_name <chr>, key_name <chr>, mode_name <chr>,
    ## #   key_mode <chr>

…and take a look at the structure…

``` r
str(studio_album_tracks)
```

    ## spec_tbl_df [31 × 25] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
    ##  $ artist_name       : chr [1:31] "Spice Girls" "Spice Girls" "Spice Girls" "Spice Girls" ...
    ##  $ artist_id         : chr [1:31] "0uq5PttqEjj3IH1bzwcrXF" "0uq5PttqEjj3IH1bzwcrXF" "0uq5PttqEjj3IH1bzwcrXF" "0uq5PttqEjj3IH1bzwcrXF" ...
    ##  $ album_id          : chr [1:31] "4jbWZmf7kRxCBD6tgVepYh" "4jbWZmf7kRxCBD6tgVepYh" "4jbWZmf7kRxCBD6tgVepYh" "4jbWZmf7kRxCBD6tgVepYh" ...
    ##  $ album_release_date: Date[1:31], format: "2000-01-01" "2000-01-01" ...
    ##  $ album_release_year: num [1:31] 2000 2000 2000 2000 2000 2000 2000 2000 2000 2000 ...
    ##  $ danceability      : num [1:31] 0.769 0.829 0.614 0.736 0.86 0.798 0.671 0.571 0.709 0.536 ...
    ##  $ energy            : num [1:31] 0.819 0.764 0.788 0.779 0.71 0.751 0.75 0.481 0.872 0.539 ...
    ##  $ key               : num [1:31] 10 5 11 8 1 5 1 0 7 11 ...
    ##  $ loudness          : num [1:31] -3.94 -3.78 -5.55 -5.16 -4.21 ...
    ##  $ mode              : num [1:31] 0 0 1 1 0 0 1 1 0 1 ...
    ##  $ speechiness       : num [1:31] 0.0431 0.0431 0.027 0.0401 0.0356 0.0486 0.0279 0.0251 0.0443 0.0272 ...
    ##  $ acousticness      : num [1:31] 0.0293 0.0287 0.155 0.0172 0.00259 0.009 0.188 0.177 0.253 0.744 ...
    ##  $ instrumentalness  : num [1:31] 3.70e-03 3.29e-06 0.00 3.30e-03 3.57e-05 0.00 0.00 0.00 1.06e-02 5.68e-06 ...
    ##  $ liveness          : num [1:31] 0.0744 0.0512 0.157 0.118 0.0387 0.186 0.296 0.18 0.287 0.094 ...
    ##  $ valence           : num [1:31] 0.82 0.919 0.405 0.573 0.884 0.809 0.407 0.0734 0.858 0.307 ...
    ##  $ tempo             : num [1:31] 110 104 116 101 110 ...
    ##  $ track_id          : chr [1:31] "1NwDWbpg9dPH12xBd2ibrv" "0r5d5LmhLQwJVEw0kTEExp" "5EE1Uzg0JvtBhs6TRs33R0" "2O8kqbUJS1vkL3x9mF7WzM" ...
    ##  $ time_signature    : num [1:31] 4 4 4 4 4 4 4 4 4 4 ...
    ##  $ duration_ms       : num [1:31] 255866 254666 298293 251000 226266 ...
    ##  $ track_name        : chr [1:31] "Holler" "Tell Me Why" "Let Love Lead The Way" "Right Back At Ya" ...
    ##  $ track_number      : num [1:31] 1 2 3 4 5 6 7 8 9 10 ...
    ##  $ album_name        : chr [1:31] "Forever" "Forever" "Forever" "Forever" ...
    ##  $ key_name          : chr [1:31] "A#" "F" "B" "G#" ...
    ##  $ mode_name         : chr [1:31] "minor" "minor" "major" "major" ...
    ##  $ key_mode          : chr [1:31] "A# minor" "F minor" "B major" "G# major" ...
    ##  - attr(*, "spec")=
    ##   .. cols(
    ##   ..   artist_name = col_character(),
    ##   ..   artist_id = col_character(),
    ##   ..   album_id = col_character(),
    ##   ..   album_release_date = col_date(format = ""),
    ##   ..   album_release_year = col_double(),
    ##   ..   danceability = col_double(),
    ##   ..   energy = col_double(),
    ##   ..   key = col_double(),
    ##   ..   loudness = col_double(),
    ##   ..   mode = col_double(),
    ##   ..   speechiness = col_double(),
    ##   ..   acousticness = col_double(),
    ##   ..   instrumentalness = col_double(),
    ##   ..   liveness = col_double(),
    ##   ..   valence = col_double(),
    ##   ..   tempo = col_double(),
    ##   ..   track_id = col_character(),
    ##   ..   time_signature = col_double(),
    ##   ..   duration_ms = col_double(),
    ##   ..   track_name = col_character(),
    ##   ..   track_number = col_double(),
    ##   ..   album_name = col_character(),
    ##   ..   key_name = col_character(),
    ##   ..   mode_name = col_character(),
    ##   ..   key_mode = col_character()
    ##   .. )
    ##  - attr(*, "problems")=<externalptr>

…and a peek at the mean values for the various musical qualities for
their full body of work.

``` r
sum_df <- studio_album_tracks %>%
  summarize_if(is.numeric, mean, na.rm = TRUE)
head(sum_df)
```

    ## # A tibble: 1 × 15
    ##   album_release_year danceability energy   key loudness  mode speechiness
    ##                <dbl>        <dbl>  <dbl> <dbl>    <dbl> <dbl>       <dbl>
    ## 1              1998.        0.654  0.743  5.84    -5.99 0.548      0.0444
    ## # … with 8 more variables: acousticness <dbl>, instrumentalness <dbl>,
    ## #   liveness <dbl>, valence <dbl>, tempo <dbl>, time_signature <dbl>,
    ## #   duration_ms <dbl>, track_number <dbl>

I’m going to make a smaller table to isolate the data that I’m
interested in looking more closely at, the musical quality of each song.

``` r
music_q <- studio_album_tracks %>% 
  select(album_name, track_number, track_name, duration_ms, key_mode, key_name, mode_name, key, mode, tempo, time_signature, valence, danceability, energy, instrumentalness, acousticness, loudness)
head(music_q)
```

    ## # A tibble: 6 × 17
    ##   album_name track_number track_name     duration_ms key_mode key_name mode_name
    ##   <chr>             <dbl> <chr>                <dbl> <chr>    <chr>    <chr>    
    ## 1 Forever               1 Holler              255866 A# minor A#       minor    
    ## 2 Forever               2 Tell Me Why         254666 F minor  F        minor    
    ## 3 Forever               3 Let Love Lead…      298293 B major  B        major    
    ## 4 Forever               4 Right Back At…      251000 G# major G#       major    
    ## 5 Forever               5 Get Down With…      226266 C# minor C#       minor    
    ## 6 Forever               6 Wasting My Ti…      254773 F minor  F        minor    
    ## # … with 10 more variables: key <dbl>, mode <dbl>, tempo <dbl>,
    ## #   time_signature <dbl>, valence <dbl>, danceability <dbl>, energy <dbl>,
    ## #   instrumentalness <dbl>, acousticness <dbl>, loudness <dbl>

``` r
skim(music_q)
```

|                                                  |         |
|:-------------------------------------------------|:--------|
| Name                                             | music_q |
| Number of rows                                   | 31      |
| Number of columns                                | 17      |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_   |         |
| Column type frequency:                           |         |
| character                                        | 5       |
| numeric                                          | 12      |
| \_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_\_ |         |
| Group variables                                  | None    |

Data summary

**Variable type: character**

| skim_variable | n_missing | complete_rate | min | max | empty | n_unique | whitespace |
|:--------------|----------:|--------------:|----:|----:|------:|---------:|-----------:|
| album_name    |         0 |             1 |   5 |  10 |     0 |        3 |          0 |
| track_name    |         0 |             1 |   4 |  31 |     0 |       31 |          0 |
| key_mode      |         0 |             1 |   7 |   8 |     0 |       19 |          0 |
| key_name      |         0 |             1 |   1 |   2 |     0 |       10 |          0 |
| mode_name     |         0 |             1 |   5 |   5 |     0 |        2 |          0 |

**Variable type: numeric**

| skim_variable    | n_missing | complete_rate |      mean |       sd |        p0 |       p25 |       p50 |       p75 |      p100 | hist  |
|:-----------------|----------:|--------------:|----------:|---------:|----------:|----------:|----------:|----------:|----------:|:------|
| track_number     |         0 |             1 |      5.68 |     3.04 |      1.00 |      3.00 |      6.00 |      8.00 |     11.00 | ▇▅▅▅▃ |
| duration_ms      |         0 |             1 | 248674.97 | 40070.00 | 166533.00 | 227746.50 | 251000.00 | 271066.50 | 326826.00 | ▃▃▇▅▃ |
| key              |         0 |             1 |      5.84 |     3.77 |      0.00 |      2.00 |      6.00 |      9.00 |     11.00 | ▇▂▆▅▇ |
| mode             |         0 |             1 |      0.55 |     0.51 |      0.00 |      0.00 |      1.00 |      1.00 |      1.00 | ▆▁▁▁▇ |
| tempo            |         0 |             1 |    114.74 |    27.09 |     80.14 |    100.01 |    107.02 |    121.01 |    190.16 | ▆▇▂▁▂ |
| time_signature   |         0 |             1 |      4.00 |     0.00 |      4.00 |      4.00 |      4.00 |      4.00 |      4.00 | ▁▁▇▁▁ |
| valence          |         0 |             1 |      0.66 |     0.24 |      0.07 |      0.52 |      0.75 |      0.84 |      0.96 | ▁▃▃▃▇ |
| danceability     |         0 |             1 |      0.65 |     0.13 |      0.30 |      0.58 |      0.69 |      0.73 |      0.86 | ▁▁▆▇▃ |
| energy           |         0 |             1 |      0.74 |     0.12 |      0.48 |      0.68 |      0.75 |      0.82 |      0.99 | ▂▃▇▃▂ |
| instrumentalness |         0 |             1 |      0.06 |     0.16 |      0.00 |      0.00 |      0.00 |      0.01 |      0.73 | ▇▁▁▁▁ |
| acousticness     |         0 |             1 |      0.13 |     0.17 |      0.00 |      0.02 |      0.06 |      0.18 |      0.74 | ▇▂▁▁▁ |
| loudness         |         0 |             1 |     -5.99 |     1.85 |     -9.33 |     -7.83 |     -5.55 |     -4.78 |     -2.32 | ▆▃▇▆▂ |

### Initial thoughts

Overall the data looks quite clean, so I will start to dig a little
deeper to look for relationships within the data. I think that the
positivity of a song (valence) may be affected by the key. I’m curious
if the musical quality of the songs are affected by the mode. First I
will take a look at the average values for danceability, valence,
energy, acousticness, and instrumentalness. I am excluding speechiness,
because the mean value is very low for all songs, and liveness, as this
is a quality of the performance and not of the music itself.

``` r
sum_mode_df <- studio_album_tracks %>% 
  select(mode_name, danceability, valence, energy, acousticness, instrumentalness) %>% 
  group_by(mode_name) %>% 
  summarize_if(is.numeric, mean, na.rm = TRUE)
rdf3 <- sum_mode_df %>% 
  pivot_longer(!mode_name, names_to = "quality") %>% 
  arrange(-value)
ggplot(data = rdf3, aes(x = quality, y = value, color = mode_name)) +
  geom_point(alpha = .8, size = 3) +
  scale_color_manual(values = c("#fab249", "#c84ec8")) +
  coord_flip() +
  annotate(geom = "label", x = 4.5, y = .8, label = "mean valence differs by .223", hjust = "right") +
  labs(title = "Musical Quality", subtitle = "Comparing the musical qualities of <b><span style = 'color:#fab249;'>major</span></b> and <b><span style = 'color:#c84ec8;'>minor</span></b> keys") +
  theme_minimal() +
  theme(legend.position = "none", plot.subtitle = element_markdown())
```

![](tidytuesday_202151_files/figure-gfm/plot%20musical%20quality%20means-1.png)<!-- -->

The average valence of the songs in a minor key is much higher than the
songs in major keys. Usually we associate major keys with more uplifting
music and minor keys with sad or dark music. Let’s look more closely at
the valence of the songs in both major and minor keys.

``` r
ggplot(data = music_q, aes(x = mode_name, y = valence)) +
  geom_violin(fill = "#f3e697", color = "#ffdb01", alpha= .5) +
  stat_summary(fun = "mean", geom = "crossbar", width = .3, color = "#f778e4") +
  geom_point(color = "#c84ec8") +
  coord_flip() +
  labs(
    x = "Mode", 
    y = "Level of Valence", 
    title = "Mode/Valence", 
    subtitle = "Spice Girls songs written in minor keys tend to be perceived as positve:", 
    caption = "<b>Data:</b> Spotify, <b>Graphic:</b> Garret P", 
    alt = "This violin plot compares the mode of Spice Girls songs with the valence. The songs written in minor keys tend to be perceived as positive."
    )+
  theme_minimal() +
  theme(plot.caption = element_markdown()) +
  annotate(geom = "text", x = 1.4, y = .03, label = "Songs in a major key have a wide range of valence", hjust = "left")
```

![](tidytuesday_202151_files/figure-gfm/plot%20valence%20vs.%20mode-1.png)<!-- -->

### Digging deeper

I’m surprised that the songs in minor keys rate a higher valence. I want
to find out if there are any other strong correlations between these
variables. I will check correlations for all relevant numeric columns.

``` r
cor_music <- music_q %>% 
  select(key, mode, tempo, valence, danceability, energy, instrumentalness, acousticness,loudness) %>% 
  cor() %>% 
  reshape::melt()
```

    ## Warning in type.convert.default(X[[i]], ...): 'as.is' should be specified by the
    ## caller; using TRUE

    ## Warning in type.convert.default(X[[i]], ...): 'as.is' should be specified by the
    ## caller; using TRUE

``` r
head(cor_music)
```

    ##             X1  X2       value
    ## 1          key key  1.00000000
    ## 2         mode key -0.07443911
    ## 3        tempo key -0.06456481
    ## 4      valence key  0.09928623
    ## 5 danceability key -0.01986873
    ## 6       energy key  0.06501044

``` r
ggplot(data = cor_music, aes(x = X1, y = X2, fill = value)) +
  geom_tile() +
  scale_fill_gradient2(low = "#ffdb01", high = "#f778e4", mid = "white") +
  labs(x = "Variables", y = "Variables") +
  annotate(
    geom = "curve", x = 3, y = 5, xend = 2, yend = 4, curvature = .2, arrow = arrow(length = unit(2, "mm"))
  ) +
  annotate(
    geom = "curve", x = 5, y = 3, xend = 5, yend = 4, curvature = .2, arrow = arrow(length = unit(2, "mm"))
  ) +
  annotate(geom = "label", x = 3.1, y = 5, label = "valence and mode", hjust = "left") +
  annotate(geom = "label", x = 5, y = 3, label = "valence and danceability", hjust = "center") +
  labs(title = "Correlation", subtitle = "Correlation between various musical qualities:") +
  theme(axis.text.x = element_text(angle = 45, hjust = 1))+
  theme_minimal()
```

![](tidytuesday_202151_files/figure-gfm/correlation%20matrix-1.png)<!-- -->

This seems to confirm a strong positive between mode and valence. There
also seems to be a correlation between dancebility and valence, let’s
plot to look at it more closely.

``` r
ggplot(data = music_q, aes(x = danceability, y = valence)) +
  geom_smooth(alpha = .4, fill = "#f3e697", color = "#f778e4", method = "gam") +
  geom_point(color = "#c84ec8") +
  labs(title = "Valence/Danceability", subtitle = "A positive correlation exists between valence and danceability:") +
  annotate(geom = "text", x = .3, y = 1, label = "Size of the points indicates tempo", hjust = "left") +
  annotate(geom = "label", x = .3, y = .6, label = "The Lady is a Vamp", hjust = "left") +
  annotate(geom = "label", x = .35, y = .4, label = "Too Much", hjust = "left") +
  annotate(geom = "label", x = .58, y = .1, label = "Time Goes By", hjust = "left") +
  theme(legend.position = "none")+
  theme_minimal()
```

    ## `geom_smooth()` using formula 'y ~ s(x, bs = "cs")'

![](tidytuesday_202151_files/figure-gfm/valence%20vs.%20danceability-1.png)<!-- -->

When plotting all the songs from across all three albums we can see that
most fall within the range of high valence and moderate danceability.
There seems to be three songs that don’t match fit with this group as
well. Each of these songs are also relatively slower than the majority
of other songs.

The data shows that in general, as the positivity of a song increases so
does the danceability. Let’s also compare danceability with mode.

``` r
ggplot(data = music_q, aes(x = mode_name, y = danceability)) +
  geom_violin(fill = "#f3e697", color = "#ffdb01", alpha= .5) +
  stat_summary(fun = "mean", geom = "crossbar", width = .3, color = "#f778e4") +
  geom_point(color = "#c84ec8") +
  coord_flip() +
  labs(x = "mode", title = "Mode/Danceability", subtitle = "Relationship between modes and danceability:")+
  theme_minimal()
```

![](tidytuesday_202151_files/figure-gfm/danceability%20vs.%20mode-1.png)<!-- -->

Given the strong positive correlation between danceability and
positivity it is no surprise that songs in minor keys also good for
dancing when compared with songs in major keys.

### Conclusions

The Spice Girls produced many songs in minor keys that may be perceived
as positive and good to dance to. This was contrary to my expectation
that songs in a minor key would be categorized as less positive. I would
like to dig deeper later to explore the correlation between the themes
in the lyrics and how it affects positivity.
