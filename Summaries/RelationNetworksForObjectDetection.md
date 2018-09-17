
Weekly report

# Relation Networks for Object Detection

## key points :

1. Introduces plug-in relation module to compute inplace relations between detected objects (bbox)
2. Heavily inspired from self-attention paper (Scaled Dot-Product Attention)
3. Shallow relation module for duplicate removal replaces NMS, so the detector is trained end-to-end (first of its kind ?)

### Salient points of the relation module

1. Parallel and learnable
2. No extra supervision
3. Unlike stock attention, this also uses geometric features to compute relations ina addition to appearance (pixel) features
3. Translation invariant (geometric features are also scale invariant)
4. In place and stackable

### Relation module (ORM)

f_a is the appearance feature : pixel information
all appearance features are projected into  asubspace using Wk and Wq matrices
Query weight is the *f_a* object we are concerned with.
The key weight is *f_a* for all other objects wrt. to which a relation is computed
scaled dot product is a 1-object to all objects relation

* Appearance feature *f_a* - pixel information of each object
* *f_a* projected into a subspace 
* object-object score computed via dot product
* geometric feature f_g - bounding box
* f_g -> networwk -> w_g
* f_g, w_g : translation & scale invariant
* joint weighted sum to get relation vector of one object
* many branches for different types of features
* concat all branches residually
* relation output is same dimension as the input image
* (Fuse?) with appearance to get new feature map

### NMS relacement module

* ORM returns a non-duplicate score map
* score map multiplied with original score to get final score (element wise weighted muleiplication)

### Key Observation

* Learnt attention weights closely mimic the class co-occurence probability (implicitly learns it)
* Not efficient for dense anchors or dense region proposals, but dense proposals may be redudnant to begin with
* complexity is proportional to sq. of region proposals, so can't use for sliding window methods like YOLO, SSD
* 

# Some questions

1. In 
