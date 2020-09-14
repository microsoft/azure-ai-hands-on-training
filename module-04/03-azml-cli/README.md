# Azure Machine Learning CLI

This exercise shows how to train, register and deploy a model using the `az ml` CLI.

## Using the CLI for training jobs

Let's make sure we are in the correct folder:

```cli
cd module-04/03-azml-cli/
```

### Attach to folder

Let's first set the default workspace and resource group for the folder we are in. This will also include subfolders and later allows us to not always having to specify the workspace and resource group in each call.

```cli
az ml folder attach -g <your resource group> -w <your workspace name>
```

### Submit training job

Next, we need to retrieve the training dataset's `id` (we've created the dataset in tutorial `00-single-training-step`):

```
az ml dataset show -n german-credit-train-tutorial
```

Note the `id` and open [`config/train-amlcompute.runconfig`](config/train-amlcompute.runconfig). In this file, set the dataset id in the last section:

```
data:
  training_dataset:
    dataLocation:
      dataset:
        id: xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxx << Replace with your dataset's id!
````

Save the file - now we can submit a training job to our compute cluster:

```cli
rm run.json # Make sure this file does not exist
az ml run submit-script -c config/train-amlcompute -e german-credit-train-amlcompute -t run.json
```

* The `-c` option specifies the runconfig that we want to use for training. In this case, `config/train-amlcompute` refers to the file [`config/train-amlcompute.runconfig`](config/train-amlcompute.runconfig). Have a look at the file, this contains the full training job defintion.
* The `-e` option referes to the experiment name, under which we want to the training run.
* The `-t` option outputs a reference to the experiment run as a json file, which can be used to directly register the model from it.

## Using the CLI for model registration

Given the `run-metadata-file` from the experiment run (`run.json`), we can now register the model from it in AML. In this case, the `asset-path` points to the path structure within the experiment run:

```cli
az ml model register --name credit-model --asset-path outputs/model.pkl --run-metadata-file run.json
```

## Using the CLI for model deployment

Finally, we can deploy our model to Azure. For sake of this tutorial, we'll only deploy to Azure Container Instances, but deployment to AKS is very similar:

```cli
az ml model deploy -n credit-model-aci -m credit-model:1 --inference-config-file config/inference-config.yml --deploy-config-file config/deployment-config-aci-qa.yml --overwrite
```

* `-n` refers to the deployment name
* `-m` refers to the registered model and its version
* `--inference-config-file` defines the runtime environment for our model
* `--deploy-config-file` defines on what infrastructure the deployment should happen

We can test the ACI endpoint using the following request (make sure to replace the URL with your container's URL). You can find the same code also in [`test_webservice.ipynb`](test_webservice.ipynb):

```python
import requests

url = 'http://xxxxxxx-xxxx-xxxx-xxxx-xxxxxxxx.xxxxxx.azurecontainer.io/score'

test_data = {
  'data': [{
    "Age": 20,
    "Sex": "male",
    "Job": 0,
    "Housing": "own",
    "Saving accounts": "little",
    "Checking account": "little",
    "Credit amount": 100,
    "Duration": 48,
    "Purpose": "radio/TV"
  }]
}

headers = {'Content-Type': 'application/json'}
response = requests.post(url, json=test_data, headers=headers)

print("Prediction (good, bad):", response.text)
```

Lastly, we can remove the ACI-based service via:

```cli
az ml service delete --name credit-model-aci
```

See the included [`config/deployment-config-aks-prod.yml`](config/deployment-config-aks-prod.yml) for the deployment definition to AKS. The inference config itself won't need to be changed!