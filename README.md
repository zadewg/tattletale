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

A redacted mailbox JPEG image Fig.1. (a) is bruteforced with a dictionary made from photographs of similar number tags Fig.1. (b). This step is aided from other sources of intelligence (OSINT et al.), similar mailboxes appearances can be obtained from a simple ebay search. Note that it is trivial to generate dictionaries for text documents.




&nbsp;

| ![Estimation](/NUS/fig_7.jpg)
|:--:| 
| **Fig.2.** Results on the redacted image in Fig. 1(a). |

&nbsp;



#### Attack model:

For the vulnerability to be exploited, the analysed image must have been compressed before and after the redaction process, -the standard, as digitalizing, manipulating, then exporting involves two compression steps-, and the compression first quality factor should be lower than the second.

&nbsp;

| ![Effectiveness](/NUS/fig_5.jpg)
|:--:| 
| **Fig.3.** (a) Success rate for binary image with JPEG compression quality δ 1 = 50 (b) Success rate for binary image using second method with wavelet transform quantization step δ 2 = 1/100 |

---
