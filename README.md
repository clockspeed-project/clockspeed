Clockspeed Yumekawa
===================
[![Build Status](https://travis-ci.org/clockspeed-project/clockspeed.svg?branch=master-v0.16.3)](https://travis-ci.org/clockspeed-project/clockspeed)
![GitHub All Releases](https://img.shields.io/github/downloads/clockspeed-project/clockspeed/total)

https://clockspeed.org


The Meaning of Yumekawa
-----------------------
Clockspeed's first node software is called `Yumekawa (夢川)`. It can be translated in some ways.
- "Yume (夢)" means dream and "Kawa (川)" means river. So its `Dream River` in japanese.
- The second letter "Kawa" stands for "Kawaii (可愛い)". In this case the meaning is `Dreamy Cute`.
- Also Yumekawa is replaces the word `Core` (ie: Bitcoin Core). We think it sounds a bit centralized.


License
-------
Clockspeed Yumekawa is released under the terms of the MIT license. See [COPYING](COPYING) for more
information or see https://opensource.org/licenses/MIT.
- Copyright (c) 2009-2010 Satoshi Nakamoto
- Copyright (c) 2009-2018 The Bitcoin Core developers
- Copyright (c) 2013-2019 Alexander Peslyak - Yespower 1.0.1
- Copyright (c) 2016-2018 The Zcash developers - DigiShieldZEC
- Copyright (c) 2018-2020 The Clockspeed Yumekawa developers


Minimum Requirement
-------------------
- CPU: 1 Core
- RAM: 2048 MB (at least 3 GB [swap](https://github.com/clockspeed-project/doc/blob/master/swap.md))
- DISK: HDD 5 GB


Depends on Bitcoin Core
-----------------------
Exactly the same as dependencies of [Bitcoin Core v0.16.3](https://github.com/bitcoin/bitcoin/tree/49e34e288005a5b144a642e197b628396f5a0765).

- Debian 10 (Recommended, No PPA)
```bash
sudo apt-get install -y \
software-properties-common build-essential libtool autotools-dev automake pkg-config \
libssl-dev libevent-dev bsdmainutils libboost-all-dev \
libminiupnpc-dev libzmq3-dev libqt5gui5 libqt5core5a \
libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev \
protobuf-compiler libqrencode-dev help2man
```

- PPA is *only* for Ubuntu. No `libdb4.8-dev` and `libdb4.8++-dev` packages on Debian.

- <details><summary>Old Ubuntu</summary>

  * Ubuntu 16.04
  ```bash
  sudo add-apt-repository -y ppa:bitcoin/bitcoin && \
  sudo apt-get update && \
  sudo apt-get install -y \
  libdb4.8-dev libdb4.8++-dev \
  software-properties-common build-essential libtool autotools-dev automake pkg-config \
  libssl-dev libevent-dev bsdmainutils libboost-all-dev \
  libminiupnpc-dev libzmq3-dev libqt5gui5 libqt5core5a \
  libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev \
  protobuf-compiler libqrencode-dev help2man
  ```

  * Ubuntu 18.04+
  ```bash
  sudo add-apt-repository -y ppa:luke-jr/bitcoincore && \
  sudo apt-get update && \
  sudo apt-get install -y \
  libdb4.8-dev libdb4.8++-dev \
  software-properties-common build-essential libtool autotools-dev automake pkg-config \
  libssl-dev libevent-dev bsdmainutils libboost-all-dev \
  libminiupnpc-dev libzmq3-dev libqt5gui5 libqt5core5a \
  libqt5dbus5 qttools5-dev qttools5-dev-tools libprotobuf-dev \
  protobuf-compiler libqrencode-dev help2man
  ```
</details>


Build
-----
- Debian 10+ (Recommended, No PPA)
```bash
./autogen.sh && \
./contrib/install_db4.sh `pwd` && \
export BDB_PREFIX=$PWD/db4 && \
./configure BDB_LIBS="-L${BDB_PREFIX}/lib -ldb_cxx-4.8" BDB_CFLAGS="-I${BDB_PREFIX}/include" && \
make -j$(nproc) && \
make check -j$(nproc)
```

- (optional) Following can be deleted `rm -rf db4/ && rm -f db-4.8.30.NC.tar.gz`

- <details><summary>Old Ubuntu</summary>

  * Ubuntu 16.04+
  ```bash
  ./autogen.sh && \
  ./configure && \
  make -j$(nproc) && \
  make check -j$(nproc)
  ```
</details>


Options after Build
-------------------
- (optional) Reduce binary size using strip (about 90% file size reduction)
```bash
strip ./src/clockspeed-cli && \
strip ./src/clockspeedd && \
strip ./src/qt/clockspeed-qt && \
strip ./src/clockspeed-tx && \
strip ./src/test/test_clockspeed
```

- (optional) After bump version on `configure.ac`, update binary docs (manpages) using help2man `.1` files
```bash
make -j$(nproc) && ./contrib/devtools/gen-manpages.sh
```

- (optional) build for Windows and OSX you may need `--disable-shared` option with make.

- (optional) Add seeds/nodes from [DNSSEED](https://github.com/clockspeed-project/clockspeed-seeder)  
  https://github.com/clockspeed-project/clockspeed/tree/master-v0.16.3/contrib/seeds


Unit Test
---------
All Clockspeed Yumekawa developers should execute this unit test. Some updates may break these tests in some occasions.

- Test All
```bash
./src/test/test_clockspeed test_bitcoin --log_level=test_suite
```

- (optional) Test Partially: ie `blockencodings_tests`
```bash
./src/test/test_clockspeed test_bitcoin --log_level=test_suite --run_test=blockencodings_tests
```

- (optional) Test QT (GUI)
```bash
./src/qt/test/test_clockspeed-qt
```


Run
---
The options `-rpcuser`, `-rpcpassword`, and `-printtoconsole` are optional. `server=1` needed by RPC servers or cpuminer when solo-mining.

- Mainnet: debug mode: `net` for Network
  > ./src/qt/clockspeed-qt -server=1 -rpcuser=rpcuser -rpcpassword=rpcpassword **-debug=net** -printtoconsole

- Testnet
  > ./src/qt/clockspeed-qt **-testnet**

- Regtest
  > ./src/qt/clockspeed-qt **-regtest**

- Reference  
  https://en.bitcoin.it/w/index.php?title=Running_Bitcoin&oldid=66644


CLI
---
- `-prunedebuglogfile`: Prune (limit) filesize of debug.log
  > ./src/qt/clockspeed-qt -prunedebuglogfile

  > 2020-09-15 19:41:34 DEBUG.LOG PRUNED at 10000063


Known Issues
------------
- Transaction too large:
  * This is a part of BTC, and hopefully will be fixed in next *Taproot* Softfork.
- Slow update balance on wallet:
  * This slow is a part of BTC.
  * Update total balance *every minute (12 blocks)* interval.
  * A workaround at this moment. [source](https://github.com/clockspeed-project/clockspeed/commit/72436c90b29844cf507895df053103f9b6840776#diff-2e3836af182cfb375329c3463ffd91f8)
- Poor performance on ARM CPUs (32/64-Bit):
  * No ARM optimization for Yespower yet.
- Poor performance on 32-Bit OS:
  * No SSE2 optimization for Yespower yet. [source](https://github.com/clockspeed-project/clockspeed/blob/d977987a83aba115d50a9130f0d7914330d1bc75/src/crypto/yespower-1.0.1/yespower-opt.c#L59)
- Slow startup on low memory machines:
  * Startup can take up to some hours on 1cpu 1024ram (+swap 3GB) VPS.
  * Workaround is just increase RAM at least 2 GB.


Release Process using GITIAN
----------------------------
- All Clockspeed Yumekawa developers should do following GITIAN release process. It's the safest way to distribute binaries to people.
- Please use GITIAN release with checking PGP signature, or compile it yourself on your own machine.

https://gist.github.com/cryptozeny/3501c77750541208b9dd1a9e9719fc53
