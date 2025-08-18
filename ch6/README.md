## Chapter 6: Vector Quantized Sign Language Production - Qualitative Evaluation

By applying Vector Quantisation (VQ) to sign language data, we first learn a codebook of short motions that can be combined to create a natural sequence of sign. Where each token in the codebook can be thought of as the lexicon of our representation. Then using a transformer, we perform a translation from spoken language text to a sequence of codebook tokens. Each token can be directly mapped to a sequence of poses allowing the translation to be performed by a single network.

Overview of the approach to Sign Language Production (SLP) using Vector Quantization (VQ:

![system_overview]
*Fig. 1: A overview of our approach to Sign Language Production ( SLP). Showing from top to bottom 1) the source spoken language sentence, 2) the data-driven token representation of sign, 3) the synthesized pose sequence, and, 4) the original video.*

## Contents
- [Codebook Visualization](#codebook-visualization)
  - [RWTH-PHOENIX-Weather-2014**T**](#rwth-phoenix-weather-2014t)
  - [Meine DGS Annotated](#meine-dgs-annotated)
- [Translation Examples](#translation-examples)
  - [German Sign Language - Deutsche Gebärdensprache](#german-sign-language---deutsche-gebärdensprache)
  - [Meine DGS Annotated](#meine-dgs-annotated-1)
  - [Stitching Module](#stitching-module)
  - [SignGAN Module](#signgan-module)
  - [Comparison to Progressive Transformer](#comparison-to-progressive-transformer)
- [Publicly Released Code](#publicly-released-code)

    
Previous approaches to SLP attempt to regress pose directly from the spoken language. This leads to under-articulated signing, as the signer regresses to the mean. Whereas, here by first learning a codebook we can ensure our new lexicon is expressive.

## Codebook Visualization

Here we present example tokens from the Codebooks.

### RWTH-PHOENIX-Weather-2014**T**

<div align="center">

<table style="border-collapse: collapse;">
<tr>
  <td><img src="videos/codebook_examples/phix/Codebook_432.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_97.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_127.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_75.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_575.gif" width="150" height="150" /></td>
</tr>
<tr>
  <td><img src="videos/codebook_examples/phix/Codebook_436.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_296.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_714.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_51.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_42.gif" width="150" height="150" /></td>
</tr>
<tr>
  <td><img src="videos/codebook_examples/phix/Codebook_279.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_645.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_0.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_898.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_180.gif" width="150" height="150" /></td>
</tr>
<tr>
  <td><img src="videos/codebook_examples/phix/Codebook_664.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_249.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_330.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_383.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_368.gif" width="150" height="150" /></td>
</tr>
<tr>
  <td><img src="videos/codebook_examples/phix/Codebook_742.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_689.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_321.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_28.gif" width="150" height="150" /></td>
  <td><img src="videos/codebook_examples/phix/Codebook_449.gif" width="150" height="150" /></td>
</tr>
</table>

</div>

### Meine DGS Annotated


<div align="center">

<table style="border-collapse: collapse;">
<tr>
    <td><img src="videos/codebook_examples/mdgs/Codebook_5622.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_2131.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_1393.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_2093.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_4144.gif" width="150" height="150" /></td>
  </tr>
  <tr>
    <td><img src="videos/codebook_examples/mdgs/Codebook_2077.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_4307.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_2465.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_3088.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_4250.gif" width="150" height="150" /></td>
  </tr>
  <tr>
    <td><img src="videos/codebook_examples/mdgs/Codebook_3263.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_4366.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_3806.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_3172.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_5402.gif" width="150" height="150" /></td>
  </tr>
  <tr>
    <td><img src="videos/codebook_examples/mdgs/Codebook_2011.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_1857.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_3847.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_4726.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_3627.gif" width="150" height="150" /></td>
  </tr>
  <tr>
    <td><img src="videos/codebook_examples/mdgs/Codebook_2838.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_4594.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_1894.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_5814.gif" width="150" height="150" /></td>
    <td><img src="videos/codebook_examples/mdgs/Codebook_3786.gif" width="150" height="150" /></td>
  </tr>
</table>

</div>

## Translation Examples

Here we present translation examples.

**Left skeleton** - the ground truth extracted from the original videos. 

**Middel skeleton** - applying the codebook to quantize the ground truth sequence.

**Right skeleton**  - the translation output from the Text-to-Tokens transformer. 

Note, in the following examples we show the baseline model without the stitching module. In "Comparison to Progressive Transformer" below we add the stitching module to show its effectiveness for creating smoother natural signing sequences.

### German Sign Language - Deutsche Gebärdensprache

#### RWTH-PHOENIX-Weather-2014**T**

<div align="center">

<img src="./videos/translation_examples/phix/Sequence_1.gif" width="850" height="300" />
<img src="./videos/translation_examples/phix/Sequence_19.gif" width="850" height="300" />
<img src="./videos/translation_examples/phix/Sequence_305.gif" width="850" height="300" />
<img src="./videos/translation_examples/phix/Sequence_387.gif" width="850" height="300" />
<img src="./videos/translation_examples/phix/Sequence_404.gif" width="850" height="300" />

</div>


Failure case:

The PHOENIX14T dataset only contains a single view of the signer. As a result, our pose estimator struggles to capture some high-frequency movements, as can be seen in the ground truth data. 
In addition, for longer sequences, the model can struggle to capture all the fine-grain detail in the handshape.

<div align="center">

<img src="./videos/translation_examples/phix/Sequence_395.gif" width="850" height="300" />
<img src="./videos/translation_examples/phix/Sequence_364.gif" width="850" height="300" />

</div>

#### Meine DGS Annotated

<div align="center">

<img src="./videos/translation_examples/mdgs/1178133-6_15.gif" width="850" height="300" />
<img src="./videos/translation_examples/mdgs/1178133-53_18.gif" width="850" height="300" />
<img src="./videos/translation_examples/mdgs/1428475-13374607-13392508-23_41.gif" width="850" height="300" />

</div>

Failure case:

The Meine DGS dataset is captured from two native deaf signers having a conversation, while the Phoenix dataset comes from TV weather broadcasts. The result is that the camera setups used are distinctly different. Signers from the Meine DGS dataset are looking at each other while the camera is off-center. The result is a slight morphing in the face mesh shape.

Plus, as Meine DGS is a challenging dataset, errors can occure from the transltion. As expected from the low resoruce language. The following examples visulise the errors.

<div align="center">

<img src="./videos/translation_examples/mdgs/1290121-67_2.gif" width="850" height="300" />
<img src="./videos/translation_examples/mdgs/1290121-73_5.gif" width="850" height="300" />

</div>

#### Stitching Module

<div align="center">

<img src="./videos/stitching_example/example1.gif" width="850" height="300" />
<img src="./videos/stitching_example/example2.gif" width="850" height="300" />
<img src="./videos/stitching_example/example3.gif" width="850" height="300" />


</div>

#### SignGAN Module

Here we present outputs from the SignGAN module. The SignGAN module is trained to generate realistic signing sequences. But we do note some artifacts in the generated sequences, caused by projecting the skeleton to 2D.

<div align="center">

<img src="./videos/gan_examples/01April_2011_Friday_tagesschau-3388.gif" width="800" height="300" />
<img src="./videos/gan_examples/26July_2010_Monday_tagesschau-6266.gif" width="800" height="300" />
<img src="./videos/gan_examples/04July_2010_Sunday_tagesschau-7206.gif" width="800" height="300" />


</div>

#### Comparison to Progressive Transformer

Here we compare our full approach to the progressive transformer. We apply stitching module. Hence, the examples show smooth continuous signing.

<div align="center">

<img src="./videos/pt_comparison/02November_2010_Tuesday_heute-2742_35.gif" width="850" height="300" />
<img src="./videos/pt_comparison/12December_2011_Monday_tagesschau-8282_23.gif" width="850" height="300" />
<img src="./videos/pt_comparison/30September_2012_Sunday_tagesschau-4038_4.gif" width="850" height="300" />


</div>
<p align="right">(<a href="#readme-top">back to top</a>)</p>

## Publicly Released Code

📌 Companion codebase: [Sign VQ Transformer](https://github.com/walsharry/Sign-VQ-Transformer)

<!-- MARKDOWN LINKS & IMAGES -->
[system_overview]: Images/vq_slp_overview.png
[license-shield]: https://img.shields.io/github/license/othneildrew/Best-README-Template.svg?style=for-the-badge
[license-url]: https://github.com/othneildrew/Best-README-Template/blob/master/LICENSE.txt

Copyright (c) 2025 Harry Walsh
