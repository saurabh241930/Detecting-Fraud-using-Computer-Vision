# Detecting-Fraud-using-Computer-Vision
This repository contains the overview of all the approaches that we are applying & applied in order to detect Fraud

### Objective : To Detect Frauds during check-ins & chek-outs of hotel 

**`Brief overview of pipeline`**  : This project is a part of our one the biggest client and also one of the biggest hotel booking service ,Fraud here actually happens  during customer's check-ins or check-out for its room ,If that time the hotel receptionaist at desk is not registering the record of customer presence in their registory system(online & ofline)the hotel owner is commiting Fraud,because by doing this the our client company is not getting its profit cut

## So this the what process invloved in check-ins & check-outs are made of :

* Take the customer details
* Create the record in Physical register
* Create the record in Online System

So maybe you're wondering this can be monitored manually why we need to use Machine Learning here,because there over **`50k`** locations it is impossible to do manually

## Pipeline : 

We broke down the complete pipeline into 3 step process

* [Initiating the model after customer arrives]()
* [Identify Video actions]()
* [Create the timestamps of results]()


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


# the model so far outputs 3D feature maps (height, width, features)
```

### `Identify Video actions (if 1st step is true)` (Video Classification Model) :

Detect video activity using CNN-LSTM model

Lets breakown what is role of **CNN & LSTM** here

`CNN` : We are using here cnn model only just for extracting image cnn features data

 `LSTM` : After extracting those features from that sequence of video frames we will pass that in lstm network

Example : our **video classification models** looks for 2 major action in and they should be in proper sequence

<img src="https://i.imgur.com/sudSbPL.png" border=0>


```
                   Boolean Flag 1                                           Boolean Flag 2                
                       |     |                                                 |     |                        
       | Identify the 1st Registration Activity|     >>        | Identify the 2nd Registration Activity| 
       
 ```
                                                                                        

and if Model is not able to identify the any of the activity in proper sequence that would be consider as Fraud

To track down the whole process we have three activity classifier


