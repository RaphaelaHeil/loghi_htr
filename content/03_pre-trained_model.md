# Using a Pre-Trained Model to Recognise Text

## Workshop Hands-On Exercise

Use the provided model, trained on images by Ibsen, to recognise the texts, written by Bonnevie. 



## What you need
- a pre-trained text recognition model
- a pre-trained segmentation model, or existing line segmentations for your images, in PAGE XML format
- images that you wish to recognise
- the script `inference-pipeline.sh`


## Modifying `inference-pipeline.sh`
1. Open the file `inference-pipeline.sh` in a text editor of your choice
2. If you already have line segmentations (in PAGE XML) that you would like to use to recognise text lines, change line 11 from `BASELINELAYPA=1` to `BASELINELAYPA=0`
3. If your data is not segmented yet, modify lines 15 and 16, so that they contain the **absolute** paths to the segmentation model files, i.e. 
    - the configuration (a file ending in `.yaml`)
    - model checkpoint (a file ending in `.pth`), 
    e.g.:
    ```bash
    LAYPABASELINEMODEL=/home/demo/loghi_material/segmentation_model/config.yaml
    LAYPABASELINEMODELWEIGHTS=/home/demo/loghi_material/segmentation_model/model_best_mIoU.pth
    ```
4. If you have a Laypa region model, instead of a Laypa baseline model, set `BASELINELAYPA=0` and `REGIONLAYPA=1` and modify lines 19 and 20 as described in the previous step.
4. Modify line 25, so that it points to the directory in which the text recognition model is stored, e.g.:
    ```bash
    HTRLOGHIMODEL=/home/demo/loghi_material/ibsen_model/best_val
    ```
5. On line 26, change the model name. This value will be used as the name of the directory in which the recognition results are placed. It should therefore contain some reference to which model was used. Choose between:
    - the name of your model, which can e.g. be found in the model's `config.json`
    - an entirely different name, of your choosing, as long as it does not contain any spaces! 
6. If your computer does not have a GPU, or Loghi reports issues in using it, set line 45 to -1, i.e.: `GPU=-1`

Feel free to experiment with the settings in lines 30 - 43 but note the comments in the file about the purpose and limitations of these values! 

**Don't forget to save your changes, before moving on to the next step! :wink:**



## Running `inference-pipeline.sh`

1. Open a new shell 
2. In your shell, navigate to the location of `inference-pipeline.sh` 
3. Execute the script by running `./inference-pipeline.sh PATH_TO_DATASET`, where `PATH_TO_DATASET` is the path to the directory containing your images, e.g. `./inference-pipeline.sh /home/demo/loghi_material/bonnevie_images/test`
    - if your system reports `permission denied: ./inference-pipeline.sh`, run `chmod 755 inference-pipeline.sh` to make the script executable, and try running it again

Loghi will now start to process all images in the directory that you specified. 

... looking at the console log ...


## Inspecting the Recognition Results

... 