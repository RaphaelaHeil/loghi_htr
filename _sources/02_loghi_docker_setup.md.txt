# Setting Up Loghi with Docker


```{objectives}
- Download the required Docker images.
- Locate all of the files, required for subsequent lessons.
```

```{prerequisites}

Make sure that you have downloaded and unzipped the [Lesson Material](lesson-material), and know where it is currently stored (e.g. in your `Downloads` folder).

```


## Downloading the docker containers

1. Open the file `htr-train-pipeline.sh` in the lesson material
2. Note down the version number that is specified on line 2. It is typically in the format of three numbers, separated by dots, e.g.: `2.1.2`. Alternatively, `latest` may be stated
3. One by one, download the three Loghi components by executing the respective command below. Replace `VERSION`with the number that you identified in the previous step:
    1. Loghi's layout analysis tool `Laypa`: 
        > `docker pull loghi/docker.laypa:VERSION`

        > e.g. `docker pull loghi/docker.laypa:2.1.2`
    3. Loghi's core HTR functionality: 
        > `docker pull loghi/docker.htr:VERSION`

        > e.g. `docker pull loghi/docker.htr:2.1.2`
    5. Miscellaneous related tools: 
        > `docker pull loghi/docker.loghi-tooling`

        > e.g. `docker pull loghi/docker.htr-tooling:2.1.2`

````{note}
If you have previously worked with Loghi, you can check which components and versions are installed by running `docker images`. 

To filter the list, e.g. if you have downloaded many other images, run: 
```bash
docker images -f reference='loghi/*'
```
````


## Locating Relevant Files

There are several file and directory paths that are needed while working with Loghi. This includes the scripts to run the toolsuite, the data (images, transcriptions), and potentially pre-trained models. If you are working with the provided workshop material, place the extracted files in a location as described below, depending on your operating system. 


```{warning}
Please note that issues may occur if your paths contain spaces, such as: 

`/home/user/test folder/a file name.png`

In general, it is not advisable to use spaces in file oder directory names at all. If you would like to create a visual separation between words, you can for example use an underscore: 

`/home/user/test_folder/a_file_name.png`

```

It may be useful to create a text file, in which you can collect all of the different paths, so that you can copy from it for each of the different steps. Concretely, you will need:  
- path to the directory containing the workshop material (most of the paths below will lie within this directory)
- path to the Loghi scripts
    - create-train-data.sh
    - htr-train-pipeline.sh
    - inference-pipeline.sh
    - finetune.sh 
- path to the directory containing the training data, consisting of:
    - the image files
    - a subdirectory called "page", which contains the corresponding transcriptions in PAGE XML format
        ```{note}
        Note that an image and its corresponding transcription file have to have the same name, e.g. `.../doc_page01.png` and `.../page/doc_page01.xml` (where `...` is the path to the training data)
        ```
- path to the directory in which to place the reformatted training data
- path to the directory(ies) containing any other datasets, for example for the evaluation or for recognising new document images
- path to the directory in which to place the newly created text recognition model
- path to the directory containing an already trained text recognition model
    ```{note}
    Note that by default, Loghi places new model files in a subdirectory called `best_val`, your path should therefore generally look like: `.../modelname/best_val/`
    ```
- path to the directory containing a pre-trained Laypa segmentation model



### MacOS
You can place the files in a location that is convenient to you. File paths can be copied by highlighting the file or folder in the Mac Finder and either: 
- pressing `Command` + `Option` + `C`
- or by right-clicking the file/folder, holding the `Option` key to reveal the menu item `Copy ... as Pathname` and clicking that option


### Linux
You can place the files in a location that is convenient to you. In order to copy a file path on linux, highlight the file/folder and press `Ctrl` + `C`, or rightclick and choose `Copy`. When you paste the copied value in a text editor or command-line, the absolute path will be pasted automatically, instead of the actual file.

### Windows

```{prerequisites}
If you have not intereacted with the WSL before, open the `Command Prompt` or `PowerShell`, type `wsl` and press `Enter`. You will then be prompted to choose a username and password, to complete the WSL setup. 
```


On Windows, it will be easiest if you place your data within the folder structure of the Windows Subsystem for Linux (WSL), as described below. If this is not an option for you, [this article](https://learn.microsoft.com/en-us/windows/wsl/wsl2-mount-disk) may be an alternative for you instead. 

1. Open a File Explorer
2. Click into the address bar, to make it editable, enter `\\wsl$`, and press `Enter`
    - if this does not work, try entering `\\wsl.localhost` instead
3. In the displayed list, double-click on the Linux distribution that you chose during the WSL installation. If you did not pick a specific distribution, Ubuntu (followed by a version number, such as 22.04) has very likely been configured as the default 
4. Open the directory `home`, which should contain a directory with the username you chose during the WSL setup. You are free to place your files anywhere within this directory (i.e. within `/home/username`). 

To obtain paths within WSL, you can e.g.: 
- for directories: use the shell (command `cd`) to navigate into the directory and run `pwd`
- for files: navigate to where the file is located and run `readlink -f FILENAME`, e.g. `readlink -f test.png`
- install the file manager Nautilus ([instructions](https://learn.microsoft.com/en-us/windows/wsl/tutorials/gui-apps#install-nautilus)) and open it by looking for the name of your Linux distribution (default: Ubuntu) in the *Windows* start menu and choosing the app `Files`, in the submenu. In the Nautilus file manager, follow the Linux instructions, described above. 

