# Experiment Factory :heart: LabJS

If you want to make your own experiment interactively, [LabJS](https://github.com/getify/LABjs) can help you!
If you want to then build and deploy your experiments into reproducible experiment containers, you
can use the [expfactory builder](https://expfactory.github.io/builder), demonstrated here to empower you!

![img/labjs.png](img/labjs.png)

## Design Your Experiment
The first thing you likely want to do is to design your experiment. Take a look at the [getting started](https://github.com/FelixHenninger/lab.js) section of the README.md file linked in the main repository. There is ample documentation about a started kit, along with a tutorial to build your first experiment. You will use the [LabJS builder interface](https://labjs.felixhenninger.com/) to design your experiment, and when you finish, the Experiment Factory (v3.0) is an export option:

![img/export.png](img/export.png)

This will export a zip file of all the files needed to plug into the Experiment Factory! To help you learn and get started, we are providing an example export ([stoop-task-export.zip](stoop-task-export.zip)) of a Stroop task in this folder. At this point, the only thing we need to do is:

 1. Clone this repository
 2. Move the experiment into the "experiments" folder
 3. Connect!

Full documentation is available with the [expfactory builder](https://expfactory.github.io/builder), but we will
guide you here as well.

## 0. Clone the repository
The repository has a hidden folder, [.circleci](.circleci) with the build and deploy setup that you need. The easiest way to get this to your computer is to fork it to your Github account, and then clone:

```bash
git clone https://www.github.com/<username>/builder-labjs
cd builder-labjs
```

## 1. Export and Extract
At this point you would design your experiment in LabJS, and export it as shown in the instructions above. We've done this for you, for a stroop task. Let's first extract the exported experiment. It will dump the required files into a folder in the present working directory.

```
unzip stroop-task-export.zip
ls
stroop-task
```

Take a look at the `config.json` in the folder. It will provide metadata exported about your experiment, and you can customize this if needed before building your container.

```
cat stroop-task/config.json 
{
  "name": "Stroop task",
  "exp_id": "stroop-task",
  "url": "https://github.com/felixhenninger/lab.js/examples/",
  "description": "An implementation of the classic paradigm introduced by Stroop (1935).",
  "contributors": [
    "Felix Henninger <mailbox@felixhenninger.com> (http://felixhenninger.com)"
  ],
  "template": "lab.js",
  "instructions": "",
  "time": 5
}
```

Finally, we can move the entire thing into the "experiments folder for it to be discovered by the builder.

```bash
mkdir -p experiments
mv stroop-task experiments/
```
If you wanted to add additional experiments from the [library](https://expfactory.github.io/experiments)
you could add a single line (space separated) to an experiments.txt file in the main folder.

## 2. Build
We now will recruit the builder to turn our folder into a reproducible experiment container!
Unlike the instructions in [expfactory-labjs](https://www.github.com/expfactory/expfactory-labjs),
we don't need to do any building or use of Docker locally. We just need to:

 1. Create a container repository on Docker Hub to correspond to the name you want to build
 2. Commit and push the code to Github
 3. Connect the repository to Circle Ci, and
 4. Add this name to the variable `CONTAINER_NAME`, along with `DOCKER_USER` and `DOCKER_PASS` to the set of encrypted environment variables in our CircleCI project settings.

Once you've done those steps, that's it! The container will be built and pushed to Docker Hub on each commit. 


## 3. Run
Once your container is deployed, you can run and use it! [There are many ways to do that](https://expfactory.github.io/expfactory/usage). Here is a simple headless start:

```
docker run -d -p 80:80 vanessa/expfactory-stroop start
```
and you will see the familiar interface to choose your task and get started. Have fun!

![img/stroop.png](img/stroop.png)
