# Training a Model from Scratch


```{objectives}
Learn how to _fine-tune_ an existing text recognition model, ...
```


```{prerequisites}
- script `finetune.sh`
- path to pre-trained model checkpoint
- path to fine-tuning data
    - arranged according to Loghi's data format (cf [Creating training data from existing annotations](04_create_train_data))
- path to output directory for the fine-tuned model
```

```{exercise}
Use the data, provided in ..., and the pre-trained model checkpoint `name`, included in the lesson material to fine-tune the Ibsen model on Bonnevie's handwriting. Afterwards, use the newly trained model to recognise the handwriting in the test set and compare the new results with the ones you obtained using the Ibsen model, in step [Using a pre-trained model](03_pre-trained_model). 
```


## Modifying `finetune.sh`
1. Open the file `finetune.sh` in a text editor of your choice
2. Modify line 12 and fill in the path to the directory, containing your pre-trained model, e.g.:
    ```{code-block} bash
    :linenos:
    :lineno-start: 12
    HTRBASEMODEL=/home/demo/.../ibsen_small_8_30/best_val
    ```
3. If your computer does not have a GPU, or Loghi reports issues in using it, set line 22 to -1
    ```{code-block} bash
    :linenos:
    :lineno-start: 22
    GPU=-1
    ```
4. ...
    ```{code-block} bash
    :linenos:
    :lineno-start: 25
    listdir=/mnt/d/data/oslo_data/bonnevie_train_val
    trainlist=$listdir/training_all_train.txt
    validationlist=$listdir/training_all_val.txt
    ```
5. ...
    ```{code-block} bash
    :linenos:
    :lineno-start: 45
    batch_size=8
    model_name=bonnevie_small_8_30
    learning_rate=0.0003
    ```
6. ...
    ```{code-block} bash
    :linenos:
    :lineno-start: 52
    outputdir=/mnt/d/data/oslo_data/$model_name
    ```


## Running `finetune.sh`
1. In your shell, navigate to the location of `finetune.sh`
2. Execute the script by running `./finetune.sh`
    - if your system reports `permission denied: ./finetune.sh`, run `chmod 755 finetune.sh` to make the script executable, and try running it again


## Inspecting the Output

### Command-line messages
There will be a lot of log messages .... the most relevant ones are the ones providing updates on the fine-tuning and validation progress: 
... 




### 





## Using the Fine-tuned Model
... 