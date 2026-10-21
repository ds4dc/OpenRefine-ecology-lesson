---
title: Looking Up Data
teaching: 15
exercises: 0
---

::::::::::::::::::::::::::::::::::::::: objectives

- Use URLs to fetch data from the web based on columns in an OpenRefine project
- Add columns to parse JSON data returned by web services

::::::::::::::::::::::::::::::::::::::::::::::::::

:::::::::::::::::::::::::::::::::::::::: questions

- How do I fetch data from an Application Programming Interface (API) to be used in OpenRefine?
- How do I reconcile my data by comparing it to authoritative datasets

::::::::::::::::::::::::::::::::::::::::::::::::::

## Looking up data from a URL

OpenRefine can retrieve data from URLs. This can be used in various ways, including looking up additional information from a remote service, based on information in your OpenRefine data. 

Up till now we have been working with scientific names, now we switch to working with the names of the people that collected the specimen, as found in the `recordedBy` column. If you add a text facet on this column you will see that there are many different ways that personal names have been abbrevatiated, and many different ways that individual names have been joined together in a team. 

Luckily the Bionomia project (https://bionomia.net) has a service that parses the recordedBy values and returns the different parts of the name (title, given name, family name etc) for each member of the recordedBy team. You can try it out with data that you type or paste into a webpage here: https://bionomia.net/parse

Instead of us copying and pasting data into this useful utility, we can use Open Refine to send our data values to the service and record the results.

Typically this is a two step process, firstly a step to retrieve data from a remote service, and secondly to extract the relevant information from the data you have retrieved.

To retrieve data from an external source, use the drop down menu at any column heading and select `Edit column` > `Add column by fetching URLs...`.

This will prompt you for a GREL expression to create a URL. This URL will use existing values in your data to build a query. When the query runs OpenRefine will request each URL (for each line) and retrieve whatever data is returned (this may often be structured data, but could be HTML). The data retrieved will be stored in a cell in the new column that has been added to the project. You can then use OpenRefine transformations to extract relevant information from the data that has been retrieved. Two specific OpenRefine functions used for this are:

- `parseHtml()`
- `parseJson()`

The `parseHtml()` function can also be used to extract data from XML.

### Retrieving parsed recordedBy team names from Bionomia

In this case we are going to use the [Bionomia parsing API](https://bionomia.net/developers/parse). Note that API providers may impose rate limits or have other requirements for using their data, so it's important to check the site's documentation. To reduce the impact on the service, use a value of `500` in the `Throttle Delay` setting to specify the number of milliseconds between requests.

The syntax for requesting a team name parse from Bionomia is `https://api.bionomia.net/parse.json?names={recordedBy}` where {recordedBy} is replaced with the recordedBy value from the dataset.

- Using the dropdown menu of `recordedBy`, select `Edit column` > `Add column by fetching URLs...`

- In the `New column name` field, enter "bionomia\_JSON"

- In the field for `Throttle delay` enter 500

- In the expression box type the GREL `"https://api.bionomia.net/parse.json?names="+escape(value,'url')`
  
  At this point, your screen should be similar to this:
  
  ![](fig/or362-data-from-url.png){alt='Add column by fetching URLs screen capture'}

- Click 'OK'

You should see a message at the top on the OpenRefine screen indicating it is fetching some data, with progress showing the percentage of the proportion of rows of data successfully being fetched. Wait for this to complete.

At this point you should have a new column containing a long text string in a format called 'JSON' (this stands for JavaScript Object Notation, although very rarely spelt out in full). The results should look like this figure:

![](fig/or372-fetch-gbifjson.png){alt='Example of the results of fetching data from Bionomia'}

OpenRefine has a function for extracting data from JSON (sometimes referred to as 'parsing' the JSON). The `parseJson()` function is explained in more detail at the [Format-based functions page](https://docs.openrefine.org/manual/grelfunctions/#format-based-functions-json-html-xml).

- In the new column you've just added use the dropdown menu to access `Edit column` > `Add column based on this column...`

- Add the new column name: "recordedBy_first_family"

- In the Expression box type the GREL `parseJson(value)[0]['parsed'][0]['family']`

- You should see in the Preview the familyname of the first collector in recordedBy displayed, similar to this screen:
  
  ![](fig/or362-parse-json.png){alt='Parse JSON to extract familyname of the first collector'}

The reason for using `Add column based on this column` is that this allows you to retain the full JSON and extract further data from it if you need to. If you only wanted the first collector name  and did not need any other information from the JSON you could use `Edit cells` > `Transform...` with the same GREL expression.


:::::::::::::::::::::::::::::::::::::::  challenge

## Exercise

We typically find specimen duplicates using collector name, recordnumber (and year). Now that we have standardised the collector name by extracting the family name of the first collector from recordedBy, can you make a column that can help us locate specimen duplicates in the dataset?
Which specimen collecting event has the most duplicate specimens in the dataset?

:::::::::::::::  solution

## Solution

We can make a new column (`rb_name_num_year`) by joining `recordedBy_first_family`, `recordNumber` and `year` and separating the values by spaces.

Adding a facet on `rb_name_num_year`, and sorting by count shows that the collecting event `Beaman 10029 1984` has 5 duplicate specimens in the dataset.

:::::::::::::::::::::::::

::::::::::::::::::::::::::::::::::::::::::::::::::



:::::::::::::::::::::::::::::::::::::::: keypoints

- OpenRefine can look up custom URLs to fetch or process data based on what's in an OpenRefine project
- Such API calls can be custom built

::::::::::::::::::::::::::::::::::::::::::::::::::


