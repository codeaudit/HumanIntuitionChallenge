#Human Intuition Challenge
Webapp to deliver human intuition challenge as well as collect data that can be used to train a neural network

## Start Up NodeJS Web App
cd server

node app

http://localhost:3000/

### Switch View Modes in Webapp

The web application can be in one of two modes: "view" or "edit"

"view" mode is used to deliver the human intuition test to people without them having the ability to edit any of rule sets or generate data. It is basically a locked down version of the app.

"edit" mode could be considered admin mode where you can view/modify the rule sets and generate data for machine learning

To change modes simply hit one of the following urls:

http://localhost:3000/services/mode/edit

http://localhost:3000/services/mode/view

You should see a json response of success and then you can navigate back to the homepage


### Restoring the Default Schemas

Feel free to play around with the schemas, add more, modify the transformations, go nuts!

If you want to restore the schemas to the default A and B schemas used in the Human Intuition Challenge navigate to this endpoint:

http://localhost:3000/services/setdefaultschemes


### Generating Data For Machine Learning

Once you have the rule sets setup to generate random strings for each schema, you can generate as much data as you want for machine learning.

First, make sure you are in "edit" mode. This will expose a "generate data" button on the homepage (see instructions above to get the webapp into edit mode)

Simply click the "Generate Data" button and the server will spit out a file in the 'server/exports/' directory.

This file is the file the TensorFlow code is looking for when attempting to train.

You can also view the generated data via a rest endpoint. Navigate here to view the json data:

http://localhost:3000/services/results


### Changing the amount of data generated for machine learning

If you want to increase the amount of data generated, you need to modify a parameter in the UI code.

Find the ui/scripts/modules/MachineLearning/services/DataGeneratorService.svc.js file

Then change the "DATA_POINTS_PER_SCHEME" variable in that file.


## Run Machine Learning

There are several neural networks that can be trained using the data exported from the previous steps above

### Single Hidden Layer

This neural network has a single hidden layer with no activation function

```bash
python hidden_layers_one.py
```

### Two Hidden Layers

This neural network has two hidden layers with Relu activation functions

```bash
python hidden_layers_two.py
```

### Combined Single Hidden Layer and Two Hidden Layers (mixed)

This neural network was built to try to take advantage of all possible rules that can be used to generate strings. I found this one to work the best so far. The inputs get sent into two different channels, one a single hidden layer with no activation function and one channel that has two hidden layers with Relu. The outputs from those channels are merged into one more hidden layer with no activation function.

```bash
python hidden_layers_mixed.py
```


## Start Up Tensor Board

TensorFlow comes with an awesome setup for visualizing the graph of the network and analysis of variables. All three neural networks above currently export "accuracy" that can be viewed in TensorBoard. Simply run the neural networks so the log files get created, then run this command:

```bash
tensorboard --logdir=logs
```



