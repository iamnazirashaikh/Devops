# Intermediate image in Docker

- When you build a Docker image using a Dockerfile, Docker processes each instruction (e.g., RUN, COPY, ADD, CMD, etc.) in the file sequentially.
- For each instruction, Docker creates a new image layer based on the output of that instruction.
- These layers are stacked on top of each other to form the final image.
- Each of these layers is an intermediate image, but Docker doesn't usually retain them after the final image is built unless caching is involved.
- By default, the docker images command hides intermediate images. However, you can view them using the -a (or --all) flag `docker images -a`
- Example: Consider a simple Dockerfile: 
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
   
## Purpose of Intermediate Images:
- **Caching**: If a step in the Dockerfile has already been executed and hasn't changed, Docker can reuse the corresponding intermediate image rather than rebuilding that layer from scratch. This speeds up the build process significantly.
- **Layer Reuse**: Intermediate images (layers) can be shared between multiple images if they use the same base layers, reducing storage space and build time.



## Managing Intermediate Images:

- Automatic Cleanup: Docker usually removes intermediate images automatically after the final image is built to free up space.
- Manual Removal: If you want to remove dangling intermediate images (those not associated with any tagged image), you can use: `docker system prune`

In summary, intermediate images are the temporary layers that Docker creates during the build process. They play a crucial role in optimizing builds through caching and layer reuse.

<hr>

# Dangling Images:

Images that are not tagged and are not referenced by any (running/stopped) containers.
To list dangling images 
```
docker images -f "dangling=true"
```
To remove dangling images
```
docker image prune
```
Remove dangling images without any confirmation
```
docker image prune -f
```

Docker image name format: `repository:tag` example `ubuntu:20.04` where ubuntu is the repository and 20.04 is the tag.
Intermediate layer images which are created during build process are untagged i.e they do not have names

# Difference between dangling image & intermediate image
You have a Dockerfile with several steps.
Docker creates intermediate images for each step of the build process.
Once the build is complete, if some of these intermediate images are not tagged or referenced, they become dangling images.

List dangling images:
```
docker images -f "dangling=true"
```
Remove dangling images:
```
docker image prune
```

Summary
Dangling Images: Are untagged and not referenced by any container, and they can be cleaned up to free disk space.
Intermediate Images: Are temporary images created during the build process of a Docker image, which may become dangling images if not tagged or used after the build.
