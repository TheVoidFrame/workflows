# Qwen-Image 2.1 - three ComfyUI workflows

From the video by The Void Frame. Free to use.

| File | What it does |
|---|---|
| `qwen-image-21-t2i.json` | text to image |
| `qwen-image-21-i2i.json` | image to image (edits, up to 10 pictures) |
| `qwen-image-21-pose.json` | ControlNet pose (put a person into the pose of another photo) |

Download a `.json` file and drag it onto the ComfyUI window to open it.
Every workflow also has a "How to use" note inside it.

## What you need

- A recent ComfyUI (end of September 2026 or newer).
- No custom nodes: every node in these workflows is part of ComfyUI itself. If a node shows up red, update ComfyUI.

All three workflows are set to the int8 files.
That is the version the video recommends: half the memory of the full model, and it never lost to it in the tests.

## Models

Put each file in the ComfyUI `models` folder named before it.

From https://huggingface.co/Comfy-Org/Qwen-Image-2.1

All three workflows:
- `diffusion_models/qwen_image_2.1_int8_convrot.safetensors` (7.3 GB)
- `text_encoders/qwen3vl_8b_int8_convrot.safetensors` (9.4 GB)
- `vae/qwen_image_2.1_vae_bf16.safetensors` (0.7 GB)

Text to image, only if you switch the prompt enhancer on:
- `text_encoders/qwen3.5_9b_qwen_image_2.1_pe_t2i.int8_convrot.safetensors` (9.5 GB)

Image to image, only if you switch the prompt enhancer on:
- `text_encoders/qwen3.5_9b_qwen_image_2.1_pe_i2i.int8_convrot.safetensors` (9.5 GB)

ControlNet pose:
- `model_patches/qwen_image_2.1_fun_controlnet_union_int8_convrot.safetensors` (3.8 GB)

From https://huggingface.co/Comfy-Org/SDPose

ControlNet pose (finds the pose in your photo):
- `checkpoints/sdpose_wholebody_fp16.safetensors` (1.9 GB)

Full precision instead of int8: the same Comfy-Org page has `qwen_image_2.1_bf16`, `qwen3vl_8b_bf16` and the ControlNet patch in bf16.
Switch the loaders to those files; the "How to use" note in each workflow has the table.
nvfp4 is a community build (https://huggingface.co/BennyDaBall/Qwen-Image-2.1-NVFP4); it is smaller but made more spelling mistakes in the tests.

## Where your own files go

Pictures: load them in the "Load Image" nodes inside ComfyUI (the files land in ComfyUI's `input` folder).
The workflows open with ComfyUI's `example.png` in those nodes; replace it with your own picture.

**Text to image:** write your prompt in the "Prompt" node and press Run.

**Image to image:**
- "Picture 1 (edit target)" is the picture you edit; the result takes its shape.
- "Picture 2" to "Picture 10" are extra reference pictures, switched off (bypassed). To use one: click it, press Ctrl+B, load your picture.
- Use them in order: Picture 2 before Picture 3.
- In the instruction, call them `<image1>`, `<image2>` and so on.

**ControlNet pose:**
- "Pose picture" = the photo with the pose you want; only the pose is taken from it, nothing of the person.
- "Reference picture" = a normal photo of the person you want in that pose, face large and clear; it is switched off, press Ctrl+B to use it.
- "Pose skeleton the model follows" shows the stick figure that was found; if limbs are missing there, use a clearer pose picture.

**LoRA (all three workflows):**
- Your LoRA files go in ComfyUI's `models/loras` folder.
- Each workflow has three LoRA slots ("LoRA 1" to "LoRA 3"), switched off. To use one: click it, press Ctrl+B, pick your LoRA file in the list.
- Leave the slots you do not use switched off.

## Good to know

- Keep cfg at 1; the negative prompt only does something above 1.
- Transparent picture (text to image): write "This is an RGBA format image with transparency. [your description]. The image has an alpha channel and a transparent background."
- The licence of Qwen-Image 2.1 is a research licence: no commercial use without permission from Qwen. Read it on the model page: https://huggingface.co/Qwen/Qwen-Image-2.1
