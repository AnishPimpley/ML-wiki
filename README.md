# Random-ML-QnA
Some quirks and not so obvious things about ML models. A FAQ style document. For personal reference

#### Why do Resnet (and later) models training curve see step-wise improvments in accuracy after seemingly coverging ?
The learning rate is stepped by a factor of 10 every few 1000 iterations. Everytime the LR is decreased, we see a large improvment in accuracy.

#### Misalignment of feature maps explained
The performance of a semantic segmentation model depends on accurate superimposition of predicted segmentation maps and ground truth.      
Superimposition implies that the final features maps should be aligned perfectly with the ground truths. In this context, it would require the center and scale of the final segmentation map to be identical to ground truths and input feature maps.
         
However, certain downsampling operations, namely max pooling and strided convolutions will translate the center of the downsampled output feature map with respect to the input feature map. (Note: Padding method for downsampling = 'SAME', computed as D_out = Ceil(D_in/Stride) )
This misalignment caused by translation is unavoidable for certain combinations of input feature map and filter size.
The combination are as follows: (for 'SAME' paddingMode, and Stride =2 )     
Here SAME implies padding chosen such that OutputDImension = Ceiling( inputDimension / Stride )

##### Common combinations and their effects on filterSize 
* Stride: 2, padding : SAME, kernel: odd, Input: odd => No misalignment
* Stride: 2, padding : SAME, kernel: even, Input: odd => center misaligned
* Stride: 2, padding : SAME, kernel: odd, Input: even => center misaligned
* Stride: 2, padding : SAME, kernel: even, Input: even => No misalignment
* Stride: 1, padding : NONE, kernel: odd, Input: odd => No misalignment (dimension decreases by 1)
