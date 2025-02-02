
## changes to loss
use Class-Weighted BCE Loss  - need to see ratio of fire to non fire pixels -> change to focal loss if need be


## changes to metrics 
change f1 for Spatial F1-score so that its actually good???
Cohen's Kappa is similar 
Precision-Recall AUC
Intersection over Union (IoU) 
Precision-Recall with Buffer Zones



## Add in the features that Om has
- NDVI (mean,std) (Normalized Difference Vegetation Index (vegetation greeness))
- LST  (mean,std) (done) 
- PRCP_GPM (mean,std) (Global Precipitation Measurement) 
- Elevation_DEM (elevation digitial electronic model)
- LandWater_DEM (give it a good guess)
- Slope_DEM
- ALBEDO (mean,std)

## a bunch of random stuff for smoteish alteration:
use GAN (Generative Adversarial Networks) to generate more data but idt will help with the imbalance issue
Monitor Spatial Bias: Ensure the model doesn’t favor regions with majority-class dominance.
Temporal-Spatial Data Augmentation
    For the minority class, augment data by:
    Adding noise (within realistic bounds for geospatial features).
    Applying slight spatial shifts (if coordinates are features).
    Perturbing time steps (e.g., lagging/leading sequences).
    Example: If a geospatial event is rare, simulate similar events in neighboring regions or time windows.

Temporal Data Augmentation: Introduce synthetic samples by shifting or interpolating between timesteps.
Geospatial Interpolation: Use spatial interpolation methods (e.g., kriging or bilinear interpolation) to generate synthetic geospatial data.
Cost-Sensitive Learning: Modify the loss function to penalize misclassifications of minority classes more heavily.

Temporal Data Augmentation: Introduce synthetic samples by shifting or interpolating between timesteps.
Geospatial Interpolation: Use spatial interpolation methods (e.g., kriging or bilinear interpolation) to generate synthetic geospatial data.
Cost-Sensitive Learning: Modify the loss function to penalize misclassifications of minority classes more heavily.


## do at some point
investigate why MSL is so big in loss ??? (i think theres still some gridding)
run program to see what the ratio of fire to no fire is
add smote - no smote beacuse of geospatial?
confusion matrices
platt scaling / isotonic regression - use after model is already good to tweak accuracy% (if model is 50% correct it but it says wildfire with 80% confidence, is used to lower outputted confidence)


## current calcs

pre modified loss (23 batches)
2t loss: 4.93794550493476e-06
10u loss: 1.7159114804599085e-06
10v loss: 1.5614158428434166e-06
msl loss: 0.000561613473109901
fire loss: 7.137238299037563e-06
lst loss: 0.0030216278973966837
t loss: 2.0135273359755956e-07
u loss: 2.6278055997863703e-07
v loss: 2.155295959482828e-07
q loss: 2.327139166319412e-14
z loss: 6.408500485122204e-05
Loss: 0.0002


post modified loss:

fire loss: 0.010981019095271804
lst loss: 0.020034542307257652
-> 
2t loss: 5.283165592118166e-06
10u loss: 1.7294519238930661e-06
10v loss: 1.636983824937488e-06
msl loss: 0.0006271835882216692
fire loss: 0.008410677631618506
lst loss: 0.0030021679122000933
t loss: 1.8691474679144449e-07
u loss: 2.7673857516674616e-07
v loss: 2.10157764968244e-07
q loss: 2.4465557346958072e-14
z loss: 6.814231892349198e-05
Loss: 0.0086

previous before 
Accuracy: 56.6987
Precision: 0.3208
Recall: 13.6365
F1 Score: 0.6253
IoU: 0.3141

now after stuff
Accuracy: 0.9822
Precision: 0.0213
Precision (Buffer): 0.0554
Recall: 0.0823
Recall (Buffer): 0.1886
F1 Score: 0.0339
IoU: 0.0172
Precision-Recall AUC: 0.0181
Spatial F1-Score: 0.0856

changes:
- increased weight of fire in loss (20 -> 1000)
- improved bce loss
    - added weight towards fire (getting fires wrong incurs much more loss) need to tweak weight!!
    - added proximity weights that gives points to close nough fires
    - dialated fire masks to give more leeway
    - distance regularization to increase loss for fires in the middle of nowhere
- added more accuracy metrics
    - Precision-Recall with Buffer Zones
    - Precision-Recall AUC
    - Spatial F1-score




