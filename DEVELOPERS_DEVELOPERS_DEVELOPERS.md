# building and using locally

The `ciclops` GitHub Action runs using a Docker container that encapsulates the
Python script that was developed to do the CI test analysis.

Build the container with

``` shell docker build -t gh-ciclops . ```

To run it, you will need to mount the local directory where you have the JSON
artifacts onto the container. You can do that with the `--mount` command-line
option to `docker`.

The Dockerfile provides an `ENTRYPOINT` for the python script where the magic
happens, and a `CMD` with the default options to give to the script.

**NOTE**: by convention, we assume the directory where the tests are run will be
mounted into `/ported` in the docker container.

To run the Python script locally, assuming the test that holds the JSON
artifacts is `./test-artifacts`, you would run:

``` shell
docker run -it --mount src="$(pwd)",target=/ported,type=bind  gh-ciclops --dir=/ported/test-artifacts
```
