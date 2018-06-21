# Random-ML-QnA
Some quirks and not so obvious things about ML models. A FAQ style document. For personal reference

#### Why do Resnet (and later) models training curve see step-wise improvments in accuracy after seemingly coverging ?
> The learning rate is stepped as well. Everytime the LR is decreased by a farctor of 10, we see a large improvment in accuracy.
