# Lab 3: Reading, inspecting, and writing data

<!-- NOTE: 
You can preview this README.md document by clicking the 'Preview' button in the RStudio toolbar. 
-->

## Preparation

- Read/ annotate: [Recipe \#3](https://lin380.github.io/tadr/articles/recipe_3.html). You can refer back to this document to help you at any point during this lab activity.

## Objectives

- Read datasets from packages and from plain-text files
- Inspect and report characteristics of datasets
- Write datasets to a plain-text file (.csv or .tsv)

## Instructions

### Setup

1. Create a new R Markdown document. Title it "Lab #3" and provide add your name as the author. 
2. Edit the front matter to have rendered R Markdown documents print pretty tabular datasets: 

```yaml
output: 
  html_document: 
    df_print: kable
```

3. Delete all the material below the front matter.
4. Add a code chunk directly below the header named 'setup' and add the code to load the following packages
  - tidyverse
  - tadr

### Tasks

1. Create two level-1 header sections named: "Package data" and "Local data".
2. Follow the instructions that follow adding the relevant prose description and code chunks to the corresponding sections.

*Remember:*

- Add code comments (`# code comments...`) to your code lines to clarify what each step of your code does. 
- Use Markdown syntax as necessary to format your responses
- You can use keyboard shortcuts inside code chunks/ the R Console such as
  - ⌥ - ('option + -') for the `<-` operator
  - ⇧⌘M ('shift + command + M') for the ` %>% ` operator
  - ⇥ ('tab') for code completion hints
  

**Package data**

You've already loaded the tadr package in the R session, now inspect the `swda` dataset. Apply your conceptual and R programming knowledge to address the following points: 

- Describe the type of data that is represented in the `swda` dataset. 
- Provide an overview ('glimpse' hint, hint) of the structure of the dataset (rows, columns, column types).
- Describe what the values in `dialect_area` represent. 
- Subset the dataset and only select the `doc_id`, `speaker_id`, `sex`, and `dialect_area` columns. Assign the result to a new object (`swda_sex_dialect`). 
- Show the first 10 rows of the `swda_sex_dialect` object. (Use `slice_head`.)
- Find out how many women versus men there are in this corpus. (Isolate the rows for which there are distinct value pairs for `speaker_id` and `sex`, group by `sex`, and then count the grouped rows.)
- Find out which dialect area has the most represented speakers in this corpus. (Isolate the rows for which there are distinct value pairs for `speaker_id` and `dialect_area`, group by `dialect_area`, and then counting the grouped rows. You may want to arrange the results so that the largest counts (`n`) appear in descending order.)


**Local data**

Now we are going to read a local dataset in plain-text format (`endangered_languages.csv`) into an R session. This dataset is in the `data/` directory. Apply your conceptual and R programming knowledge to address the following points:

- Read the `endangered_languages.csv` dataset in your R session and assign it to a new object (`end_langs`).
- Provide an overview of the structure of the dataset (rows, columns, column types). 
- Visit [the source of this data](https://www.theguardian.com/news/datablog/2011/apr/15/language-extinct-endangered). Describe what this dataset represents. 
- Find the distinct values for the `degree_of_endangerment` column.
- Based on the source site, briefly describe what each value of `degree_of_endangerment` means.
- Filter the dataset to only return the 'Vulnerable' languages and assign the result to a new object (`end_langs_vulnerable`). 
- With the new object (`end_langs_vulnerable`), identify the five 'Vulnerable' languages with the *most* speakers in the `end_langs` dataset.
- Write the `end_langs_vulnerable` object to a plain-text file (.csv or .tsv) in the `data/` directory. Give it the same name as the object with the extension (.csv or .tsv).  
- Copy and paste the following code into a new code chunk and provide some interpretation of what this plot shows or suggests. 

```r
end_langs_factor <-
  end_langs %>%
  mutate(degree_of_endangerment = factor(
    degree_of_endangerment,
    levels = c(
      "Vulnerable",
      "Definitely endangered",
      "Severely endangered",
      "Critically endangered",
      "Extinct"
    )
  ))
world_map <-
  map_data(map = "world") %>%
  filter(region != "Antarctica")

ggplot(data = world_map, aes(long, lat)) +
  geom_polygon(aes(group = group), fill = "white", color = "grey50") +
  geom_point(
    data = end_langs_factor,
    aes(x = longitude,
        y = latitude,
        color = degree_of_endangerment),
    size = .5
  ) +
  scale_color_discrete(direction = -1) +
  theme(legend.position = "bottom", legend.direction = "vertical") +
  labs(
    title = "Endangered Languages",
    y = "",
    x = "",
    color = "Degree of endangerment"
  )
```

### Assessment

Add a section which describes your learning in this lab.

Some questions to consider: 

  - What did you learn?
  - What was most/ least challenging?
  - What resources did you consult? 
  - What more would you like to know about?

## Submission

1. To prepare your lab report for submission on Canvas you will need to Knit your R Markdown document to PDF or Word. 
  - Note: you will have to add the front matter line to pretty-print tables under the `pdf_document:` or `pdf_document2:` (if you want cross-references to tables or figures) output. 

```yaml
output:
  pdf_document:
    df_print: kable
```
  
2. Download this file to your computer.
3. Go to the Canvas submission page for Lab #3 and submit your PDF/Word document as a 'File Upload'. Add any comments you would like to pass on to me about the lab in the 'Comments...' box in Canvas.
