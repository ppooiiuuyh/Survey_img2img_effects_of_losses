# CycleGAN
*Unpaired Image-toImage Translation using Cycle-Consistent Adversarial Networks[[paper]](https://arxiv.org/pdf/1703.10593.pdf)

<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/cyclegan.png">
</p>


<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L(G,F,D_X,D_Y)&space;=&space;L_{GAN}(G,D_Y,X,Y)&space;&plus;&space;L_{GAN}(F,D_X,Y,X)&space;&plus;&space;\lambda&space;L_{cyc}(G,F)">
</p>

######
######
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


######
######
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


######
######
######
######
# Augmented CycleGAN
*Augmented CycleGAN: Learning Many-to-Many Mappings from Unpaired Data (ICML 2018) [[paper]](https://arxiv.org/pdf/1802.10151.pdf)

<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/augcyclegan.PNG">
</p>


<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{A&space;\times&space;Z_b&space;\rightarrow&space;B&space;\times&space;Z_a}&space;=&space;L_{GAN}^B(D_B,G_{AB})&space;&plus;&space;L_{GAN}^{Z_a}(D_{Z_a},E_A,G_{AB})&space;&plus;&space;\gamma_1&space;L_{CYC}^A(G_AB,G_BA,E_A)&plus;\gamma_2&space;L_{CYC}^{Z_b}(G_{AB},E_B)">
</p>


* CycleGAN의 translation이 deteministic해지는 문제를 해결하면서 multimodal한 output을 mapping할 수 있도록 latent Z를 augment함

######
######
## CycleGAN with Stochastic Mappings
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{GAN}^B(G_{AB},D_B)&space;=&space;\mathbb{E}_{b&space;\sim&space;P_d(b)}[logD_B(b)]&plus;\mathbb{E}_{a&space;\sim&space;P_d(a),z&space;\sim&space;p(z)}[log(1-D_(G_{AB(a,z)}))]">
</p>


* CycleGAN을 stochastic하게 만들기 위하여 G에 노이즈 latent z를 추가
* 하지만 cycle loss는 노이즈 z를 무시하는쪽으로 학습시킨다.
* (그러나 실험적으로는 꽤나 잘나왔다. 다만 A->B->A->B 과정에서 multimodal한 output을 얻어지지 않았다. steganograpy 현상 발생)
