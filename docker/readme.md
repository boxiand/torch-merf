1. Install nvidia-container-runtime:

`sudo apt-get install nvidia-container-runtime`

2. Edit/create the `/etc/docker/daemon.json` with content:
```
{
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
         } 
    },
    "default-runtime": "nvidia" 
}
```
3. Restart docker daemon:

`sudo systemctl restart docker`

4. Build your image (now GPU available during build):

`docker build -t nerfstudio .`

5. Run the container

`docker run --gpus all --rm -it -v ${host workspace}:/workspace nerfstudio`


