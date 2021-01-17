*NOTE:* This file is a template that you can use to create the README for your project. The *TODO* comments below will highlight the information you should be sure to include.


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

## Screen Recording
*TODO* Provide a link to a screen recording of the project in action. Remember that the screencast should demonstrate:

## Standout Suggestions
*TODO (Optional):* This is where you can provide information about any standout suggestions that you have attempted.
