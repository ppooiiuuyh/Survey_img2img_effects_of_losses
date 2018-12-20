# ◆  CycleGAN
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


# ◆ Augmented CycleGAN
*Augmented CycleGAN: Learning Many-to-Many Mappings from Unpaired Data (ICML 2018) [[paper]](https://arxiv.org/pdf/1802.10151.pdf)
<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/augcyclegan2.PNG" width="400">
</p>

<p align="center">
<img src="https://raw.githubusercontent.com/ppooiiuuyh/Survey_img2img_effects_of_losses/master/assets/augcyclegan.PNG">
</p>


<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{A&space;\times&space;Z_b&space;\rightarrow&space;B&space;\times&space;Z_a}&space;=&space;L_{GAN}^B(D_B,G_{AB})&space;&plus;&space;L_{GAN}^{Z_a}(D_{Z_a},E_A,G_{AB})&space;&plus;&space;\gamma_1&space;L_{CYC}^A(G_AB,G_BA,E_A)&plus;\gamma_2&space;L_{CYC}^{Z_b}(G_{AB},E_B)">
</p>


* CycleGAN의 translation이 deteministic해지는 문제를 해결하면서 multimodal한 output을 mapping할 수 있도록 latent Z를 augment함
* 일반적으로 noise z를 input에 concatenation하는 것과는달리 z로 부터 선형함수(신경망으로 근사)로 mu와 sigma를 구한 후 G의 일부 intermediate layer (코드상에서는 3레이어)에 instance norm을 적용하는 방식을 사용하였다.

## CycleGAN with Stochastic Mappings
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{GAN}^B(G_{AB},D_B)&space;=&space;\mathbb{E}_{b&space;\sim&space;P_d(b)}[logD_B(b)]&plus;\mathbb{E}_{a&space;\sim&space;P_d(a),z&space;\sim&space;p(z)}[log(1-D_(G_{AB(a,z)}))]">
</p>
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{CYC}^A(G_{AB},G_{BA})&space;=&space;\mathbb{E}_{a&space;\sim&space;P_d(a),z_1,z_2&space;\sim&space;p(z)}||G_{BA}(G_{AB}(a,z_1),z_2))-a||_1">
</p>


* CycleGAN을 stochastic하게 만들기 위하여 G에 노이즈 latent z를 추가.
* 하지만 cycle loss는 노이즈 z를 무시하는쪽으로 학습시킨다.
* (그러나 실험적으로는 꽤나 잘나왔다. 다만 A->B->A->B 과정에서 multimodal한 output을 얻어지지 않았다. steganograpy 현상 발생).


* 따라서 다음처럼 변경 (Augmented)
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?\tilde{b}&space;=&space;G_{AB}(a,z_b),&space;\tilde{z_b}&space;=&space;E_A(a,\tilde{b})">
</p>

* 주어진 pair (a,zb) ~ Pd(a)P(zb) 에서 pair(B~, z~a) 를 생성.
* cycle-consistency with stochastic을 strutured mapping이 될 수 있도록 돕는다.

## Marginal Matching Loss
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{GAN}^{Z_a}(E_A,G_{AB},D_Z_a)&space;=&space;\mathbb{E}_{z_a&space;\sim&space;p(z_a)}[logD_Z_a(z_a)]&space;&plus;&space;\mathbb{E}_{a&space;\sim&space;p_d(a),z_b&space;\sim&space;p(z_b)}[log(1-D_Z_a(\tilde&space;z_a))]">
</p>

* Ea로 부터 인코딩되는 z~a가 p(za)의 분포를 가지도록 만든다.
* z에 대한 GAN

## Augmented Cycle Consistency Loss
<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{CYC}^A(G_{AB},G_{BA},E_A)&space;=&space;\mathbb{E}_{a\sim&space;P_{d(a)},&space;z_b&space;\sim&space;p(z_b)}[||a'-a||_1]">
</p>

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?\tilde&space;b&space;=&space;G_{AB}(a,z_b),&space;\tilde&space;z_a&space;=&space;E_A(a,&space;\tilde&space;b),&space;a'=G_{BA}(\tilde&space;b,&space;\tilde&space;z_a)">
</p>

<p align="center">
<img src="https://latex.codecogs.com/gif.latex?L_{CYC}^{Z_b}(G_{AB},G_{BA},E_A)=\mathbb{E}_{a&space;\sim&space;P_d(a),z_b\sim&space;P_d(z_b)}||z'_b&space;-&space;z_b||_1">
</p>


<p align="center">
<img src="https://latex.codecogs.com/gif.latex?\tilde&space;z_b&space;=&space;E_B(a,&space;\tilde&space;b),&space;\tilde&space;b&space;=&space;G_{AB}(a,z_b)">
</p>

* z에 대한 cycle consistency loss가 추가되었다
* z에 대한 marginal matching loss가 z를 normal distribution을 가지도록 만드는 역할을 한다면, z에대한 cycle consistency loss는 z,a-> b~ 과정에서 b~ 에 z에 대한 정보가 더 명시적으로 포함되도록 만든다. 즉 z를 무시하지 않는쪽으로 학습되도록 만든다.

