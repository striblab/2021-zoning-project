- [Overview](README.md)
- [Methodology](methodology.md)
- [Advice for others](howto.md)
- [Suggested reading](reading.md)

# Data and documentation

The Star Tribune has compiled zoning and other related data related to 102 Twin Cities-area municipalities into simplified files for analysis.

### Copyright and license
The Star Tribune's zoning spreadsheets posted here are open data, licensed under the Open Data Commons Open Database License (ODbL) by the Star Tribune.

You are free to copy, distribute, transmit and adapt the spreadsheets, so long as you:

Credit the Star Tribune as specified below.
Inform the Star Tribune that you are using the spreadsheets in your work by emailing Michael Corey at michael.corey@startribune.com.
If you alter or build upon our data, you may distribute the result only under the same license. The full legal code explains your rights and responsibilities.

How to credit the Star Tribune
We require that you use the credit “Star Tribune, city and county governments and the U.S. Census Bureau." The credit must link to https://github.com/striblab/2021-zoning-project, unless the credit is appearing in printed media.

You must also make it clear to anyone who requests access to the data that it is available under the Open Database License. If you are distributing the spreadsheets in a data form, you can name and link directly to the license.

### Standardized zoning types
The Star Tribune categorized each city's zoning codes into several standardized categories to make analysis at a metro level possible.

|Zoning type|Description|
|---|---|
|only detached|Zoning districts that only allow single-family detached homes as a permitted use. Categorized as residential.|
|rural residential|Zoning districts that only allow single-family detached homes but have minimum lot size requirements exceeding 5 acres. Many of these are labeled in zoning codes as "intended for future development" and require lots to be divided for sale at no less than 10, 20 or even 40 acres. Included in our residential calculations and counted as single-family zoning.|
|middle housing|Zoning districts that allow "middle" housing as the most dense form of housing. This includes townhomes, duplexes, triplexes, fourplexes and mobile homes. Often these areas also allow single-family detached homes, as well. Categorized as residential.|
|MF residential|Multi-family residential zoning districts that allow apartment complexes of four units or more as the most dense form of housing. Categorized as residential.|
|mixed|Areas allowing both residential and commercial uses. Typically the residential uses were solely multi-family housing. Categorized as residential.|
|non-residential|Any zoning districts that are for commercial, industrial or other non-residential purposes.|
|agriculture|Areas defined in zoning codes as being primarily used for agriculture, although these areas allow single-family homes. Not included in our residential calculations.|
|PUD-MIXED|Areas under Planned Unit Development Agreement(s) that intended to result in a mixed use development. Categorized as residential.|
|PUD-NONRES|Areas under Planned Unit Development Agreement(s) that intended to result in non-residential development.|
|PUD-R|Areas under Planned Unit Development Agreement(s) that intended to result in residential development. Categorized as residential. It wasn't feasible to identify the specific type of housing allowed in each area. That would have required reading each PUD agreement.|
|PUD-unknown|Areas under Planned Unit Development Agreement(s) where the Star Tribune couldn't identify whether the intended development would be residential, non-residential or mixed use. Not included in our residential calculations.|

### Fields in the data
#### [city_zoning_sizes.csv](https://static.startribune.com/news/projects/all/2021-zoning/city_zoning_sizes.csv)
City-level zoning statistics, derived from city parcel data. Note that each standardized zoning type may appear more than once in each city, if more than one local code has been determined to match the standardized zoning type. Data collected in 2019.

|Column name|Format|Description|
|---|---|---|
|city|string|City name|
|zoning_code_orig|string|A city's original zoning code. Varies widely by city.|
|zoning_cat_general_2|string|Star Tribune standardized zoning type. (see above)|
|min_lot_size_per_unit|integer|The zone's minimum lot size per residential unit, in square feet|
|pct_total_land_area|float|Percentage of city's total land area that has this zoning type.|
|pct_res_land_area|float|Percentage of city's total residential land area that has this zoning type. (Calculated for non-residential zoning types too, but probably not useful for those.)|
|zone_area_acres|float|Area the city with this zoning code, in acres.|

#### [city_zones_dissolved.shp](https://static.startribune.com/news/projects/all/2021-zoning/city_zones_dissolved.zip)  (101 MB, zipped)

This shapefile shows the Star Tribune's categorization of zoning types throughout the metro, with zoning types dissolved to the city level for faster rendering. Minimum lot sizes per unit included for residential zoning types. Minneapolis "only detached" zones have been manually converted to "middle housing" to account for changes in the city's zoning after the data was retrieved. Single family zones with a minimum lot size per unit greater than or equal to 20 acres have been reclassified as "rural residential" and are not included in the stories' maps or analysis.

|Column name|Format|Description|
|---|---|---|
|id|integer|Database ID|
|city_name|string|City name|
|zone_type|string|Star Tribune standardized zoning type (see above)|
|min_lot_sz|string|Minimum lot size per unit, in square feet. Only filled out for residential zones.|
|zone_area|string|Area of the dissolved zone, in square meters|


#### [block_group_zoning.csv](https://static.startribune.com/news/projects/all/2021-zoning/block_group_zoning.csv) (630 KB) and [block_group_zoning.shp](https://static.startribune.com/news/projects/all/2021-zoning/block_group_zoning.zip) (69 MB, zipped)

A block-group-level dataset including a standardized zoning type and block group identifiers, generated from county parcel data maintained by [MetroGIS](https://www.metrogis.org/how-do-i-get/parcel-data.aspx) and U.S. Census TIGER files. Shapefile field names are the same as CSV field names, but clipped to 10 characters. We have filtered out stacked parcels (often in areas with mobile homes, sometimes with condos) to avoid double-counting areas. Block groups will appear more than once in the data if they cross city boundaries or zoning types. Parcel data retrieved November 2019. Note: County-provided and city-provided parcel data will not always match depending on when the data was acquired and differences in city and county data maintenance practices.

|Column name|Format|Description|
|---|---|---|
|county|string|County name|
|cty_fips|string|Census 3-digit county FIPS code. Example: 053|
|tract|string|Census tract ID. Example: 026605|
|blk_grp|integer|Census block group ID. Example: 1|
|geoid|string|Full Census GEOID of block group, including state code. Example: 270530266051|
|city_name|string|City name|
|zone_type|string|Star Tribune standardized zoning type. (see above)|
|zone_area_acres|float|Area of the block group/city/zoning type geometry, in acres|
