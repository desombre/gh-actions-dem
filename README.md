# GH Actions Examples

## Getting started

Workflows are defined in `.yml`/`.yaml` files that have to be located in the directory `.github/workflows`.
A workflow is a collection of actions.
When the workflow is availabe on the default branch, github willl pick it up and run it according to the triggers.

### Structure

The workflows file should start with the name of the action, e.g. `name: GitHub Actions Demo`.

The next thing that should be defined, are the triggers of the workflow with the `on` keyword.
You can define restrictions for some triggers, e.g. a tag having to follow a specific format or a merge having to happen on a specific branch.
Some, but not all triggers are:
- `push`:  push a commit or tag
- `workflow_dispatch`: manual trigger, here you can define inputs that can or have to be provided
- `issues`: issue is modified, here you can define what modification should trigger the workflow

After theses things are defined you start defining jobs. Jobs have a name, in this case defined by the key you use in the yaml file for the job, and at minimum the `runs-on`and `steps` defined. Additionally when you have a multi-job workflow you might want to define the order by using the `needs` keyword.  

`runs-on` defines on what platform the actions should be run. This can be one or multiple platforms. These can be [hosted by github](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners) or [self-hosted](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners).


### Calling external workflows

Instead of running a command directly with `run`, you can also call another workflow using the `uses` keyword. This can be en external one found on the [marketplace](https://github.com/marketplace?type=actions) or an internal one.

You can provide the workflow with inputs using the `with` keyword.

### Using composite actions

Instead of re-using a whole workflow, you can use [composite actions](https://docs.github.com/en/actions/creating-actions/creating-a-composite-action) to re-use actions in other workflows.

## Self-hosted runners

A self hosted runner can be defined in the settings of an organzation or repository. 

The basic setup is automated with a script provided by github itself. Using tags you can define what workflow runs on which self-hosted runner.

## Triggering actions
### gh cli

Github has a cli tool you can use to interact the it, this includes workflows.
You can find the installation guide [here](https://cli.github.com/).

To get the workflows of a repository make sure you are in the folder on your machine, then run `gh workflow list`. You will see the workflows with their status and ids.
```
NAME                             STATE   ID       
--------------------------------------------------
CI                               active  110063155
GitHub Actions Re-usable Demo    active  110063352
GitHub Actions Self-hosted Demo  active  110063353
GitHub Actions Demo              active  110063354
```

To trigger a `workflow_dispatch` run `gh workflow run [<workflow-id> | <workflow-name>] [flags]`. In our case for example: `gh workflow run "GitHub Actions Demo" --field tag=0.0.1`.

This creates a new workflow run. You can see all / the latest runs using `gh run list`. This also shows you the status in a short overview format.
```
STATUS  TITLE                            WORKFLOW                        BRANCH  EVENT              ID   ...                 

✓       GitHub Actions Demo              GitHub Actions Demo             master  workflow_dispatch  10176662883  ...

```

To get the details of a run after the fact use `gh run view [run-id]` or if you want to see the live progress you can use `gh run watch [run-id]`.

Using the cli you can create scripts to automate use cases for your own needs, e.g. run the tests and when successful, deploy to dev.

## Running locally

You can also run workflows on you machine using [`act`](https://github.com/nektos/act).
This is most commonly used to debug/work on workflows that are WIP. 

Act has event support, e.g. you can simulate a push to trigger workflows and debug the behavior. Alternatively you can trigger workflows by their filename.

You can get an overview of existing jobs and workflows with `act --list`.

To run a job using `act -j [job]` or a workflow using `act -W [workflow]`.

Act will try to find the platform the job ha to run on and using docker run the action in a container.

If you used a self-hosted runner act will not know what platform it actually has to use and you will see the following error message: `🚧  Skipping unsupported platform -- Try running with '-P local=...'`. Using the flag `-P`you can define what image you want act to use, e.g. `act -W ./.github/workflows/self-hosted.yml -P local=ubuntu`.

_Note:_ `act` can also be installed as an add-on to the github-cli



# FAQ
*Why does my workflow not show up?*
- Did you follow the correct folder / file structure?
- Is it pushed to the default branch?

*Why does my workflow not get picked up by the self-hosted runner?*
- The runner could be down, check the settings in github to see the status.
- The runner could be busy. Runners are single-threaded, try installing multiple runners on the same machine.
