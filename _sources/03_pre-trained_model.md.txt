# Using a Pre-Trained Model to Recognise Text

```{objectives}
Learn how to use a trained model to recognise text in new images.
```

```{prerequisites}
- images that you wish to recognise
- a pre-trained text recognition model
- a pre-trained segmentation model, or existing line segmentations for your new images, in PAGE XML format
- the script `inference-pipeline.sh`
```


## Modifying `inference-pipeline.sh`
1. Open the file `inference-pipeline.sh` in a text editor of your choice
2. If you already have line segmentations (in PAGE XML) that you would like to use to recognise text lines, change line 11 from `BASELINELAYPA=1` to `BASELINELAYPA=0`
3. If your data is not segmented yet, modify lines 15 and 16, so that they contain the **absolute** paths to the segmentation model files, i.e. 
    - the configuration (a file ending in `.yaml`)
    - model checkpoint (a file ending in `.pth`), 
    ```{code-block} bash
    :linenos:
    :lineno-start: 15
    LAYPABASELINEMODEL=/home/demo/loghi_material/segmentation_model/config.yaml
    LAYPABASELINEMODELWEIGHTS=/home/demo/loghi_material/segmentation_model/model_best_mIoU.pth
    ```
4. If you have a Laypa **region** model, instead of a Laypa **baseline** model, set `BASELINELAYPA=0` and `REGIONLAYPA=1` and modify lines 19 and 20 as described in the previous step.
    ```{code-block} bash
    :linenos:
    :lineno-start: 19
    LAYPAREGIONMODEL=/home/demo/loghi_material/segmentation_model/config.yaml
    LAYPAREGIONMODELWEIGHTS=/home/demo/loghi_material/segmentation_model/model_best_mIoU.pth
    ```
4. Modify line 25, so that it points to the directory in which the text recognition model is stored
    ```{code-block} bash
    :linenos:
    :lineno-start: 25
    HTRLOGHIMODEL=/home/demo/loghi_material/ibsen_model/best_val
    ```
    ```{note}
    Make sure that the path points to the innermost directory in the tree, here `best_val`. If you are uncertain, check that the directory contains a file called `model.keras`, as well as the files `charlist.txt`, and `config.json`.  
    ```
5. On line 26, change the model name. This value will be used as the name of the directory in which the recognition results are placed. It should therefore contain some reference to which model was used. Sensible choices are:
    - the actual name of your model, which can for example be found in the model's `config.json`
    - an entirely different name of your choosing, as long as it **does not** contain any spaces! 
6. If your computer does not have a GPU, or Loghi reports issues in using it, set line 45 to -1: `GPU=-1`

Feel free to experiment with the settings in lines 30 - 43 but note the comments in the file about the purpose and limitations of these values!  



## Running `inference-pipeline.sh`

1. [Open a new shell](open-shell) 
2. Using `cd`, navigate to the location of `inference-pipeline.sh` 
3. Execute the script by running `./inference-pipeline.sh PATH_TO_DATASET`, where `PATH_TO_DATASET` is the path to the directory containing your images, e.g. 
    ```bash
    ./inference-pipeline.sh /home/demo/loghi_material/bonnevie_images/test
    ```
    if your system reports `permission denied: ./inference-pipeline.sh`, run `chmod 755 inference-pipeline.sh` to make the script executable, and try running it again

```{exercise}
If you are using the provided material to follow this lesson, enter the path to the Bonnevie test images, as indicated above (`.../bonnevie_images/test`).
```

Once you have started the `inference-pipeline` script, Loghi will start to process all of the images in the specified directory. If you have configured a Laypa segmentation model, the script will begin by recognising and refining text lines in the images. Loghi will write a variety of log messages to the command-line, allowing you to follow the progress. Eventually, the actual text recognition will commence, indicated by the phrase `Running HTR`. After presenting pieces of information about the model, e.g. the architecture and the character set it was trained on, each line in each of the images will be processed individually. Loghi will log the individual transcriptions, and store them as PAGE XML in the dataset folder. 

To inspect the created PAGE XML files, navigate into your dataset directory and into the folder with the name that you chose for the field `modelname` in an earlier step. Each of the xml files corresponds to the recognised text in the image with the same name. 