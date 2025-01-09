# Dependency track

```cmd
sudo mkdir -p /data/dependency-track/data

sudo docker run -d \
  --name dependency-track \
  -p 9009:8080 \
  -v /data/dependency-track/data:/data \
  dependencytrack/apiserver
```
