---
title: Advent of R Functions
author: Dr. Mowinckel
date: '2022-12-01'
slug: 2022-12-01-advent-of-r-functions
categories: []
tags:
  - Advent calendar
  - R
  - R functions 2022
image: 'featured.jpg'
---


It is advent!
And we all know by now how much I LOVE advent and Christmas. 
Keeping true to who I am and that I finally have some extra energy for things like this, this advent brings you a series of 24 pieces of code I often use in my work, and that I hope can be of interest and help to you.

## 1<sup>st</sup> of December - Creating a directory
I often find myself doing quite some file handling in my work, often leading to many of the same things happening over and over in slightly different contexts. 
If you try to create files in a directory that does not exists, R will throw an error


```r
penguins <- palmerpenguins::penguins
write.table(penguins, "new_folder/penguins.csv")
```

```
## Warning in file(file, ifelse(append, "a", "w")): cannot open file 'new_folder/
## penguins.csv': No such file or directory
```

```
## Error in file(file, ifelse(append, "a", "w")): cannot open the connection
```

So, I very often have this piece of code in any file-writing function I make to create the folder. 
But R will also throw annoying warnings if the folder already exists, which I also don't like.


```r
dir.create("new_folder") # no warning
dir.create("new_folder") # produces warning
```

```
## Warning in dir.create("new_folder"): 'new_folder' already exists
```



The solution, is to check if the directory already exists, and make it if it does not.



```r
if(!dir.exists("new_folder")) dir.create("new_folder")
```


Actually, I often end up using a little convenience function for this, since I do it quite often.


```r
dir_create <- function(x, ...){
  if(!dir.exists(x)) 
    dir.create(x, recursive = TRUE, ...)
}
dir_create("new_folder")
dir_create("new_folder")
```


Here, I can have a function to easily create new folders, with any extra arguments to `dir.create` passed along using the `...` (ellipsis), and only make the directory if it does not already exist.
This is a staple bit of code for me.
Hope it will help you get your scripts tidier!



<!-- Let's kick it off by a piece of code that I often use on our offline server, where we store and use sensitive human data.  -->
<!-- Here, I have less options in terms of R packages, so I often rely on base-R funcitonality, to avoid issues with compilers not being equal between my workflows.  -->
<!-- And if you have no idea what I just said, ignore it, be happy you don't need to deal with stuff like that! -->

<!-- Ok, so we kick of with being able to easily split a data.frame by some grouping and save them into separate files. -->
<!-- I'll first show you the entire code, then we will start breaking it down. -->

<!-- ```{r} -->
<!-- penguins <- palmerpenguins::penguins -->
<!-- save_files <- function(data, group, directory) { -->
<!--   # Create directory to place files in, called "csvs" -->
<!--   if(!dir.exists(directory)) dir.create(directory) -->

<!--   tmp <- palmerpenguins::penguins |> -->
<!--     split(group) -->

<!--   .filename <- function(data){ -->
<!--     species <- paste0(unique(data$species), ".csv") -->
<!--     species <- tolower(species) -->
<!--     file.path(directory, species) -->
<!--   } -->
<!--   names(tmp) <- sapply(tmp, .filename) -->

<!--   sapply(tmp, function(x) { -->
<!--     write.csv(x, -->
<!--               .filename(x), -->
<!--               row.names = FALSE) -->
<!--   }) -->
<!-- } -->
<!-- save_files(penguins, ~species, "csvs") -->

<!-- # Check that they are there -->
<!-- list.files("csvs", full.names = TRUE) -->
<!-- ``` -->

<!-- ### Splitting into several data.frames -->
<!-- Let start by how I'm breaking up the penguins dataset into several data.frames -->

<!-- ```{r} -->
<!-- penguins |>  -->
<!--   split(~species) -->
<!-- ``` -->

<!-- The `split()` function is super neat for things like this.  -->
<!-- When done on a data.frame, you can provide a formula to split by variables in your data.  -->
<!-- In this case, we are telling it to split by species, and therefore it produces one data.frame per species in the original dataset, each data.frame reduced to rows only containing that species.  -->
<!-- Really neat, if you ask me! -->

