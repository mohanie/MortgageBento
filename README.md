# MortgageBento

Consumer Complaints Classifier
All Complaints used in this project are sourced from the <a href="https://www.consumerfinance.gov/data-research/consumer-complaints/search/?from=0&searchField=all&searchText=&size=25&sort=created_date_desc">Consumer Complaint Database</a>


## Flow

1. User creates a project in Watson Studio using a Jupyter notebook, Python 3.5, and Spark.
2. User uses Db2 Warehouse in the Cloud to load and read data.
3. User uses PySpark to create a pipeline, train a model, and store the model using Watson Machine Learning.

## Prerequisites

* An [IBM Cloud Account](https://console.bluemix.net).

* An account on [IBM Watson Studio](https://dataplatform.ibm.com).


# Steps

1. [Clone the repository](#1-clone-the-repository)
1. [Create Watson services in IBM Cloud](#2-create-watson-services-in-ibm-cloud)
1. [Save the credentials for your Watson Machine Learning Service](#3-save-the-credentials-for-your-watson-machine-learning-service)
1. [Import Test and Training into IBM Db2 Warehouse on Cloud](#4-Import-Test-and-Training-into-IBM-Db2-Warehouse-on-Cloud)
1. [Create a notebook in IBM Watson Studio](#5-create-a-notebook-in-ibm-watson-studio)
1. [Run the notebook in IBM Watson Studio](#6-run-the-notebook-in-ibm-watson-studio)
1. [Configure AI OpenScale](#7-configure-ai-openscale)
1. [Connect AI OpenScale to your machine learning model](#8-connect-ai-openscale-to-your-machine-learning-model)
1. [Add Feedback Data using Watson Studio](#9-add-feedback-data-using-watson-studio)

### 1. Clone the repository

```
$ git clone https://github.com/mohanie/MortgageBento.git
$ cd MortgageBento
```

### 2. Create Watson services in IBM Cloud

* Create a new project by clicking `+ New project` and choosing `Data Science`:

![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/project_choices.png)

> Note: Services created must be in the same region, and space, as your Watson Studio service.
> Note: If this is your first project in Watson Studio, an object storage instance will be created.

* Under the `Settings` tab, scroll down to `Associated services`, click `+ Add service` and choose `Watson`:

![](https://github.com/IBM/pattern-images/blob/master/watson-studio/add_service.png)

* Search for `Machine Learning`, Verify this service is being created in the same space as the app in Step 1, and click `Create`.

  ![](https://raw.githubusercontent.com/IBM/pattern-images/master/machine-learning/create-machine-learning.png)

* Alternately, you can choose an existing Machine Learning instance and click on `Select`.

  ![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/watson-studio-add-existing-ML.png)

* The Watson Machine Learning service is now listed as one of your `Associated Services`.

* Click on the `Settings` tab for the project, scroll down to `Associated services` and click `+ Add service` ->  `Spark`.

* Either choose an `Existing` Spark service, or create a `New` one.

  ![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/add_existing_spark_service.png)

  ![](https://raw.githubusercontent.com/IBM/pattern-images/master/watson-studio/add_new_spark_service.png)

### 3. Save the credentials for your Watson Machine Learning Service

* In a different browser tab go to [http://console.bluemix.net](http://console.bluemix.net) and log in to the Dashboard.

* Click on your Watson Machine Learning instance under `Services`, click on `Service credentials` and then on `View credentials` to see the credentials.

  ![](https://raw.githubusercontent.com/IBM/pattern-images/master/machine-learning/ML-service-credentials.png)

* Save the username, password and instance_id to a text file on your machine. You’ll need this information later in your Jupyter notebook.

### 4 Import Test and Training into IBM Db2 Warehouse on Cloud

* Create a [Db2 Warehouse on Cloud Service](https://console.bluemix.net/catalog/services/db2-warehouse-on-cloud/) instance (an entry plan is offered).

* Get the authentication information for DB2, which can be found under the `Service Credentials` tab of the Db2 Warehouse on Cloud service instance created in IBM Cloud. Click `New credential` to create credentials if you do not have any.

* Download csv files [MORTGAGE_ISSUES_TRAINING](https://github.com/mohanie/MortgageBento/blob/master/data/MORTGAGE_ISSUES_TRAINING.csv) and [MORTGAGE_ISSUES_TEST](https://github.com/mohanie/MortgageBento/blob/master/data/MORTGAGE_ISSUES_TEST.csv)

* Click the `Open` icon to open the console.

![](https://github.com/IBM/pattern-utils/raw/master/db2-cloud/DB2CloudOpenConsole.png)

* Click the `Load Data` icon.

![](https://github.com/IBM/pattern-utils/raw/master/db2-cloud/DB2CloudLoadData.png)

*  Browse for `MORTGAGE_ISSUES_TRAINING.csv` file and Select `NEXT`

*  Select your Schema

*  Select `NEW TABLE` and give it the name `MORTGAGE_ISSUES_TRAINING`

*  Select `CREATE` and Select `NEXT`

*  Select `BEGIN LOAD`

*  Repeat steps 3 to 8 to load `MORTGAGE_ISSUES_TEST.csv` file

### 5. Create a notebook in IBM Watson Studio

* In [Watson Studio](https://dataplatform.ibm.com) using the project you've created, click on `+ Add to project` -> `Notebook` 
* Select the `From file` tab.
* Enter a name for the notebook.
* Optionally, enter a description for the notebook.
* Under `Notebook file` Browse to your notebook file
* Under `Select runtime` Select the **Spark services** created earlier.
![](https://github.com/mohanie/MortgageBento/blob/master/images/SparkServices.png?raw=true)
* Click the `Create` button.


### 6. Run the notebook in IBM Watson Studio

* Enter your DB2 Warehouse credentials in cell 1 in  `dashDBloadTraining` and `dashDBloadTest`. Also in cell 12 in `db2_service_credentials`

* Enter your Watson Machine Learning credentials in cell 10 in `wml_credentials`.

* Move your cursor to each code cell and run the code in it. Read the comments for each cell to understand what the code is doing. **Important** when the code in a cell is still running, the label to the left changes to **In [\*]**:.
  Do **not** continue to the next cell until the code is finished running.

### 7. Configure AI OpenScale

**Note** : Before configuring AIOS ensure you have created a Model to monitor using the Notebook imported above.

* Create an AI OpenScale Service
![](https://github.com/mohanie/MortgageBento/blob/master/images/AIOS_Tile.png?raw=true)

* Launch the AI OpenScale Service
![](https://github.com/mohanie/MortgageBento/blob/master/images/LaunchAIOS.png?raw=true)

* Select a `Begin` to get started.

* Select a your Watson Machine Learning instance.
![](https://github.com/mohanie/MortgageBento/blob/master/images/WMLServices.png?raw=true)

* Select you machine learning model from the list displayed.
![](https://github.com/mohanie/MortgageBento/blob/master/images/gs-set-deploy0.png?raw=true)

* Select a `Use the free Lite plan database` option.
![](https://github.com/mohanie/MortgageBento/blob/master/images/gs-set-lite-db.png?raw=true)

* Review summary and select `Save`.

* Select `Yes` on confirmation message.

* Select `Exit to Dashboard`

### 8. Connect AI OpenScale to your machine learning model

* Select a Model monitor
![](https://github.com/mohanie/MortgageBento/blob/master/images/monitor.png?raw=true)

* Select `Configure accuracy`
![](https://github.com/mohanie/MortgageBento/blob/master/images/ConfigAccuracy.png?raw=true)

* Select `Accuracy`
![](https://github.com/mohanie/MortgageBento/blob/master/images/Accuracy.png?raw=true)

* Select `Multi-class classification` for Algorithm type.
![](https://github.com/mohanie/MortgageBento/blob/master/images/MultiClass.png?raw=true)

* Select `Next`
* Set accuracy threshold to 60%
![](https://github.com/mohanie/MortgageBento/blob/master/images/Threshold.png?raw=true)

* Select `Next`
* Set minimum sample size to 10
![](https://github.com/mohanie/MortgageBento/blob/master/images/Minsize.png?raw=true)

* Select `Next`
* Set maximum sample size to 10
![](https://github.com/mohanie/MortgageBento/blob/master/images/Maxsize.png?raw=true)

* Select `Next`
* Review the summary data and cick `Save`
![](https://github.com/mohanie/MortgageBento/blob/master/images/gs-setup-summary4.png?raw=true)

* Select `OK` on confirmation message.
* Select `Exit`
* Select `Insights` to return to main AI OpenScale page
![](https://github.com/mohanie/MortgageBento/blob/master/images/Insights.png?raw=true)

### 9. Add Feedback Data using Watson Studio
* In [Watson Studio](https://dataplatform.ibm.com) Select your AI OpenScale machine learning model.
* Select the `Evaluation` tab.
* Select the `Configure Performance Monitoring`
![](https://github.com/mohanie/MortgageBento/blob/master/images/monitoring.png?raw=true)

* In `Configure performance monitoring`
![](https://github.com/mohanie/MortgageBento/blob/master/images/ConfigMonitoring.png?raw=true)

* Set `Spark Service or Environment` to your Spark service
* Set `Metric details` to accuracy with a threshold of 60
* Set `Feedback data connection` to your IBM Db2 Warehouse on Cloud
![](https://github.com/mohanie/MortgageBento/blob/master/images/NewConnection.png?raw=true)

* Click `Create`
![](https://github.com/mohanie/MortgageBento/blob/master/images/Create.png?raw=true)

* Set `Select feedback data reference` to a IBM Db2 Warehouse Schema
![](https://github.com/mohanie/MortgageBento/blob/master/images/Schema.png?raw=true)

* Enter a table name for your Feedback data.
![](https://github.com/mohanie/MortgageBento/blob/master/images/FeedbackTable.png?raw=true)

* Set `Record count required for re-evaluation` to 10
* Select `Save`

* After configuring Performance Monitoring add the Feedback data.

* Select `Add feedback data` and load the sample feedback sample CSV file.[MortgageFeedback](https://github.com/mohanie/MortgageBento/blob/master/data/MortgageFeedback.csv)
![](https://github.com/mohanie/MortgageBento/blob/master/images/FeedbackData.png?raw=true)

* Select `New evaluation`
![](https://github.com/mohanie/MortgageBento/blob/master/images/NewEvaluation.png?raw=true)

* The Insights page provides an overview of metrics for your deployed models.
![](https://github.com/mohanie/MortgageBento/blob/master/images/AIOS.png?raw=true)
