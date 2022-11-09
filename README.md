# GitHub Action For Testing If Code Is Remotely Runnable.

Launch the current PR as a ClearML experiment and enqueue it. Once it is being processed start a timeout. The task has to report at least 1 iteration before the timeout expires. Including this check will ensure that every PR is able to be run by a ClearML agent if needed.

## Example usage

```yaml
name: Check remotely runnable
on:
  pull_request:
    branches: [ main ]
    types: [ assigned, opened, edited, reopened, synchronize ]

jobs:
  check-remotely-runnable:
      runs-on: ubuntu-20.04
      steps:
        - name: Check remotely runnable
          uses: thepycoder/clearml-actions-check-remotely-runnable@main
          with:
            CLEARML_API_ACCESS_KEY: ${{ secrets.ACCESS_KEY }}
            CLEARML_API_SECRET_KEY: ${{ secrets.SECRET_KEY }}
            CLEARML_API_HOST: ${{ secrets.CLEARML_API_HOST }}
            CLEARML_TIMEOUT: 600
            CLEARML_QUEUE: 'GPU Queue'
```

## Inputs

1. `CLEARML_API_ACCESS_KEY`: Your ClearML api access key. You can get on by following the steps [here](https://clear.ml/docs/latest/docs/getting_started/ds/ds_first_steps) or reuse one from you `clearml.conf` file. 
2. `CLEARML_API_SECRET_KEY`: Your ClearML api secret key. You can get on by following the steps [here](https://clear.ml/docs/latest/docs/getting_started/ds/ds_first_steps) or reuse one from you `clearml.conf` file. 
3. `CLEARML_API_HOST`: The ClearML api server address. If using the free tier, that's `api.clear.ml` if you have a self-hosted server, you'll have to point this to wherever it is deployed.
4. `CLEARML_TIMEOUT`: How long to wait for the first iteration before failing, not including time waiting in queue, in seconds. Note: this does include docker setup and package installations! (default: 600)
5. `CLEARML_QUEUE`: Name of the queue to place the experiment into. (default: "default")