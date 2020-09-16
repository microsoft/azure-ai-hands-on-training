
# Module 4 - Advanced AML Concepts for Professional Data Scientists and ML Engineers

## Hands-On exercises

We recommend to run these exercises in a Compute Instance on Azure Machine Learning. To get started, open Jupyter on the Compute Instance, select `New --> Terminal` and clone this repo:

```cli
git clone https://github.com/microsoft/azure-ai-hands-on-training.git
```

Then navigate to the cloned folder in Jupyter and start with the exercises.

### Exercise 1 - Simple training pipeline

* This exercise shows how to construct and execute a basic AML pipeline for model training
* Walk through [00-single-training-step/pipeline.ipynb](00-single-training-step/pipeline.ipynb)

### Exercise 2 - Multi-step pipeline

* This exercise shows how to construct and execute a two-step pipeline that runs data preparation and model training afterwards
* Walk through [01-multi-step-pipeline/pipeline.ipynb](01-multi-step-pipeline/pipeline.ipynb)

### Exercise 3 - Batch scoring pipeline using ParallelRunStep

* This exercise how to construct and run a batch scoring pipeline using `ParalleRunStep`, including storing the batch results on a pre-defined location on the datastore
* Walk through [02-parallel-run-step/pipeline.ipynb](02-parallel-run-step/pipeline.ipynb)

### Exercise 4 - Azure Machine Learning CLI

* This exercise shows how to train, register and deploy a model using the `az ml` CLI
* Follow the [instructions](03-azml-cli/README.md)

## Additional Links

* [Plenty of pipeline examples](https://github.com/Azure/MachineLearningNotebooks/tree/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines)
* [Pipline example with Parameters for passing in Dataset](https://github.com/Azure/MachineLearningNotebooks/blob/master/how-to-use-azureml/machine-learning-pipelines/intro-to-pipelines/aml-pipelines-showcasing-datapath-and-pipelineparameter.ipynb)
* [Detailed tutorial for `ParallelRunStep`](https://docs.microsoft.com/en-us/azure/machine-learning/how-to-use-parallel-run-step)
* [`az ml` reference](https://docs.microsoft.com/en-us/cli/azure/ext/azure-cli-ml/ml?view=azure-cli-latest)