---
title: Transforming Data
teaching: 20
exercises: 15
---

::::::::::::::::::::::::::::::::::::::: objectives

- Learn about clustering and how it is applied to group and edit typos
- Split values from one column into multiple columns
- Manipulate data using previous cleaning steps with undo/redo
- Remove leading and trailing white spaces from cells

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How can we transform our data to correct errors?

::::::::::::::::::::::::::::::::::::::::::::::::::

## Data splitting

We can split data from one column into multiple columns if the parts are separated by a common separator (say a comma, or a space).

1. Let us suppose we want to split the `species_binomial` column into separate columns, one for genus and one for species.
2. Click the down arrow next to the `species_binomial` column. Choose `Edit Column` > `Split into several columns...`
3. In the pop-up, in the `Separator` box, replace the comma with a space (the box will look empty when you're done).
4. Important! Uncheck the box that says `Remove this column`.
5. Click `OK`. You should get some new columns called `species_binomial 1`, `species_binomial 2`, and `species_binomial 3`.
6. Notice that in some cases these newly created columns are empty (you can check by text faceting the column). Why? What do you think we can do to fix it?

The entries that have data in `species_binomial 3` but not the first two `species_binomial` columns had an extra space at the beginning of the entry. Leading and trailing white spaces are very difficult to notice when cleaning data
manually. This is another advantage of using OpenRefine to clean your data - this process can be automated.

In newer versions of OpenRefine (from version 3.4.1) there is now an option to
clean leading and trailing white spaces from all data when importing the data initially and creating the project.

:::::::::::::::::::::::::::::::::::::::  challenge

## Exercise

Look at the data in the column `coordinates` and split these values to obtain latitude and longitude. Make sure that the option for `Guess cell type` is checked and that `Remove this column` is not. Rename the new columns.

What type of data does OpenRefine assign to the new columns?

:::::::::::::::  solution

## Solution

Both new columns will appear with green text, indicating they are numeric. The option for `Guess cell type` allowed OpenRefine to guess that these values were numeric.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Undoing / Redoing actions

It is common while exploring and cleaning a dataset to make a mistake or decide to change the order of the process you wish to conduct. OpenRefine provides `Undo` and `Redo` operations to roll back your changes.

1. Click `Undo / Redo` in the left side of the screen. All the changes you have made will appear in the left-hand panel.
  The current stage in the data processing is highlighted in blue (i.e. step 4. in the screenshot below). As you click
  on the different stages in the process, the step identified in blue will change and, far more importantly, the data
  will revert to that stage in the processing.
  
  ![](fig/or362-undoredo.png){alt='OpenRefine tab for Undo/Redo actions'}

2. We want to undo the splitting of the column `species_binomial`. Select the stage just
  before the split occurred and the new `species_binomial` columns will disappear.

3. Notice that you can still click on the last stage and make the columns reappear, and toggle back and forth between these states. You can also select the state more than one steps back and revert to that state.

4. Let's leave the dataset in the state before `species_binomial` was split.

## Trimming leading and trailing whitespace

Words with spaces at the beginning or end are particularly hard for humans to identify from strings without these spaces (as we have seen with the `species_binomial` column). However, blank spaces can make a big difference to computers, so we usually want to remove them.

1. In the header for the column `species_binomial`, choose `Edit cells` > `Common transforms` > `Trim leading and trailing whitespace`.
2. Notice that the `Split` step has now disappeared from the `Undo / Redo` pane on the left and is replaced with a `Text transform on 2 cells`

:::::::::::::::::::::::::::::::::::::::  challenge

## Exercise

Repeat the splitting of column `species_binomial` exercise after trimming the whitespace.

:::::::::::::::  solution

## Solution

On the `species_binomial` column, click the down arrow next to the `species_binomial` column and
choose `Edit Column` > `Split into several columns...` from the drop down menu. Use a blank character as a separator,
as before. You should now get only two columns `species_binomial 1` and `species_binomial 2`.



:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::

## Renaming columns

We now have the genus and species parts neatly separated into 2 columns - `species_binomial 1` and `species_binomial 2`.
We want to rename these as `genus` and `species`, respectively.

1. Let's first rename the `species_binomial 1` column. On the column, click the down arrow and then `Edit column` > `Rename this column`.
2. Type "genus" into the box that appears.

:::::::::::::::::::::::::::::::::::::::  challenge

## Exercise

Try to change the name of the `species_binomial 1` column to `genus`. What problem do you encounter? How can you fix the problem?

:::::::::::::::  solution

## Solution

1. On the `species_binomial 1` column, click the down arrow and then `Edit column` > `Rename this column`.
2. Type "genus" into the box that appears.
3. A pop-up will appear that says `Another column already named genus`. This is because there is another column with the same name where we've recorded the species abbreviation.
4. You can use another name for the `species_binomial 1` or change the name of the existing `genus` column and then rename the `species_binomial 1` column.
  
  

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::


## Combining columns to create new ones

The date for each row in the data file is split in three columns: `year`, `month` and `day`. We can create a new column with the date in the format we want by combining these columns.

- Click on the menu for the `year` column and select `Edit column` > `Join columns...`.

- In the window that opens up, check the boxes next to the columns `year`, `month`, and `day`.

- Enter `-` as a separator.

- Some of the records do not have month or day values, we only know the year of collection. We can select `skip nulls` to skip over these records, so that we only build a date where we are sure it is precise.

- Select the option `Write result in new column named` and write `eventDate` as the name for the new column.

- Click `OK`
  
  ![](fig/or372-joincols.png){alt='OpenRefine window for joining columns'}

You can change the order of the columns by dragging the columns in the left side of the window.

## Data clustering

Clustering allows you to find groups of entries that are not identical but are
sufficiently similar that they may be alternative representations of the same thing (term or data value).
For example, the two strings `New York` and `new york` are very likely to refer to the same concept and just have a
capitalization difference. Likewise, `Björk` and `Bjork` probably refer to the same person. These kinds of variations
occur a lot in scientific data. Clustering gives us a tool to resolve them.

OpenRefine provides different clustering algorithms. The best way to understand how they work is to experiment with them.

The dataset has several near-identical entries in `species_binomial`. For example, there is a misspelling of *Decaspermum fruticosum* (*Decaspermum fruticosm*).

1. If you removed it, reinstate the `species_binomial` text facet (you can also remove all the other facets to gain some space).
  In the `species_binomial` text facet box - click the `Cluster` button.

2. In the resulting pop-up window, you can change the `Method` and the `Keying Function`. Try different combinations to see what different mergers of values are suggested.

3. If you select the `nearest neighbour` method and the `Levenshtein` distance function, it should identify one cluster:
  
  ![](fig/or362-clustering.png){alt='OpenRefine window for clustering'}

4. Note that the `New Cell Value` column displays the new name that will replace the value in all the cells in the
  group. You can change this if you wish to choose a different value than the suggested one.

5. Tick the `Merge?` checkbox beside each group, then click `Merge selected & Close` to apply the corrections to the dataset and close the window.

6. The text facet of `species_binomial` will update to show the new summary of the column. It will now have nine options:
  
  ![](fig/or362-clustering-result.png){alt='Facet of species_binomial after clustering'}

:::::::::::::::::::::::::::::::::::::::::  callout

## Clustering Documentation

Full documentation on clustering can be found at the [OpenRefine Clustering Methods In-depth](https://openrefine.org/docs/technical-reference/clustering-in-depth) page of the OpenRefine manual.


::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: keypoints

- Clustering can identify outliers in data and help us fix errors in bulk

::::::::::::::::::::::::::::::::::::::::::::::::::


