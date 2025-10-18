*To complete this milestone, you can edit [this `.rmd`
file](https://github.com/UBC-STAT/STAT545.github.io/blob/main/content/mini-data-analysis/mini-project-2.Rmd)
directly. Fill in the sections that are commented out with
`<!--- start your work here--->`. When you are done, make sure to knit
to an `.md` file by changing the output in the YAML header to
`github_document`, before submitting a tagged release on canvas.*

# Welcome to the rest of your mini data analysis project!

In Milestone 1, you explored your data. and came up with research
questions. This time, we will finish up our mini data analysis and
obtain results for your data by:

-   Making summary tables and graphs
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

We will also explore more in depth the concept of *tidy data.*

**NOTE**: The main purpose of the mini data analysis is to integrate
what you learn in class in an analysis. Although each milestone provides
a framework for you to conduct your analysis, it’s possible that you
might find the instructions too rigid for your data set. If this is the
case, you may deviate from the instructions – just make sure you’re
demonstrating a wide range of tools and techniques taught in this class,
and indicate *why* you had to deviate. Feel free to contact the
instructor in these cases.

# Instructions

**To complete this milestone**, edit [this very `.Rmd`
file](https://github.com/UBC-STAT/STAT545.github.io/blob/main/content/mini-data-analysis/mini-project-2.Rmd)
directly. Fill in the sections that are tagged with
`<!--- start your work here--->`.

**To submit this milestone**, make sure to knit this `.Rmd` file to an
`.md` file by changing the YAML output settings from
`output: html_document` to `output: github_document`. Commit and push
all of your work to your mini-analysis GitHub repository, and tag a
release on GitHub. Then, submit a link to your tagged release on canvas.

**Points**: This milestone is worth 50 points: 45 for your analysis, and
5 for overall reproducibility, cleanliness, and coherence of the Github
submission.

**Research Questions**: In Milestone 1, you chose four research
questions to focus on. Wherever realistic, your work in this milestone
should relate to these research questions whenever we ask for
justification behind your work. In the case that some tasks in this
milestone don’t align well with one of your research questions, feel
free to discuss your results in the context of a different research
question.

# Learning Objectives

By the end of this milestone, you should:

-   Understand what *tidy* data is, and how to create it using `tidyr`.
-   Generate a reproducible and clear report using R Markdown.
-   Manipulating special data types in R: factors and/or dates and
    times.
-   Fitting a model object to your data, and extract a result.
-   Reading and writing data as separate files.

# Setup

Begin by loading your data and the tidyverse package below:

    library(datateachr) # <- might contain the data you picked!
    library(tidyverse)
    library(stringr)
    library(forcats)

# Task 1: Process and summarize your data

From Milestone 1, you should have an idea of the basic structure of your
dataset (e.g. number of rows and columns, class types, etc.). Here, we
will start investigating your data more in-depth using various data
manipulation functions.

### 1.1 (1 point)

First, write out the 4 research questions you defined in milestone 1
were. This will guide your work through milestone 2:

<!-------------------------- Start your work below ---------------------------->

1.  Why are so many of the discounted game prices higher than the
    non-discounted prices?

2.  How does the initial price of a game and its computer requirements
    affect its audience perception?

3.  Have discounts applied to games affected how they are reviewed
    recently compared to overall?

4.  What genre of games are the most popular to release and play over
    time?

    <!----------------------------------------------------------------------------->

Here, we will investigate your data using various data manipulation and
graphing functions.

### 1.2 (8 points)

Now, for each of your four research questions, choose one task from
options 1-4 (summarizing), and one other task from 4-8 (graphing). You
should have 2 tasks done for each research question (8 total). Make sure
it makes sense to do them! (e.g. don’t use a numerical variables for a
task that needs a categorical variable.). Comment on why each task helps
(or doesn’t!) answer the corresponding research question.

Ensure that the output of each operation is printed!

Also make sure that you’re using dplyr and ggplot2 rather than base R.
Outside of this project, you may find that you prefer using base R
functions for certain tasks, and that’s just fine! But part of this
project is for you to practice the tools we learned in class, which is
dplyr and ggplot2.

**Summarizing:**

1.  Compute the *range*, *mean*, and *two other summary statistics* of
    **one numerical variable** across the groups of **one categorical
    variable** from your data.
2.  Compute the number of observations for at least one of your
    categorical variables. Do not use the function `table()`!
3.  Create a categorical variable with 3 or more groups from an existing
    numerical variable. You can use this new variable in the other
    tasks! *An example: age in years into “child, teen, adult, senior”.*
4.  Compute the proportion and counts in each category of one
    categorical variable across the groups of another categorical
    variable from your data. Do not use the function `table()`!

**Graphing:**

1.  Create a graph of your choosing, make one of the axes logarithmic,
    and format the axes labels so that they are “pretty” or easier to
    read.
2.  Make a graph where it makes sense to customize the alpha
    transparency.

Using variables and/or tables you made in one of the “Summarizing”
tasks:

1.  Create a graph that has at least two geom layers.
2.  Create 3 histograms, with each histogram having different sized
    bins. Pick the “best” one and explain why it is the best.

Make sure it’s clear what research question you are doing each operation
for!

<!------------------------- Start your work below ----------------------------->

    # steam_games tibble for reference
    steam_games

    ## # A tibble: 40,833 × 21
    ##       id url    types name  desc_snippet recent_reviews all_reviews release_date
    ##    <dbl> <chr>  <chr> <chr> <chr>        <chr>          <chr>       <chr>       
    ##  1     1 https… app   DOOM  Now include… Very Positive… Very Posit… May 12, 2016
    ##  2     2 https… app   PLAY… PLAYERUNKNO… Mixed,(6,214)… Mixed,(836… Dec 21, 2017
    ##  3     3 https… app   BATT… Take comman… Mixed,(166),-… Mostly Pos… Apr 24, 2018
    ##  4     4 https… app   DayZ  The post-so… Mixed,(932),-… Mixed,(167… Dec 13, 2018
    ##  5     5 https… app   EVE … EVE Online … Mixed,(287),-… Mostly Pos… May 6, 2003 
    ##  6     6 https… bund… Gran… Grand Theft… NaN            NaN         NaN         
    ##  7     7 https… app   Devi… The ultimat… Very Positive… Very Posit… Mar 7, 2019 
    ##  8     8 https… app   Huma… Human: Fall… Very Positive… Very Posit… Jul 22, 2016
    ##  9     9 https… app   They… They Are Bi… Very Positive… Very Posit… Dec 12, 2017
    ## 10    10 https… app   Warh… In a world … <NA>           Mixed,(904… May 31, 2019
    ## # ℹ 40,823 more rows
    ## # ℹ 13 more variables: developer <chr>, publisher <chr>, popular_tags <chr>,
    ## #   game_details <chr>, languages <chr>, achievements <dbl>, genre <chr>,
    ## #   game_description <chr>, mature_content <chr>, minimum_requirements <chr>,
    ## #   recommended_requirements <chr>, original_price <dbl>, discount_price <dbl>

    # Research Question 1: Why are so many of the discounted game prices higher than the non-discounted prices?

    # Determine the discount trends over time.

    # Make a tibble with the relevant data.
    q1_data <- steam_games %>%
      select(name, release_date, original_price, discount_price) %>%
      
      # Remove rows with improperly formatted dates, convert to year.
      filter(!is.na(release_date)) %>%
      filter(grepl(", ", release_date)) %>%
      filter(!grepl("!", release_date)) %>%
      
      # New column for how many dollars each game was discounted for.
      mutate(discount_dollar = original_price - discount_price) %>%
      # New column for what percentage each game was discounted for.
      mutate(discount_percent = as.numeric(discount_dollar / original_price * 100)) %>%
      
      # Remove rows with no discounts
      filter(!is.na(discount_percent)) %>%
      
      # Convert release date strings to date values.
      mutate(release_year = as.numeric(word(release_date, 2, sep = ", "))) %>%
      
      # Remove release date column
      select(-release_date) %>%
      
      # Exercise 1: Compute the range, mean, and two other summary statistics of one numerical variable across the groups of one categorical variable from your data.
     
      # Filter out games with a normal discount of 0-100%
      filter(discount_percent < 0) %>%
      
      # Group by release year
      group_by(release_year) %>%
      
      # Summary of discounts
      summarize("Max" = max(discount_percent), "Min" = min(discount_percent),
                "Median" = median(discount_percent), "Mean" = mean(discount_percent),
                "25% Quartile" = quantile(discount_percent, 0.25),
                "75% Quartile" = quantile(discount_percent, 0.75))

    # Exercise 6: Create a graph of your choosing, make one of the axes logarithmic, and format the axes labels so that they are "pretty" or easier to read.
    q1_graph <- ggplot(q1_data, aes(x = release_year, y = `Mean`)) +
      
      # Boxplots summarizing the percentage games from each year were discounted
      geom_boxplot(aes(group = release_year, ymin = `Min`, lower = `25% Quartile`,
                       middle = `Median`, upper = `75% Quartile`, ymax = `Max`), 
                   stat = "identity", width = 0.75) +
      
      # Format y axis as negative logarithmic
      scale_y_continuous(breaks = c(-10^(5:0), 0), transform = "pseudo_log") +
      guides(y = guide_axis_logticks(negative.small = 1)) +
      
      # Labels
      ggtitle("Negative Discounts of Games Released Over the Years") +
      xlab("Release Year") +
      ylab("% Discounted")

    q1_graph

![](mini-project-2_files/figure-markdown_strict/Research_Q_1-1.png)

    # Research Question 2: How does the initial price of a game and its computer requirements affect its audience perception?

    # Create tibble of relevant data
    q2_data <- steam_games %>%
      select(name, all_reviews, recommended_requirements, original_price) %>%
      
      # Filter games that have data in all fields.
      na.omit() %>%
      
      # Extract audience reviews and filter out ones without a consensus.
      mutate(all_reviews = gsub(",.*$", "", all_reviews)) %>%
      filter(grepl("Positive|Negative|Mixed", all_reviews)) %>%
      mutate(all_reviews = fct_relevel(all_reviews, "Overwhelmingly Positive",
                                       "Very Positive", "Mostly Positive", "Positive",
                                       "Mixed", "Negative", "Mostly Negative",
                                       "Very Negative", "Overwhelmingly Negative"))

    # Make new column for each requirement

    category_list = list("Recommended", "OS", "Processor", "Memory", "Graphics",
                         "Storage", "DirectX", "Network", "Sound Card")

    # For loop to extract text for each requirement in recommended_requirements.
    for(category in category_list){
      title_str <- paste("^.*", category, ":,", sep = "")
      
      # Remove everything before category name + ":", inclusive.
      # Then remove everything after the first comma.
      # If there isn't info in for this category, replace with an "NA".
      q2_data <- q2_data %>%
        mutate(col_1 = sub(title_str, "", recommended_requirements),
               col_2 = sub(",.*$", "", col_1),
               !!category := sub("^.*:", NA, col_2))
    }
      
    # Remove original recommended_requirements column and extraneous columns.
    q2_data <- select(q2_data, -recommended_requirements, -col_1, -col_2)

    # Exercise 2: Compute the number of observations for at least one of your categorical variables. Do not use the function table()!

    # Define high requirements for a game as the most recent version of hardware. 
    # Make a list of keywords to search for in each category.
    high_reqs <- list("64-bit processor", "Windows 7", "i7", "16 GB", "GeForce", "GB",
                      "Version 11", "Broadband", "DirectX")

    q2_data <- mutate(q2_data, "# Requirements" = 0)

    # Tally 1 to temp column for each section if it needs the most recent hardware.
    for(i in 1:length(category_list)){
      col_name <- toString(category_list[i])
      
      # Add the tally to # Requirements column
      q2_data <- q2_data %>%
        mutate("# Requirements" = `# Requirements` + ifelse(grepl(!!high_reqs[i], 
                                                                  !!sym(col_name)), 
                                                            1, 0))
    }

    q2_data <- q2_data %>%
      # Assign what's considered low to high requirements.
      mutate("Requirements" = ifelse(`# Requirements` <= 3, "Low", 
                                     ifelse(`# Requirements` <= 6, 
                                            "Medium", "High"))) %>%
      mutate("Requirements" = fct_reorder(`Requirements`, `# Requirements`, mean)) %>%
      group_by(`Requirements`) %>%
      select(name, all_reviews, Requirements, `# Requirements`, original_price)

    # Exercise 8: Create a graph that has at least two geom layers.
    q2_graph <- ggplot(q2_data, aes(x = all_reviews, fill = Requirements)) +
      # Bar graph of how many games are in each rating with each requirement level
      geom_bar(position = "dodge") +
      
      # Boxplot of the game's original price
      geom_boxplot(aes(x = all_reviews, y = original_price / 0.3)) +
      
      # Separate scale for number of games vs original price
      scale_y_continuous(
        name = "Number of Games", 
        sec.axis = sec_axis(~.*0.3, name = "Original Price ($)")
      ) +
      
      # Labels
      ggtitle("System Requirements of Games", 
              subtitle = "Bar Graph: # of Games with Requirements; Boxplot: Price Range of Games") +
      xlab("User Reviews") +
      theme(axis.text.x = element_text(angle = 45, hjust = 1, size = 7))

    q2_graph

![](mini-project-2_files/figure-markdown_strict/Research_Q_2-1.png)

    # Research Question 3: Have discounts applied to games affected how they are reviewed recently compared to overall?

    q3_data <- steam_games %>%
      select(name, recent_reviews, all_reviews, original_price, discount_price) %>%
      
      # New column for how many dollars each game was discounted for.
      mutate(discount_dollar = original_price - discount_price) %>%
      # New column for what percentage each game was discounted for.
      mutate(discount_percent = as.numeric(discount_dollar / original_price * 100)) %>%
      # Filter out games that have a negative discount
      filter(discount_percent >= 0) %>%
      
      # Extract audience reviews and filter out ones without a consensus.
      mutate(recent_reviews = gsub(",.*$", "", recent_reviews)) %>%
      filter(grepl("Positive|Negative|Mixed", recent_reviews)) %>%
      # Extract audience reviews and filter out ones without a consensus.
      mutate(all_reviews = gsub(",.*$", "", recent_reviews)) %>%
      filter(grepl("Positive|Negative|Mixed", recent_reviews)) %>%
      
      # Filter games that have data in all fields.
      na.omit() %>%
      select(-original_price, -discount_price, -discount_dollar) %>%
      
      # Exercise 4: Compute the range, mean, and two other summary statistics of one numerical variable across the groups of one categorical variable from your data.
      
      # Convert ratings to numerical representations
      mutate(recent_rating = recode(recent_reviews, "Overwhelmingly Positive" = 1,
                                     "Very Positive" = 2, "Mostly Positive" = 3,
                                     "Positive" = 4, "Mixed" = 5, "Negative" = 6,
                                     "Mostly Negative" = 7, "Very Negative" = 8,
                                     "Overwhelmingly Negative" = 9)) %>%
      mutate(all_rating = recode(all_reviews, "Overwhelmingly Positive" = 1,
                                  "Very Positive" = 2, "Mostly Positive" = 3,
                                  "Positive" = 4, "Mixed" = 5, "Negative" = 6,
                                  "Mostly Negative" = 7, "Very Negative" = 8,
                                  "Overwhelmingly Negative" = 9)) %>%
      
      # Determine change in audience perception
      mutate(rating_change = all_rating - recent_rating) %>%
      
      # Summary of discounts
      group_by(all_reviews) %>%
      summarize("Max" = max(discount_percent), "Min" = min(discount_percent),
                "Median" = median(discount_percent), "Mean" = mean(discount_percent),
                "25% Quartile" = quantile(discount_percent, 0.25),
                "75% Quartile" = quantile(discount_percent, 0.75), 
                "Mean Rating Change" = mean(rating_change))

    # Exercise 8: Create a graph that has at least two geom layers.
    q3_graph <- ggplot(q3_data, aes(x = all_reviews, y = Mean)) +
      # Boxplots summarizing the percentage discounted for each review rating
      geom_boxplot(aes(group = all_reviews, ymin = `Min`, lower = `25% Quartile`,
                       middle = `Median`, upper = `75% Quartile`, ymax = `Max`), 
                   stat = "identity", width = 0.75) +
      geom_point(aes(x = all_reviews, y = `Mean Rating Change`)) +
      
      # Separate scale for discounted price and change in rating
      scale_y_continuous(
        name = "% Discounted", 
        sec.axis = sec_axis(~./12, name = "Change in Recent Compared to Overall Rating")
      ) +
      
      # Labels
      ggtitle("Games Discounts Compared to Overall Reviews") +
      xlab("Overall Reviews")

    q3_graph

![](mini-project-2_files/figure-markdown_strict/Research_Q_3-1.png)

    # Research Question 4: What genre of games are the most popular to release and play over time?

    q4_data <- steam_games %>%
      select(name, release_date, genre) %>%
      
      # Remove rows with improperly formatted dates, convert to year.
      filter(!is.na(release_date)) %>%
      filter(grepl(", ", release_date)) %>%
      filter(!grepl("!", release_date)) %>%
      
      # Convert release date strings to date values.
      mutate(release_year = suppressWarnings(as.numeric(word(release_date, 2, 
                                                             sep = ", ")))) %>%
      select(-release_date) %>%
      drop_na() %>%
      
      # Separate genre column into individual items.
      separate_wider_delim(genre, delim = ",", names_sep = "_", 
                           too_few = "align_start") %>%
      
      # Exercise 2: Compute the number of observations for at least one of your categorical variables. Do not use the function table()!
      
      # Make multiple rows for each game with one genre column
      pivot_longer(cols = starts_with("genre"), names_to = "temp", values_to = "genre",
                   values_drop_na = TRUE) %>%
      select(-temp) %>%
      
      # Sort by genre
      group_by(release_year, genre) %>%
      rename("Genre" = genre) %>%
      summarise(n = n(), .groups = "drop_last")

    # Exercise 7: Create a graph of your choosing, make one of the axes logarithmic, and format the axes labels so that they are "pretty" or easier to read.
    q4_graph <- ggplot(q4_data, aes(x = release_year, y = n, color = Genre)) +
      geom_line() +
      scale_y_continuous(trans = "log10") +
      
      # Labels
      ggtitle("Genres of Games Over Time") +
      xlab("Release Year") +
      ylab("# of Games")

    q4_graph

![](mini-project-2_files/figure-markdown_strict/Research_Q_4-1.png)

<!----------------------------------------------------------------------------->

### 1.3 (2 points)

Based on the operations that you’ve completed, how much closer are you
to answering your research questions? Think about what aspects of your
research questions remain unclear. Can your research questions be
refined, now that you’ve investigated your data a bit more? Which
research questions are yielding interesting results?

<!------------------------- Write your answer here ---------------------------->
<!----------------------------------------------------------------------------->

# Task 2: Tidy your data

In this task, we will do several exercises to reshape our data. The goal
here is to understand how to do this reshaping with the `tidyr` package.

A reminder of the definition of *tidy* data:

-   Each row is an **observation**
-   Each column is a **variable**
-   Each cell is a **value**

### 2.1 (2 points)

Based on the definition above, can you identify if your data is tidy or
untidy? Go through all your columns, or if you have &gt;8 variables,
just pick 8, and explain whether the data is untidy or tidy.

<!--------------------------- Start your work below --------------------------->
<!----------------------------------------------------------------------------->

### 2.2 (4 points)

Now, if your data is tidy, untidy it! Then, tidy it back to it’s
original state.

If your data is untidy, then tidy it! Then, untidy it back to it’s
original state.

Be sure to explain your reasoning for this task. Show us the “before”
and “after”.

<!--------------------------- Start your work below --------------------------->
<!----------------------------------------------------------------------------->

### 2.3 (4 points)

Now, you should be more familiar with your data, and also have made
progress in answering your research questions. Based on your interest,
and your analyses, pick 2 of the 4 research questions to continue your
analysis in the remaining tasks:

<!-------------------------- Start your work below ---------------------------->

1.  *FILL\_THIS\_IN*
2.  *FILL\_THIS\_IN*

<!----------------------------------------------------------------------------->

Explain your decision for choosing the above two research questions.

<!--------------------------- Start your work below --------------------------->
<!----------------------------------------------------------------------------->

Now, try to choose a version of your data that you think will be
appropriate to answer these 2 questions. Use between 4 and 8 functions
that we’ve covered so far (i.e. by filtering, cleaning, tidy’ing,
dropping irrelevant columns, etc.).

(If it makes more sense, then you can make/pick two versions of your
data, one for each research question.)

<!--------------------------- Start your work below --------------------------->

# Task 3: Modelling

## 3.0 (no points)

Pick a research question from 1.2, and pick a variable of interest
(we’ll call it “Y”) that’s relevant to the research question. Indicate
these.

<!-------------------------- Start your work below ---------------------------->

**Research Question**: FILL\_THIS\_IN

**Variable of interest**: FILL\_THIS\_IN

<!----------------------------------------------------------------------------->

## 3.1 (3 points)

Fit a model or run a hypothesis test that provides insight on this
variable with respect to the research question. Store the model object
as a variable, and print its output to screen. We’ll omit having to
justify your choice, because we don’t expect you to know about model
specifics in STAT 545.

-   **Note**: It’s OK if you don’t know how these models/tests work.
    Here are some examples of things you can do here, but the sky’s the
    limit.

    -   You could fit a model that makes predictions on Y using another
        variable, by using the `lm()` function.
    -   You could test whether the mean of Y equals 0 using `t.test()`,
        or maybe the mean across two groups are different using
        `t.test()`, or maybe the mean across multiple groups are
        different using `anova()` (you may have to pivot your data for
        the latter two).
    -   You could use `lm()` to test for significance of regression
        coefficients.

<!-------------------------- Start your work below ---------------------------->
<!----------------------------------------------------------------------------->

## 3.2 (3 points)

Produce something relevant from your fitted model: either predictions on
Y, or a single value like a regression coefficient or a p-value.

-   Be sure to indicate in writing what you chose to produce.
-   Your code should either output a tibble (in which case you should
    indicate the column that contains the thing you’re looking for), or
    the thing you’re looking for itself.
-   Obtain your results using the `broom` package if possible. If your
    model is not compatible with the broom function you’re needing, then
    you can obtain your results by some other means, but first indicate
    which broom function is not compatible.

<!-------------------------- Start your work below ---------------------------->
<!----------------------------------------------------------------------------->

# Task 4: Reading and writing data

Get set up for this exercise by making a folder called `output` in the
top level of your project folder / repository. You’ll be saving things
there.

## 4.1 (3 points)

Take a summary table that you made from Task 1, and write it as a csv
file in your `output` folder. Use the `here::here()` function.

-   **Robustness criteria**: You should be able to move your Mini
    Project repository / project folder to some other location on your
    computer, or move this very Rmd file to another location within your
    project repository / folder, and your code should still work.
-   **Reproducibility criteria**: You should be able to delete the csv
    file, and remake it simply by knitting this Rmd file.

<!-------------------------- Start your work below ---------------------------->
<!----------------------------------------------------------------------------->

## 4.2 (3 points)

Write your model object from Task 3 to an R binary file (an RDS), and
load it again. Be sure to save the binary file in your `output` folder.
Use the functions `saveRDS()` and `readRDS()`.

-   The same robustness and reproducibility criteria as in 4.1 apply
    here.

<!-------------------------- Start your work below ---------------------------->
<!----------------------------------------------------------------------------->

# Overall Reproducibility/Cleanliness/Coherence Checklist

Here are the criteria we’re looking for.

## Coherence (0.5 points)

The document should read sensibly from top to bottom, with no major
continuity errors.

The README file should still satisfy the criteria from the last
milestone, i.e. it has been updated to match the changes to the
repository made in this milestone.

## File and folder structure (1 points)

You should have at least three folders in the top level of your
repository: one for each milestone, and one output folder. If there are
any other folders, these are explained in the main README.

Each milestone document is contained in its respective folder, and
nowhere else.

Every level-1 folder (that is, the ones stored in the top level, like
“Milestone1” and “output”) has a `README` file, explaining in a sentence
or two what is in the folder, in plain language (it’s enough to say
something like “This folder contains the source for Milestone 1”).

## Output (1 point)

All output is recent and relevant:

-   All Rmd files have been `knit`ted to their output md files.
-   All knitted md files are viewable without errors on Github. Examples
    of errors: Missing plots, “Sorry about that, but we can’t show files
    that are this big right now” messages, error messages from broken R
    code
-   All of these output files are up-to-date – that is, they haven’t
    fallen behind after the source (Rmd) files have been updated.
-   There should be no relic output files. For example, if you were
    knitting an Rmd to html, but then changed the output to be only a
    markdown file, then the html file is a relic and should be deleted.

Our recommendation: delete all output files, and re-knit each
milestone’s Rmd file, so that everything is up to date and relevant.

## Tagged release (0.5 point)

You’ve tagged a release for Milestone 2.
