

# Relation Networks for Object Detection

![](https://raw.githubusercontent.com/AnishPimpley/ML-wiki/master/media/relation%20network%20for%20obj%20detection%20graph.png)

### key points :

1. Introduces plug-in relation module to compute inplace relations between detected objects (bbox)
2. Heavily inspired from self-attention paper (Scaled Dot-Product Attention)
3. Shallow relation module for duplicate removal replaces NMS, so the detector is trained end-to-end (first of its kind ?)

### Salient points of the relation module

1. Parallel and learnable
2. No extra supervision
3. Unlike stock attention, this also uses geometric (bbox) features to compute relations ina addition to appearance (pixel) features
3. Translation invariant (geometric features are also scale invariant)
4. In place and stackable

### Relation module (ORM)

![](https://raw.githubusercontent.com/AnishPimpley/ML-wiki/master/media/Object%20relation%20module.png)

2 parts o f relation module : Appearance (pixel data) and Geometric (Bbox)

##### Appearance module
* f_a is the appearance feature : pixel information      
* Query weight is wrt. the *f_a* object we are concerned with.          
* The key weight is wrt. *f_a* for all other objects wrt. to which a relation is computed           
* all appearance features are projected into a subspace using Wk and Wq matrices           
* The the dot product between the 2 projected features is computed in the *dk* dimensional space         
* scaled dot product is a 1-object to all objects relation            

##### Geometric module
* geometric feature f_g - bounding box
* bbox is represented in translation & scale invariant manner as a 4 element vector
* This vector is embedding into a high dimensional space called *Episolon_g* using the same method as "attention is all you need"
* *Episilon_g* is tranformed with learned geometric weight *W_g* and relu is applied to its result

##### Fusing and output of module
* The geometric and appearance relations are combined using equation 3 below
* many branches for different types of feature relations
* concat all branches residually
* relation output is same dimension as the input image
* fuse with appearance features (which is input) to get new output feature map

##### Computationally these 4 equations govern the output :

![](https://raw.githubusercontent.com/AnishPimpley/ML-wiki/master/media/eq1.png)
![](https://raw.githubusercontent.com/AnishPimpley/ML-wiki/master/media/eq2.png)
![](https://raw.githubusercontent.com/AnishPimpley/ML-wiki/master/media/eq3.png)

where *f_r(n)* is the output.


### NMS relacement module

* ORM returns a non-duplicate score map
* score map multiplied with original score to get final score (element wise weighted muleiplication)

### Key Observation

* Learnt attention weights closely mimic the class co-occurence probability (implicitly learns it)
* Not efficient for dense anchors or dense region proposals, but dense proposals may be redudnant to begin with
* complexity is proportional to sq. of region proposals, so can't use for sliding window methods like YOLO, SSD as too inefficient
* So in a sense only works well with models that use sparse region proposals.
* Does not appear to capture any semantic relationship between labels (like cat is an animal, or the image being a particular scene)
* does capture co-occurance very well, but learns it only from this dataset and no general knowledge model
