# AI-based object cropping using user text prompts
#### The notebook contains instructions on how to use it, as well as all the code.
#### This solution is based on Meta's [Segment Anything](https://segment-anything.com/) model, as well as [Grounding DINO](https://huggingface.co/docs/transformers/en/model_doc/grounding-dino).
#### There are, of course, different variants for this very thing, such as [Grounded SAM](https://github.com/IDEA-Research/Grounded-Segment-Anything), but I couldn't get it to work on my machine, so I decided to write a script for my personal use.
#### The gist of it is that the script will: 
- take files from `<script_folder>/content/input`
- process the images one by one, cropping the desired objects out 
- save the result in `<script_folder>/content/output`
- the folder hierarchy and file names will be preserved/mirrored for the output

### For configuration, check the 'Settings' cell of the notebook

#### <font color='lightgreen'>Supported extensions: .png, .jpg, .jpeg, .webp, .gif</font>
##### Yes, it does process GIFs frame by frame :D

## Based on the following:
Understanding how and why it works: <br>
> https://www.youtube.com/watch?v=0Fpb8TBH0nM <br>

Example for using SAM and processing its output: <br>
> https://github.com/IDEA-Research/Grounded-Segment-Anything/blob/main/grounded_sam.ipynb

## Dependencies
To start off, you need to install <font color='green'>__CUDA__</font>:<br> 
> __https://developer.nvidia.com/cuda-downloads__

After you have installed the <font color='green'>__CUDA toolkit__</font>, you will have to install the latest version of <font color='ff8000'>__PyTorch__</font>.
> __https://pytorch.org/get-started/locally/__
<br>
This site will generate the command you need to install the package.

In your Python environment, run the following command to install missing dependencies:
> `pip install diffusers transformers accelerate scipy safetensors`

## AI Models
This script uses two models:
- [Segment Anything](https://github.com/facebookresearch/segment-anything) is a strong segmentation model. But it need prompts (like boxes/points) to generate masks.
- [Grounding DINO](https://github.com/IDEA-Research/GroundingDINO) is a strong zero-shot detector which enable to generate high quality boxes and labels with free-form text.

## Script setup
Create a folder on your system. For the sake of this tutorial, we will call it `mainFolder` <br>
Once you are in `mainFolder`, copy the Grounding DINO repository by running the command
> __<font color='#ff8000'>git clone https://github.com/IDEA-Research/GroundingDINO.git</font>__

In the same folder, clone the SAM repository:
> __<font color='#ff8000'>git clone https://github.com/facebookresearch/segment-anything.git</font>__

You also need to download a checkpoint for SAM, which has to be placed in `mainFolder`.
> __https://dl.fbaipublicfiles.com/segment_anything/sam_vit_h_4b8939.pth__

Once you have all the files, you must do the following using your Python environment:
>1. Navigate to `mainFolder\GroundingDINO` and run the command `pip install -e .`
>
>3. Navigate to `mainfolder\segment_anything` and run the command `pip install -e .`

If there are no errors, then you have successfully installed the required dependencies.

The script will look for images to process in `mainFolder\content\input` , and will generate the output in `mainFolder\content\output` . It will mirror the folder structure from the input folder.
<br>
Once you have finished a batch of files, <font color='red'>make sure to remove them from the input folder</font>, as they will get processed again otherwise.
