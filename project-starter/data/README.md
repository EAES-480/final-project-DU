# data

This folder contains the dataset used for the final project.

## data_final_project.csv

**Dimensions:** 6868 rows × 26 columns

**Unit of observation:** Each row represents a soil horizon. Multiple rows may belong to the same pedon (soil profile).

### Codebook

- `source`: Data source or dataset identifier for each pedon record  
- `pedon_id`: Unique identifier for each soil profile (pedon)  

- `order`: Soil taxonomic order (e.g., Spodosols, Inceptisols), indicating general soil classification  

- `lat`: Latitude of the pedon location (decimal degrees)  
- `lon`: Longitude of the pedon location (decimal degrees)  
- `latlon_q`: Quality flag indicating reliability of location coordinates  

- `horizon_number`: Sequential number indicating the position of the horizon within the pedon  
- `horizon`: Horizon designation (e.g., O, A, B), describing soil layer type  
- `horizon_type`: Generalized horizon category (e.g., Mineral or Organic)  

- `depth1`: Upper depth of the horizon (cm)  
- `depth2`: Lower depth of the horizon (cm)  
- `depth`: Thickness of the horizon (cm), typically calculated as depth2 − depth1  

- `bulk_density`: Soil bulk density (g/cm³), representing mass per unit volume of soil  
- `bd_method`: Method used to measure or estimate bulk density  

- `cf`: Coarse fragment content (proportion or %) within the soil horizon  
- `cf_method`: Method used to determine coarse fragment content  

- `cconc`: Carbon concentration within the horizon (e.g., % or g C per unit mass)  
- `cconc_method`: Method used to determine carbon concentration  

- `ccontent`: Carbon content within the horizon (e.g., carbon stock per unit area for that layer)  

- `mineral_d`: Depth of mineral soil within the profile (cm)  
- `ff_d`: Depth of the forest floor (organic layer) within the profile (cm)  
- `total_d`: Total depth of the soil profile considered (cm)  

- `total_c`: Total carbon stock within the full profile (not standardized to depth)  

- `ccontent_1m`: Carbon content standardized to 1 m depth (may be interpolated or adjusted)  
- `total_c_1m`: Total soil carbon stock integrated to 1 m depth (response variable used in this project)  

- `pedon_start`: Logical indicator marking the first row of each pedon (TRUE = start of new soil profile)