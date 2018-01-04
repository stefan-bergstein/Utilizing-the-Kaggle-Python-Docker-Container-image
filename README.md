# Accelerate Data Science with Containers 

Getting started with data science and applying machine learning was never as simple as now. Many free and paid online tutorials and courses help you to get started. I’ve recently started to learn, play and work on Data Science & Machine Learning on Kaggle.com. In this brief post, I’d like to share my experience with the Kaggle Python Docker image, which simplifies the Data Scientist’s life ….

**Outline:**
* [Why did I start to use containers for Data Science & Machine Learning with Python?](#why-did-i-start-to-use-containers-for-data-science--machine-learning-with-python)
* [How did I start?](#how-did-i-start)
* [Experiment with Kaggle notebooks in your local sandbox](#experiment-with-kaggle-notebooks-in-your-local-sandbox)
* [Alternatives](#any-alternatives)
* [References](#references)

### Why did I start to use containers for Data Science & Machine Learning with Python?

The short answer is simple: I had to push long-running and compute intense machine learning jobs from my laptop computer to an old, but powerful desktop workstation. 

Setting up a 2nd Python development and test machine many isn’t that hard, but still can be a hazel. **`pip install`** is not my best friend because it fails far too often especially behind enterprise HTTP proxies. Therefore I prefer to use Anaconda anyway. The Kaggle Python Docker image looked interesting for further reasons:

* The image contains the same software libraries as the Kaggle online runtime environment. This would allow me to play and develop in a local, private environment and upload only significant changes to the online version of my Kaggle notebooks and code.
* As a Newbie Kaggler, you might want to learn from, modify and try out the solutions of advanced users without forking too many scripts and notebooks. Downloading scripts and notebooks and running them in your own environment offers you a lot of freedom and flexibility.
* Using the Kaggle Python Docker image made it very simple for me to create a minimal, shared Jupyter Notebook environment for a side project with a co-worker.

### How did I start?
My desktop workstation with Ubuntu 16.04 had already a reasonable Docker version (17.05.0-ce) installed and there I just had to pull the Kaggle Python Docker image. For any details for the Docker installation see [this blog post](http://blog.kaggle.com/2016/02/05/how-to-get-started-with-data-science-in-containers/) or https://docs.docker.com.

```
$ docker pull kaggle/python  
```

The image isn’t very small and therefore it can take some time to download all layers. Once downloaded, you can list the image including the size info: 
```
$ docker image ls kaggle/python
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
kaggle/python       latest              09a349977ca7        2 weeks ago         12.5GB
```

**Let’s do a quick check before starting a Docker Jupyter Notebook environment:**

As already described on [Kaggle.com](http://blog.kaggle.com/2016/02/05/how-to-get-started-with-data-science-in-containers/), it is helpful to create a shell function and add the function also to your `.bash_profile`:

`$ kpython(){ docker run -v $PWD:/tmp/working -w=/tmp/working --rm -it kaggle/python python "$@" ; }`
 
Now we can use `kpython` instead of `python` and print, for example, the python and Keras version of the Docker image:
```
$ kpython -c 'import sys; print("Python version ", sys.version); import keras; print("Keras version ", keras.__version__)'

Python version  3.6.3 |Anaconda custom (64-bit)| (default, Nov 20 2017, 20:41:42)
[GCC 7.2.0]
Using TensorFlow backend.
Keras version  2.1.2
```

**So, a Jupyter Notebook environment can be started with the following docker run command:**

First, create a working directory on the workstation for your notebooks and files. Then start the container.
```
$ mkdir kaggle && cd kaggle
$ docker run -v $PWD:/tmp/working -w=/tmp/working -p 8888:8888 --rm -it kaggle/python jupyter notebook --no-browser --ip="0.0.0.0" --notebook-dir=/tmp/working --allow-root
```
Watch for the output with the token:
```
[C 16:36:23.778 NotebookApp]
    Copy/paste this URL into your browser when you connect for the first time, to login with a token:
        http://0.0.0.0:8888/?token=7b337f55206514624014efe0e84c94c8ad768c94587d700b
```

Since I want to run a Jupyter Notebook service, I usually start the container as detached (-d) instead in foreground mode (-it):
```

$ docker run -v $PWD:/tmp/working -w=/tmp/working -p 8888:8888 --rm -d kaggle/python jupyter notebook --no-browser --ip="0.0.0.0" --notebook-dir=/tmp/working --allow-root
```

You can display the logs of the running container to see the tocken:
```
$ docker ps
CONTAINER ID        IMAGE                       COMMAND                  CREATED             STATUS              PORTS                    NAMES
158fec0f6eaa        kaggle/python               "/usr/bin/tini -- ..."   2 weeks ago         Up 2 weeks          0.0.0.0:8888->8888/tcp   angry_perlman
$ docker logs 158fec0f6eaa

[I 21:04:14.464 NotebookApp] The Jupyter Notebook is running at: http://0.0.0.0:8888/?token=7b337f55206514624014efe0e84c94c8ad768c94587d700b
```
I am connecting from the browser on my laptop computer to the desktop workstation where the Kaggle container is running. Therefore I replace 0.0.0.0 with the IP address or FQDN of the workstation.
```
http://mylinuxbox:8888/?token=2c997056b24406afdfd7e0e1d10861989656e1ef5e22e812
```

Here we go, a fresh Jupyter Notebook ...

![Fresh Jupyter Notebook](/images/empty-notebook.jpg)

## Experiment with Kaggle notebooks in your local sandbox

In this section I’d like to show how you can download Kaggle notebooks and play with a note in your own local environment. 

> Note, please join the Kaggle community and don’t go private only. 
> Share your work, questions and experience with other users.

**Overview**
1. Create a directory structure
1. Download a notebook
1. Download the input data
1. Run the notebook


### Create a directory structure

Kaggle notebooks usually read input data file form the directory `../input/`. 

E.g. `../input/train.csv` and `../input/test.csv`.

A similar directory structure is required on your local system so that notebooks run without any modification:

```
working dir:
 - code
   - notebook.ipynb
 - input
   - train.csv
   - test.csv
```

You can either create the directories on the Linux system manually or via the Jupyter web UI. 

The structure should look like this:
![directory structure](/images/dirs-notebook.jpg)


### Download a notebook
Next, download the notebook that you like to try out. I am illustrating this with my one of my [public Kaggle notebooks](https://www.kaggle.com/stefanbergstein).

On kaggle.com, navigate to the [notebook and code tab](https://www.kaggle.com/stefanbergstein/keras-deep-learning-on-titanic-data), then download the `.ipynb` file on your local computer.
![download code](/images/download-code-notebook.jpg)

After that, upload the notebook into the `code` directory of your local environment. Navivate to the `code` directory and `Upload` the notebook (.ipynb) file. Your result should look like this:

![try out notebook](/images/try-out-notebook.jpg)


### Download the input data

Well ... the notebook needs some input data. On kaggle.com, navigate to the notebook and data tab, then download the `train.csv` and `test.csv` files on your local computer.

![download data](/images/download-data-notebook.jpg)

Next, upload the input data into the `input` directory of your local environment. The result should look like this:

![input data](/images/input-data.jpg)

### Run the notebook
Now it is time to run the notebook in your local Kaggle environment. Navivate to the code directory and open the notebook.
![open-notebook.jpg](/images/open-notebook.jpg)

The notebook opens in a new browser tab and from here you can run, modify, try-out anything ...

![run-notebook.jpg](/images/run-notebook.jpg)

## Any alternatives? 

Sure …

1. Kaggle.com makes it easy to fork kernels (notebooks, scripts) online. In case you don't see any need to develop in your private environment,  Kaggle.com is a good place for practicing data science.
1. Amazon Web Services offers [AWS Deep Learning AMIs](https://aws.amazon.com/de/machine-learning/amis/) with a lot of pre-installed software. 
1. Nothing beats your personalized dev/test environment

## References
* [Kaggle Python docker image](https://github.com/Kaggle/docker-python)

* [How to get started with data science in containers](http://blog.kaggle.com/2016/02/05/how-to-get-started-with-data-science-in-containers/)

* [Setup a data science workflow with kaggle python docker image on laptop](http://mathalope.co.uk/2017/08/02/how-to-setup-a-data-science-workflow-with-kaggle-python-docker-image-on-laptop/)
