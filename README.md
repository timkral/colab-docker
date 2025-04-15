## Colab Docker

Docker images for local runtimes that can be used with Google Colab. This respository contains the following images:

- Python39: A runtime with the python version downgraded to 3.9. As of this writing, the Google Colab default is 3.11 and some older libraries do not function under this version.

## Instructions

### Building Images
Each image has its own subdirectory. Images should be built from their subdirectory as the root. For example:

```
$ cd python39
python39/ $ docker build -t colab-python39 . -f Dockerfile-python39
```

#### Common Errors
In rebuilding an image, you may encounter an error related to invalid signatures with the debian repositories:

```
0.839 W: GPG error: http://deb.debian.org/debian-security bookworm-security InRelease: At least one invalid signature was encountered.
0.839 E: The repository 'http://deb.debian.org/debian-security bookworm-security InRelease' is not signed.
```

In this case, simply delete the current image and re-build it from scratch.

### Running Containers
Once an image is built, you can run it from the command line like so:
```
python39/ $ docker run -p 8888:8888 --name colab-python39 colab-python39
```

Or if you use DOcker Desktop or some other UI, you can run in daemon mode:
```
python39/ $ docker run -p 8888:8888 --name colab-python39 -d colab-python39
```

The startup is successful if you can see a published URL at the end of the logs. This will be important for the next section. If running in daemon mode, you can examine the logs from the Docker Desktop logs tab.
```
[C 15:57:56.408 NotebookApp] 
    
    To access the notebook, open this file in a browser:
        file:///root/.local/share/jupyter/runtime/nbserver-1-open.html
    Or copy and paste one of these URLs:
        http://0e8b0aa9d264:8888/?token=XXX
     or http://127.0.0.1:8888/?token=XXX
```

### Connecting Runtimes
_*NOTE*_: Before you begin this section, you will need to start your container and find the connection URL (i.e. the URL with the token query parameter).

Google Colab has well documented instructions for connecting to a local runtime [here](https://research.google.com/colaboratory/local-runtimes.html).

As of this writing, you can drop down a menu in the upper right of a Colab notebook and select `Connect to a local runtime`. This produces a modal in which you can paste your connection URL (including the token query parameter) and click `Connect`.

#### Common Errors
I use Brave as my default browser and the out-of-the-box security settings do not allow a Colab notebook to connect to a local runtime. As a workaround, I've successfully used Firefox.
