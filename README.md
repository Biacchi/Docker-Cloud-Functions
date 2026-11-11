# Docker-Cloud-Functions

A tiny experiment: package a Python script in a Docker image so it runs the same way anywhere. The only script so far is `scripts/hello.py`, which prints "hello world". Big things have small beginnings.

It is an early learning project. The idea was to prepare an image that could later run Google-related data scripts (Sheets, BigQuery) like a cloud function would.

## How the Dockerfile works

- Starts from `python:3.12-slim`
- Installs the packages in `requirements.txt` (gspread, google-auth, BigQuery client, Google API client, pandas, pyarrow). `hello.py` does not use any of them yet; they are there for the future scripts.
- Copies the repo into `/app`
- Uses `python` as the `ENTRYPOINT`, so whatever you pass after the image name is the script to run

## How to run

Build the image:

```
docker build -t biacchi/docker-cloud-functions:1.0 .
```

Run a script (mounting the current folder so you can edit scripts without rebuilding):

```
docker run --rm -v ${PWD}:/app biacchi/docker-cloud-functions:1.0 scripts/hello.py
```

(`${PWD}` is PowerShell syntax. On bash use `$(pwd)`.) You should see `hello world`.

My original notes ran the container by image ID instead of by tag; the tag above is the readable version of the same thing.

## What is in here

```
Dockerfile        the image recipe
requirements.txt  pinned Python dependencies
scripts/hello.py  the one and only script
.dockerignore     keeps junk out of the build context
```

## What I learned

- Writing a Dockerfile with an `ENTRYPOINT` so the image behaves like a command
- Bind-mounting code into a container
- Pinning dependency versions

## Known limitations

- Only a hello-world script, and the Google libraries are installed but unused.
- No credentials are included. Any future script that talks to Google would need them supplied at runtime (never baked into the image).
- Pinned versions are from 2024 and have not been refreshed.
- No LICENSE file yet, so by default all rights are reserved.
