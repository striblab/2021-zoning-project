- [Overview](README.md)
- [Advice for others](howto.md)
- [Data](data.md)
- [Suggested reading](reading.md)

# Methodology

When Minneapolis approved changes to its land use plan and zoning ordinance to allow duplexes or triplexes in areas previously reserved for single-family homes, Star Tribune journalists wondered how much of the metro area is zoned to only allow single-family homes.

Because this is not compiled by any one organization, we requested zoning maps from more than 100 cities in the Twin Cities metro area that are served by the regional sewer system and are considered urban or suburban by the Met Council, a regional policy-making and planning agency that guides the region’s growth.

We asked for GIS mapping files, preferably showing the zoning assigned to each parcel of land. In a few cases, cities gave us maps that identified the locations of the various zoning districts and we had to marry that to <a href="https://gisdata.mn.gov/dataset/us-mn-state-metrogis-plan-regional-parcels">parcel data</a> available from each county.

Some cities responded promptly. For others, we had to obtain the files through a contractor that handles the city’s zoning and/or GIS work. Overall, it took about three months to get all of the files.

Most provided the files for free; one city required us to pay a $57 fee. There were 16 cities that claimed to not have shapefiles, but they had PDF files showing their most current zoning map. These were all small cities with very few zoning districts. This made it possible for us to create GIS shapefiles by comparing the PDF maps to the county parcel shapefile.

We had to give up altogether on three very small cities (Mendota, Willernie and Birchwood Village) that didn’t have PDF maps and didn’t respond to our inquiries. We chose to exclude Corcoran, which has a suburban designation, but very little of the city is served by the regional sewer system at this time.

To put all the maps together, the Star Tribune standardized the various zoning districts. Zoning districts can vary quite a bit from place to place, even though they might use the same code -- such as R1.

We decided to assign each residential zoning district to a bucket depending on the most dense form of housing allowed by right (no conditions, no conditional use permit or other form of special city council approval required).

We read municipal zoning ordinances from every city to not only figure out what bucket each district should be assigned to, but also collect other information such as minimum lot size requirements for single-family districts, maximum density caps for multi-family districts, parking and garage requirements and minimum living area requirements for homes.

The buckets were: single-family detached, multi-family housing of more than 4 units, and districts that allow townhomes, duplexes, triplexes, fourplexes or mobile homes (this is often called “middle housing”). We also identified mixed use areas that allow both residential and non-residential use.

Areas zoned as a Planned Unit Development were categorized separately since these areas are governed by agreements between a city and a developer, often overriding existing zoning regulations.

There were numerous instances where a district would allow multi-family housing with additional conditions, or through a conditional use permit or a Planned Unit Development agreement. In those cases, the districts were assigned to a lower density bucket, depending on what was allowed by right. A few cities also had multi-family zoning districts in their code, but it hadn’t yet been applied to any land yet.

To calculate how much residential land is zoned for the various housing types, we first had to decide what we would count as residential land. We chose to include mixed use areas, since a growing number of suburbs are using these spaces to allow apartment complexes. And we chose to exclude agricultural land.

This data was collected in 2019, just as most cities were in the midst of approving their 2040 Comprehensive Plans, and our publication was delayed due to the pandemic, the murder of George Floyd and the riots that followed. Those comprehensive plans, which are done every 10 years, can result in zoning changes. The Star Tribune adjusted Minneapolis’ data in our analysis to reflect the elimination of single-family zoning districts that had already taken place by that time, but this analysis doesn’t reflect any changes that might have occurred in other cities since then.

The Star Tribune sought guidance from a variety of local and national experts before creating this plan and also modeled some of its work on other studies, such as: <a href="https://papers.ssrn.com/sol3/papers.cfm?abstract_id=3472145">Robert Ellickson’s</a> study of zoning in New Haven, Austin, Texas and Silicon Valley; the <a href="https://belonging.berkeley.edu/segregationinthebay">Othering & Belonging Institute’s</a> analysis of zoning and segregation in the San Francisco Bay Area; and <a href="https://www.desegregatect.org/">Desegregate Connecticut’s</a> analysis of zoning in that state.

To measure the relationship between zoning and racial segregation, the Star Tribune conducted several analyses using our standardized parcel-level zoning maps and block group demographic data from the 2015-2019 American Community Survey.

The simplest analysis was to pair zoning with racial and ethnic demographic data. Since zoning districts and census block groups don’t line up geographically, we assigned each block group a category based on how much of the area was zoned for the various housing types. For our calculations included in the story, block groups with 90% or more area zoned for only single-family homes were defined as single-family neighborhoods. Block groups with 40% or more area zoned for multi-family were defined as primarily multi-family.

To further confirm the findings that people of color are disproportionately living in areas zoned for multi-family housing, the Star Tribune used linear regression to measure this relationship between zoning and segregation. <a href="https://www.mercatus.org/scholars/salim-furth">Salim Furth</a>, senior research fellow and director of the Urbanity project at the Mercatus Center at George Mason University, assisted with the regression analysis. The analysis, which controlled for distance from downtown and age of the housing stock, found that for every 1 percentage point increase in the share of multi-family housing, there will be an average 3.6 percentage point increase in the share of people of color.
