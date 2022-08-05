### docker 環境準備サンプル

* pip3 install chromedriver-binary はピッタリバージョンが無くて失敗することもよくある
  * その場合は、存在する近しいバージョンをテキトーに指定 (バージョン無指定でも良いかも)

#### Rocky Linux 版

```sh
$ sudo docker run -v ${PWD}/tool:/tool -v ${PWD}/output:/output -v /dev/shm:/dev/shm \
  --net=bridge -h docker -it --rm rockylinux:8

[docker]# dnf install -y python3 https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm \
  && pip3 install selenium \
  && pip3 install chromedriver-binary=="$( rpm -q google-chrome-stable | awk -F- '{print $4}' )"
```

#### Ubuntu 版

```sh
$ sudo docker run -v ${PWD}/tool:/tool -v ${PWD}/output:/output -v /dev/shm:/dev/shm \
  --net=bridge -h docker -it --rm ubuntu:20.04

[docker]# apt update \
  && apt install -y curl python3-pip \
  && curl https://dl.google.com/linux/direct/google-chrome-stable_current_amd64.deb -O \
  && ( dpkg -i google-chrome-stable_current_amd64.deb || apt --fix-broken install -y ) \
  && pip3 install selenium \
  && pip3 install chromedriver-binary=="$( dpkg -l google-chrome-stable | tail -1 | awk '{print $3}' | cut -d- -f1 )"
```

### 実行サンプル

```sh
[docker]# /tool/headless-chrome -o /output/go.html -w 1280,2000 -s /output/go.png -l https://nettv.gov-online.go.jp/
[docker]# file /output/*
/output/go.html: HTML document, UTF-8 Unicode text, with very long lines
/output/go.png:  PNG image data, 1280 x 2000, 8-bit/color RGBA, non-interlaced
```
