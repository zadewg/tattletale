# TATTLETALE

<!--

`TATTLETALE` is a an information leak vulnerability in JPEG redacted documents exploiting Block Direct Cosine Transform (DCT) compresion that allows an attacker to conduct statistical buteforce attacks to recover blacked out information.

-->

`TATTLETALE` is a security exploit on JPEG redacted documents exploiting an information leak in Block Direct Cosine Transform (DCT) compresion that allows an attacker to recover blacked out information through a statistical-bruteforce approach. 

#### From Nicholas Zhong-Yang Ho, Ee-Chien Chang research at NUS:

> *Since digital images are usually lossily compressed via quantization in the frequency domain, each pixel in the spatial domain will be “spread” to its surroundings, similar to the Gibbs-effect, before it is redacted. Hence, information of the original pixels might not be completely purged by replacing pixels in the compressed image.*

&nbsp;

| ![Sanitized](/NUS/fig_6.jpg)
|:--:| 
| **Fig.1.** (a) Image captured by a Nokia 6125 mobile phone and then redacted. (b) Templates of postal boxes. |

&nbsp;

A postal box image was taken with a Nokia 6125 mobile phone (“normal” JPEG compression quality, image size at 640 × 480, grey scale effect). This image is then redacted and compressed with quality δ 2 = 90 as shown in Fig. 1(a). The redacted text in the top and bottom left is “10-335” and “10-339” respectively. We assume that the adversary knows the first compression quality δ 1, and he knows that the text is one of the five candidates indicated in Fig. 1(b).

To prepare the templates, high quality 5 megapixels images of similar postal boxes were taken with a FujiFilm FinePix 31fd digital camera. The high quality images were then digitally adjusted to estimate the templates as shown in Fig. 1(b).

&nbsp;

| ![Estimation](/NUS/fig_7.jpg)
|:--:| 
| **Fig.2.** Results of second method on the redacted image in Fig. 1(a). |

&nbsp;

| ![Effectiveness](/NUS/fig_5.jpg)
|:--:| 
| **Fig.3.** (a) Success rate for binary image with JPEG compression quality δ 1 = 50 (b) Success rate for binary image using second method with wavelet transform quantization step δ 2 = 1/100 |

#### Attack model:

For the vulnerability to be exploited, the analysed image must have been compressed before and after the redaction process, -the standard, as digitalizing, manipulating, then exporting involves two compression steps-, and the compression first quality factor should be lower than the second.

&nbsp;

---
