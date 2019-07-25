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

We decided to do this by flaging the whole the activity into child activity

Example like our model would start just after the customer comes to desk

<img src="https://i.imgur.com/gCf4NzK.jpg" border=0>

after that it our **video classifier models** looks for 2 major action in and they should be in proper sequence

```
                   Boolean Flag 1                                           Boolean Flag 2                
                       |     |                                                 |     |                        
       | Identify the 1st Registration Activity|     >>        | Identify the 2nd Registration Activity| 
       
 ```
                                                                                        

and if Model is not able to identify the any of the activity in proper sequence that would be consider as Fraud

To track down the whole process we have three activity classifier


