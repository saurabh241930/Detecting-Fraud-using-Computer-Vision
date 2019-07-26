# Detecting-Fraud-using-Computer-Vision
This repository contains the overview of all the approaches that we are applying & applied in order to detect Fraud

### Objective : To Detect Frauds during check-ins & chek-outs at hotel reception

**`Brief overview of pipeline`**  : This project is a part of our one the biggest client and also one of the biggest hotel booking service ,Fraud here actually happens  during customer's check-ins or check-out for its room ,If that time the hotel receptionaist at desk is not registering the record of customer presence in their registory system(online & ofline)the hotel owner is commiting Fraud,because by doing this the our client company is not getting its profit cut

## So this the what process invloved in check-ins & check-outs are made of :

* Take the customer details
* Create the record in Physical register
* Create the record in Online System

So maybe you're wondering this can be monitored manually why we need to use Machine Learning here,because there over **`50k`** locations it is impossible to do manually

## Pipeline : 

We broke down the complete pipeline into 3 step process

* [Initiating the model after customer arrives](https://github.com/saurabh241930/Detecting-Fraud-using-Computer-Vision/blob/master/README.md#initiating-the-model-after-customer-arrives-binary-classification-model-)
* [Identify Video actions](https://github.com/saurabh241930/Detecting-Fraud-using-Computer-Vision/blob/master/README.md#identify-video-actions-if-1st-step-is-true-video-classification-model-)
* [Create the timestamps of results](https://github.com/saurabh241930/Detecting-Fraud-using-Computer-Vision/blob/master/README.md#creating-timestamps-of-results-of-detections-)


### `Initiating the model after customer arrives` (Binary Classification Model) :

Detect if Customer is present on the reception using **VGG-16** based CNN model classifier that we trained on our images

Example : like our model would start just after the customer comes to desk

<img src="https://i.imgur.com/ihbQLkl.png" border=0>

Example prediction code of VGG model  in keras **(Not the real code)**

```python
from keras.preprocessing.image import load_img
from keras.preprocessing.image import img_to_array
from keras.applications.vgg16 import preprocess_input
from keras.applications.vgg16 import decode_predictions
from keras.applications.vgg16 import VGG16
# load the model
model = VGG16()
image = load_img('example.jpg', target_size=(224, 224))
image = img_to_array(image)
image = image.reshape((1, image.shape[0], image.shape[1], image.shape[2]))
image = preprocess_input(image)
yhat = model.predict(image)
label = decode_predictions(yhat)
label = label[0][0]
print('%s (%.2f%%)' % (label[1], label[2]*100))

OUTPUT :

customer present(90%)

```

### `Identify Video actions (if 1st step is true)` (Video Classification Model) :

Detect video activity using CNN-LSTM model

Lets breakown what is role of **CNN & LSTM** here

`CNN` : We are using here cnn model only just for extracting image cnn features data

 `LSTM` : After extracting those features from that sequence of video frames we will pass that in lstm network

Example : our **video classification models** looks for 2 major action in and they should be in proper sequence

<img src="https://i.imgur.com/RqKxbf0.png" border=0>



Dummy Code for this model (**Not the real code**):


**`INCEPTION_V3-CNN-EXTRACTOR`**


```python
from keras.preprocessing import image
from keras.applications.inception_v3 import InceptionV3, preprocess_input
from keras.models import Model, load_model
from keras.layers import Input


model = InceptionV3(weights='imagenet', include_top=False)

img_path = 'video_frames.jpg'
img = image.load_img(img_path, target_size=(224, 224))
x = image.img_to_array(img)
x = np.expand_dims(x, axis=0)
x = preprocess_input(x)

features = model.predict(x)


```

**`LSTM MODEL`**

```python

from keras.layers import Dense, Flatten, Dropout, ZeroPadding3D
from keras.layers.recurrent import LSTM


 model = Sequential()
 model.add(LSTM(2048, return_sequences=False,input_shape=(N,2048),dropout=0.5))
 model.add(Dense(512, activation='relu'))
 model.add(Dropout(0.5))
 model.add(Dense(video_classes, activation='softmax'))
```
### `Creating Timestamps of results of detections` :

Now from here pipeline was straight foward  

we created timestamps of 3 events

`1st` when customer spotted
`2nd` & `3rd` when any of Registration activity detected in any order

Using that we can estimate time difference between events can which was a indicator threshold for fraud evaluation


Example: **Tc,Tr1 & Tr2** were the timestamps of `customer spotted`,`Registration 1` &` Registration 2` respectively

so if time difference between **`Tc & Tr1 & Tr2`** goes more than usual time limit or any of the `registration event` is missing that instance would be supervised manually and check if its **Fraud** or something else




