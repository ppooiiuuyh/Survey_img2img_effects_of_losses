Survey_img2img_effects_of_losses
##CycleGAN
##### Identity loss

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{identity}(G,F)&space;=&space;\mathbb{E}_{y\sim&space;P_{data(y)}}[||G(y)-y||_1]&space;&plus;&space;\mathbb{E}_{x&space;\sim&space;P{data(X)}}[||F(x)-x||_1]">
</p>

[*identity loss 관련 issue:] [https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix/issues/322](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix/issues/322)