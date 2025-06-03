# THLB Impact Analysis
This repository contains tools which help to answer the question of what impact
an input shape / Polygon would have on the Timber Harvesting Land Base (THLB). A 
common reason someone may want this analysis complete would be to determine the 
impact of proposed Wildlife Habitat Areas (WHA) on the timber industry. The 
goal is to quantify THLB reduction due to non-harvest and conditional harvest 
areas.

>[!Note]
><strong>Gross Land Base (GLB): </strong>Tenure Area or Total Area<br />
><strong>Forest Management Land Base (FMLB): </strong>Forested Area that contributes to forest management objectives (GLB, minus water, long term leases, other tenures, non forested areas)<br />
><strong>Legally Harvestable Land Base (LHLB): </strong>Area within the FMLB where timber harvesting is legal, subject to forest mamagement objectives and requiremnets. <br />
><strong>Timber Harvesting Land Base (THLB): </strong>Area within the THLB where it is economical to harvest under current management practices (LHLB minus environmentally sensitive areas, steep slopes, non merchantable timber, etc.)

## THLB Data
The location of THLB can be found through an internal local directory as an ESRI
file geodatabase or via a [file transfer protocol (ftp) page](https://www.for.gov.bc.ca/ftp/HTS/external/!publish/DataCatalogue_FAIB_Data/THLB/). The internal file geodatabase is preferred for this analysis as it is
seamless (not divided by Timber Supply Area). The ftp links have been separated 
into Timber Supply Area / Timber Farm License areas due to size limitations. 

>[!Note]
><strong>Tree Farm Licences (TFL) </strong>are area-based tenures which grant nearly exclusive rights to harvest timber and manage forests on a specified area of land.><br />
><br />
><strong>Timber Supply Areas (TSA) </strong>are geographically defined areas used for timver supply planning. Harvesting rights are allocated through volume-based tenures such as Forest Licences rather than exclusive area based rights. 
><br />

[Tree Farm Licence](https://www2.gov.bc.ca/gov/content/industry/forestry/forest-tenures/timber-harvesting-rights/tfl) holders (aka, Licencees) have their own process for determining THLB within those areas. Until very recently, those THLB calculations have not been made available to the provincial government. This has made conservation planning difficult. In order to get an impression of THLB within the TFL areas, [Forest Analysis and Inventory Branch (FAIB)](https://www2.gov.bc.ca/gov/content/industry/forestry/managing-our-forest-resources/forest-inventory) has created a raster product called the 'proxyTHLB'. Each pixel in the 'proxyTHLB' is 1 square hectare and the pixel values represent the thlb_factor. This product is not published, but access can be requested by reaching out to FAIB.

The Provincial THLB Data comes in polygon format and contains several unique fields, most notably:
- **thlb_fact**: A Float Value, between 0 and 1, representing the amount of forest within the associated polygon which contains harvestable timber. For example, a polygon with a tblb_fact of 0.75 would contain 75% harvestable timber.
- **tsr_report_year**: The year that the polygon was created as part of a Timber Supply Review, which must be completed at least every 10 years. It is good practive to review this data to ensure data currency.
- **thlb_area_ha**: This is the area of the polygon multiplied by the thlb_fact and should represent the amount of harvestable land in hectares within the associated polygon.

The proxyTHLB Data comes in raster format and contains just the single value:
- **thlb_fact**: A Float Value, between 0 and 1, representing the amount of forest within the associated pixel which contains harvestable timber. For example, a 1ha pixel with a tblb_fact of 0.75 would contain 0.75ha of merchantable timber.

In addition to the THLB data, you will require **input geometries** (ie, your
area of interest). Some parts of the analyis also require **VRI (R1)** for determining forest age. 

## Manual Process
This analysis was first produced **Manually** In ArcGIS Pro using the following 
procedure:

**Prepare the Workspace**
- Create an Area of Interest Surrounding all your polygons (in this case, proposed WHAs) and clip all input data to those AOEs. This will help in reducing processing time for all future steps.
- Ensure the workspace environmnet settings have the projection set to BC Albers.

**Prepare the THLB Data**
- The proxyTHLB Data must be merged with the provincial thlb data. This requires a few steps:
    - The raster cells contain float values, but to convert to vector, they need to be integers. To accomplish this, use the **Raster Calculator** and apply the forumula **Int("cell_value" * 1000)**
    - Then use the **Raster to Polygon** function to convert your raster. Make sure **NOT** to simplfy the output. 
    - Once your raster data is converted, create a new field in the polygon feature class called **thlb_fact** and calculate it's value as **float(!cell_calue / 1000)** to convert back to a float.
    - You can now clip this feature class to your area of interest, and then **merge** the proxyTHLB feature class with the provincial thlb feature class. Make sure that you are using the appropriatie field mappings during the merge. There should be a single **thlb_fact** column in your resulting feature class.

>[!Note]
>Some clients have additional requests for their THLB analysis. For example, they may like to see the impact of their WHA divided by which Timber Supply Area or TFL they are in. In the identify steps below, you can repeat the process with the necessary feature classes. 

**Identify Overlaps**
- Use the **identify** tool with your input polygons (eg, the WHAs) and your merged thlb feature class. Your output should resemble the original input polygons, but have many "slivers". Each sliver should have an associated **thlb_fact** value. 
- Add 2 new float fields to your feature class: ["area_ha", "thlb_area"]. Use the **Calculate Geometry** function for the "area_ha" field and determind the area in hectares. Then use the **Calculate Field** tool on the "thlb_area" and use the function **!area_ha * !thlb_fact**. 

**Summary Statistics**
- Use the **Summary Statistics** function to create your data output. Choose "thlb_area" as your statistics field (and any other fields required) and ensure the method is set to **SUM**. Case Fields are optional, but in the case of this analysis, be sure to select the field containing the unique identifer of the proposed WHAs. 
- Always perform a manual review of the table dataset with the cartographic represention of THLB to ensure the results intuitively make sense. 
- (optional) Join the resulting summary statistcs table back to the original input polygon to attach the summary results back to the proposed WHAs.

>[!Important]
>The process above describes the basic steps to perform a THLB analysis request. It is common for clients to ask for more detailed information. For example, a client might want to know the breakdown of THLB impact based on Mature/Immature Forests (which uses the **VRI Proj_Age_1 attribute**), The breakdown by different administrative areas, and the breakdown by Old Growth Defferal Area (OGDA) or Non-OGDA. These requests can by accomodated by performing additional **Identify** functions and then including those categories in the **CASE** section of the **Summary Statistics** tool.

<!-- 
- Sapsucker
- Williams Sapsucker, At-
- THLB, designed based on the habitat attributes
- Going into forest consultated and going into THLB Impacts
- Trying to go in with our own numbers
- Timber Supply Impact

- Proposed, Within the Proposed WHA Just within the Kootenay Boundary
WHSE_WILDLIFE_MANAGEMENT.WCP_WHA_PROPOSED_SP, refer to the proposal to see the numbers needed to run
4-140, 4-347 to 4-378


- Scripted
- Mostly in the Boundary TSA, One is in the Cranbrook, and some in the 

- Splitting out the THLB into Mature and Immature, 
- Map Product
- Just an Excel File summarizing the in the 

- Immature, Mature, Total
- Each Row
- Merchantable Timber
- Look into it, 
- 
- Timeline, May 23rd for Turnaround time,


- Want to know how much, a bunch of these polygons overlap with old growth deferral areas
- Not legal,  
- So how 


Categories, Hectares
Immature, Mature, Immature-OGMA, Mature OGMA, Total


4-140
4-347-4-378

Connect with Will Burt regarding Mature/ Immature Discrete Classes (80 years?)

Look into Why so many of the WHAs are not in the THLB
- 

WHSE_FOREST_VEGETATION.OGSR_SUPPRTD_OG_HRVST_DFRL_SP

Asb about THLB that exists, but not classified as Mature/Immature (IE, not in the VRI)

Ask Will who has access to upload proposed WHAs

Old Growth Strategic Review, Separate from OGMAs, maybe done by FAIB, Tried to identify all the ogam, and certain priority conservation areas
- Areas that FAIB wants to Defer, and also a technical advisory panel

58.87 / 76.99
 -->