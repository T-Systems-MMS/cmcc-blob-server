# CoreMedia Blobs

CoreMedia Content Cloud can make use of externally stored blobs (for images, videos, etc.) These must be available over http. This Docker image packages a blob archive in an nginx container that can be run locally to make these blobs available.

# Download blobs from CoreMedia

At the time of this writing, the most recent Blueprint workspace and blob archive was available from https://releases.coremedia.com/cmcc-11/overview/distributions/2201.1. You might need a login to access this page.

# Building The Image

1. Download the blob archive.
1. Extract the contents into the `blob/` directory, rearranging the blob files into the correct directory structure:
  ```
  ./unpack-blobs-zip ~/Downloads/cmcc-11-blueprint-workspace-content-blobs.zip
  ```
1. Build the image:
  ```
  DOCKER_BUILDKIT=1 docker build -t coremedia-blob:2201.1 ./
  ```
1. (Optionally) Push the image to your registry.

Note that the image will be close to 1 GB in size; make sure your network connection and registry are up to that.

# Using the Image

To run the blob server locally with Docker, making it available on port 8080:

```
docker run --rm -p 80:8080 coremedia-blob:2201.1
```

# Making blobs available in a k8s deployment

When deploying CoreMedia to k8s, you can run this image as a StatefulSet and a Service, to make the blobs available to the Content Server and other components. It is not necessary to expose the Service to the outside, since only CoreMedia components will access them. See the example [k8s.yaml](./k8s.yaml).

When importing content, make sure to specify the blob parameters to `serverimport`:
```
serverimport ... -bloburl http://blob-server/ --blobreferences --threads 5 ...
```
