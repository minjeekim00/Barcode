# Official Code Implementation for Barcode
This is a repository for the paper "Barcode Method for Generative Model Evaluation driven by Topological Data Analysis".

# Basic Concepts

- (Mutual) Fidelity : How well the generative model generates fake images.
- (Mutual) Diversity : How diverse the generated images are.

However, these two are relative values, not absolute values. Therefore, we suggest users to calculate **Relative Fidelity** and **Relative Diversity**.

- Relative Fidelity : (Fidelity between real and generated images) / (Fidelity between real and real images)
- Relative Diversity : (Diveristy between real and generated images) / (Diversity between real and real images)

These two can be considered as normalized fidelity and normalized diversity compared to real data.

# Usage

Take a look in

    barcode_example.py
    barcode.py

**barcode_example.py** is composed as:

```
barcode_realfake = Barcode(real, fake)
barcode_realreal = Barcode(real, real)
barcode_fakefake = Barcode(fake, fake)

rf_f, rf_d = barcode_realfake.get_barcode()
rr_f, rr_d = barcode_realreal.get_barcode()
ff_f, ff_d = barcode_fakefake.get_barcode()

print("Mutual Fidelity   : {:.3f} | Mutual Diversity   : {:.3f}".format(rf_f, rf_d))
print("Relative Fidelity : {:.3f} | Relative Diversity : {:.3f}".format(rf_f/rr_f, rf_d/(np.sqrt(rr_d) * np.sqrt(ff_d))))
```

**barcode_example.py** imports **barcode.py**. Therefore, **barcode.py** should be contemplated as well.

If relative fidelity is close to 1, quality of the generated images as good as the original images.

If relative diversity is close to 1, the generative model generates images as diverse as the original images.

# Environment

This code is written in tensorflow 1.15 version. However, the **barcode.py** code is executed in numpy ndarray format, therefore it does not depend on framework environment such as pytorch or tensorflow. All you have to do is:

1. Get embedding vectors from real and generated images using pretrained CNN network, such as [Inception v3](https://www.cv-foundation.org/openaccess/content_cvpr_2016/papers/Szegedy_Rethinking_the_Inception_CVPR_2016_paper.pdf).
2. Transform embedded tensors to numpy ndarray format.
3. Calculate metrics following **barcode_example.py**
