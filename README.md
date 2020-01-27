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

