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



### Attack model

For the vulnerability to be exploited, the analyzed image must have been compressed before and after the redaction process, (*the standard, as digitalizing, manipulating, then exporting involves two compression steps*). The first compression quality factor should also be lower than the second.

`The redaction process goes as follows:`

1. The original file is compressed with quality factor *Q1*. This step can be understood as a document being scanned or exported, or as a raw image being JPEG converted.  
![FIRST](/NUS/for_1_1(1).jpg)  

2. The sub-image *r* is redacted with a Mask *M*, usually a rectangular black or white box.  
![SECOND](/NUS/for_2_1(1).jpg)  

3. The redacted image is now exported (compressed again) with quality factor *Q2*.
![THIRD](/NUS/for_3.jpg)  

---

The goal for an adversary now is to reverse the redaction process, obtaining the sub-image *r*. To do so, the compression quality factors *Q1, Q2* are required.

 - Obtaining *Q2* is trivial, as it is avaible from the JPEG image header. In the case this metadata is purged from the file, it can be easily and accurately aproximated with *ImageMagic* and other common tools.
 

 - As to *Q1*, it can be estimated with the [software](https://github.com/zadewg/tattletale/blob/master/run.m) contained in this repository. The mathematics behind that process are out of the scope of this documentation and can be consulted from sources [2], [3].
 
 > NOTE: Matlab code currently returns the quantization table only, a quantization table to factor converter is in development.
 
&nbsp;


 
 #### First method - Comparison in the spatial domain
 
When the attacker can reproduce the redacted file before it is compressed, a straight forward comparaison can be carried out. This is the case with text documents.
 
By mimicking the exact same font and format, a lot of approximaions to *R* can be quickly derived. This templates should then be compressed with *Q1*, redacted, and then recompressed with *Q2*. The following python code compresses an image with a given quality factor [1, 100]. 
 
 ``` Python
import StringIO
from PIL import Image
im1 = Image.open(IMAGE_FILE)

#create an empty string buffer    
buffer = StringIO.StringIO()
im1.save(buffer, "JPEG", quality=10)

#write the buffer to a file 
with open("./photo-quality10.jpg", "w") as handle:
    handle.write(buffer.contents())
 ```
&nbsp;

#### Second method - Comparison in the transformed domain

Unlike the first method, in this case an estimation of *R* can not be obtained. This is the case with photographs and other files.

`The following steps are performed to determine the sanitized data:`

1. An estimation of *I* is obtained by replacing the redacted section of *I3* by *T*, an element of the dictionary.
2. The image *~I* iscompressed with *Q1*, and the redacted section *S* is extracted.
3. A new image *~I3* is composed by replacing the redacted section in *I3* by *S*.
4. *~I3* is compressed again with *Q1*. The element *T* that minimizes the quantization error between *I3, ~I3* is the closest to the sanitized data.

There are several ways to measure the quantization error in step 4. In NUS research a Weighted Euclidean Distance where the weight is the inverse of the step size is employed. Suppose *C={c1,c2,...,ck}* a set of *k* coefficients, and *s1* the quantization step size for *c1*, the quantization error is:

![Q_ERR](/NUS/QE.jpg)


Note that the effect of the second compression is not taken into consideration and is treated as noise.

---

&nbsp;

| ![Effectiveness](/NUS/fig_5.jpg)
|:--:| 
| **Fig.2.** (a) Success rate for binary image with JPEG compression quality δ 1 = 50 (b) Success rate for binary image using second method with wavelet transform quantization step δ 2 = 1/100 |

&nbsp;

As mentioned before, the ration between the first and second compression factors determines the feasibility of this attack. The second compression directly affects how accurately the first one can be estimated. The graph above details the effectiveness of the attack for different factors.

---

### References

* [Residual Information of Redacted Images Hidden in the Compression Artifacts](https://www.comp.nus.edu.sg/~changec/publications/2008_IH_Residual_Information_Redacted_Image.pdf)

* [Image Forgery Localization via Block-Grained Analysis of JPEG Artifacts](https://ieeexplore.ieee.org/document/6151134)

* [Estimation of Primary Quantization Matrix for Steganalysis of Double-Compressed JPEG Images - BU](http://ws2.binghamton.edu/fridrich/Research/paper_3_color.pdf)

---
STILL UNDER DEVELOPMENT

**mapez** - [telegram](https://t.me/mapezz)
