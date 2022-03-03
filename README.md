# CoreMedia BLobs

CoreMedia Content Cloud can make use of externally stored blobs (for images, videos, etc.) These must be available over http. This Docker image packages a blob archive in an nginx container that can be run locally to make these blobs available.

# Download blobs from CoreMedia

At the time of this writing, the most recent Blueprint workspace and blob archive was available from: https://releases.coremedia.com/cmcc-11/overview/distributions/2201.1. You might need a login to access this page.

# Building The Image

1. Download the blob archive.
1. Extract the contents into the `blob/` directory, flattening the directory structure and ignoring duplicate files:
  ```
  unzip -d blobs -j -n ~/Downloads/cmcc-11-blueprint-workspace-content-blobs.zip
  ```
1. Run `DOCKER_BUILDKIT=1 docker build -t coremedia-blob:2110.2 .`

# Using the Image

To run the blob server locally with Docker, making it available on port 8080:

```
docker run --rm cmcc-11-blueprint-workspace-content-blobs.zip -p 80:8080
```
