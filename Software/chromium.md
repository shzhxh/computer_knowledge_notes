```
gitclone https://chromium.googlesource.com/chromium/tools/depot_tools.git
vim ~/.bashrc	# 把depot_tools加入PATH
source ~/.bashrc

＃ 下载源码，如下方法三选一
mkdir chromium && cd chromium && fetch chromium
git clone --depth 1 https://github.com/chromium/chromium.git
apt-get source chromium-browser

cd chromium
gn gen
```

