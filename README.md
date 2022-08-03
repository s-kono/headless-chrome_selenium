## 実行サンプル

```sh
$ sudo -i docker run -v ${PWD}/tool:/tool -v ${PWD}/output:/output -v /dev/shm:/dev/shm \
  --net=bridge -h rl8 -it --rm rockylinux:8
```

```sh
[rl8]# curl -L https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm -O \
  && dnf install -y python3 google-chrome-stable_current_x86_64.rpm
[rl8]#
[rl8]# ver=$( rpm -qpi google-chrome-stable_current_x86_64.rpm | grep Version | awk '{print $NF}' ) \
  && mv -v google-chrome-stable_current_x86_64.rpm "google-chrome-stable_${ver}_x86_64.rpm" \
  && pip3 install selenium \
  && pip3 install chromedriver-binary=="${ver}"
[rl8]#
[rl8]# /tool/headless-chrome -o /output/go.html -w 1280,2000 -s /output/go.png -l https://www.e-survey.go.jp/
[rl8]# file /output/*
/output/go.html: HTML document, UTF-8 Unicode text, with very long lines
/output/go.png:  PNG image data, 1280 x 2000, 8-bit/color RGBA, non-interlaced
```

* pip3 install chromedriver-binary はピッタリバージョンが無くて失敗することもよくある
  * その場合は、存在する近しいバージョンをテキトーに指定 (バージョン無指定でも良いかも)
