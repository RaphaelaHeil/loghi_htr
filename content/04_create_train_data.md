# Creating training data from existing annotations

During training, Loghi expects that the data is arranged in a specific format. This script will convert a dataset in the structure

```bash
finetune
├── no-nb_digimanus_300599_0003.jpg
├── no-nb_digimanus_300599_0004.jpg
├── no-nb_digimanus_300599_0005.jpg
├── ...
└── page
    ├── no-nb_digimanus_300599_0003.xml
    ├── no-nb_digimanus_300599_0004.xml
    ├── no-nb_digimanus_300599_0005.xml
    ├── ...
```

to the desired structure, outlined in detail further below. 

Note that this step is only necessary for training data. If you already have a trained model and want to use it to recognise unseen datasets, you will only need images of the individual pages. 


## Workshop Hands-On Exercise

Use the provided data for Bonnevie to create a training and a validation set, so that these can be used in the next step to finetune a model specific to her handwriting. 

## What You Need
- images and corresponding annotations in PAGE XML 
- the script `create-train-data.sh`


## Modifying `create-train-data.sh`
You will only have to modify `create-train-data.sh` in case you want to change one of the following settings, otherwise you can leave it as-is:
- `trainsplit=90` -- indicates the percentage (90%) that should be used as training set. Rest (10%) is used as validation set
- `include_text_styles=1` -- set to 0 if you **don't** want text styles to be included in the output
- `skip_unclear=1` -- set to 0 if you **don't** want to skip text regions marked as unclear


## Running `create-train-data.sh`
1. In your shell, navigate to the location of `create-train-data.sh`
2. Execute the script by running `./create-train-data.sh INPUT_PATH OUTPUT_PATH`, where `INPUT_PATH` is the path to your image directory, and `OUTPUT_PATH` is the path, the reformatted dataset should be written to, e.g. `./create-train-data.sh /home/demo/loghi_material/bonnevie_images/finetune /home/demo/loghi_material/finetune_data`
    - if your system reports `permission denied: ./create-train-data.sh`, run `chmod 755 create-train-data.sh` to make the script executable, and try running it again

## Inspecting the Reformatted Data

You should end up with a structure similar to the one below, with the filenames adapted to your dataset of choice. 

```
finetune_data
├── no-nb_digimanus_300599_0003
│   ├── no-nb_digimanus_300599_0003-line_1603435838929_122.box
│   ├── no-nb_digimanus_300599_0003-line_1603435838929_122.png
│   ├── no-nb_digimanus_300599_0003-line_1603435838929_122.txt
│   ├── no-nb_digimanus_300599_0003-r1l10.box
│   ├── no-nb_digimanus_300599_0003-r1l10.png
│   ├── no-nb_digimanus_300599_0003-r1l10.txt
│   ├── ...
├── no-nb_digimanus_300599_0004
│   ├── ...
├── ...
├── training_all.txt
├── training_all_train.txt
└── training_all_val.txt
```

Loghi has processed each of the original page images and has extracted the individual text lines, as defined in the PAGE XML. One directory has been created per document image, with three files per text line within:

- `LINENAME.png` -- image of the segmented line, extracted using a polygonal bounding box (if provided in the PAGE XML) 
- `LINENAME.txt` -- contains the line's transcription in plain text
- `LINENAME.box` -- a [Tesseract]() box file, describing the text line in the format: `<symbol> <left> <bottom> <right> <top> <page>`, where `<left> <bottom> <right> <top>` are the bounding box coordinates of either the glyph or the whole line. `<page>` indicates the index in the case of multi-page TIFF images, or `0` for other image types (e.g. JPGs). 

    Box file example for a line with the bottom left corner at `x=0, y=0`, and the top right corner at `x=3230, y=162`: 
    ```
    b 0 0 3230 162 0
    e 0 0 3230 162 0
    l 0 0 3230 162 0
    ...
    ```



Besides the line-level data, Loghi has also created the following three index files:

- `training_all.txt` -- paths to **all** of the extracted line images, with corresponding transcriptions, tab-separated
- `training_all_train.txt` -- the portion of all text lines that is designated as **training** set
- `training_all_val.txt` -- the portion of all text lines that is designated as **valiation** set

    ```{note}
    The training and validation sets do not overlap, i.e. a given text line is **either** in the training **or** in the validation set!  
    ```