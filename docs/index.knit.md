---
title: "Project 3 Focus Group Participant Survey"
author: "WB"
date: "2024-10-31"
output:
  html_document:
    toc: yes
    toc_float: yes
    number_sections: yes
    theme: lumen
    highlight: tango
    df_print: paged
    code_folding: hide
  pdf_document:
    toc: yes
---
<style>
.custom-box {
  border: 1px solid #b3cde0;
  background-color: #e0f3f8;
  padding: 15px;
  border-radius: 5px;
}
</style>

# Introduction 
This tutorial is an introduction to the most basic types of analysis you will undertake on survey data. It assumes a basic understanding of R, which can be achieved through many of the beginner courses you can find online like [CodeAcademy](https://www.codecademy.com/learn/learn-r&ved=2ahUKEwiDj5uJg5GJAxXWEzQIHf6JF2cQFnoECBcQAQ&usg=AOvVaw0UG42DTYemHsqsia3tBd2k) or [DataCamp](https://www.datacamp.com/courses/free-introduction-to-r&ved=2ahUKEwiDj5uJg5GJAxXWEzQIHf6JF2cQFnoECBgQAQ&usg=AOvVaw3T3T58_k6QXIeZSZKbBlDD). Once you have learned the basic structure of syntax in R and the most common functions you'll be using during an analysis project, we can begin to layer our theoretical understanding on how to undertake analysis. In particular, hopefully you can become familiar with the %>% operator as well, since this is critical for our understanding of how to structure syntax. 

There are many methods on how to approach analyses from cleaning to data presentation/visualization. Most are great but I will just present the way I was taught and how I tackle datasets. The overarching stages can be categorized into checking --> cleaning <-> analysis -> presentation. 

# Checking

## Setting up our R session

Here we set up the environment for our R session by loading all the relevant libraries (packages) used to clean, sort and analyze our data. I always start with a few key packages that I *know* I will use during my session however, this list is not exhaustive. If this is your first time loading these packages, **you will have to install them**. You should also try to update them regularly.



Always remember to come back to this section and add new packages that you have loaded during your exploration and analyses to ensure that when you close this R session and come back to it, your code still works! rm(list = ls()) is just a bit of code that clears your environment that I start off with when I switch between projects!

Now we set our working directory to call our file wherever we saved it. There are many different packages and options for loading in the dozens of file types you will encounter. Excel datasheets can be saved in a range of different formats (xlsx, xls, csv etc) but I often default to csv. I use the read.csv command (from the base version of R) to read in the data into a dataframe I call "df_raw". 


``` r
setwd("/Users/will/Documents/UCSD/Project 3/Aim 1/Survey/P3Tutorial/docs")
df_raw <- read.csv("FGsurvey.csv", header = T, skip=1) %>% 
  slice(-1)
```
I'll usually open the .csv file before loading it in to give it a quick look-over and make sure it is the correct file (collaborators are notorious for sending the wrong files) and get a feel for the data structure. I have included some additional options in the above code as well that you can see to tailor our raw dataframe: header = T ensures that the first row of the .csv will be used as the column headers; skip=1 just skips reading in the first row of the dataset, which in our case was doubling up on the column names since that was included as a separate row (row 1). When looking at our .csv file, we could also see that row 2 seemed to be an artifact from exporting the qualtrics survey, which is something we do not need in our dataframe. Normally, I would have said 'skip=2' to skip both of the first two rows but it was throwing an error, and rather than spending time troubleshooting that, I can simply pipe-in a new command to drop that row from our new dataframe 'df_raw'. 

## Stop, check and think

In R Studio, in the top right corner you can see in our "Environment" tab our imported data within the new dataframe called "df_raw". **Our first job is to look at whether this new dataframe makes sense**. It says there are 29 observations of 29 variables. Frequently we will have some intuition on whether this seems reasonable for our dataset; we know from our interviews that there are a confirmed 29 respondents to this survey, so this seems reasonable. R Studio makes this easy by providing functionality to visualize dataframes/tables so we can quickly inspect our data. This can be achieved using the 'View()' command. For data that contains few respondents or observations (<1000), this is a quick and easy way to get a quick insight into our data. It is similar to viewing it within Excel.

Another check is to look at all the variable names included within the dataset. We can do this with the ls() function.

``` r
ls(df_raw)
```

```
##  [1] "Distribution.Channel"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               
##  [2] "Duration..in.seconds."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              
##  [3] "End.Date"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
##  [4] "External.Data.Reference"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            
##  [5] "Finished"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
##  [6] "How.long.have.you.been.in.your.current.role."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
##  [7] "IP.Address"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
##  [8] "Location.Latitude"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
##  [9] "Location.Longitude"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
## [10] "Please.enter.your.date.of.birth..MM.DD.YYYY.."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
## [11] "Please.enter.your.first.name"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
## [12] "Please.enter.your.last.name"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        
## [13] "Please.indicate.your.ethnicity.below."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              
## [14] "Please.select.your.race.from.the.options.below....More.than.one.race..please.specify....Text"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       
## [15] "Please.select.your.race.from.the.options.below....Other..please.specify....Text"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
## [16] "Please.select.your.race.from.the.options.below....Selected.Choice"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                  
## [17] "Progress"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
## [18] "Recipient.Email"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    
## [19] "Recipient.First.Name"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               
## [20] "Recipient.Last.Name"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                
## [21] "Recorded.Date"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
## [22] "Response.ID"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                        
## [23] "Response.Type"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
## [24] "Start.Date"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         
## [25] "User.Language"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                      
## [26] "What.is.your.current.position.job.title."                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                           
## [27] "Which.gender.best.describes.you....Other.gender.category..please.specify....Text"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   
## [28] "Which.gender.best.describes.you....Selected.Choice"                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 
## [29] "You.are.being.invited.to.participate.in.a.research.study.titled.Developing.and.Testing.a.Team.Communication.Training.Implementation.Strategy.for.Depression.Screening.in.a.Pediatric.Health.Care.System..This.study.is.being.led.by.Drs..Nicole.Stadnick..Ph.D...MPH.and.Lauren.Brookman.Frazee.Ph.D..from.UC.San.Diego..with.collaboration.from.Drs..Amy.Bryl.M.D..and.Cynthia.Kuelbs.M.D..from.Rady.Children.s.Hospital.San.Diego....You.were.selected.to.participate.because.of.your.role.as.a.pediatric.medical.specialty.unit.physician..nursing.provider..medical.assistant.or.behavioral.health.care.team.member..The.purpose.of.this.study.is.to.understand.the.challenges.and.supports.of.depression.care.in.pediatric.health.care.systems....Do.you.agree.to.participate..Selecting..yes..will.lead.you.to.a.few.questions.about.your.background..Selecting..no..will.take.you.to.the.end.of.this.survey."
```

Here it returns 29 entries, which makes sense since as we noted in the environment box top right, we have 29 variables. This is important as we can a) see the names of the variables we'll need to be calling for our analysis; b) recognize that these names are in confusing forms for quick analysis i.e., do we really want ot have to type out "What.is.your.current.position.job.title." every time we want to include the position title in a check or analysis. We can make a mental note of this and return later to clean these variable names into a more usable format. 

## Important note on survey software
Most survey software will have in-built variables they automatically collect when a response is recorded. This means that even though you didn't explicitly code these questions into a Qualtrics survey, it will still collect these data and make them available to you within your exported dataset. We can see here, for example, there are variables that we didn't code during the survey design including 'distribution channel', 'duration in seconds', 'IP Address', 'longitude and latitude'. These data can often be useful to us (IP Address is useful for checking the authenticity of responses for public facing surveys that can be spammed by bots) HOWEVER, these can also create questions of privacy for a participant. Something to be aware of. Moving on...

## Peeking at our data

We can also use the head() or tail() command, which gives a quick snippet of the first (head) or last (tail) 6 values for the called dataframe. This is useful for datasets that get into the hundreds, thousands, tens or even hundreds of thousands where viewing the whole dataset is both cumbersome and not useful. Here I called the the head and tail values for the variable Start.Date, which we can assume is the date in which they started filling out the survey. *In reality, we should never assume what any variables or values represents and should ALWAYS make sure collaborators give us their codebook.*

``` r
head(df_raw$Start.Date)
```

```
## [1] "2024-07-24 15:36:33" "2024-07-25 12:16:09" "2024-07-26 10:14:18"
## [4] "2024-07-26 10:04:30" "2024-07-26 10:17:56" "2024-07-26 10:19:00"
```

``` r
tail(df_raw$Start.Date)
```

```
## [1] "2024-08-09 14:02:30" "2024-08-09 15:52:13" "2024-08-19 17:05:15"
## [4] "2024-08-23 12:48:59" "2024-08-23 13:03:56" "2024-08-23 13:04:01"
```

This is a good second-step to checking whether the data makes sense. All these dates look reasonable for when the focus groups were held but what if we had an entry that said 2023? We'd then do a deeper dive into what happened. For now, look at the heads/tails of a few more variables and if all seems reasonable, we can move on.

Our actions at this step may vary significantly depending on our understanding of the project, field and dataset. For us, we understand that there are only supposed to be 29 respondents and the dates were reasonable, but this is because we (you) were there and can verify it. This will not always be the case with data you are cleaning so it is important to think deeply about what you are looking at. 

There are a number of reasons why weird things can occur in our dataset that would make us intervene at different stages: during exporting from survey software missing values get introduced, corrupted data during changing of file formats, even perhaps someone else made an error during the cleaning process before we recieve this data! Be aware, and always pester those who sent you data about inconsistencies. 

# Cleaning

## Value of a codebook
Cleaning a dataset is perhaps the most critical aspect of an analysis project. Doing it correctly from the get-go will save you a lot of time, effort and ensure reproducibility. I will only provide a brief introduction here as I am very much a 'learn by doing' type operator, especially since there are infinite ways to approach cleaning that will vary from person-to-person. 

Ideally, we would have access to a codebook, which allows us to fully understand what questions were asked in the dataset, and what the answers were coded as within our dataset. This allows us to accurately assign all the data to recognizable and interpretable values for our own purposes. Importantly, this also allows us to see which questions are asked as part of a 'set' so we can properly categorize and score these questions. 

## Kick-off
I almost always start by cleaning data that is common to basically all datasets: demographics. Fortunately, this survey is basically *all* demographics so let's explore some of the common demographics collected in surveys such as gender identity, age, ethnicity/race etc. I often first tabulate or summarize these data using the 'table' or dplyr functions as it gives a nice, concise visual of all the possible values for each question.

Firstly, I always create a **new** dataframe called 'df', which is where all the cleaning will take place. This allows use to a) reset our dataframe by reassigning 'df <- df_raw' at any time if we somehow make an error; b) ensure we maintain an unedited raw dataframe for reference throughout the process.

``` r
# Asign new dataframe 
df <- df_raw
# Filter out empty observations
df <- df %>% 
  dplyr::filter(Finished=="True")
```
Secondly, the 'filter' command is extremely powerful to help in both cleaning AND analysis. Here, I asked R to filter our dataset according to whether a respondent's value for 'Finished' (another automatically generated variable by Qualtrics) equaled false, indicating that they DID complete the survey. Since every respondent did complete the survey this was not necessary but in future analyses, you may have **a priori** rationale to exclude incomplete responses. This is one-of-many judgement calls you'll make as a scientist analyzing datasets. 

## Variable manipulation

``` r
# Convert date of birth variable into usable format
df$dob <- mdy(df$Please.enter.your.date.of.birth..MM.DD.YYYY..)

# Convert date of birth to age in years
df <- df %>%
    mutate(age = floor(interval(dob, Sys.Date()) / years(1)))

# Table of age group responses
head(df$age)
```

```
## [1] 24 33 26 46 29 29
```
So, we did not have an age variable ready-to-go but with a few quick steps we created one. First, we had to convert our date-of-birth variable, which appeared as a character variable, into a usable format. I used a function 'mdy' from the lubridate package. You can assign dates in a variety of formats but we chose month, day, year to be consistent with American standards. 

Then we used the 'mutate' function to create a new variable called age, which was derived by creating an interval between today's date (Sys.Date()) and our generated date-of-birth variable (dob), converted into years (/ years(1)). floor() just ensures the result is rounded down to the nearest whole year. Take a quick peek at your newly created variable to make sure it looks good before moving on. 

<div class="custom-box">
Another aspect to the above code you may have noticed is that I called the package name 'dplyr' before I called the function 'filter'. This is a great habit to get in if you can as oftentimes different packages will use the same name for a function. As an example, 'filter' appears across the package dplyr **and** stats, both are fairly common packages that you may load into your analysis pipelines at some point, and this WILL create conflicts unless you specific the filter from the package you want. </div>

## Visualize
Now we can present our data:

``` r
# Median
df %>% 
  summarise(
    median_age = median(age, na.rm = TRUE),
    iqr_age = IQR(age, na.rm = TRUE)
  ) %>% 
  print()
```

```
##   median_age iqr_age
## 1         41      15
```

``` r
# Boxplot
boxplot(df$age)
```

<img src="index_files/figure-html/Clean and summarize-1.png" width="672" />
We present our age variable at its median (41 years) with interquartile range (15 years) using the summarise & print function. The boxplot function from base R also allows us to easily visualize this along with any skewness. Age is a good example of a variable that you will routinely see as skewed, which depending on the context of the data, can be totally fine or totally catastrophic! *If you receive data from a supposed randomized controlled trial and found that age is skewed, you may have some serious concerns!!!*

## Renaming
Before we proceed with any further processing, I often like to rename variables to make them more intuitive and manageable. One idiosyncrasy from importing the csv file in the way that we did was that variable names have periods in lieu of spaces (R does not allow for spaces in variable names). I prefer one word variable names so will assign these variables new names with the dplyr function 'rename'. 

``` r
df <- df %>% 
  dplyr::rename(gender = Which.gender.best.describes.you....Selected.Choice,
                race = Please.select.your.race.from.the.options.below....Selected.Choice,
                race_text1 = Please.select.your.race.from.the.options.below....More.than.one.race..please.specify....Text,
                race_text2 = Please.select.your.race.from.the.options.below....Other..please.specify....Text,
                ethnicity = Please.indicate.your.ethnicity.below.,
                position = What.is.your.current.position.job.title.,
                length = How.long.have.you.been.in.your.current.role.,
                consent = You.are.being.invited.to.participate.in.a.research.study.titled.Developing.and.Testing.a.Team.Communication.Training.Implementation.Strategy.for.Depression.Screening.in.a.Pediatric.Health.Care.System..This.study.is.being.led.by.Drs..Nicole.Stadnick..Ph.D...MPH.and.Lauren.Brookman.Frazee.Ph.D..from.UC.San.Diego..with.collaboration.from.Drs..Amy.Bryl.M.D..and.Cynthia.Kuelbs.M.D..from.Rady.Children.s.Hospital.San.Diego....You.were.selected.to.participate.because.of.your.role.as.a.pediatric.medical.specialty.unit.physician..nursing.provider..medical.assistant.or.behavioral.health.care.team.member..The.purpose.of.this.study.is.to.understand.the.challenges.and.supports.of.depression.care.in.pediatric.health.care.systems....Do.you.agree.to.participate..Selecting..yes..will.lead.you.to.a.few.questions.about.your.background..Selecting..no..will.take.you.to.the.end.of.this.survey.)
```
As you can see, some of the previous variables names were extremely long, riddled with periods. Different fields will have different ideas about what makes a good variable name but my preference is just to pick something that is short AND descriptive. Your process for this will depend on the particular project: some people insist that best-practice is to rename every variable in the dataset at the beginning to be consistent; I basically never do that as 1) I rarely use every variable in a dataset, 2) I should have an analysis plan before I commence data analysis that tells me what variables I am using, so would just rename the ones I will use, 3) value my time and sanity.

## Race & Ethnicity
Without going off on too much of a tangent, it is important to recognize challenges and limitations associated with the collection of race and ethnicity data. This exercise is largely concerned with how we process data post-collection but I think it is always useful to consider the design and collection phases also. So, why do we collect race and/or ethnicity data for a research project? These questions are going to vary based on the cultural context, policies and laws, and ultimately your research aims. These are both commonly included in many surveys however, there is always discussion around whether to include one over the other, both or neither. Aside from the above points, always worth remembering that many people's literacy around the difference between race and ethnicity can be quite low, which means the quality of your data may not be stellar. Perhaps we will see an example below...

First, let's look at all the responses to the first race question, which asked people to choose from a list of options:

``` r
race_table <- as.data.frame(table(df$race))
race_table <- race_table %>% dplyr::mutate(Perc = Freq/sum(Freq)*100) %>% 
  dplyr::arrange(desc(Freq))

race_table %>%
  kable(format = "html", caption = "Number of responses by race") %>%
  kable_paper(bootstrap_options = c("striped", "hover", "condensed", "responsive", full_width = F, position = "left"))
```

<table class=" lightable-paper" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; margin-left: auto; margin-right: auto;'>
<caption>Number of responses by race</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Var1 </th>
   <th style="text-align:right;"> Freq </th>
   <th style="text-align:right;"> Perc </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> White </td>
   <td style="text-align:right;"> 13 </td>
   <td style="text-align:right;"> 44.8 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Asian </td>
   <td style="text-align:right;"> 7 </td>
   <td style="text-align:right;"> 24.1 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Other, please specify: </td>
   <td style="text-align:right;"> 3 </td>
   <td style="text-align:right;"> 10.3 </td>
  </tr>
  <tr>
   <td style="text-align:left;">  </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 6.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> More than one race, please specify: </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 6.9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Prefer not to answer </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 6.9 </td>
  </tr>
</tbody>
</table>
We can see that the first table looks different, more processed than ones that we used previously. This is because we are using 'kable' from the kable and kableExtra package, which allows us to produce much nicer looking tables with far more customizability. The way I often approach making kable tables is by first creating a new dataframe from a table of the variable of interest, which can be seen on the first line. This creates a dataframe that computes the counts of each response within that variable, and I have included extra code to calculate the percentage of each response using the mutate function. I have also piped in an 'arrange' command to order the counts of the responses in descending order. Then I rename the columns, using the base R function of colnames, to something descriptive - in the previous step, R automatically created a column in our dataframe called 'Freq' to describe our count data but I prefer Count. Finally, we arrive at the actual kable syntax. There are a lot of customization options here so I won't describe them all but feel free to use the ?kable documentation or see many of the online explanations. 

A few obvious things here: 

  1) White respondents make up just under half the total sample. 
  2) Two respondents preferred not to answer. 
  3) Multiple people selected either 'Other' or 'More than one race'.
  4) Two people skipped this question entirely. This is represented in our table as a row without a title.  
  
Let's start with 4...

## Missingness
Missing responses can be a serious concern for us during analysis by contributing to what is known as 'non-response' bias. Certain 'groups' (groups here is referring to any grouping variable e.g., age, gender, language spoken, number of cars owned etc) within our data may skip certain questions at higher frequencies where other 'groups' may not. Perhaps those who self-identify as black often skip this question (in this case, we can see in our data that it was 2 Medical Assistant's who identified as Hispanic ethnicity that skipped). This can cause us to make incorrect inferences during analysis, which can undermine our entire study! 

To be clear, we are not doing any statistical analysis that would be affected by missing data. In our case, we don't know why these people skipped. It may be that they also preferred not to answer but decided to just skip instead of selecting that answer (maybe they didn't read it), and in that case we could make a decision to combine those two rows. I will leave it up to you to decide whether you think this would be an appropriate way to manipulate our data. 

At the survey design stage we should do everything in our power to avoid skippable questions however, we also have to weigh the potential for people to completely drop-out of a survey if they cannot skip a question! Being unable to skip a question also means less flexibility within your survey for if things go wrong e.g., what if for whatever reason, no options applied to a respondent? what if there was a technical issue with the question? what if they don't beleive in the concept of race? These are critical things to think about during survey design and can help avoid challenges later down the track. Ultimately, if a question is skippable, was it really that critical of a data point for you to capture?

If you're interested in learning more about missingness in data, there are entire fields of statistics dedicated to this that you can engage with. I frequently use multiple imputation to account for missing data where I believe it is non-randomly missing. 

Let's move on to 3...

## Text responses
Requiring people to manually input text in your survey is always a gamble. If you spend any amount of time cleaning and analyzing survey data, you'll see some weird, wonderful, and frustrating things. Most commonly, you will simply encounter people misspelling things, which makes automating the cleaning process increasingly difficult but at least you know exactly how to treat the data (you can simply fix the spelling error). Similar with trolling: you can simply remove that datapoint if it is a silly or offensive response. Things get increasingly difficult when someone writes out a response that doesn't quite make sense. Let's take a look at the text response for our race questions.

``` r
print(df$race_text1[df$race_text1 != ""])
```

```
## [1] "Native/Italian "
```

``` r
print(df$race_text2[df$race_text2 != ""])
```

```
## [1] "mexican"   "Hispanic "
```
Above are the results from telling R to 'print' the responses for those variables that are not empty (represented by !=""). Here we can see what we talked about previously in that many people do not understand differences between race and ethnicity. We can see that in both text questions 1 and 2, three people give answers that we wouldn't usually consider 'races': Mexican is a nationality, not a race; Hispanic is an ethnicity, not a race; "Native" certainly could be a race but Italian definitely is not (this final entry also emphasizes another complication to text responses in that the respondent has put two answers in one question, which we'll later need to separate out). One more important aspect is that one person who selected 'Other, please specify' and another who selected 'More than one race' did not specify in-text. 

These problems arose from only a sample of 29. Imagine how messy text responses can get when you have tens of thousands of respondents! I mention this not to completely dissuade you from using text responses in a survey; sometimes text responses are extremely useful, even necessary. Text responses within surveys however, can sometimes highlight an aspect of data collection that is counterintuitive: in an attempt to force specificity from respondents, we can sometimes muddy our own data and make things less clear than if we'd provided fewer chances for specificity.

What can we do about this then?

``` r
df %>%
  dplyr::select(race) %>%
  print()
```

```
##                                   race
## 1                                White
## 2                                White
## 3                                Asian
## 4                                White
## 5                                     
## 6               Other, please specify:
## 7                                White
## 8               Other, please specify:
## 9                                Asian
## 10 More than one race, please specify:
## 11                               White
## 12                               White
## 13                Prefer not to answer
## 14                               White
## 15                               Asian
## 16                Prefer not to answer
## 17                               White
## 18                               White
## 19                                    
## 20                               Asian
## 21                               Asian
## 22                               White
## 23              Other, please specify:
## 24                               White
## 25 More than one race, please specify:
## 26                               White
## 27                               White
## 28                               Asian
## 29                               Asian
```
I made another important decision for how we manipulate our data that could have significant effects during analysis: I chose to simply ignore the additional text responses to race and recode them simply as 'Other'. I made this decision based on a couple points: 
    1) Two of the three respondents who selected 'Other' gave text responses that I do not consider a valid option for 'race'. 
    2) One of the three respondents did not specify at all. 

We had the option of excluding these datapoints entirely however, these respondents saw the options and conciously chose 'Other', so we have to assume that there is a reason they did not select the other options, nor skip the question. Ultimately, this is a judgement call that there is often no 'perfect' answer to. We made a similar decision with 'more than one race'.

For this process, we utilized the 'recode' function from the dplyr package to adjust the values. Interestingly, you can see that we also added in the title "missing" for the missing values. Recode doesn't let us assign a value to "" (which is often how we'd tell R there is an empty value) so we use a little work around by first converting the empty cell to NA, then using the function replace_na
to reassign it the value "Missing".

``` r
eth_table <- as.data.frame(table(df$ethnicity)) 

eth_table <- eth_table %>% 
  mutate(Perc = Freq/sum(Freq)*100) %>% 
  arrange(desc(Freq))

colnames(eth_table) <- c("Ethnicity", "Count", "%")

eth_table %>%
  kable("html", caption = "Number of responses by ethnicity") %>%
  kable_paper(bootstrap_options = c("striped", "hover", "condensed", "responsive", full_width = F, position = "left"))
```

<table class=" lightable-paper" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; margin-left: auto; margin-right: auto;'>
<caption>Number of responses by ethnicity</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Ethnicity </th>
   <th style="text-align:right;"> Count </th>
   <th style="text-align:right;"> % </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> Non-Hispanic </td>
   <td style="text-align:right;"> 17 </td>
   <td style="text-align:right;"> 58.6 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Hispanic </td>
   <td style="text-align:right;"> 10 </td>
   <td style="text-align:right;"> 34.5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> Prefer not to answer </td>
   <td style="text-align:right;"> 2 </td>
   <td style="text-align:right;"> 6.9 </td>
  </tr>
</tbody>
</table>
Ethnicity is much more straightforward and contains no missing responses. This is shows the advantage of presenting fewer options to respondents.

What about length?

``` r
length_table <- as.data.frame(table(df$length))

length_table$Var1 <- factor(length_table$Var1, levels = c("< 1 year", "1-3 years", "3-5 years", "5-10 years", "10+ years"), labels = c("< 1 year", "1-3 years", "3-5 years", "5-10 years", "10+ years"))

colnames(length_table) <- c("Length", "Count")

length_table %>%
  arrange(Length) %>% 
  kable("html", caption = "Length of time in current role") %>%
   kable_paper(bootstrap_options = c("striped", "hover", "condensed", "responsive", full_width = F, position = "left"))
```

<table class=" lightable-paper" style='font-family: "Arial Narrow", arial, helvetica, sans-serif; margin-left: auto; margin-right: auto;'>
<caption>Length of time in current role</caption>
 <thead>
  <tr>
   <th style="text-align:left;"> Length </th>
   <th style="text-align:right;"> Count </th>
  </tr>
 </thead>
<tbody>
  <tr>
   <td style="text-align:left;"> &lt; 1 year </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 1-3 years </td>
   <td style="text-align:right;"> 9 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 3-5 years </td>
   <td style="text-align:right;"> 4 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 5-10 years </td>
   <td style="text-align:right;"> 5 </td>
  </tr>
  <tr>
   <td style="text-align:left;"> 10+ years </td>
   <td style="text-align:right;"> 7 </td>
  </tr>
</tbody>
</table>
This code looks different to previous lines because I converted the length_table dataframe to a factor variable. Sometimes this is necessary to ensure we can properly order the table; if we don't do this, the row orders of the table appear random. 

'Position' needs a decent bit of clean-up due to its text-based response but since we're likely not going to report their positions verbatim, we can do a rough clean-up for now just to get the basics of regex down. Regex is a portmanteau of 'regular expression', which refers to the sequence of characters we use to match patterns within strings. This helps us mine large swatches of text data, manipulate it and then present it.

``` r
df <- df %>%
  mutate(
    position_category = case_when(
      str_detect(position, regex("nurse|outpatient RN|RN", ignore_case = TRUE)) ~ "nurse",
      str_detect(position, regex("physician|fellow|Director", ignore_case = TRUE)) ~ "physician",
      str_detect(position, regex("manager|administrative", ignore_case = TRUE)) ~ "administrative",
      str_detect(position, regex("professor|prof", ignore_case = TRUE)) ~ "professor & physician",
      str_detect(position, regex("medical|CMA", ignore_case = TRUE)) ~ "medical assistant",
      TRUE ~ "other"  # Assign 'other' for any positions that don't match
    )
  )
table(df$position_category)
```

```
## 
##        administrative     medical assistant                 nurse 
##                     4                     7                     5 
##             physician professor & physician 
##                     7                     6
```
This block of code contains quite a few different parts. First, we use 'mutate', as we have previously, to create a new variable called "position_category". We then use 'case_when' (from dplyr package) and 'str_detect' (from the 'stringr' package) to tell R to assign new values (represented by ~ " ") within this new variable by matching strings within the old variable 'position'. 'regex' (I believe is a base R function) simply tells R that we are using regular expressions within the parentheses; ignore_case=TRUE is useful to tell R not to be pedantic about capitalizations, of which we have inconsistent capitalizations within our responses. Finally, the vertical '|' sign is used commonly to denote or/either i.e., we accept a match for either/or response. The ampersand sign would be used as it would in English to denote that we require BOTH strings to fully match to satisfy a match. I usually also add-in a bit of a failsafe to create an 'other' value that if certain values do not match any of our strings, it will be assigned other. Fortunately, our regex captures all the possible values and assigns them accordingly. 

For our first example, we're simply tell R to assign any strings in the old variable 'position' that match "nurse or outpatient RN or RN" to the new value of "nurse" in our new variable 'position category'. 

So, we've cleaned all our sociodemographics and want to present them. Here is my go-to option:

``` r
df$length <- factor(df$length, levels = c("< 1 year", "1-3 years", "3-5 years", "5-10 years", "10+ years"))

label(df$age) <- "Age (years)"
label(df$ethnicity) <- "Ethnicity"
label(df$length) <- "Time in current position<sup>a</sup>"

footnote1 <- "<sup>a</sup> 'Time was queried categorically."

table1::table1(~ age + ethnicity + length, data=df, topclass="Rtable1-zebra", caption = "Table 1. Focus group participant demographics.", footnote = c(footnote1), render.continuous=c(.="Median [Min, Max]"))
```

```{=html}
<div class="Rtable1"><table class="Rtable1-zebra"><caption>Table 1. Focus group participant demographics.</caption>

<thead>
<tr>
<th class='rowlabel firstrow lastrow'></th>
<th class='firstrow lastrow'><span class='stratlabel'>Overall<br><span class='stratn'>(N=29)</span></span></th>
</tr>
<tfoot><tr><td colspan="2" class="Rtable1-footnote"><p><sup>a</sup> 'Time was queried categorically.</p>
</td></tr></tfoot>
</thead>
<tbody>
<tr>
<td class='rowlabel firstrow'>Age (years)</td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Median [Min, Max]</td>
<td>41.0 [24.0, 61.0]</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Missing</td>
<td class='lastrow'>2 (6.9%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Ethnicity</td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>Hispanic</td>
<td>10 (34.5%)</td>
</tr>
<tr>
<td class='rowlabel'>Non-Hispanic</td>
<td>17 (58.6%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>Prefer not to answer</td>
<td class='lastrow'>2 (6.9%)</td>
</tr>
<tr>
<td class='rowlabel firstrow'>Time in current position<sup>a</sup></td>
<td class='firstrow'></td>
</tr>
<tr>
<td class='rowlabel'>< 1 year</td>
<td>4 (13.8%)</td>
</tr>
<tr>
<td class='rowlabel'>1-3 years</td>
<td>9 (31.0%)</td>
</tr>
<tr>
<td class='rowlabel'>3-5 years</td>
<td>4 (13.8%)</td>
</tr>
<tr>
<td class='rowlabel'>5-10 years</td>
<td>5 (17.2%)</td>
</tr>
<tr>
<td class='rowlabel lastrow'>10+ years</td>
<td class='lastrow'>7 (24.1%)</td>
</tr>
</tbody>
</table>
</div>
```
The table1 function comes from the table1 package, and it is an incredibly simple and elegant function; I think of table1 similarly to how one thinks back on their first love. With this functionality, you will rarely need to fiddle around in MS Word trying to make your table 1 look nice. We can customize the row colors, names, positioning, types of summary statistics, stratify our entire table... it goes on and on. Refer to a comprehensive tutorial [here](https://cran.r-project.org/web/packages/table1/vignettes/table1-examples.html) if you are interested!

Next time, we can start using ggplot. If table1 was our first love, then ggplot is seeing the birth of your child: everything changes, and you can never see the world the same again. 
