## Mozc UT2 Dictionary
##### 20171002
Mozc NEologd UT Dictionary is [here](http://www.geocities.jp/ep3797/mozc-neologd-ut.html).  
Mozc UT Dictionary (Discontinued) is [here](http://www.geocities.jp/ep3797/mozc-ut.html).

\
I lost a disk partition that includes tools for making mozc-ut dictionary.  
I used yahoo or google's "hit numbers" to sort words in mozc-ut1,  
but I can't do it again.  
They don't provide free fast search API now.  
I wrote mozc-ut2 from scratch.  
I splitted Wikipedia's articles into 1 million files and got hit numbers by [Hyper Estraier](http://fallabs.com/hyperestraier/).  
mozc-ut2 will add over 500,000 words.

#### Default entries
My big thanks go to the authors/maintainers.

* alt-cannadic  
* neologd ([mecab-user-dict-seed](https://github.com/neologd/mecab-ipadic-neologd/tree/master/seed))
* hatena keywords  
* SKK-JISYO.L  
* EDICT  
* station names from [Wikipedia](https://ja.wikipedia.org/wiki/日本の鉄道駅一覧)  
* katakana-English dictionary generated from EDICT  
Type "いんたーねっと" and press space ⇨ Internet  
If you don't want to use it, run  
```
$ /usr/lib/mozc/mozc_tool --mode=config_dialog
```
and uncheck "Katakana to English conversion" in "Dictionary" tab.  
* Japanese names (I wrote it)

#### Optional entries (See "Advanced: Add optional entries")
* English-Japanese dictionary generated from Japanese WordNet  
Press Caps Lock, type "dolphin" and press Tab.  
![](images/mozc/mozc_01.png)
If you need a text dictionary file, check [this](wordnet-ejdic-ut_01.html) page.
* niconico daihyakka IME dictionary

#### License
* altcanna, jinmei, skk: GPL
* neologd: [Apache License, Version 2.0](https://github.com/neologd/mecab-ipadic-neologd/blob/master/README.md)  
* hatena: [free for personal use](http://developer.hatena.ne.jp/ja/documents/keyword/misc/catalog)
* edict: [CC-BY-SA 3.0](http://www.edrdg.org/edrdg/licence.html)
* ekimei: [CC-BY-SA 3.0](https://ja.wikipedia.org/wiki/Wikipedia:ウィキペディアを二次利用する)
* zip code: [public domain](http://www.post.japanpost.jp/zipcode/dl/readme.html)
* Japanese WordNet: http://nlpwww.nict.go.jp/wn-ja/license.txt
* niconico: unknown
* my ruby/shell scripts: GPL

\
I think we can redistribute hatena's yomigana-hyouki pairs,  
but I can't believe we can redistribute niconico's pairs.  
If you want to make redistributable mozc-ut,  
don't uncomment #NICODIC="true" in generate-dictionary.sh.

#### Download
https://osdn.net/users/utuhiro/pf/utuhiro/files/  
Patched source code:  
mozc-ut2-ver.date.tar.xz  
Patch:  
mozcdic-ut2-date.tar.bz2

#### Install
See mozc's official [Build Instructions](https://github.com/google/mozc#build-instructions).  
If you are using Arch Linux (tested on Antergos Linux),  
you can make and install packages as follows:  
```
$ mkdir mozc-tmp
$ mv mozc-ut2-ver.date.tar.xz mozc-tmp/
$ cd mozc-tmp/
$ tar xf mozc-ut2-ver.date.tar.xz
$ cp mozc-ut2-ver.date/PKGBUILD .
$ makepkg -f
$ makepkg -i
```

#### Advanced: Add optional entries
1. Get the latest Mozc.  
```
$ mkdir mozc-tmp
$ mv mozcdic-ut2-date.tar.bz2 mozc-tmp/
$ cd mozc-tmp/
$ tar xf mozcdic-ut2-date.tar.bz2
$ cd mozcdic-ut2-date/src/
$ sh get-latest-mozc.sh
$ mv mozc-ver.tar.bz2 ../..
$ cd ..
```

1. Choose optional entries.  
```
$ leafpad generate-dictionary.sh
```
If you want to use an English-Japanese dictionary, uncomment the following line.
```
#EJDIC="true"
```
If you want to use a niconico dictionary, uncomment the following line.  
```
#NICODIC="true"
```
1. Generate mozc-ut.  
You need ruby > 1.9.
```
$ ./generate-dictionary.sh
```

#### Advanced: Refresh hit numbers with the latest Japanese Wikipedia articles
* You need 35GB disk space (use SSD) and it will take 8 hours.  
* This will download the latest edict/hatena/niconico/skk-jisyo files, and  
refresh hit numbers with the latest Japanese Wikipedia articles.  

1. Install ruby and gcc-5.4.0.  
estcmd built with gcc-7.2.0 caused segfault.  
I sent mails to the author, but I couldn't get a reply.  
```
$ pacman -S ruby gcc5
```

1. Install QDBM and Hyper Estraier.  
I use [Hyper Estraier](http://fallabs.com/hyperestraier/) to get hit numbers.  
```
$ wget http://fallabs.com/qdbm/qdbm-1.8.78.tar.gz
$ tar xf qdbm-1.8.78.tar.gz
$ cd qdbm-1.8.78/
$ ./configure --prefix=/usr --enable-zlib
$ make -j4 CC=/usr/bin/gcc-5
$ sudo make install
$ wget http://fallabs.com/hyperestraier/hyperestraier-1.4.13.tar.gz
$ tar xf hyperestraier-1.4.13.tar.gz
$ cd hyperestraier-1.4.13/
$ ./configure --prefix=/usr --enable-zlib
$ make -j4 CC=/usr/bin/gcc-5
$ sudo make install
$ cd ../..
```

1. Put mozcdic-ut2 into mozc-tmp.  
```
$ mkdir -p mozc-tmp
$ mv mozcdic-ut2-date.tar.bz2 mozc-tmp/
$ cd mozc-tmp/
$ tar xf mozcdic-ut2-date.tar.bz2
```

1. Get alt-cannadic.  
Get [alt-cannadic-110208.tar.bz2](https://osdn.net/projects/alt-cannadic/downloads/50881/alt-cannadic-110208.tar.bz2/).
```
$ mv alt-cannadic-110208.tar.bz2 mozcdic-ut2-date/alt-cannadic/
```

1. Change SEEDVER of mecab-user-dict-seed.  
Check [mecab-user-dict-seed.yyyymmdd.csv.xz](https://github.com/neologd/mecab-ipadic-neologd/tree/master/seed) and  
change SEEDVER in neologd/generate-dictionary.sh.  
```
$ cd mozcdic-ut2-date/neologd/
$ leafpad generate-dictionary.sh
```
Change SEEDVER.  

1. Change MOZCVER and DICVER.  
```
$ cd ../
$ leafpad generate-dictionary.sh
```
Change MOZCVER and DICVER.  
```
$ cd src/
$ leafpad generate-release.sh
```
Change DICVER.  

1. Refresh hit numbers with the latest Japanese Wikipedia articles.  
```
$ sh update-dictionary.sh
```

\
[HOME](http://www.geocities.jp/ep3797/index.html)
