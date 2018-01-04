# Accelerate Data Science with Containers 

Getting started with data science and applying machine learning was never as simple as now. Many free and paid online tutorials and courses help you to get started. I’ve recently started to learn, play and work on Data Science & Machine Learning on Kaggle.com. In this brief post, I’d like to share my experience with the Kaggle Python Docker image, which simplifies the Data Scientist’s life ….

### Why did I start to use containers for Data Science & Machine Learning with Python?

The short answer is simple: I had to push long-running and compute intense machine learning jobs from my laptop computer to an old, but powerful desktop workstation. 

Setting up a 2nd Python development and test machine many isn’t that hard, but still can be a hazel. **`pip install`** is not my best friend because it fails far too often especially behind enterprise HTTP proxies. Therefore I prefer to use Anaconda anyway. The Kaggle Python Docker image looked interesting for further reasons:

* The image contains the same software libraries as the Kaggle online runtime environment. This would allow me to play and develop in a local, private environment and upload only significant changes to the online version of my Kaggle notebooks and code.
* As a Newbie Kaggler, you might want to learn from, modify and try out the solutions of advanced users without forking too many scripts and notebooks. Downloading scripts and notebooks and running them in your own environment offers you a lot of freedom and flexibility.
* Using the Kaggle Python Docker image made it very simple to create a minimal, shared Jupyter Notebook environment for a side project with a co-worker.

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
Let’s do a quick check before starting a Docker Jupyter Notebook environment:
