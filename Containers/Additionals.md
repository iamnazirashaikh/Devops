# Intermediate image in Docker

- When you build aWhen you build a Docker image using a Dockerfile, Docker processes each instruction (e.g., RUN, COPY, ADD, CMD, etc.) in the file sequentially.
- For each instruction, Docker creates a new image layer based on the output of that instruction.
- These layers are stacked on top of each other to form the final image.
- Each of these layers is an intermediate image, but Docker doesn't usually retain them after the final image is built unless caching is involved.
- By default, the docker images command hides intermediate images. However, you can view them using the -a (or --all) flag `docker images -a`
-  
## Purpose of Intermediate Images:
- **Caching**: Docker uses intermediate images to optimize builds through caching. If a step in the Dockerfile has already been executed and hasn't changed, Docker can reuse the corresponding intermediate image rather than rebuilding that layer from scratch. This speeds up the build process significantly.
- **Layer Reuse**: Intermediate images (layers) can be shared between multiple images if they use the same base layers, reducing storage space and build time.

## Example:
Consider a simple Dockerfile: 
```bash
FROM ubuntu:20.04
RUN apt-get update
RUN apt-get install -y curl
```

When you build this Dockerfile:

1. Docker first pulls the ubuntu:20.04 image as the base layer.
2. The RUN apt-get update command creates an intermediate image (layer) with the updated package lists.
3. The RUN apt-get install -y curl command creates another intermediate image with curl installed.
4. The final image is a combination of these layers.

## Managing Intermediate Images:

- Automatic Cleanup: Docker usually removes intermediate images automatically after the final image is built to free up space.
- Manual Removal: If you want to remove dangling intermediate images (those not associated with any tagged image), you can use: `docker system prune`

In summary, intermediate images are the temporary layers that Docker creates during the build process. They play a crucial role in optimizing builds through caching and layer reuse.
