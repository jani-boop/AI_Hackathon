# Media Description Generator
When AI turns journalist
https://drive.google.com/open?id=1JJG2-gYKEV-EtBzlzdFjLU_jKTUWDhFK
https://colab.research.google.com/drive/197PbtRyc89vw9HmlNFjj6Bhi6nozoCnq
**NEW LINK FOR CSV TRANSFORMS**
https://colab.research.google.com/drive/1kNTOH2rIp9U9NIr19Mae4CrMEcet3fme
## Purpose

* Making information more accessible to people with disability.
     *Use case:* Providing descriptive facts from ambient sounds to the hearing-impaired. 
     
* Providing unbaised reporting while it gets more relevant in the trend of [microtargeting](https://en.wikipedia.org/wiki/Microtargeting).
    *Use case:* A standard repository of facts which form the basis of all news outlets.
    
* Richer subtitle generation for non-speech sounds.
    *Use case:* Streaming
     
* Obataining relevant facts from a large swaths of data which is otherwise resource intensive for humans.
    *Use case:* Detecting unusual activity in lengthy surveillance media for security.
    
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
<img src="https://github.com/tejasvi/AI_Hackathon/raw/master/random_init.JPG?sanitize=true">

#### ImageNet Initialization
<img src="https://github.com/tejasvi/AI_Hackathon/raw/master/img_init.JPG?sanitize=true">

## Execution

See `main.ipynb`. The trained model was saved while training and is loaded during inference only. As a prototype, currently audio files are transcribed only after complete processing. The classification and speech recognition can be run parallely for real-time processing. Though for simplicity currently it runs in sequence.
