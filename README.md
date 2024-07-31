# GH Actions Examples

## Getting started

Actions are defined in `.yml`/`.yaml` files that have to be located in the directory `.github/workflows`.
A workflow is a collection of actions.
When the workflow is availabe on the default branch, github willl pick it up and run it according to the triggers.

### Structure

The actions file should start with the name of the action, e.g. `name: GitHub Actions Demo`.

The next thing that should be defined, are the triggers of the action with the `on` keyword.
You can define restrictions for some triggers, e.g. a tag having to follow a specific format or a merge having to happen on a specific branch.
Some, but not all triggers are:
- `push`:  push a commit or tag
- `workflow_dispatch`: manual trigger, here you can define inputs that can or have to be provided
- `issues`: issue is modified, here you can define what modification should trigger the workflow

After theses things are defined you start defining jobs. Jobs have a name, in this case defined by the key you use in the yaml file for the job, and at minimum the `runs-on`and `steps` defined. Additionally when you have a multi-job workflow you might want to define the order by using the `needs` keyword.  

`runs-on` defines on what platform the actions should be run. This can be one or multiple platforms. These can be [hosted by github](https://docs.github.com/en/actions/using-github-hosted-runners/about-github-hosted-runners/about-github-hosted-runners) or [self-hosted](https://docs.github.com/en/actions/hosting-your-own-runners/managing-self-hosted-runners/about-self-hosted-runners).


### Calling external actions

Instead of running a command directly with `run`, you can also call another action using the `uses` keyword. This can be en external one found on the [marketplace](https://github.com/marketplace?type=actions) or an internal one.

You can provide the action with inputs using the `with` keyword.

## Self-hosted runners

A self hosted runner can be defined in the settings of an organzation or repository. 

The basic setup is automated with a script provided by github itself. Using tags you can define what workflow runs on which self-hosted runner.

## Triggering actions
### gh cli
### VS code add-on

## Running locally

_Note:_ `act` can also be installed as an add-on to the github-cli



# FAQ
*Why does my workflow not show up?*
- Did you follow the correct folder / file structure?
- Is it pushed to the default branch?

*Why does my workflow not get picked up by the self-hosted runner?*
- The runner could be down, check the settings in github to see the status.
- The runner could be busy. Runners are single-threaded, try installing multiple runners on the same machine.