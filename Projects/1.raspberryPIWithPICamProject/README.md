# RaspberryPI + PICam Project
Build a RTMP & HLS Live Streaming Service using RaspberryPI, PICam and nginx_rtmp Server

## Used HW
1. RaspberryPI
2. PiCam

## Used SW
1. Docker
2. nginx-rtmp
3. ffmpeg
4. VLC player(for test rtmp)

## How to build Live Streaming Server
- Download shell script to install Docker
  - $ curl -fsSL get.docker.com -o get-docker.sh
- Execute get-docker.sh
  - $ sudo bash get-docker.sh
- Check the Docker Process
  - $ ps auwx|grep docker
- Enable use of docker as a general account
  - $ sudo usermod -aG docker pi
- Download nginx-rtmp docker source
  - $ wget https://github.com/brocaar/nginx-rtmp-dockerfile/archive/master.zip
- Unzip and making 'config' directory in the nginx-rtmp directory
  - $ unzip master.zip
  - $ cd nginx-rtmp-dockerfile-master
  - $ mkdir config
- For setting timezone copy the '/etc/localtime' to 'config' directory
  - $ cp /etc/localtime nginx-rtmp-dockerfile-master/config
- Adjust 'nginx.conf' file(Because not encoding video frame in the server side, video frame encoding in the client side. server's role is just transport.) then copy these file to 'config' directory([Adjusted config file](nginx.conf))
  - $ cp nginx.conf config
- Adjust Dockerfile(Change the docker image based OS to resin/rpi-raspbian)
  - [Adjusted docker image](Dockerfile)
- Build nginx-rtmp
  - $ docker build -t nginx-rtmp
- Check the nginx-rtmp image file is registered correctly
  - $ docker images
- Execute nginx-rtmp docker instance
  - $ sudo docker run --name nginx-rtmp -p 1935:1935 -p 8080:80 -v ${pwd}/config/nginx.conf:/config/nginx.conf -v ${pwd}/config/localtime:/etc/localtime nginx_rtmp
    - Run 2 files copied to 'config' directory by adding them to mapping
- Check the docker process
  - $ docker ps

## How to transmission to using RTMP Protocol
$ sudo raspivid -w 1280 -h 720 -t 0 -b 1500000 -fps 20 -o - | ffmpeg -y -f h264 -i - -c:v copy -an -f flv -rtmp_buffer 100 -rtmp_live live rtmp://{ip address}:1935/live/101
- '101' is the streaming name, you can change what you want

## How to transmission to using HLS Protocol
$ sudo raspivid -w 1280 -h 720 -t 0 -b 1500000 -fps 20 -o - | ffmpeg -y -f h264 -i - -c:v copy -an -f flv -rtmp_buffer 100 -rtmp_live live rtmp://{ip address}:1935/hls/101
- '101' is the streaming name, you can change what you want