# data-512-homework_1
## Professionalism &amp; Reproducibility

## Goal of this project
* Construct, analyze, and publish a dataset of monthly article traffic for a select set of pages from English Wikipedia from July 1, 2015 through September 30, 2024.
* Develop and follow best practices for open scientific research while doing so.

## Licenses
1. Code used in `data_acquisition.ipynb` - This code example extended by the code snippets developed by Dr. David W. McDonald for use in DATA 512, a course in the UW MS Data Science degree program. This code is provided under the [Creative Commons](https://creativecommons.org) [CC-BY license](https://creativecommons.org/licenses/by/4.0/). Revision 1.3 - August 16, 2024
2. This dataset was created in accordance with the [Wikimedia Foundation Terms of Use](https://foundation.wikimedia.org/wiki/Policy:Terms_of_Use), which states that we are good to create this dataset and use it for our analysis as long as we don't violate their UCoC and other terms and policies.
3. The list of rare diseases seems to be derived from the [National Organization for Rare Diseases](https://rarediseases.org/), and care was taken to ensure that this project was in adherance to their [terms and conditions](https://rarediseases.org/terms-conditions/).

## Links to API documentation
1. Pageviews data was fetched using the [Wikimedia Analytics API](https://doc.wikimedia.org/generated-data-platform/aqs/analytics-api/reference/page-views.html). Careful consideration was given to adhere to not bombard their platform with too many requests - The Pageviews API asks that we not exceed 100 requests per second, we add a small delay to each request.
2. Python's [urllib3](https://urllib3.readthedocs.io/en/stable/) to fetch the data
3. [Pandas](https://pandas.pydata.org/docs/) to handle data processing
4. [Matplotlib](https://matplotlib.org/stable/index.html) to generate the plots

## Data files and intermediaries created
The `data_acquisition.ipynb` notebook creates the following data files -
1. `rare-disease_monthly_mobile_201501-202409.json` - has the pageviews for mobile users for each disease
2. `rare-disease_monthly_desktop_201501-202409.json` - has the pageviews for desktop users for each disease
3. `rare-disease_monthly_cumulative_201501-202409.json` - has the cumulative pageviews for all users for each disease

All three files share the same data schema, which is listed below - 
```JSON
{
    "disease1": [
        {
            "project": "en.wikipedia",
            "article": "18p",
            "granularity": "monthly",
            "timestamp": "2015070100",
            "access": "all-access",
            "views": 24
        },
        {
            "project": "en.wikipedia",
            "article": "18p",
            "granularity": "monthly",
            "timestamp": "2015080100",
            "access": "all-access",
            "views": 24
        },
        ...(and so on for all months till September 30, 2024)
    ],
    ...(and so on for each disease)
}

```

Note: `timestamp` is in the format of `YYYYMMDDHH`.

## Gotchas / Issues / Special considerations

1. While using the code snippet provided by Dr. David, I ran into an issue where we had a page with title `Sulfadoxine/pyrimethamine`. This DID NOT WORK as the encoding in the code did not encode the `/`. The previous batch would not have run across this issue possibly because I see no reason for a `/` to be present in the titles of dinosaurs or movie titles. I had to modify the below snippet - 

```python
urllib.parse.quote(request_template['article'].replace(' ','_'))
```

to this -

```python
urllib.parse.quote(request_template['article'].replace(' ','_'), safe='')
```

in order for it to work.

2. If you are running `data_acquisition.ipynb`, [this is exactly how long it will take](https://www.youtube.com/watch?v=OWAevdIrLE8) for the API calls to finish, so grab some popcorn. Pro tip: Don't, just use the JSON files.
