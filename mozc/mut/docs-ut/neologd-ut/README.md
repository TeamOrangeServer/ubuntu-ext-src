## Mozc NEologd UT Dictionary

20171007

Mozc UT2 Dictionary is [here](mozc-ut2.html). Mozc UT Dictionary (Discontinued) is [here](mozc-ut.html).

### Overview

I modified mecab-ipadic-NEologd's "[user-dict](https://github.com/neologd/mecab-ipadic-neologd/tree/master/seed)" for Mozc. The "user-dict" will be updated twice per week(!) (Monday and Thursday) automatically. mecab-ipadic-NEologd was written by Toshinori Sato (@overlast). mozcdic-neologd-ut will add over 800,000 words.

### Download

<https://osdn.net/users/utuhiro/pf/utuhiro/files/>

- Patched source code: mozc-neologd-ut-ver.date.tar.xz

- Patch: mozcdic-neologd-ut-date.tar.bz2

### Install

See mozc's official [Build Instructions](https://github.com/google/mozc#build-instructions). If you are using Arch Linux (tested on Antergos Linux), you can make and install packages as follows:

```
mkdir mozc-tmp
mv mozc-neologd-ut-ver.date.tar.xz mozc-tmp/
cd mozc-tmp/
tar xf mozc-neologd-ut-ver.date.tar.xz
cp mozc-neologd-ut-ver.date/PKGBUILD .
makepkg -f
makepkg -i
```

### Advanced: Generate your mozc-neologd-ut

1. Get the latest Mozc.

  ```
  mkdir mozc-tmp
  mv mozcdic-neologd-ut-date.tar.bz2 mozc-tmp/
  cd mozc-tmp/
  tar xf mozcdic-neologd-ut-date.tar.bz2
  cd mozcdic-neologd-ut-date/src/
  sh get-latest-mozc.sh
  mv mozc-ver.tar.bz2 ../..
  cd ..
  ```

2. Get the latest mecab-user-dict-seed.date.csv.xz

  <https://github.com/neologd/mecab-ipadic-neologd/tree/master/seed>

3. Put it into mecab-ipadic-neologd/

  ```
  rm mecab-ipadic-neologd/mecab-user-dict-seed.date.csv.xz
  mv mecab-user-dict-seed.date.csv.xz mecab-ipadic-neologd/
  ```

4. Change version numbers.

  ```
  leafpad generate-dictionary.sh
  ```

  Change "MOZCVER", "DICVER" and "REVISION" numbers.

5. Generate mozc-neologd-ut.

  ```
  ./generate-dictionary.sh
  ```

[HOME](http://www.geocities.jp/ep3797/index.html)
