# weekly report 1 about 4K4D

## Code reproduction

We tried to reproduce it using a cloud server, but it relies on ‘easyVolcap’, a volumetric video framework.We tried several cloud servers but did not find an available GUI.Right now we're trying to run 4K4D locally with a 4060ti video card, but we're also experiencing a lack of video memory, which we're trying to resolve.

## doubtful point

The biggest problem I see with this paper is that it is not as ‘advanced’ as the article makes it out to be, and this problem is mainly in the training time.

Compared to other models in the same field, the training duration of 4K4D seems extremely exaggerated, reaching **24h** (RTX4090) for a 200-frame sequence length, compared to **40 minutes** for Enerf's fine-tuning after 11,000 iterations, and **a few minutes** （8 on D-NERF, 30 on HyperNeRF)for 4DGS.

The key to this is that it computes and stores all the depth data as ‘preprocessing’, which is the reason for the large training time. Trading huge training hours for an increase in rendering frame rate, but 24 hours of training hours for only 200 fps rendering, seems like an excessive gap.

## Strengths and what we can learn from

It proposes a microscopic Depth Peeling algorithm that effectively excludes interfering information.It consists of K rendering passes. Consider a particular image pixel u. In the first pass, it first uses the hardware rasterizer to render point clouds onto the image, which **assigns the closest-to-camera point x0 to the pixel u**. Denote the depth of point x0 as t0. Subsequently, in the k-th rendering pass, all points with depth value tk smaller than the recorded depth of the previous pass tk−1 are discarded, thereby resulting in the k-th closest-to-camera point xk for the pixel u. Discarding closer points is implemented in our custom shader, so it still supports the hardware rasterization. After K rendering passes, pixel u has a set of sorted points {xk|k = 1,...,K}.

According to your meshreduce article, we actually have a dataset that includes depth, which would omit the depth calculation step, whereas 4K4D uses '**compute and store the depth before rendering**’ maybe we can make some improvements for that.

## next plan

Solve existing problems and continue to work on code reproduction, and try to further your understanding of this paper and the field.

Please let us know if you have any guidance!