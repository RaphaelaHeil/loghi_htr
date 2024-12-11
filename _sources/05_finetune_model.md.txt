# Fine-Tuning a Small Custom Model

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
If you are using the provided material to follow this lesson, use the Bonnevie dataset that you reformatted in the previous step, as well as the Ibsen model that was used to obtain the initial recognitions. 

Once you have followed the steps below to create a model, fine-tuned on Bonnevie's handwriting, rerun the recognition with the new model, following the same steps as for the Ibsen-specific model ([Using a pre-trained model](03_pre-trained_model)). Compare the transcriptions in the two sets of PAGE XML files. 

Which model produces better transcriptions? What mistakes do one or both of the models make? Are there text lines that are recognised incorrectly by both models? Compare with the document images - are the respective lines hard to read, for example because of noise? Anything else that stands out to you?  
```


## Modifying `finetune.sh`
1. Open the file `finetune.sh` in a text editor of your choice
2. Modify line 12 and fill in the path to the directory, containing your pre-trained model, e.g.:
    ```{code-block} bash
    :linenos:
    :lineno-start: 12
    HTRBASEMODEL=/home/demo/loghi_material/ibsen_model/best_val
    ```
3. If your computer does not have a GPU, or Loghi reports issues in using it, set line 22 to -1
    ```{code-block} bash
    :linenos:
    :lineno-start: 22
    GPU=-1
    ```
4. On line 25, enter the path to the reformatted dataset. If you did not modify the names of the originally generated index files, leave lines 26 and 27 as-is, otherwise enter the respective filenames. 
    ```{code-block} bash
    :linenos:
    :lineno-start: 25
    listdir=/home/demo/loghi_material/finetune_data
    trainlist=$listdir/training_all_train.txt
    validationlist=$listdir/training_all_val.txt
    ```
5. On line 34, specify the number of epochs you want to train your model for. An epoch corresponds to one cycle of training the model on all of the training data. For finetuning, especially in the given scenario, 10 to 15 epochs should result in a decent model. When training from scratch, more epochs may be needed. More epochs corresponds to longer training times. 
    ```{code-block} bash
    :linenos:
    :lineno-start: 34
    epochs=5
    ```

6. If you are **not** using a GPU, or if your GPU has limited memory, choose a lower number for the batch size, for example 2. Otherwise you can experiment with larger numbers, e.g. 16. Larger batch sizes should reduce the training time but require more memory/better hardware. 
    ```{code-block} bash
    :linenos:
    :lineno-start: 45
    batch_size=8
    ```
7. On line 46, enter a model name of your choice, without spaces! 
    ```{code-block} bash
    :linenos:
    :lineno-start: 46
    model_name=bonnevie_finetuned
    ```
8. Line 47 controls the learning rate. For most cases, `0.0003` is a good starting point but you are of course free to experiment with larger or smaller values. Very small values may result in slow learning, whereas very large values can result in erratic training behaviour. 
    ```{code-block} bash
    :linenos:
    :lineno-start: 47
    learning_rate=0.0003
    ```
9. On line 52, specify the output location for your newly trained model
    ```{code-block} bash
    :linenos:
    :lineno-start: 52
    outputdir=/home/demo/loghi_material/$model_name
    ```


## Running `finetune.sh`
1. In your shell, navigate to the location of `finetune.sh`
2. Execute the script by running `./finetune.sh`
    - if your system reports `permission denied: ./finetune.sh`, run `chmod 755 finetune.sh` to make the script executable, and try running it again


Loghi will now start to train (fine-tune) the model. After logging some general pieces of information, such as the architecture, Loghi will provide regular progress updates. Besides the epoch and number of processed batches, Loghi will also log losses and error rates. After each training epoch, Loghi will evaluate the model's performance on the validation set. If the current model outperforms the previous best, the model state is saved as the new best model. 

In order to terminate the training before all of the epochs have been run, use the key combination `Ctrl` + `C`. 

The model state that has performed best up until the point of termination, or the end of the specified number of epochs, is available in the specified model output path, within the `best_val` directory. This model can now be used to recognise new document images, as described in [Using a pre-trained model](03_pre-trained_model). 

