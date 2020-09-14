# Py-Archaeology

The contents of this repo are designed as a classic example of "well, it works on my laptop" data science. 

1. Fork the repo, so you have your own space to work. 

2. Get the notebook running! You have a couple of options here - you can try to get the notebook running locally on your machine, or you can go straight ahead and get it running on OpenShift. (Remember, on OpenShift any changes you make to the notebooks or environment will not be saved unless you push them back to your git repo before spinning down the notebook pod.)

	#### Solution 
	- try following these tips to get your python environment set up:
		 - use pyenv to control versions of python
		- use pip to manage python packages
	- Install the notebooks and dependencies
		1.  Clone this repository:  `git clone https://github.com/sophwats/py-archaeology/`
		2.  Change to this repository's directory:  `cd py-archaeology`
		3.  Install the dependencies:  `pipenv install`
		4.  Run the notebooks:  `pipenv run jupyter notebook
	- Ammend the notebook so it runs. I've made these updates in the `my-reproducible-model.ipynb` notebook.  You will need to:
		1. Change the path to point to the data on your local machine
		2. Change the column name calls from `Message` to `Text` so they match with the data in your local files. 
		- Some extra changes to make the notebook a better communication tool (optional):
			1. Replace comments with Markdown
			2. Split code so that each cell does 'one' thing
			3. Add prose to explain what the notebook is showing
			4. Add xgboost and cloudpickle to the Pipfile. 


3. Create a notebook image containing all the dependencies for the notebook.
	- When creating a custom notebook image, you need to point to a git directory which the s2i build can run against. The directory needs to contain a requirements.txt file and any notebooks and data you want to appear in your image. 
	- See [here](https://github.com/willb/jupyter-notebooks#creating-custom-notebook-images) for an example of building a notebook image. 
	- _Hint: Try building on the [jupyter minimal notebook image](https://hub.docker.com/r/jupyter/minimal-notebook)
				- You'll first have to add this image to your OpenShift project._
	#### Solution
 
   1. Make sure you have added xgboost and cloudpickle to your PipFile
   2. Remove un-used dependencies from the Pipfile
		- it's a good idea to check the notebooks run again on this reduced environment. 
		- if you're using `pipenv` you can do this by typing `pipenv uninstall --all` followed by `pipenv install`.
	3. Create a `requirements.txt` file by typing `pipenv lock -r > requirements.txt` 
	
	4. Log into your OpenShift cluster from the terminal, and make sure you are working in your project. 
	5. Type `oc get imagestreams` to see which image streams are present in your project. I'm going to use the `s2i-spark-minimal-notebook:3.6` as a base image: 
	`oc new-build --name classifier-notebook --image-stream  s2i-spark-minimal-notebook:3.6 --code https://github.com/sophwats/py-archaeology\#solution`
	(You will need to replace the git repo with yours!) 
4. Add the image to the JupyterHub launcher in the Open Data Hub, and run the notebook.

	#### Solution
	
	- From the OpenShift Console, go into your OpenDataHub instance through installed operators. From there, you can edit the YAML so that the `notebook_image` field points to your new image. You should now be able to go to your JupyterHub route and see the image listed in the pre-loads. Check your notebook runs!
	

5. Use [this](https://github.com/willb/simple-model-s2i/blob/pipeline-s2i/.s2i/assemble) s2i builder to create a model service running on OpenShift. (You might have to change the structure or output of the notebook). 
	- The pipeline is already installed in your OpenShift project
		- See [here](https://github.com/willb/fraud-notebooks) and [here](https://github.com/aicoe-fde/ml-workflows-notebook) for examples of a git repos which work with this pipeline. 
6. Use the model service to make predictions. _(Take a look at the 'Pipelines' notebooks in the two repos listed in Step 5 to see examples of how to interact with the model service.)_
7. Use Prometheus to monitor the model's predictions. 


