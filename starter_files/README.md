
# Operationalizing Machine Learning Project

In this project, I use AutoML to train lots of models on the Bank Marketing dataset and deploy the best one to a REST endpoint that can be consumed by other applications. I then create a pipeline using the Azure SDK that will automate the model training process. This pipeline is itself deployed to an endpoint so it can be started with a simple REST API call.

## Architectural Diagram
![alt text](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/Flow.png?raw=true)


For this project, there are 8 steps:

1. Authentication
2. Automated ML Experiment
3. Deploy the best model
4. Enable logging
5. Swagger Documentation
6. Consume model endpoints
7. Create and publish a pipeline
8. Documentation 

## Key Steps
### Authentication <a name="auth" />
Since I used the Udacity provided lab, this step was taken care of for me by the lab. But in essence, it ensures that the Azure Machine Learning Extention (az) command line interface is allowed to communicate with the Azure ML Studio.
### Automate ML Experiment <a name="automl" />
For this step, Bank Marketing dataset was uploaded to the Azure ML Datastore using the Azure ML Datasets interface.
![alt text](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/1%20dataset.png?raw=true)

Next, AutoML Classification experiment using the Azure ML AutoML UI. This experiment trained several different models at the same time on an Azure compute cluster allowing  to find a model with high accuracy.

![alt text](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/3%20best%20model.png?raw=true)

The best model from this AutoML experiment, which turned out to be a VotingEnsemble model. This makes sense since Voting Ensembles take the output from several different types of classification modles and gives them a weighted vote to get the classifaction decision.

### Deploy the Best Model <a name="best_model" />
Now that the best model was identified, deployed it as a REST endpoint by simply clicking the 'Deploy' button in the Azure ML Studio from the Model details UI.

![alt text](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/4%20endpoint%20deployed.png?raw=true)

### Enable Logging <a name="logging" />
To enable logging, I use a python [logging script](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/logs.py) that turned on the 'Application Insights' for the deployed endpoint and output the logs. In the first screenshot below, you can see the endpoint shows 'Application Insights enabled' is true, and the second shows the log output. These logs let you see details about how well the endpoint is running.
![Application Insights On](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/6%20app%20insight%20enabled.png?raw=true)
![Log output](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/5%20logs.py.png?raw=true)

### Swagger Documentation <a name="swagger" />
Next,deployed a Swagger UI docker container and a simple server to be able to display the swagger documentation for this endpoint using the 
swagger.sh and serve.py scripts. 
We can use this UI to see what GETs and POSTs can be performed on the endpoint. 
In addition, it allows you to see the correct JSON format for the POST if we want to get inference results from our model. 
You can see exactly what fields are expected and get an idea for the data type of each field (ie dates, strings, ints, etc).

![Swagger UI](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/swagger%20ui.PNG?raw=true)

### Consume Model Endpoint <a name="consume" />
Next, run a simple endpoint.py using the data format shown in Swagger to ensure that the endpoint could actually be consumed by an application. Below we can see the response that the endpoint returned

![Endpoint Output](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/endpoint%20output.PNG?raw=true)

Next used Apache Benchmark to run a benchmark against the endpoint to ensure it runs efficiently. We can see in the benchmark output that none of the 10 requests failed and that they all came back extremely quickly.

![Benchmark Output1](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/benchmark%20output%201.PNG?raw=true)
![Benchmark Output2](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/benchmark%20output%202.PNG?raw=true)

### Create and publish a pipeline <a name="publish" />
Next I used a aml-pipelines-with-automated-machine-learning-step.ipynb to create and publish the whole pipeline. In the notebook, we first import the various libraries and packages we'll need, mostly from the Azure SDK. Then we connect to the Azure Workspace and search for a compute cluster, creating one if it doesn't exist. We also check to see if the Bankmarketing dataset is uploaded and upload it if it isn't. Then we configure an AutoML Step which will perform an AutoML search for an optimal model as described in [Section 2](#automl). The full pipeline is then set up using this AutoML step with the dataset being fed into it. The notebook runs the pipeline and does some testing of model results to ensure it's working.

Here we see the pipeline:
![Pipelines](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/10%20pipeline%20created.png?raw=true)

After the pipeline has been created, we use the Azure SDK and our notebook to publish it to a REST endpoint. This will allow us to run the pipeline using a simple REST API call.

The published endpoint:
![Pipeline Endpoints](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/11%20pipeline%20endpoints.png?raw=true)

Details of the published pipeline endpoint, including the REST URL:
![Published Pipeline](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/Published%20Pipeline.png?raw=true)

Lastly, in the notebook we make a REST call using a POST to make sure the pipeline kicks off as we expect it to. Once it's running, we can check in on the run's progress both through the Azure ML Studio UI and the RunDetails widget in our notebook.

view of the pipeline running from the RunDetails widget in Jupyter Notebook:
![RunDetails Run](https://github.com/hanumanraje/machine-Learning-with-Azure-/blob/master/starter_files/ScreenShoot/14%20pipeline%20runwidget.png?raw=true)

### Documentation <a name="docs">
I created documentation in the form of this README file.
  
## Suggestions for Future Work 
For future work, I would suggest making further tweaks to the AutoML step of the pipeline. There are a lot of settings involved and making changes to them could improve the search space and help find an even better model solution. There are also additional steps that could be added to the pipeline, perhaps doing some dataset cleanup or feature engineering before the AutoML step, or doing additional steps after the AutoML step has completed.

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
