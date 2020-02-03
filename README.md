
# Face2Text

<ul>
<li>Dataset used is the one described in the paper <a href="https://arxiv.org/abs/1803.03827" >Face2Text: Collecting an Annotated Image Description Corpus for the

Generation of Rich Face Descriptions</a> </li>

<li>It was made available to us on request. </li>

<li>The dataset consists of the around 5685 annotated images chosen randomly from CelebA dataset.</li>

<li>The annotations describe the images as naturally as possible and focus on capturing the person's facial expression and/or their emotional state.</li>

<li>We seem to be the first to work on this dataset for task of face description. </li></ul>

# How to run code

Download the jupyter notebook Face2Text_notebook.ipynb and run it

# Model's Performance 

The following are model's predictions for image descriptions:

![Test Image 1](https://github.com/jani-boop/AI_Hackathon/blob/master/test1.png)

![Test Image 2](https://github.com/jani-boop/AI_Hackathon/blob/master/test2.png)

![Test Image 3](https://github.com/jani-boop/AI_Hackathon/blob/master/test3.png)

# Future Improvements

<ul><li>Use a pretrained network trained for face recognition task to get image encodings. This can help identify the nuances in facial features and give better performance</li>

<li>Fine tune beam search</li>

<li>Perform hyperparameter tuning</li>

</ul>

=======
# Media Description Generator
-When AI turns journalist
## Purpose

* Making information more accessible to people with disability.
     *Use case:* Providing descriptive facts from ambient sounds to the hearing-impaired. 
     
* Providing unbaised reporting while it gets more relevant in the trend of [microtargeting](https://en.wikipedia.org/wiki/Microtargeting).
    *Use case:* A standard repository of facts which form the basis of all news outlets.
    
* Richer subtitle generation for non-speech sounds.
    *Use case:* Improved subtitles on streaming services.
     
* Obataining relevant facts from a large swaths of data which is otherwise resource intensive for humans.
    *Use case:* Detecting unusual activity in lengthy surveillance media to strengthen security.
    
* Archival of hefty media in an extremely compact format.(See [WaybackMachine](https://archive.org/web/))
    *Use case:* Keeping a record of current events serving as a time-capsule of humanity.
    
## Approach
<img src="https://github.com/tejasvi/AI_Hackathon/raw/master/overview.svg?sanitize=true">

* The audio is first converted to WAV format which is the uncompressed form.

* The audio is then converted into a spectrogram which is the visual representation of sound frequencies.

* The spectrograms can then be treated as images where we can benefit from transfer learning using pretrained models (like Resnet).

* After being fed separatly to classification and speech recognition network, appropriate keyword description is generated.

* The obtained keywords can further be used to generate readable sentences using [NLG](https://en.wikipedia.org/wiki/Natural-language_generation). However it proved to be much tedious to prototype.

<img src="https://github.com/tejasvi/AI_Hackathon/raw/master/planned.svg?sanitize=true">


* The problem scope can be extended to object level reseasoning in images. This will provide more context to the description by using videos along with audio.
 

## Implementation


### Data and preprocessing

The dataset used in this project is [Freesound Audio Tagging](https://arxiv.org/pdf/1906.02975) which contains snippets of audio with sound type labels.

The sounds are further divided into ~11 hr *curated* set and ~80 hr *noisy* set. We used only curated set for training due to resource constraints and for stronger transfer learning demonstration.

For spectrogram generation, widely used librosa library is used. Each audio of length *n* seconds is converted into 128x128n grayscale image.


There are 80 possible labels inlcuding *Gasp,Printer, Gong, Bark, Male singing,etc*. The audio falling in human voice category are fed to speech recognition model.

### Architecture

For classification, ResNet18 architecture pre-trained for ImageNet is used. The evaluation metric used is [LwLRAP](https://www.kaggle.com/pkmahan/understanding-lwlrap) which is believed to be most effective and widely used with spectrograms.

Speech recognition uses [DeepSpeech](https://github.com/mozilla/DeepSpeech) architecture published by Mozilla. 

### Transfer learning

The parameters of ResNet18 are initialized for ImageNet dataset instead of being random. The random initialization demonstrably converged slower than intializion to ImageNet weights.
#### Random Initialization
<img src="https://github.com/tejasvi/AI_Hackathon/raw/master/random_init.JPG">

#### ImageNet Initialization
<img src="https://github.com/tejasvi/AI_Hackathon/raw/master/img_init.JPG">

However freezing inner layers not gave good results. Instead we had to unfreeze all layers to get fast convergence. It may be because the images of ImageNet have much different features therefore inner layers also require training.


## Execution

(See [`main.ipynb`](https://github.com/tejasvi/AI_Hackathon/blob/master/main.ipynb)). The trained model was exported after training and is loaded during inference. As a prototype, currently audio files are transcribed in batches. However, it is possible to do classification and speech recognition parallely for real-time processing. For simplicity it currently runs in sequence.

## Resources

* Implementation of LwLARP metric is taken from [Dan Ellis](https://colab.research.google.com/drive/1AgPdhSp7ttY18O3fEoHOQKlt_3HJDLi8).
* For data profiling, [pandas-profiling](https://github.com/pandas-profiling/pandas-profiling) is used to obtain general overview of data.
* Use of [FastAI](http://fast.ai) library to structure and train the model.
* Other resources include StackOverflow, Medium and the rest.
