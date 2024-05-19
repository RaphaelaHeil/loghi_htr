# Local Automatic Text Recognition with Loghi

In this lesson, we will take a look at _Automatic Text Recognition_ (ATR), using the tool _Loghi_, developed by the _KNAW Humanities Cluster_ in Amsterdam. 

The material provides a general introduction to ATR and does not assume prior experience with text recognition, or deep learning in general. Basic familiarity with the unix shell is recommended - an introduction can for example be found here: [The Carpentries: _The Unix Shell_ lesson](https://swcarpentry.github.io/shell-novice/). A cheat-sheet with the most important commands for this lesson is provided in the [lesson material](shell_cheat-sheet).

```{prereq}
1. A Docker installation. Instructions can be found [here](docker).
2. Loghi (**download**, **instructions**)
3. Data to train your model on 
  a) **data that is used for demonstration purposes in this lesson**
  b) **your own data**
```


```{toctree}
:maxdepth: 1
:caption: The lesson
motivation.md
atr_intro.md
loghi_docker.md
loghi_components.md
pre-trained_recognition.md
training_data.md
train_custom.md
fine-tuning.md
```

```{toctree}
:maxdepth: 1
:caption: Material
docker.md
shell_cheat-sheet.md
further_reading.md
```