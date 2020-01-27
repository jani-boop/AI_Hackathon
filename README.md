# Media Description Generator
When AI turns journalist

## Purpose

* Making information more accessible to people with disability.
     *Use case:* Providing descriptive facts from ambient sounds to the hearing-impaired.

* Unbaised reporting will get more relevant as the trend of [microtargeting](https://en.wikipedia.org/wiki/Microtargeting) rises.
    *Use case:* A standard repository of facts which is supported by every news outlet.

* Obataining relevant facts from a large swaths of data which is otherwise resource intensive for humans.
    *Use case:* Detecting unusual activity in lengthy surveillance media for security.

* Archival of hefty media in an extremely compact format.
    *Use case:* Keeping a record of current events serving as a time-capsule of humanity. (Inspired by [WaybackMachine](https://archive.org/web/))

## Approach

We are taking a audio clip and preprocessing it to make 2D image like array.
This 2D like array is converted into spectrogram.
This spectrogram is then fed into a CNN for the classifcation.
After the classification,if the audio file contained any human speech,its description is printed out.

## Implementation

The dataset we used in this project was downloaded from Kaggle - **FSDKaggle2019**, and it employs audio clips from the following sources:

Freesound Dataset (FSD): a dataset being collected at the MTG-UPF based on Freesound content organized with the AudioSet Ontology
The soundtracks of a pool of Flickr videos taken from the Yahoo Flickr Creative Commons 100M dataset (YFCC)

This dataset has around 24,500 audio clips in training set in two folders spanning from 2 seconds to 19 seconds and another 5000 in test set,and it has 80 classes(types) of Sound ranging from *Waves_and_surf* to *Toilet_flush* and also has 5 classes of human audio files!

The size of entire dataset is 24.4 GB without extraction,therefore we are using a smaller dataset
(4-4.5GB) to show our prototype beccause of time and disk space in Google Colabarotory constraints.

Each audio file is converted into 128 x L, L is audio seconds x 128; [128, 256] if sound is 2s long.


We also make use of librosa library which is specialize for audio for our preprocessing audio to spectrograph

We use pretrained resnet.18 trained on Imagenet for the classification where we make use of **transfer learning** to iniatilize parameters in our model and fine tune the last few layers to specialize our model in learning to classify the spectrograms.

After trainig,the audio file is classifed from one of the 80 different classes,if it is from any of the following classes
**Child_speech_and_kid_speaking**
**Male_singing**
**Male_speech_and_man_speaking **
**Female_singing**
**Female_speech_and_woman_speaking**

We send the audio clip to our second Model specialized in Speech to Text Generation **DeepSpeech**
which gives back Description of the audio spoken.
