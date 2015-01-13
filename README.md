Bitcoin Core v0.10.0.0 with addrindex (Binaries)
==============================================

##Q&A

**NOTICE**:
* On Jan 9, 2015 Counterparty started reccommending deterministically built Bitcoin Core 0.10.0 binaries from [this Github repo](https://github.com/btcdrak/bitcoin/releases/tag/addrindex-0.10.0). 

**Q: What's this?** 

A: Various binaries of Bitcoin Core 0.10.0 with jmcorgan addrindex patch. It's very similar to what I put on the other page with binaries I built [here](https://github.com/rippler/btc-jmcorgan-addrindex-v0.9.2.0-fca268c-beta), but the only difference is that Bitcoin Core is v0.10.0.

These can be used with [counterpartyd](https://github.com/CounterpartyXCP/counterpartyd) for Windows, Debian and Raspbian, but they can also be used stand-alone (but if you wanted to do that you'd probably want to download Bitcoin Core from the usual place like bitcoin.org rather than have a patched version).

**Q: Where did source code come from and how did you build the binaries?**  

A: It's the current (as of Jan 12, 2014) bitcoin master branch with jmcorgan's addrindex patch. You can get it this way here:
`git clone --branch=addrindex https://github.com/btcdrak/bitcoin bitcoin-core-0.10.0-addrindex`.
* Windows binaries were built **with and without** GUI and/or wallet support, using a 32-bit version of MinGW on Windows 7 x64.
* Ubuntu and Raspbian bianaries were all built without wallet support, using the same approach that the Counterparty Federated Node setup script uses for Bitcoin Core 0.9.3 with necessary minor adjustments required for v0.10.0.

* Ingridients used on Windows: Boost 1.57.0, BerkelyDB 4.8.30, GMPlib 6.0.0a, libpng 1.6.14, OpenSSL 1.0.1j, Miniupnpc-1.9.20141128, Protocol Buffers 2.6.1, QRencode 3.4.4, Qt 5.3.2, gcc v4.9.2.

**NOTE**: Because of issues following a recent OpenSSL update (see [this](http://sourceforge.net/p/bitcoin/mailman/message/33221963/) discussion), extra caution is warranted. At the moment latest OpenSSL binaries haven't been released for Windows, Ubuntu 14.04 (Stable) and Raspbian (Stable). Bitcoin Core 0.10.0-rc2 (below it's available for Debian) supposedly can work with latest OpenSSL fixes.

**Q: Is it stable?**  

A: Counterparty daemon (`counterpartyd`) now officially supports this version. As mentioned at the very top, officialy reccommended builds are the same version, but come from the same Github repo that hosts the patched source.

**Q: Any gotchas?**  

A: Yes, in terms of Bitcoin Core. This version has important differences, as the indexes aren't the same so if you want to play with this one, it's best to use a different data directory from your current v0.x (if you want to keep the both). Read the release notes [here](https://github.com/bitcoin/bitcoin/blob/0.10/doc/release-notes.md). The main and obvious practical change is that with v0.10 you must use `bitcoin-cli` to interface with `bitcoind`.

In terms of what this patch adds to Bitcoin Core 0.10.0, there are no gotchas. All addrindex does is it allows `bitcoind` to build another index (of all seen addresses (`addrindex`)) so you can have another index in addition to the built-in, optional transaction index (`txindex`). When you start a regular Bitcoin Core you may (if it's enabled) see the first line, and when you start these binaries you may see the both: 

```
2014-12-26 09:12:09 LoadBlockIndexDB(): transaction index enabled
2014-12-26 09:12:09 LoadBlockIndexDB(): address index enabled
```

If you want to be extra cautious, you can use a walletless version.

##Installation and Removal

###Windows

***Short***: Install a non-patched Bitcoin Core v0.10.0 and then replace its binaries with these patched binaries (or use the Windows installer and manually create config file(s) `bitcoin.conf`.)

***Detailed***:

* Download the archive and unzip it somewhere. You may see up to 4 executables (bitcoin-qt.exe, bitcoind.exe, bitcoin-cli.exe, bitcoin-tx.exe), but you will use only those that you need.
* Install Bitcoin Core 0.10.0 (x86) and choose **not** to start it.
* Make a new subdirectory in your Bitcoin Core install directory and move the original binaries to it (e.g. move `bitcoin-qt.exe`, `bitcoind.exe`, and `bitcoin-cli.exe` to `C:\Program Files (x86)\Bitcoin\original`).
* Make another subdirectory and copy the patched binaries to it (copy **patched** `bitcoin-qt.exe`, `bitcoind.exe`, and `bitcoin-cli.exe` to `C:\Program Files (x86)\Bitcoin\patched`).
* Now overwrite the original files in their original locations (copy the patched `bitcoind.exe` and `bitcoin-cli.exe` to `C:\Program Files (x86)\Bitcoin\daemon` directory and copy `bitcoin-qt.exe` to `C:\Program Files (x86)\Bitcoin\`)
* Start Bitcoin Core as you normally would. 

**NOTE**: If you want to enable `addrindex` make sure your bitcoin.conf has `addrindex=1` in it before you fire it up. If you have parts of the blockchain already downloaded using v0.10.0, add that parameter to your config file first and run bitcoind from CLI with `--reindex` once to let it index addresses from whatever transactions you had downloaded before. `txindex=1` is also necessary. Refer to Counterparty documentation or Community Wiki for details.

How to switch back to the original files or uninstall:

* In case you decide to switch or uninstall, stop the program, find your original files in `C:\Program Files (x86)\Bitcoin\original` and copy them over the patched files in their respective locations.

####Windows Installer

This is the first Bitcoin Core installer I ever built so I'd call it EXPERIMENTAL. I'd recommend it for casual test/dev work. Personally I plan to use it my own testing of `counterpartyd` on Windows once `counterpartyd` can work with it. The installer was built on Windows 7 x64 and it appears to cleanly install and uninstall and starts normally (I haven't tried to download the entire blockchain with it).

Two things, though: (1) Uninstall any Bitcoin Core/Qt that you have prior to installing this one, and (2) When prompted, make sure to read `README.txt` and do **not** accept to run Bitcoin Core (more details in the included `README.txt`, but basically you don't want to start Bitcoin Core without `addrindex` enabled as that would defy the idea of using this installer in the first place).

Known issues: 
* Installer (incorrectly) suggests install path `C:\Program Files (x86)\Bitcoin Addrindex` or somesuch, but the package is still installed correctly (in the same place where Bitcoin Core is supposed to go). It's a mistake in packaging and can be ignored.

###Ubuntu 14.04
####Install
There are various scenarios and some of them can get complicated. I am not going to describe all of them since this is a niche package. **NOTE**: You *must* have OpenSSL 1.0.1k installed (`sudo apt-get update; sudo apt-get upgrade`). (Ubuntu back-ports OpenSSL versions so the fix is contained in their version `1.0.1f-ubuntu2.8`.)
#####Clean Ubuntu 14.04 System
Before you install this package, you need to install some dependencies:
```
sudo apt-get install libboost-chrono-dev libboost-filesystem1.54-dev \
libboost-program-options-dev libboost-python-dev libboost-system1.54-dev \
libboost-system-dev libboost-thread1.54.0 build-essential libdb++-dev \
libboost-test-dev autoconf -y
```
Then install two more dependencies that aren't available in the official repos:
```
wget https://launchpad.net/~bitcoin/+archive/ubuntu/bitcoin/+files/libdb4.8_4.8.30-trusty1_amd64.deb
wget https://launchpad.net/~bitcoin/+archive/ubuntu/bitcoin/+files/libdb4.8%2B%2B_4.8.30-trusty1_amd64.deb
sudo dpkg -i libdb4.8*.deb
```
If you already have Bitcoin Core 0.9.x **with addrindex**, uninstall it first (`sudo apt-get remove` followed by the package name (on Fed Node that is `bitcoin.addrindex` and in case of Bitcoin Core 0.10.0 addrindex beta downloaded from this site, `bitcoin-core-0.10.0`) and you'd remove the latter with `sudo dpkg -r bitcoin-core-0.10.0-addrindex`. Then install this package.
```
sudo dpkg -i bitcoin-core-addrindex_0.10.0-rc2-addrindex_amd64.deb
```
You may want to create these symbolic links (optional, and unnecessary if you've done this before). Federated Node users already have them created by default.
```
sudo ln -sf /usr/local/bin/bitcoind /usr/bin/bitcoind
sudo ln -sf /usr/local/bin/bitcoin-cli /usr/bin/bitcoin-cli
```
#####Federated Node 9.49 with Bitcoin Core 0.9.2 (jmcorgan addrindex)
If you're installing this on a system with Federated Node code, all above dependencies should be present. You should just stop all services that rely on Bitcoin Core, uninstall the current Bitcoin Core (0.9.2) and install Bitcoin Core 0.10.0 addrindex.
```
sudo sv stop counterpartyd-testnet
sudo sv stop bitcoind-testnet
sudo apt-get remove bitcoin.addrindex # uninstalls patched Bitcoin Core 0.9.2-1 installed by Fed Node
sudo dpkg -i bitcoin-core-addrindex_0.10.0-rc2-addrindex_amd64.deb # this package you can download here
```
Now restart your services. Verify everything is fine with `sudo tail -f /home/xcp/.bitcoin-testnet/testnet3/debug.log` (testnet example). 

To install this package on a fresh Fed Node you could also modify setup scripts for Federated Node to install this binary and save you some time.
####Reinstall
If you need to rebuild the Federated Node, you can remove this binary if you will, then repeat installation later as explained above. What happens now is that Federated Node setup script errs when it tries to install v0.9.2 over v0.10.0, and setup continues, so in actuality it is possible to leave v0.10.0 in place (although reinstall cannot be done completely unattended because the error must be acknowledged).
####Uninstall
Stop the service and remove the package: `sudo apt-get bitcoin-core-addrindex`
####Sample configuration file and start, stop commands
* Configuration file
These are intended for the users of stand-alone `counterpartyd`. Federated Node users should leave the existing configuration file in place and shouldn't need to use these commands to start/stop service.
```
$ cat $HOME/.bitcoin/bitcoin.conf
server=1
daemon=1
rpcuser=bitcoinrpc
rpcpassword=8M7PRvxBKxx3oRbKt3VyDUL8rUWzHM6dZBEuHCnk5GAk
txindex=1
addrindex=1
rpcthreads=1000
rpctimeout=300
```
* Start service (run with `-txindex -addrindex -reindex` once if you have existing blockchain that hasn't been indexed before):
```
bitcoind --conf="$HOME/.bitcoin/bitcoin.conf"
```
* Stop service:
```
/usr/local/bin/bitcoin-cli --conf="$HOME/.bitcoin/bitcoin.conf" stop
```

###Raspbian
* Uninstall whatever other version of bitcoin-core or bitcoin-qt you may have
* Make sure you have (at least) these packages: libssl1.0.0, libgcc1, libprotobuf9, libminiupnpc10, libboost-filesystem1.55.0, libboost-chrono1.55.0, libboost-filesystem1.55.0, libboost-program-options1.55.0, libboost-system1.55.0, libboost-thread1.55.0, libboost-test1.55.0. This binary package wasn't built too smartly, so you need to be careful and "manually" ensure these dependencies are satisfied.
* Install this binary with `sudo dpkg -i bitcoin-core-0.10.0_0.10.0-beta-addrindex_armhf.deb`
* Optionally create symbolic links (see the two `sudo ln -sf` rows in the Ubuntu section above)
* Install and uninstall procedure is basically the same as on Ubuntu 14.04: if you have existing blockchain data, run once from the console with `-reindex`, etc. 
* `rpcthreads` and `rpctimeout` could be set to a low value because of the less powerful hardware. In fact I would reccommend to use this only with testnet.

##Downloads

**NOTE:** Some releases have this or that feature, some don't. Read carefully.

###Windows

* Windows (GUI and wallet support): https://www.dropbox.com/s/2d44g4mnanb6jny/bitcoin-core-0.10.0-addrindex-for-windows.zip (`MD5: 102DD17EEEABEE1BE273C7057437DDBE bitcoin-core-0.10.0-addrindex-for-windows.zip`)
* Windows (with GUI, without wallet): https://www.dropbox.com/s/tjbzdyjrs76l90e/bitcoin-core-0.10.0-addrindex-for-windows-with-gui-without-wallet.zip (`MD5: 34FFF54206E3E166765B60B7F952AAD8 bitcoin-core-0.10.0-addrindex-for-windows-with-gui-without-wallet.zip`)
* Windows (without wallet and without GUI): https://www.dropbox.com/s/0aiyubdofbxd5vz/bitcoin-core-0.10.0-addrindex-for-windows-no-wallet-no-gui.zip (`MD5: 86F4FC13E68496FD7F760311794D110C bitcoin-core-0.10.0-addrindex-for-windows-no-wallet-no-gui.zip`)
* Windows Installer (with everything): https://www.dropbox.com/s/kuiulrj68y8vgbf/Bitcoin%20Core%20addrindex%200.10.0.0.msi (`MD5: 7CAEA1BFF7B96107DC6F3FFBECA645EE Bitcoin Core addrindex 0.10.0.0.msi`)

###Linux

* Ubuntu 14.04 (No GUI, with wallet): https://www.dropbox.com/s/d4k6fe4j2x1ueep/bitcoin-core-addrindex_0.10.0-rc2-addrindex_amd64.deb (`SHA256: 772a006bbcb8f59844502bf102992b8af92dff3b6ac2fd9d0e000e94cfa9e540  bitcoin-core-addrindex_0.10.0-rc2-addrindex_amd64.deb`)
* Rasbpian (Debian 8 (Jessie), no GUI, with wallet): https://www.dropbox.com/s/2cafu1ao8sfqhen/bitcoin-core-0.10.0_0.10.0-beta-addrindex_armhf.deb (`SHA256: 7574c041b7eff3690963fd559cddbb16a303901de6a93fb05f4fafdce3f906f3  bitcoin-core-0.10.0_0.10.0-beta-addrindex_armhf.deb`)
