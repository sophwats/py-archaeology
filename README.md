# Py-Archaeology

The contents of this repo are designed as a classic example of "well, it works on my laptop" data science. 

1. Fork the repo, so you have your own space to work. 

2. Get the notebook running! You have a couple of options here - you can try to get the notebook running locally on your machine, or you can go straight ahead and get it running on OpenShift. (Remember, on OpenShift any changes you make to the notebooks or environment will not be saved unless you push them back to your git repo before spinning down the notebook pod.)

3. Create a notebook image containing all the dependencies for the notebook.
	- When creating a custom notebook image, you need to point to a git directory which the s2i build can run against. The directory needs to contain a requirements.txt file and any notebooks and data you want to appear in your image. 
	- See [here](https://github.com/willb/jupyter-notebooks#creating-custom-notebook-images) for an example of building a notebook image. 
	- _Hint: Try building on the [jupyter minimal notebook image](https://hub.docker.com/r/jupyter/minimal-notebook)
				- You'll first have to add this image to your OpenShift project._
4. Add the image to the JupyterHub launcher in the Open Data Hub, and run the notebook.

5. Use [this](https://github.com/willb/simple-model-s2i/blob/pipeline-s2i/.s2i/assemble) s2i builder to create a model service running on OpenShift. (You might have to change the structure or output of the notebook). 
	- The pipeline is already installed in your OpenShift project
		- See [here](https://github.com/willb/fraud-notebooks) and [here](https://github.com/aicoe-fde/ml-workflows-notebook) for examples of a git repos which work with this pipeline. 
6. Use the model service to make predictions. _(Take a look at the 'Pipelines' notebooks in the two repos listed in Step 5 to see examples of how to interact with the model service.)_
7. Use Prometheus to monitor the model's predictions. 


