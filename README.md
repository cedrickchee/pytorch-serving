# PyTorch Serving

A production ready starter pack for creating a lightweight responsive web app for fast.ai [PyTorch](https://pytorch.org/) models using [Starlette](https://www.starlette.io/) framework with [Uvicorn](https://www.uvicorn.org/) ASGI server for model serving.

**Demo**: [PlantDoc - the digital plant doctor](https://plantdoc.ml).

## Starlette

Starlette provides a lightweight collection of tools for building ASGI services.

It includes request and response classes, an ASGI test client, routing, and static files support.

## Deploying on Zeit Now

Deploy your trained models using the [Now](https://zeit.co/now) service from [Zeit](https://zeit.co).

Zeit Now will take care of everything behind the scene by building [Docker](https://www.docker.com/) container using `Dockerfile` and Python `requirements.txt`, so you can push the container to any Docker-compatible Cloud service.

### Quick Guide

This guide comes with a starter app (known as PlantDoc), deploying my plant disease image classification model.

#### One-time setup

**Install Now's CLI (Command Line Interface)**

First, install [Node.js](https://nodejs.org/en/). See this ["How to install Node.js on Ubuntu 18.04"](https://www.digitalocean.com/community/tutorials/how-to-install-node-js-on-ubuntu-18-04) guide.

```sh
# Installing Node.js Using Node.js Version Manager (NVM) if not already installed
$ curl -sL https://raw.githubusercontent.com/creationix/nvm/v0.33.11/install.sh -o install_nvm.sh
$ bash install_nvm.sh
$ source ~/.bashrc
$ nvm install --lts
```

Next, install Now npm package globally:

```sh
$ npm install -g now
```

**Grab starter pack for model deployment**

```sh
$ git clone https://github.com/cedrickchee/pytorch-serving.git
$ cd pytorch-serving
```

#### Per-project setup

**Upload your trained model file**

Upload your trained model file (for example `stage-2.pth`) to a Cloud storage service like Google Drive or Dropbox. Copy the download link for the file. **Note:** the download link is the one which starts the file download directly—and is normally different than the share link which presents you with a view to download the file (use https://rawdownload.now.sh/ if needed).

If you want to just test the deployment initially, you can use my plant disease classification model, you can skip this step, since that model's weights URL is already filled in the starter app.

**Customize the app for your model**

1. Duplicate `modelDefinitionTemplate.json` in the project root directory and rename to `modelDefinition.json`.
2. Open up the file `modelDefinition.json` and update the `modelUrl` value with the URL copied above.
3. In the same file, update the line `"classes": ["blight", "mosaic", "powdery_mildew", "rust"]` with the classes you are expecting from your model.

**Deploy**

On the terminal, make sure you are in the `pytorch-serving` directory, then type:

```sh
$ now

> No existing credentials found. Please log in:
> We sent an email to {username@domain}. Please follow the steps provided
  inside it and make sure the security code matches {Xxxxxxx Aaaaaaaa}.
✔ Email confirmed
> Ready! Authentication token and personal details saved in "~/.now"
```

The first time you run this, it will prompt for your email address and create your Now account for you. After your account is created, run it again to deploy your project.

```sh
$ now --build-env MODEL_URL="https://raw.githubusercontent.com/cedrickchee/pytorch-serving/master/modelDefinitionTemplate.json"

> WARN! You are using an old version of the Now Platform. More: https://zeit.co/docs/v1-upgrade
> Deploying ~/dev/repo/pytorch-serving under {username@domain}
> Synced 1 file (151B) [1s]
> https://pytorch-serving-{xxxxxxx}.now.sh [v1] [in clipboard] (sfo1) [2s]
> Building…
> Sending build context to Docker daemon  22.02kB
> Step 1/11 : FROM python:3.6-slim-stretch
> 3.6-slim-stretch: Pulling from library/python
> a5a6f2f73cd8: Pulling fs layer
> 3a6fba040982: Pulling fs layer
> 973ed4320c0c: Pulling fs layer
> 2e4c0b09f607: Pulling fs layer
> c5f847ace1b0: Pulling fs layer
> 2e4c0b09f607: Waiting
> c5f847ace1b0: Waiting
> 3a6fba040982: Verifying Checksum
> 3a6fba040982: Download complete
> a5a6f2f73cd8: Verifying Checksum
> a5a6f2f73cd8: Download complete
> 973ed4320c0c: Verifying Checksum
> 973ed4320c0c: Download complete
> 2e4c0b09f607: Verifying Checksum
> 2e4c0b09f607: Download complete
> c5f847ace1b0: Download complete
> a5a6f2f73cd8: Pull complete
> 3a6fba040982: Pull complete
> 973ed4320c0c: Pull complete
> 2e4c0b09f607: Pull complete
> c5f847ace1b0: Pull complete
> Digest: sha256:abedac233d506b945b377f3846900b7cebb2f23e724226ee6a59032cb3039057
> Status: Downloaded newer image for python:3.6-slim-stretch
>  ---> ea57895cf3f9
> Step 2/11 : RUN apt update
>  ---> Running in 56e5605b3459
... ... ...
... ... ...
... ... ...
> Removing intermediate container a2b3c22f18ce
>  ---> 0f0d66948e78
> Step 4/11 : ADD requirements.txt requirements.txt
>  ---> bfa505baeaf4
> Step 5/11 : RUN pip install -r requirements.txt
>  ---> Running in 0b00107f5e1b
> Looking in links: https://download.pytorch.org/whl/nightly/cpu/torch_nightly.html
> Collecting torch_nightly (from -r requirements.txt (line 2))
>   Downloading https://download.pytorch.org/whl/nightly/cpu/torch_nightly-1.0.0.dev20181203-cp36-cp36m-linux_x86_64.whl (69.3MB)
> Collecting fastai (from -r requirements.txt (line 3))
>   Downloading https://files.pythonhosted.org/packages/83/89/76829f37bb8ccb52b16ca6142a36fcd4955b5f1d5e36f5aa8cfbbb189a6c/fastai-1.0.33-py3-none-any.whl (135kB)
... ... ...
... ... ...
... ... ...
> Successfully built starlette uvicorn python-multipart bottleneck pyyaml regex httptools idna-ssl ujson dill cytoolz wrapt toolz
> spacy 2.0.16 has requirement regex==2018.01.10, but you'll have regex 2018.11.22 which is incompatible.
> Installing collected packages: torch-nightly, idna, urllib3, chardet, certifi, requests, typing, fastprogress, numpy, bottleneck, cymem, plac, tqdm, murmurhash, toolz, cytoolz, dill, preshed, six, msgpack, wrapt, msgpack-numpy, thinc, ujson, regex, spacy, pytz, python-dateutil, pandas, Pillow, numexpr, scipy, pyparsing, cycler, kiwisolver, matplotlib, pyyaml, torchvision-nightly, fastai, starlette, click, h11, httptools, uvloop, websockets, uvicorn, python-multipart, aiofiles, attrs, async-timeout, idna-ssl, multidict, yarl, aiohttp, dataclasses
> Successfully installed Pillow-5.3.0 aiofiles-0.4.0 aiohttp-3.4.4 async-timeout-3.0.1 attrs-18.2.0 bottleneck-1.2.1 certifi-2018.11.29 chardet-3.0.4 click-7.0 cycler-0.10.0 cymem-2.0.2 cytoolz-0.9.0.1 dataclasses-0.6 dill-0.2.8.2 fastai-1.0.33 fastprogress-0.1.18 h11-0.8.1 httptools-0.0.11 idna-2.7 idna-ssl-1.1.0 kiwisolver-1.0.1 matplotlib-3.0.2 msgpack-0.6.0 msgpack-numpy-0.4.3.2 multidict-4.5.2 murmurhash-1.0.1 numexpr-2.6.8 numpy-1.15.4 pandas-0.23.4 plac-0.9.6 preshed-2.0.1 pyparsing-2.3.0 python-dateutil-2.7.5 python-multipart-0.0.5 pytz-2018.7 pyyaml-3.13 regex-2018.11.22 requests-2.20.1 scipy-1.1.0 six-1.11.0 spacy-2.0.16 starlette-0.9.2 thinc-6.12.0 toolz-0.9.0 torch-nightly-1.0.0.dev20181203 torchvision-nightly-0.2.1 tqdm-4.28.1 typing-3.6.6 ujson-1.35 urllib3-1.24.1 uvicorn-0.3.21 uvloop-0.11.3 websockets-7.0 wrapt-1.10.11 yarl-1.2.6
> Removing intermediate container 0b00107f5e1b
>  ---> 319ce16328fb
> Step 6/11 : COPY app app/
>  ---> 7610b4ca6702
> Step 7/11 : ARG MODEL_URL
>  ---> Running in 9cb7a5434aff
> Removing intermediate container 9cb7a5434aff
>  ---> d32a5c3187fc
> Step 8/11 : RUN wget -O modelDefinition.json $MODEL_URL
>  ---> Running in 57e0ec2411e9
> --2018-12-05 05:47:41--  https://raw.githubusercontent.com/cedrickchee/pytorch-serving/master/modelDefinitionTemplate.json?token=AAI4xa9KRkv8H213JXgCZc7Q3E18gGV4ks5cEJb6wA%3D%3D
> Resolving raw.githubusercontent.com (raw.githubusercontent.com)... 151.101.40.133
> Connecting to raw.githubusercontent.com (raw.githubusercontent.com)|151.101.40.133|:443... connected.
> HTTP request sent, awaiting response... 200 OK
> Length: 312 [text/plain]
> Saving to: ‘modelDefinition.json’
>
>      0K                                                       100% 8.41M=0s
>
> 2018-12-05 05:47:41 (8.41 MB/s) - ‘modelDefinition.json’ saved [312/312]
>
> Removing intermediate container 57e0ec2411e9
>  ---> be65e17b35f8
> Step 9/11 : RUN python app/server.py
>  ---> Running in ba92487eb056
> Downloading from https://drive.google.com/uc?id=1B23_HoLRFaap3jLY0bFi8zgs7ebhAJ_4&export=download
> File written to app/models/stage-2.pth
> Removing intermediate container ba92487eb056
>  ---> 5714c82516cf
> Step 10/11 : EXPOSE 5042
>  ---> Running in b3909439a13e
> Removing intermediate container b3909439a13e
>  ---> 1e984b3f3ed2
> Step 11/11 : CMD ["python", "app/server.py", "serve"]
>  ---> Running in ff4f498cc902
> Removing intermediate container ff4f498cc902
>  ---> 496d9c80ef20
> Successfully built 496d9c80ef20
> Successfully tagged registry.now.systems/now/50fc612c4222c6d2eec16aa93e1691a8f13377c6:latest
> ▲ Storing image
> Build completed
> Verifying instantiation in sfo1
> [0] INFO: Started server process [1]
> [0] INFO: Waiting for application startup.
> [0] INFO: Uvicorn running on http://0.0.0.0:5042 (Press CTRL+C to quit)
> ✔ Scaled 1 instance in sfo1 [1m]
> Success! Deployment ready
```

Every time you deploy with now it'll create a unique **deployment URL** for the app. It has a format of `xxx.now.sh`, and is shown while you are deploying the app. When the **deployment finishes** and it shows _"> Success! Deployment ready"_ on the terminal, type in the terminal:

```sh
$ export NAME='changeme:this-is-your-name-for-the-url'
$ now alias $NAME

> Assigning alias plantdoc to deployment pytorch-serving-{xxxxxxx}.now.sh
> Success! plantdoc.now.sh now points to pytorch-serving-{xxxxxxx}.now.sh [5s]
```

This will alias the above mentioned deployment URL to `$NAME.now.sh`. You can do this everytime after you deployed. With that, you have a single URL for your app.

**Scaling**

By default all deployment goes to sleep after some inactive time. This is not good for the latest version of your app. So do this:

```sh
# You only need to do this once.
$ now scale $NAME.now.sh sfo 1

> Fetched deployment "pytorch-serving-{xxxxxxx}.now.sh" [1s]
> Scale rules for sfo1 (min: 1, max: 1) saved [1s]
> ✔ Scaled 1 instance in sfo1 [818ms]
> Success! Scale state verified [1s]
```

**Test the URL of your working app**

Go to `$NAME.now.sh` in your browser and test your app.

#### Local testing

In case you want to run the app server locally, make these changes to the above steps:

Instead of

```sh
$ now
```

type in the terminal:

```sh
$ python app/server.py serve
```

Go to http://localhost:5042/ to test your app.

## Other Docker Hosted Services

Some Docker managed Cloud platform and services where this starter pack will work:

- https://aws.amazon.com/ecs/
- https://cloud.google.com/cloud-build/docs/
- https://www.digitalocean.com/products/one-click-apps/docker/

## Credits

Thanks to fast.ai for the initial version of this guide.
