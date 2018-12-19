# CycleGAN
*Unpaired Image-toImage Translation using Cycle-Consistent Adversarial Networks[[paper]](https://arxiv.org/pdf/1703.10593.pdf)

<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/cyclegan.png">
</p>


<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L(G,F,D_X,D_Y)&space;=&space;L_{GAN}(G,D_Y,X,Y)&space;&plus;&space;L_{GAN}(F,D_X,Y,X)&space;&plus;&space;\lambda&space;L_{cyc}(G,F)">
</p>


## cycle consistent loss
<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/cyclegan_comprisons.png">
</p>

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{cyc}(G,F)=&space;\mathbb{E}_{x&space;\sim&space;P_{data}(x))}[||F(G(x))-x||_1]&space;&plus;&space;\mathbb{E}_{y&space;\sim&space;P_{data}(y))}[||G(F(x))-y||_1]">
</p>



* mode collapse 방지
* L1 loss 대신에 adversarial loss 적용해보았으나 성능향상을 얻지 못하였다고 함
* 앞항을 forward cycle loss, 뒷항을 backward cycle loss라고 부름. 




## Identity loss

<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/cyclegan_identityloss.PNG">
</p>

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{identity}(G,F)&space;=&space;\mathbb{E}_{y\sim&space;P_{data(y)}}[||G(y)-y||_1]&space;&plus;&space;\mathbb{E}_{x&space;\sim&space;P{data(X)}}[||F(x)-x||_1]">
</p>

* Regularize the generator to be near a identity mapping when real samples of the target domain are provided as the input to the generator
* 이미지가 이미 타겟도메인처럼 보일경우 더 이상 변형되지 않도록 함 -> 필요이상으로 변형되는것을 막음 -> 색까지 바뀌는것을 방지함

[*identity loss 관련 issue:] [https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix/issues/322](https://github.com/junyanz/pytorch-CycleGAN-and-pix2pix/issues/322)



# Augmented CycleGAN
*Augmented CycleGAN: Learning Many-to-Many Mappings from Unpaired Data (ICML 2018) [[paper]](https://arxiv.org/pdf/1802.10151.pdf)

<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/augcyclegan.png">
</p>


<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L(G,F,D_X,D_Y)&space;=&space;L_{GAN}(G,D_Y,X,Y)&space;&plus;&space;L_{GAN}(F,D_X,Y,X)&space;&plus;&space;\lambda&space;L_{cyc}(G,F)">
</p>
