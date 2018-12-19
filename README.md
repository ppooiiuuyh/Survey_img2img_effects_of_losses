Survey_img2img_effects_of_losses
##CycleGAN
<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/cyclegan.png">
</p>


##### Identity loss

<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/cyclegan_identityloss.PNG">
</p>

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{identity}(G,F)&space;=&space;\mathbb{E}_{y\sim&space;P_{data(y)}}[||G(y)-y||_1]&space;&plus;&space;\mathbb{E}_{x&space;\sim&space;P{data(X)}}[||F(x)-x||_1]">
</p>

* Regularize the generator to be near a identity mapping when real samples of the target domain are provided as the input to the generator
* 이미지가 이미 타겟도메인처럼 보일경우 더 이상 변형되지 않도록 함 -> 필요이상으로 변형되는것을 막음 -> 색까지 바뀌는것을 방지함

[*identity loss 관련 issue:] [https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix/issues/322](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix/issues/322)