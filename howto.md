- [Overview](README.md)
- [Methodology](methodology.md)
- [Data](data.md)
- [Suggested reading](reading.md)

<h1>Advice for others wanting to replicate our work</h1>

We learned a lot in our quest to study residential zoning and segregation in the Twin Cities area and we hope to see other journalists or academic researchers do the same in other parts of the country. To further that cause, we thought we’d share advice to help you get started.

Our original goal was to identify how much “exclusionary zoning” exists in the Twin Cities and we later expanded that to also measure the relationship between zoning and racial segregation.

There are several zoning practices that are often defined as exclusionary zoning. The biggest one is simply whether single-family detached homes are kept separate in their own districts, apart from denser forms of housing. But there are more detailed zoning rules that are known to drive up the cost of a house or even an apartment, such as minimum lot size for a house or the maximum number of apartment units that can be built per acre or the minimum amount of parking required (and whether that needs to be a garage).

Our main goal when we started on this was to measure the percentage of residential land that is zoned solely for single-family detached homes. We had seen this done elsewhere and we knew that the city of Minneapolis alone was at 70%. We assumed our suburbs would be significantly higher. And since housing is a regional issue, we wanted to know the regional number.

Just simply collecting that for your city or metro area would be a way you can tell a powerful story without doing quite as much work as we did.

<h2>Getting GIS shapefiles</h2>
The first thing you need to do is obtain GIS shapefiles that identify the zoning designations in the city or cities that you want to study. Your city should have either a planning department or a community development department that would be the best place to start. Sometimes we found these officials forwarded us to GIS professionals who worked for the city or an outside contractor that does the zoning and/or GIS work for the city. Small cities were far more likely to have a contractor and it turned out that there were just a few contractors who did work for many cities.

If you do have a situation where the contractor has control of that data, make sure you know your state’s public records law in terms of access to government data through a third-party vendor. In Minnesota, the vendors are required to provide the data, however, they could charge for their time and the city would likely pass that cost along to you. (We only had to pay for one situation like this and it ended up being only $57).

We had some very small cities that didn’t have shapefiles and somehow their contractors no longer had the GIS files used to make the last zoning maps about a decade earlier. Instead of fighting with them, we took the PDF versions of their maps and carefully matched them to parcel data that we had gotten from the county. Luckily, these were all cities that haven’t changed in decades and had less than four zoning districts.

And there were three very small cities that we gave up on altogether because they didn’t even have a PDF version of their latest zoning map, let alone shapefiles.

The ideal shapefile to request would be polygon, parcel-level that has a field identifying the zoning for each parcel. We found that most cities have this because they need to be able to easily identify the zoning on a parcel if someone comes along with a proposal to make changes to it.

Specifically ask that they include the property ID number (in Minnesota they are referred to as a PIN). This might come in handy if you need to match it back to other parcel data that might have things like property values, building types or other information an assessor might keep.

Second-best is a polygon shapefile that outlines the zoning districts, plus a polygon parcel level file (without zoning). To marry these, you would need to turn the parcel polygons into points and do a spatial join with the zoning district polygons to attach the zoning designation to each parcel. Then ultimately join that back to the parcel polygon file.

Even if you are able to get parcel level zoning files, it’s also useful to have parcel-level assessment data if you can get it. This would include things like the assessed value of the land, the house and the two things combined; anything about how the property is being used (i.e. single-family detached home, commercial building, etc.); the date the house was built; the last sale date and price; size of the parcel; size of the house.

More than half the cities provided us with their zoning shapefile within a month of our request. But some requests lingered as long as four months.

The big thing that tripped us up -- that we didn’t expect --  was that some cities have zoning districts identified as “Planned Unit Development.” These PUDs are essentially custom zoning for a specific development in the form of an agreement between the city and the developer. In some cases, the zoning shapefiles had these assigned to parts of the city. In other cases, they had overlay maps, and some cities simply didn’t track it in their shapefiles. So we found that these PUDs threatened to put a monkey wrench in our ability to determine if this would be single-family housing or something else.

We reached out to every city to ensure we had all the overlay files and to also ask if they could identify which areas were residential and which were commercial (a PUD could be used for anything), so we could at least determine if any of this land was non-residential. In the end, some of this land is counted as “residential” but we weren’t able to determine whether any of it was zoned solely for single-family. Anecdotally, we heard that most of the PUD land was being used for subdivisions, often single-family homes with a few townhomes tucked in along the major roadways, and we confirmed that by looking at land use data that is compiled by the Met Council. As a result, we think our calculation of single-family zoning is conservative.

If you are planning to combine shapefiles from multiple cities, you’ll want to make sure they are all in the same projection and that may require running some conversions in your GIS software.

If you plan to collect data from the entire metro area, think carefully about what cities you want to include. Our metro area technically includes townships (which don’t always have zoning) and rural cities that are quite different -- and separate -- from the urban and suburban areas.

Our metro has a regional organization called the Metropolitan Council that runs the regional sewer system and that boundary is a means of controlling outward growth of the metro. In other words, that boundary kind of represents where we have development versus where we still have mainly cornfields. We decided to stick with cities that are inside that boundary.

<h2>Standardizing and merging</h2>
Next step is to standardize all those zoning codes. We did that by creating a crosswalk file rather than adding new fields to each shapefile. This crosswalk was a spreadsheet with one row for each zoning district in each city (all of the ones that show up in the shapefiles) and then we manually typed in additional information, including assigning each code to various categories. There probably are more sophisticated ways to do this -- especially if you have multiple people involved. It worked for us because only one person was doing the work.

Another key to success was designing repeatable, scripted import and export processes that could be run many times as errors were found or we altered our methodology.

We created a Django app to handle importing, reprojection, storage and geographic processing of the data. We used the crosswalk to import each shapefile into a single shared table of all the parcels in the metro area, with standardized zoning codes.

We used a large PostGIS-enabled Amazon RDS instance, which was costly but allowed us to efficiently process the large amount of spatial data. We designed each table to include both a full-resolution and simplified version of each geometry. We used the full-resolution geometry for analysis, but when exporting metro-level data for display and graphics, using the simplified geometries was much faster. We created unified zoning areas for each city to avoid having to process parcel-level data for each query.

We didn’t realize we would need the crosswalk until after we had conducted a survey, using Survey Monkey, of city planners and other city officials. We got ideas for what questions to ask from a much larger survey done by the <a href="https://californialanduse.org/">Terner Center</a> at UC-Berkeley. In hindsight, we wish we had relied more on the crosswalk and asked different questions in the survey.

We intended the survey to be a way for us to understand zoning from a sort of big-picture standpoint and we didn’t realize how hard this is to achieve through a survey.

The survey, for example, only asked about the “largest” single-family zoning district (by land area), instead of asking about all of them. This was especially problematic when we asked about the largest multi-family district. Some answered with a district that required a conditional use permit; one listed a district that was created in the city ordinances, but hadn’t been applied to any land yet. Others listed out all the districts in the city that allowed multi-family. And then when it got to the next question asking about the maximum density allowed in that district, they answered “depends on which district.”

Our intention with the crosswalk was to just put each residential district into a bucket depending on the most dense form of housing allowed by right. What we should have done is built a full dataset with more details for every residential zoning district. So, we’ll offer up some advice here with that in mind.

The crosswalk should have one row for every zoning district identified in each shapefile. Columns should include city name, the shapefile name, the zoning code used in the shapefile (exactly as it appears because you’ll be doing some joining), and then a series of fields that you’ll populate based on what you find in the city’s zoning ordinance.

For example, you’ll probably want one category to simply identify residential versus non-residential. And then the key category we had was tagging the residential districts as either single-family, middle housing, multi-family,  mixed use or PUD.  More about the nuances of those category assignments below.

We also recommend you collect the following:
For single-family zoning districts:
* Minimum lot size per unit in square feet (and/or minimum lot width)
* Is a garage required? If so, is there a minimum number of spaces required? Or minimum footprint size?
* Minimum setback from the street in feet. (no need to collect side or rear setbacks)
* Minimum floor area requirement in square feet (some cities simply follow the state building code on this; and this is less common than the lot size)
*
For each multi-family district:
* Maximum density (ideally expressed in units per acre). If they don’t have one, the minimum lot size for a two-bedroom unit gets you a rough approximation of how many units would fit on an acre.
* Open space requirement. This might be a percentage of the land or a certain amount per unit.
* Height limitation, listed in stories or feet.

Also, give yourself a notes column where you can write down anything unusual and a column to identify whether something more dense would be allowed under conditions, or with a conditional use permit or other special permission.


<h2>Reading city ordinances</h2>
The place to find all this information is in the zoning section of the city ordinances.

City ordinances are consistently located online for Minnesota cities. Many are stored on national platforms such as <a href="https://library.municode.com/">Municode.</a> (FYI: we wished we had downloaded the key sections of code as PDFs since our project ended up taking far longer than we thought due to the pandemic and some cities altered their codes in the interim.)

The zoning ordinances in the Twin Cities all follow what’s known as Euclidean zoning (it’s named after Euclid, Ohio, the city whose zoning code was the subject of a <a href="https://en.wikipedia.org/wiki/Village_of_Euclid_v._Ambler_Realty_Co.">1926 Supreme Court ruling</a> affirming that separating single-family from multi-family was legal). As a result, the various zoning districts had definitions that were based on the type of housing allowed in that district.

It’s possible that you might have a city that uses other types of zoning. An increasingly common one is form-based zoning. In those cases, the ordinances won’t specify housing types but will be more likely to talk about densities. For example, it might merely say that this district allows a maximum of 18 units per acre, but nothing about single-family or multi-family.

Reading zoning ordinances is a bit daunting at first. However, it was kind of eerie how many of them were laid out in an almost identical manner. Once we got through a few cities, it got easier and faster due to this consistency. Every once in awhile we’d come across one that wasn’t in this cookie-cutter fashion and those took longer to read. (Pro tip: don’t try to read too many in one day! It’s mind-numbing.)

We almost always found a section defining each zoning district, (i.e. R1, B1, etc), followed by a description of the district, the “permitted uses” and the “conditional uses.” In many of the residential districts it might also have a section on permitted accessories (i.e. sheds, etc). In the commercial districts it might list the various kinds of businesses that were allowed or not allowed.

We looked for the “permitted uses” or the “by right” uses. Most single-family districts simply listed “single-family detached home” as the only permitted use and then would have a list of conditional uses, such as daycares or churches.

Quite often a zoning district that allowed multi-family or even townhomes might say those were only allowed if additional conditions were met or that it required a conditional use permit.

In hindsight, we should have included a category that was something like “Multi-family-special permission,” since it was common that there was a district that only allowed apartments -- no other forms of housing -- but everything still required a conditional use permit or a Planned Unit Development agreement.

We also had a “middle” housing bucket that fit all the things that are not single-family detached homes but also not apartment complexes. This included townhomes, mobile homes, duplexes, triplexes and fourplexes.

The fourplexes were the most tricky. There were districts that allowed “up to 4 units,” and then there were districts that allowed “4 units or more.” The places that allowed up to four fell into the middle housing bucket, while the other went into the multi-family bucket. Some zoning codes didn’t specify, but would say that it allowed apartment buildings or multi-family housing and then would have some density cap requirement, such as no more than 12 units per acre. We categorized these areas as multi-family.

Some districts allow almost everything -- single-family detached, townhomes, duplexes, and even some apartment complexes (maybe not high-rises, though). In those cases, we categorized it as a multi-family district because that’s the most dense form of housing allowed there.

More commonly though, cities had clear delineations: a single-family district (or two), an area where you could build townhomes, and another area where you could build apartments. Sometimes there were separate single-family districts each with different minimum lot sizes.

What we learned is that a lot of these zoning districts reflect the trend at the time they were created. So for example, one created prior to the 1950s probably has minimum lot sizes of 5,000 or 8,000 square feet, while one created in the boom of the 1990s probably has a minimum around 20,000 square feet. Newer developments are moving toward smaller lot sizes again, although usually still somewhere around 10,000 square feet.

Most cities follow a similar pattern in their codes -- R-1, R-2, etc. for residential. Perhaps RR for rural residential (although that was also sometimes used for railroad) or RE for residential estate. I found that cities that allowed mobile homes usually had a separate zone for those. A few cities use “LDR” for low-density residential and “MDR” (medium) and “HDR” (high).

Some used “MF” or just an “M” for multi-family (but some also used M for manufacturing).   B or C for business or commercial. I for industrial, or perhaps LI for light industrial. Mixed use districts were often either “MU” or “MX” but these were far more likely to use different letters that weren’t as obvious.

Some cities identified “public districts” or “institutional districts” for land that is publicly owned for things like colleges, city hall or schools. Parks were sometimes identified outright as parks, other times they were listed as public districts, and sometimes they were even just lumped into the neighboring residential district.

We tried to identify open space areas, whether they were parks or beaches or wetlands or preserves. Some of the shapefiles had polygons for water bodies.

The minimum lot size requirements were a little harder to find. It wasn’t always in the same place as the district definition. Typically this was expressed in square feet, such as the lot was required to be at least 10,000 square feet in size. Other times, the size of the lot would be controlled by width limitations -- such as that it needed to be at least 80 feet wide. Sidenote: In the planning world, a 10,000-square-foot lot is considered to be a one-quarter acre, even though it’s just shy of that.

For multi-family zoning districts, a good metric to collect is the maximum density allowed. Many suburbs had areas where you could build an apartment, but these density caps -- such as a maximum number of units per acre -- put a damper on the density, and ultimately the cost of those units.

This is expressed a little differently from place to place. Most often, it was set as a maximum units per acre. Other times, it would be a limitation on the number of units per building. This is a tricky thing to standardize because of these variations, but also because the number of units that could truly be built on a site will also be dictated by other rules such as setbacks, parking and open space requirements.

We also tried to gather information on maximum lot coverage for multi-family. But this is expressed many different ways in the code -- sometimes as a maximum “impervious surface coverage”; sometimes as a maximum “building coverage”; sometimes as a percentage of open space required. We realized it would be impossible to standardize all of this.

Stacked parcels is a hazard common to many analyses using parcel records. This was most relevant to the area calculations used in our project. Two common examples are parcels where mobile homes or condominiums are located. Some cities make many duplicate records for each unit or mobile home located on a single parcel, yielding many geometric polygons that refer to the same physical space. Making geometric unions for zoning areas by city allowed us to sidestep this issue in most cases.


<h2>Measuring segregation</h2>

Our goal was to determine whether the demographics of single-family neighborhoods varied greatly from that of multi-family or middle housing neighborhoods.

This was a challenging step for a number of reasons, starting with the fact that <a href="https://marketurbanism.com/2021/07/05/is-diversity-segregation/">measuring segregation is tough and imperfect.</a>

Even before we get to that, though, we have to deal with the fact that population data can be obtained in geographic areas such as Census block groups or tracts or city or county level, but zoning data is either at the parcel level (too detailed) or in zoning districts that aren’t always contained within some other geographic area. We found that most block groups in the Twin Cities didn’t have uniform zoning.

In addition, the most detailed level of data available -- Census block groups -- can have high margins of error when you’re dealing with small populations. As a first step to mitigate this, we compared the white non-Hispanic population to all the other groups combined.

We also found that Asians are far more likely to be homeowners and living in those single-family neighborhoods. As a result, simply looking at the BIPOC share of population washed out some of the vast differences that are apparent when you only look at Black and Latino populations.

We also tried looking at the city level. For example, what’s the demographic makeup of city A that has a “high” level of exclusionary zoning versus city B that has a “low” level of exclusionary zoning?   There are a handful of cities that inarguably have high levels of exclusionary zoning (single-family is often the only housing options in those cities) and then there are cities at the other end of the spectrum that allow duplexes in any part of the city or have a high share of land zoned for multi-family housing. But in the middle were a huge number of cities that didn’t neatly fall into those two buckets. And at the city level, the demographic makeup is really washed out. We could see cities with an above-average share of people of color, but they all seemed to be living in just one part of the city.

We ended up with two analyses -- one that’s more straightforward and easier for readers to understand and another with more rigor, but harder to explain. Both showed the same thing, though: people of color -- but especially Black and Latino -- are disproportionately living in areas zoned for multi-family housing.

For the simpler analysis, we did a spatial join between the parcel zoning data and a Census block group map to determine what percentage of each block group was zoned for each type of housing. Then from that block group map, we categorized the block groups. One with 90% or more single-family only zoning was labeled as single-family. One with 40% or more multi-family was zoned as multi-family, since multi-faily housing doesn’t need as much land. Many block groups fell somewhere between these two ends, often with a mix of single-family and middle housing.

Then we calculated the percentage of each racial or ethnic group’s population living in each type of neighborhood (single-family, multi-family, or the mixed ones that fall in the middle). We also compared white non-Hispanic to all the other groups combined. If people are evenly distributed you would expect these numbers to be fairly close to each other. But we found that, for example, nearly 25% of Black people live in areas zoned for multi-family compared to just 8% of white people.

The more rigorous analysis was a regression analysis that we did in partnership with <a href="https://www.mercatus.org/scholars/salim-furth">Salim Furth</a>, senior research fellow and director of the Urbanity project at the Mercatus Center at George Mason University.

This analysis, a Generalized Spatial Two-Stage Least Squares regression, was based on the same block group level data we used above. The dependent variable was the non-white population and the independent variable was the percent of residential land zoned in each category. We controlled for median age of the housing stock, whether a block group was in the central cities (Minneapolis and St. Paul) and the distance from its closest central city (since density tends to fade the farther out you go).

To further mitigate the margins of error in the demographic data, we used a smoothing technique that averages the population shares based on the neighboring block groups. Ultimately, our regression yielded the same results whether we used this smoothed data or the original block group data.

It’s important to note that we found little research that we could mirror our work on, most likely due to the fact that this kind of detailed zoning data is hard to come by. There are quite a few academic papers out there linking segregation and zoning but they tend to use metro-level metrics that define metro areas by the level of zoning regulation, finding those with more regulation tend to have more segregation.
