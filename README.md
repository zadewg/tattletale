# TATTLETALE

<!--
-->

`TATTLETALE` is a security exploit on JPEG redacted documents leveraging an information leak in Wavelet / Block Direct Cosine Transform (DCT) compresion that allows an attacker to recover blacked out information through a statistical-bruteforce approach. 

#### From Nicholas Zhong-Yang Ho, Ee-Chien Chang research at NUS:

> *Since digital images are usually lossily compressed via quantization in the frequency domain, each pixel in the spatial domain will be “spread” to its surroundings, similar to the Gibbs-effect, before it is redacted. Hence, information of the original pixels might not be completely purged by replacing pixels in the compressed image.*

&nbsp;

| ![Sanitized](/NUS/fig_6.jpg)
|:--:| 
| **Fig.1.** (a) Image captured by a Nokia 6125 mobile phone and then redacted. (b) Templates of postal boxes. |


A redacted mailbox JPEG image Fig.1. (a) is bruteforced with a dictionary made from photographs of similar number tags Fig.1. (b). This step is aided from other sources of intelligence (OSINT et al.), similar mailbox tag templates can be obtained from a simple ebay search. Note that it is trivial to generate dictionaries for text documents, and effectiveness is substantially higher.

---

| Data      | Quantization error   | 
| ------------- |:-------------:| 
| RANDOM        |         104,9 | 
| 10-335        | 69,1          |  
| 10-339        |           67,1|  
| 08-331        | 71,7          |  
| 11-335        | 72,8          |  
| 11-339        | 73,7          |  


The bottom left box is analyzed with the generated dictionary, the result with the least quantization error is the most accurate estimation of the blacked out data. The table above shows the quantization error for each template, the correct number is indeed 10-339.

&nbsp;



#### Attack model:

For the vulnerability to be exploited, the analyzed image must have been compressed before and after the redaction process, (*the standard, as digitalizing, manipulating, then exporting involves two compression steps*). The first compression quality factor should also be lower than the second.

`The redaction process goes as follows:`

1. The original file is compressed with quality factor *Q1*. This step can be understood as a document being scanned or exported, or as a raw image being JPEG converted.  
![FIRST](/NUS/for_1_1(1).jpg)  

2. The sub-image *r* is redacted with a Mask *M*, usually a rectangular black or white box.  
![SECOND](/NUS/for_2_1(1).jpg)  

3. The redacted image is now exported (compressed again) with quality factor *Q2*.
![THIRD](/NUS/for_3.jpg)  


The goal for an adversary now is to reverse the redaction process, obtaining the sub-image *r*. To do so, the compression quality factors *Q1, Q2* are required.

 - Obtaining *Q2* is trivial, as it is avaible from the JPEG image header. In the case this metadata is purged from the file, it can be easily and accurately aproximated with *ImageMagic* and other common tools.
 

 - As to *Q1*, it can be estimated with the software contained in this repository. The mathematics of that process is out of the scope of this documentation and can be consulted from sources [2], [3].
 
 ``` Matlab
 // soon
 ```
 

&nbsp;

| ![Effectiveness](/NUS/fig_5.jpg)
|:--:| 
| **Fig.3.** (a) Success rate for binary image with JPEG compression quality δ 1 = 50 (b) Success rate for binary image using second method with wavelet transform quantization step δ 2 = 1/100 |

---

### References

* [Residual Information of Redacted Images Hidden in the Compression Artifacts](http://ws2.binghamton.edu/fridrich/Research/paper_3_color.pdf)

* [Image Forgery Localization via Block-Grained Analysis of JPEG Artifacts](http://ws2.binghamton.edu/fridrich/Research/paper_3_color.pdf)

* [Estimation of Primary Quantization Matrix for Steganalysis of Double-Compressed JPEG Images - BU](http://ws2.binghamton.edu/fridrich/Research/paper_3_color.pdf)

---


**mapez** - [telegram](https://t.me/mapezz)
