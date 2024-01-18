## Avataar Assignment! 
### Problem Statement: Generating coherent 360-panorama from depth conditioning
---
**Task 1**:

Get a coherent 360 panorama as an output from text-to-image SD model. As part of
submission, attach results on 5 different user prompts.

There are two requirement : 
1. The scene should look consistent as per the user’s prompt
2. The generated panorama’s ends should meet i.e. seam needs to be tiled.

--- 
### Solution : 
--- 
For this task I selected these prompts:
1.    "360-degree panoramic image, of a simple room",
2.    "360-degree panoramic image, of a luxury room",
3.    "360-degree panoramic image, of a spaceship",
4.    "360-degree panoramic image, of a forest",
5.    "360-degree panoramic image, of an anime style natural landscape".

For additional tests I added this text to the prompt : "stereoscopic, equirectangular", but it didn't effect the results much so the presented results are for the above 1-5 prompts.

--- 

**Model Selection** : 

For selecting the best model I tried variations of multiple models.

1. [Stitch Diffusion](https://littlewhitesea.github.io/stitchdiffusion.github.io/) (WACV 2024) : This paper gave the best results. They used LORA to fine-tune the model on their custom dataset. Which gave then consistent 360 panoramic results.

You can check all the results here [StitchDiffusion](https://github.com/charchit7/avatar_project/tree/main/Results). The code for running the Stitch-Diffusion can be found here [Google Colab](https://colab.research.google.com/drive/1QZHh9-3pjVtqlg2Oeqq_P11BZnH7cTpH?usp=sharing).

**One of the prompt Result from StitchDiffusion**:

---
Prompt : "360-degree panoramic image, of a simple room"
![Image](https://github.com/charchit7/avatar_project/blob/main/Results/im_20240118103233_11.png_360-degree%20panoramic%20image%2C%20of%20a%20simple%20room.png)

As you can see the image is very consistent with the results. 

Comparing with Multi-Diffusion result on same prompt below:

![MultiDiff](https://github.com/charchit7/avatar_project/blob/main/assets/MultiDiffusion/MultiDiffusion_360-degree_panoramic_image%2C_of_a_simple_room_image_1.png)
---

2. LDM3D Pipeline https://arxiv.org/abs/2305.10853 : This research paper proposes a Latent Diffusion Model for 3D (LDM3D) that generates both image and depth map data from a given text prompt, allowing users to generate RGBD images from text prompts. 

You can check the results for this models here [LDM Results](https://github.com/charchit7/avatar_project/tree/main/assets/LDM3D_pipeline)

3. Multi-Diffusion https://huggingface.co/papers/2302.08113 : In this work, we present MultiDiffusion, a unified framework that enables versatile and controllable image generation, using a pre-trained text-to-image diffusion model, without any further training or finetuning.

You can check the results for this model here [Multi-Diffusion](https://github.com/charchit7/avatar_project/tree/main/assets/MultiDiffusion). The code for running **LDM3D** and **Multi-Diffusion** can be found in this [Notebook](https://github.com/charchit7/avatar_project/blob/main/testing_pano_pipelines.ipynb).

4. I also tested generating image with [PanoGen](https://pano-gen.github.io/) which is very-interesting but it requires heavy compute and storage ~90GB and they use MatterPlot3D which is not available for commercial usecase.

--- 

**Task 2**:
Leverage the solutions arrived at in part 1 to get panorama generated using both,
user’s text prompt and the depth conditioning.

- Use the same prompt with the provided depth image.
---

### Solution:

I used diffusers library for this task. The model used was `StableDiffusionXLControlNetPipeline` which seems to work pretty well. 

### controlnet:
Stable Diffusion occasionally struggles to generate images aligned with specific prompts. ControlNet addresses this by enhancing Stable Diffusion's capabilities. The fundamental concept of ControlNet is straightforward:

- Dual Weight System: It involves cloning Stable Diffusion's weights into two sets - a trainable copy and a locked copy. This dual system preserves the original information while allowing for modifications.
- Zero-Convolution Technique: This is used to connect the two sets of weights. Essentially, in the initial phase, the inputs and outputs of both the trainable and locked copies of the neural network block are aligned, as if the duplication does not exist.


The results are provided [here](https://github.com/charchit7/avatar_project/tree/main/controlnet_results). Images have similar names with respect to their prompts. For code checkout this [Notebook](https://github.com/charchit7/avatar_project/blob/main/controlnet_av.ipynb)