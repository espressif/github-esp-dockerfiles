# github-esp-dockerfiles
This is the project for the standard Docker files. The images can be reused in GH events mainly on self-hosted runners.

## Building a Docker image
- create a directory in the root of this project with the same name as the future Docker image.
- prepare a `Dockerfile` and place it in this new directory
- create a GH workflow (GH action) in `.github/workflows/build_image_<name_of_your_image>.yml` 

`RECOMMENDATION:` The existing `.yml` workflow file can be copied and edited.

- Edit paths in the workflow file
- Set the following env variables in the workflow file:

| env         | value                         | example                                       |
|-------------|-------------------------------|-----------------------------------------------|
| IMAGE_DIR   | Dockefile directory (context) | ./pyenv_python                                |
| IMAGE_NAME  | full image name               | ghcr.io/${{ github.repository }}/pyenv_python |
| IMAGE_TAG   | image version tag             | v1                                            |
| IMAGE_ARCHS | build for architectures       | linux/amd64,linux/arm,linux/arm64             |

- the image is pushed to the GH packages of this repository only when merged into the `master` branch
- from there it can be used in any other project (images are public)
