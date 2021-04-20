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

i8ui
