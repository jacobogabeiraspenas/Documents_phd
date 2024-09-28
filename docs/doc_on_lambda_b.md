### Report: Calculation of Global LAMBDA_B Dataset

#### Motivation:
In urban climate modeling, particularly with the **Weather Research and Forecasting (WRF)** model using the **Building Effect Parameterization (BEP/BEM)**, it is critical to include detailed building geometry information. This includes factors like the fraction of the surface covered by buildings and their spatial characteristics (heights, widths, and spacing). However, global datasets for these specific parameters are not readily available. 

To address this, we constructed a global dataset for **LAMBDA_B**, which represents the impact of building geometry on urban environments. This dataset is crucial for WRF-BEM simulations but had to be calculated from existing data on **building fraction**, **building height**, and **urban fraction**, as no direct global dataset for **LAMBDA_B** exists.

#### Data Sources:
To calculate **LAMBDA_B**, we used the following globally available datasets:
1. **Building Fraction (LAMBDA_P)**: The percentage of land covered by buildings within each grid cell.
   - **Source**: World Settlement Footprint 3D (WSF3D)

2. **Building Height**: Provides the average height of buildings in each grid cell.
   - **Source**: World Settlement Footprint 3D (WSF3D)

3. **Urban Fraction (FRC_URB)**: Indicates the fraction of each grid cell occupied by urban land cover (buildings, roads, etc.).
   - **Source**: Global Urban Fraction Dataset

4. **Height-to-Width Ratio (H2W)**: Represents the ratio of building height to the width of streets or spaces between buildings. This parameter was derived from **Local Climate Zones (LCZ)** data using values from **Stewart and Oke (2012)**. LCZ classes provide standardized urban form descriptors, and the mid-point values for H2W from this classification system were used to approximate the building geometry.
   - **Source**: LCZ Global Dataset, Stewart & Oke (2012)

#### Methodology:
**LAMBDA_B** was calculated using the following equation:
\[
LAMBDA_B = LAMBDA_P + LAMBDA_F
\]
Where:
- **LAMBDA_P**: The **Building Fraction**, representing the percentage of the grid cell area covered by buildings.
- **LAMBDA_F**: The building geometry contribution, which incorporates building height, width, and street width. This is given by the formula:
  \[
  LAMBDA_F = \frac{2 \times \text{Building Height}}{\text{BW} + \text{SW}}
  \]
  - **BW (Building Width)** is calculated as:
    \[
    BW = \frac{LAMBDA_P}{\text{Urban Fraction} - LAMBDA_P}
    \]
  - **SW (Street Width)** is derived from the **Height-to-Width Ratio (H2W)** using the following equation:
    \[
    SW = \frac{\text{Building Height}}{\text{H2W}}
    \]

#### Calculation Process:
1. **Mapping LCZ Mid-Point Values**: The **H2W** values for each LCZ class were taken from the **Stewart and Oke (2012)** table, which provides mid-point values for typical urban form and geometry. These H2W values were applied globally by matching LCZ classifications to corresponding grid cells.
   
2. **Aligning Data Sources**: The input datasets (building fraction, building height, urban fraction, and LCZ-based H2W) had different resolutions and projections. To ensure consistency, all datasets were resampled and reprojected onto a common global grid (approximately 100-meter resolution, WGS84 projection).
   
3. **Calculating LAMBDA_B**:
   - For each grid cell, the building fraction (**LAMBDA_P**) and geometric contribution (**LAMBDA_F**) were calculated using the formulas above.
   - The result for each cell was stored in a new global dataset representing **LAMBDA_B**, capturing both the fraction of the grid cell covered by buildings and the geometry of those buildings.

#### Challenges:
- **Lack of Unified Data**: No global dataset directly provides **LAMBDA_B**. Instead, we combined available datasets on building fraction, building height, and urban fraction, while deriving the H2W ratio from **Stewart and Oke (2012)** LCZ classifications.
- **Data Alignment**: Ensuring that all datasets aligned spatially and had the same resolution was a significant challenge. The resampling process was necessary to ensure accurate calculations across the globe.

#### Outcome:
The resulting **LAMBDA_B** dataset provides detailed information about urban morphology at a global scale. This dataset can now be used as a critical input for urban climate models like WRF-BEM, allowing them to more accurately represent the impact of building geometry on climate dynamics, such as heat and airflow in cities.

By leveraging available global datasets and the LCZ classification system, this newly generated **LAMBDA_B** dataset bridges a crucial gap in urban climate modeling, enabling more detailed and accurate simulation of urban environments.

#### References:
- **Stewart, I. D., & Oke, T. R. (2012)**. Local Climate Zones for Urban Temperature Studies. *Bulletin of the American Meteorological Society, 93*(12), 1879-1900.
