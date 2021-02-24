Docker-file from this repository is broken!
Please use instructions below.

# openpose-docker
Dockerfile to build the excellent OpenPose software from CMU.

At first, use this guides to install `nvidia-docker`:
1. [Install nvidia-container-toolkit](https://github.com/NVIDIA/nvidia-docker#ubuntu-160418042004-debian-jessiestretchbuster)
2. [Install nvidia-docker 2.0](https://github.com/nvidia/nvidia-docker/wiki/Installation-(version-2.0))

Then get an image:
```
docker pull exsidius/openpose
```

To run the container, use the following commmand (Ubuntu):

```bash
sudo docker run -it -v /home/dimon/openposedata/:/openpose/data/ --net=host -e DISPLAY --runtime=nvidia exsidius/openpose
```

We need to `-v /home/dimon/openposedata/:/openpose/data/` if we want processing any media from filesystem.

Processing images:
```bash
 ./build/examples/openpose/openpose.bin --image_dir ./data --display 0 --write_json ./data/result --write_images ./data/result --face --hand

```

Supports - 
1. CUDA 10
2. CUDnn 7
3. Python 3 (will be 3.7 soon)

To use docker without sudo [Link](https://github.com/sindresorhus/guides/blob/master/docker-without-sudo.md)



Here is the steps, to download COCO model, and save docker image ([how-to-commit-changes-to-docker-image](https://phoenixnap.com/kb/how-to-commit-changes-to-docker-image)):
* run container: `docker run -it --runtime=nvidia exsidius/openpose`
* install wget: `apt-get install wget`
* go to folder, and download COCO-model:
```
cd models/pose/coco/
wget https://github.com/foss-for-synopsys-dwc-arc-processors/synopsys-caffe-models/raw/master/caffe_models/openpose/caffe_model/pose_iter_440000.caffemodel
```
* Как только вы закончите модифицировать новый контейнер, выйдите из него:
`exit`
Предложите системе отобразить список запущенных контейнеров :
`sudo docker ps -a`
Вам потребуется **идентификатор КОНТЕЙНЕРА**, чтобы сохранить изменения, внесенные в существующее изображение. Скопируйте значение идентификатора из вывода.
* Наконец, создайте новое изображение, зафиксировав изменения, используя следующий синтаксис:
`sudo docker commit [CONTAINER_ID] [new_image_name]`
Поэтому в нашем примере это будет:
`sudo docker commit c0d4c6800b5d openpose`
Где *c0d4c6800b5d* находится **идентификатор КОНТЕЙНЕРА** и **openpose** имя нового Image.
* Ваше вновь созданное изображение теперь должно быть доступно в списке локальных изображений. Вы можете проверить, проверив список изображений снова:
`sudo docker images`
