## 実行サンプル

```sh
$ sudo -i docker run -v ${PWD}/tool:/tool -v ${PWD}/output:/output --net=bridge -h rl8 -it --rm rockylinux:8
```

```sh
[rl8]# dnf install -y vim python3 findutils
[rl8]#
[rl8]# curl -L https://dl.google.com/linux/direct/google-chrome-stable_current_x86_64.rpm -O \
  && ver=$( rpm -qpi google-chrome-stable_current_x86_64.rpm | grep Version | awk '{print $NF}' ) \
  && mv -v google-chrome-stable_current_x86_64.rpm "google-chrome-stable_${ver}_x86_64.rpm" \
  && pip3 install selenium \
  && pip3 install chromedriver-binary=="${ver}"
[rl8]#
[rl8]# rpm -Uvh "google-chrome-stable_${ver}_x86_64.rpm" 2>&1 \
  | grep need \
  | sed 's/(.*//' | awk '{print $1}' | while read -r need; do
    p=$(
      dnf whatprovides "${need}" \
      | grep " : " \
      | grep el8 \
      | head -1 \
      | awk '{print $1}' \
      | sed 's/-[0-9\.:-]\+\.el8.*//' \
    );
    echo >&2 "${p}";
    echo "${p}";
  done \
  | xargs -L 100 dnf install -y \
  && rpm -Uvh "google-chrome-stable_${ver}_x86_64.rpm"
[rl8]#
[rl8]# /tool/headless-chrome -o /output/go.html -w 640,480 -s /output/go.png -l https://nettv.gov-online.go.jp/
[rl8]# file /output/*
/output/go.html: HTML document, UTF-8 Unicode text, with very long lines
/output/go.png:  PNG image data, 640 x 480, 8-bit/color RGBA, non-interlaced
```

pip3 install chromedriver-binary はピッタリバージョンが無くて失敗することもよくあるので、その場合は存在する近しいバージョンをテキトーに指定する
