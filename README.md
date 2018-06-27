# Random-ML-QnA
Some quirks and not so obvious things about ML models. A FAQ style document. For personal reference

#### Why do Resnet (and later) models training curve see step-wise improvments in accuracy after seemingly coverging ?
The learning rate is stepped by a factor of 10 every few 1000 iterations. Everytime the LR is decreased, we see a large improvment in accuracy.

