# PyTorch Serving

A production ready starter pack for creating a lightweight responsive web app for fast.ai [PyTorch](https://pytorch.org/) models using [Starlette](https://www.starlette.io/) framework with [Uvicorn](https://www.uvicorn.org/) ASGI server for model serving.

## Starlette

Starlette provides a lightweight collection of tools for building ASGI services.

It includes request and response classes, an ASGI test client, routing, and static files support.

## Deployment

Deploy your trained models using the Now service from Zeit.

Zeit Now will take care of everything behind the scene by building [Docker](https://www.docker.com/) container using `Dockerfile` and Python `requirements.txt`, so you can push the container to any Docker-compatible Cloud service.

## Docker Hosted Services

Some Docker managed Cloud platform and services where this starter pack will work:

- https://zeit.co/now
- https://aws.amazon.com/ecs/
- https://cloud.google.com/cloud-build/docs/
- https://www.digitalocean.com/products/one-click-apps/docker/
