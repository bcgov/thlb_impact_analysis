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
seamless. The ftp links have been separated into Timber Supply Area / Timber
Farm License areas due to size limitations. 

>[!Note]
><strong>Tree Farm Licences (TFL) </strong>are area-based tenures which grant nearly exclusive rights to harvest timber and manage forests on a specified area of land.><br />
><br />
><strong>Timber Supply Areas (TSA) </strong>are geographically defined areas used for timver supply planning. Harvesting rights are allocated through volume-based tenures such as Forest Licences rather than exclusive area based rights. 

The THLB Data comes in polygon format and contains several unique fields, most notably:
- **thlb_fact**: A Float Value, between 0 and 1, representing the amount of forest within the associated polygon which contains harvestable timber. For example, a polygon with a tblb_fact of 0.75 would contain 75% harvestable timber.
- **tsr_report_year**: The year that the polygon was created as part of a Timber Supply Review, which must be completed at least every 10 years. It is good practive to review this data to ensure data currency.
- **thlb_area_ha**: This is the area of the polygon multiplied by the thlb_fact and should represent the amount of harvestable land in hectares within the associated polygon.


In addition to the THLB data, you will require **input geometries** (ie, your
area of interest). 



## Manual Process
This analysis was first produced **Manually** In ArcGIS Pro using the following 
procedure:

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