## Introduction

Automatic machine learning is a technique that provides methods and processes to make machine learning available so that we do not have to hardly programe. It improves efficiency of machine learning and it covers a lot of areas in data science.

We can preprocess and clean our data, select and filter the data, automatically choose the model, optimize hyperparameters and so on. 

From experience, There are so many AutoML APIs in different platforms, like PyCaret, which is an open source and not a hard-coded ML library in Python. The feature of using it is that it is really faster for whoever wants to use it, i.e., seasoned data scientist or newbies in data science. The goal of the caret package is to automate the major steps for evaluating and comparing machine learning algorithms for classification and regression. The main benefit of the library is that a lot can be achieved with very few lines of code and little manual configuration. The PyCaret library brings these capabilities to Python.

Azure also provides the service of AutoML, now, let's step by step explore the Azure AutoML.

## 1. Create an Azure ML workspace

Creating an Azure Ml work here is the same like what we discussed in previous article, you can check out in my article, the repository name is AzureML-Startup, there is very specific process to show about how to create an Azure ML workspace.

Now here, we can review some definition that should be referred since they are used very frequently.

![image](https://user-images.githubusercontent.com/71245576/115314233-cc27b580-a142-11eb-9d10-e3f945ceb2f0.png)

## 2. Create compute resources

Please look back on my article about how to create a compute resources in AzureML-Startup.

There are something that we can review:

In Azure ML studio, under Manage, there is Compute page. We can mangae the compute targets for data science activities and now let's discuss what kinds of compute resource you can create:

![image](https://user-images.githubusercontent.com/71245576/115314467-42c4b300-a143-11eb-82e4-3d04320b3f8f.png)

While the compute instance is being create, add a new compute cluster in compute clusters tab, which will be used to train a machine learning model, there are the settings in setting a compute cluster:

![image](https://user-images.githubusercontent.com/71245576/115314728-c8e0f980-a143-11eb-80de-f18e07ad7bc4.png)

## 3. Create a dataset

In Azure machine learning studio, view the Datasets page, now we upload a dataset into the platform.

The dataset is comma-separated data at https://aka.ms/bike-rentals, it will save as a local file named daily-bike-share.csv.

Now create a dataset from local file system:

![image](https://user-images.githubusercontent.com/71245576/115315175-c7640100-a144-11eb-8c43-ab721d415074.png)

The basic info is that the name is bike-rentals, dataset type is Tabular and I wrote "Bicycle rental data" in the Description.

![image](https://user-images.githubusercontent.com/71245576/115315300-0bef9c80-a145-11eb-99fd-64bc214689ab.png)

The next step is to set for the datastore and file selection.

![image](https://user-images.githubusercontent.com/71245576/115315415-45280c80-a145-11eb-9b92-da83e5ef1a2a.png)

Next, setting and preview, this content, is to set the file format, delimiter, encoding format, column headers and row skipping, the picture under the setting and preview part is to show a part of rows of your dataset.

We should match the settings with what your dataset looks like, then check on the picture of the sample.

![image](https://user-images.githubusercontent.com/71245576/115315528-891b1180-a145-11eb-97dc-ac771420abff.png)

After setting the scheme(for this dataset, include all columns other than Path), continue and the dataset will be created.

![image](https://user-images.githubusercontent.com/71245576/115315912-47d73180-a146-11eb-9c2c-12985969a032.png)

## 4. Train models in AutoML

As tutorial said, Azure Machine Learning includes an automated machine learning capability that leverages the scalability of cloud compute to automatically try multiple pre-processing techniques and model-training algorithms in parallel to find the best performing supervised machine learning model for your data.

You can use automated machine learning to train models for classification, regression and time series forecasting. 

Now in Azure machine learning studio, view the automated ML page under Author:

![image](https://user-images.githubusercontent.com/71245576/115319643-414cb800-a14e-11eb-987c-0719e6502e63.png)

Select the dataset bike-rentals, target column and select the compute cluster in configuration:

![image](https://user-images.githubusercontent.com/71245576/115447579-a94edd00-a1e6-11eb-983d-1e01d94e2358.png)

Then, choose the task type and set the additional configuration settings:

![image](https://user-images.githubusercontent.com/71245576/115447709-ce435000-a1e6-11eb-83fd-604baf635c89.png)

In the additional configurations, blocking all other than LightGBM and Randomforest is for avoiding long time running.

![image](https://user-images.githubusercontent.com/71245576/115448050-36923180-a1e7-11eb-8be9-3691e2a6357a.png)

Then enbale featurization and finish submissting the automated ML rundetails, it will start automatically. You can check it on the interface of Automated ML:

It is setting up the run now:

![image](https://user-images.githubusercontent.com/71245576/115448283-8670f880-a1e7-11eb-9d45-91115c5dc44a.png)


## 5. Review the models

After the tasks has finished we can review the best performaning moodel, the best model in this time is VotingEnsemble.

![image](https://user-images.githubusercontent.com/71245576/115465742-10778c00-a1fd-11eb-9afc-99af5067359a.png)

The normalized root mean squared error is the metric that we selected to indicate in this dashboard. Now let's look at other metrics to see values of other evaluation metrics for a regression model:

Select the Metrics tab and select the residuals and predicted_true charts if they are not already selected. 

![image](https://user-images.githubusercontent.com/71245576/115466248-d22e9c80-a1fd-11eb-8bdf-3faa472249f3.png)

Meanwhile we can review the charts in which the performance of the model by comparing the predicted values against the true values is showed.

![image](https://user-images.githubusercontent.com/71245576/115466408-10c45700-a1fe-11eb-8adf-b155772e1881.png)

The residual histogram in another way, shows the frequency of residual ranges.

![image](https://user-images.githubusercontent.com/71245576/115466486-2b96cb80-a1fe-11eb-84e7-2f0d692cf281.png)

Select the explanation tab. You need to click on the arrows>>next to Explanation ID to expand the explannations list. Select an explanation ID, select View previous dashboard experience on the right-hand side. Then select Global Importance. This chart shows how much each feature in the dataset influences the label prediction, like this:

![image](https://user-images.githubusercontent.com/71245576/115466738-7adcfc00-a1fe-11eb-8f2c-990fee1e909c.png)

## 6. Deploy a predictive service

This part is to introduce how to use the trained model to deploy a service like predictive service as an Azure Container Instances(ACI) or an Azure Kubernetes Service(AKS).

The tutorial said that for production scenarios an AKS deployment is recommended, for which you must create an inference cluster compute target. But in ACI service, you do not have to create an inference cluster. 

Now create up an ACI service. Select the run for the automated machine learning experiment and view the Details tab on the Automated ML page.

Select the best model and click deploy:

![image](https://user-images.githubusercontent.com/71245576/115467490-9e547680-a1ff-11eb-9ff3-31cecef69ca9.png)

Configure deploying a model and wait for deploying.

![image](https://user-images.githubusercontent.com/71245576/115467589-c217bc80-a1ff-11eb-8030-866a59a423f3.png)

The model deployment is successfully triggered:

![image](https://user-images.githubusercontent.com/71245576/115467670-e2477b80-a1ff-11eb-8d33-651cfdc79234.png)

Check on the deploy status, now it should be running and we should wait it for changing to Successful(to be succeeded). You can select to refresh its status.

After it goes to be succeeded, in Azure ML studio, veiw the Endpoints page and select the predict-rentals real-time endpoint. Then select the Consume tab and notice the REST endpoint for you service as well as the Primary Key for your service:

![image](https://user-images.githubusercontent.com/71245576/115468253-d1e3d080-a200-11eb-9839-f08e269974bb.png)

## 7. Test the deployed service

Now we know that the service has be deployed we can test it using some simple code:

Open a new browser tab and open a second instance of Azure Machine Learning studio and view the Notebooks page under the Author.

In the Notebooks page under My files, create a new file that sets the file location in Users/your user name, File name is Test-Bikes and File type is Notebook and select to overwrite if already exists.

When the new notebook has been created, ensure that the compute instance you created previously is selected in Compute box which is running,

![image](https://user-images.githubusercontent.com/71245576/115469400-92b67f00-a202-11eb-93ae-0d61022d5877.png)

Paste the following code in the rectangular cell, the code defines features for a five day period using hypothetical weather forecast data, and uses the predict-rentals service you created to predict cycle rentals for those five days.

```python
endpoint = 'YOUR_ENDPOINT' #Replace with your endpoint
key = 'YOUR_KEY' #Replace with your key

import json
import requests

#An array of features based on five-day weather forecast
x = [[1,1,2022,1,0,6,0,2,0.344167,0.363625,0.805833,0.160446],
    [2,1,2022,1,0,0,0,2,0.363478,0.353739,0.696087,0.248539],
    [3,1,2022,1,0,1,1,1,0.196364,0.189405,0.437273,0.248309],
    [4,1,2022,1,0,2,1,1,0.2,0.212122,0.590435,0.160296],
    [5,1,2022,1,0,3,1,1,0.226957,0.22927,0.436957,0.1869]]

#Convert the array to JSON format
input_json = json.dumps({"data": x})

#Set the content type and authentication for the request
headers = {"Content-Type":"application/json",
        "Authorization":"Bearer " + key}

#Send the request
response = requests.post(endpoint, input_json, headers=headers)

#If we got a valid response, display the predictions
if response.status_code == 200:
    y = json.loads(response.json())
    print("Predictions:")
    for i in range(len(x)):
        print (" Day: {}. Predicted rentals: {}".format(i+1, max(0, round(y["result"][i]))))
else:
    print(response)
```


In this code, copy your endpoint ID and primary key then paste, like this:


![image](https://user-images.githubusercontent.com/71245576/115470290-145adc80-a204-11eb-9d5a-e2959d9b0439.png)

Run it and you can see the predictions like this: 

![image](https://user-images.githubusercontent.com/71245576/115470385-3ce2d680-a204-11eb-8564-8fad5bf1754f.png)

## Reference

Create no-code predictive models with Azure Machine Learning, retrieved from https://docs.microsoft.com/en-us/learn/paths/create-no-code-predictive-models-azure-machine-learning/




