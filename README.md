# TATTLETALE

`TATTLETALE` is a an information leak vulnerability in JPEG redacted documents exploiting Block Direct Cosine Transform (DCT) compresion that allows an attacker to conduct statistical buteforce attacks to recover blacked out information.

#### From Nicholas Zhong-Yang Ho, Ee-Chien Chang research at NUS:

> *Since digital images are usually lossily compressed via quantization in the frequency domain, each pixel in the spatial domain will be “spread” to its surroundings, similar to the Gibbs-effect, before it is redacted. Hence, information of the original pixels might not be completely purged by replacing pixels in the compressed image.*


#### Vulnerable files:

The usual, correct, approach for document redaction consists on digitalizing (compressing), manipulating, and exporting (compressing) the modified file. This process inherently involves two compressing steps. For an image to be vulnerabe to this attack, the first compression should be lower than the second.


### Attack model:

&nbsp;

---
