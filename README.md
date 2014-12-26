Bitcoin Core v0.10.0.0 with addrindex (Binaries)
==============================================

##Q&A

**Q: What's this?** A: Various binaries of Bitcoin Core 0.10.0 with jmcorgan addrindex patch. It's very similar to the other page with binaries I built [here](https://github.com/rippler/btc-jmcorgan-addrindex-v0.9.2.0-fca268c-beta), but the only difference is that Bitcoin Core is version 0.10.0.0. If you install one of these binaries with a GUI you'll see `Bitcoin Core version v0.10.0.0-fb4298f (32-bit)` in **Help > About Bitcoin Core**.

Like those binaries, these would be needed for [counterpartyd](https://github.com/CounterpartyXCP/counterpartyd) for Windows, Debian and Raspbian, but they can also be used stand-alone (but if you wanted to do that you'd probably want to download Bitcoin Core from the usual place like bitcoin.org rather than have a patched version).

**Q: Where did source code come from and how did you build the binaries?**  

A: It's the current (as of Dec 25, 2014) bitcoin master branch with jmcorgan's addrindex patch. You can get it this way here:
`git clone --branch=addrindex https://github.com/btcdrak/bitcoin bitcoin-core-0.10.0-addrindex`.
* Windows binaries were built **with and without** GUI and/or wallet support, using a 32-bit version of MinGW on Windows 7 x64
* Ubuntu and Raspbian bianaries were all built without wallet support, using the same approach that the Counterparty Federated Node setup script uses for Bitcoin Core 0.9.3 with necessary minor adjustments required for v0.10.0.

Ingridients used on Windows: Boost 1.57.0, BerkelyDB 4.8.30, GMPlib 6.0.0a, libpng 1.6.14, OpenSSL 1.0.1j, Miniupnpc-1.9.20141128, Protocol Buffers 2.6.1, QRencode 3.4.4, Qt 5.3.2, gcc v4.9.2.

**Q: Is it stable?**  

A: No clue, I just finished building them. Counterparty server (`counterpartyd`) cannot use this version yet, so I have not tried it as of now (Dec 25, 2014) and probably won't until 2-3 weeks later.

**Q: Any gotchas?**  

A: Yes, in terms of Bitcoin Core. This version has important differences. Read the release notes [here](https://github.com/bitcoin/bitcoin/blob/0.10/doc/release-notes.md). 

In terms of what this patch adds to Bitcoin Core 0.10.0, there are no gotchas. All addrindex does is it allows `bitcoind` to build another index (of all seen addresses (`addrindex`)) so you can have another index in addition to the built-in, optional transaction index (`txindex`). When you start a regular Bitcoin Core you may (if it's enabled) see the first line, and when you start these binaries you may see the both: 

```
2014-12-26 09:12:09 LoadBlockIndexDB(): transaction index enabled
2014-12-26 09:12:09 LoadBlockIndexDB(): address index enabled
```


If you want to be extra cautious, you can use a walletless version.

##Installation and Removal

###Windows

***Short***: Install a non-patched Bitcoin Core v0.10 and then replace its binaries with these patched binaries.

***Detailed***:

* Download the archive and unzip it somewhere. You may see up to 4 executables (bitcoin-qt.exe, bitcoind.exe, bitcoin-cli.exe, bitcoin-tx.exe), but you will use only those that you need.
* Install Bitcoin Core 0.10.0 (x86) and choose **not** to start it.
* Make a new subdirectory in your Bitcoin Core install directory and move the original binaries to it (e.g. move `bitcoin-qt.exe`, `bitcoind.exe`, and `bitcoin-cli.exe` to `C:\Program Files (x86)\Bitcoin\original`)
* Make another subdirectory and copy the patched binaries to it (copy **patched** `bitcoin-qt.exe`, `bitcoind.exe`, and `bitcoin-cli.exe` to `C:\Program Files (x86)\Bitcoin\patched`)
* Now overwrite the original files in their original locations (copy the patched `bitcoind.exe` and `bitcoin-cli.exe` to `C:\Program Files (x86)\Bitcoin\daemon` directory and copy `bitcoin-qt.exe` to `C:\Program Files (x86)\Bitcoin\`)
* Start Bitcoin Core as you normally would. 

**NOTE**: If you want to enable `addrindex` make sure your bitcoin.conf has `addrindex=1` in it before you fire it up. If you have parts of the blockchain already downloaded using v0.10.0, add that parameter to your config file first and run bitcoind from CLI with `--reindex` once to let it index addresses from whatever transactions you had downloaded before. `txindex=1` is also necessary. Refer to Counterparty documentation or Community Wiki for details.

How to switch back to the original files or uninstall:

* In case you decide to switch or uninstall, stop the program, find your original files in `C:\Program Files (x86)\Bitcoin\original` and copy them over the patched files in their respective locations.

###Ubuntu 14.04
TODO

###Raspbian
TODO

##Downloads

**NOTE:** Some releases have this or that feature, some don't. Read carefully.

* Windows (GUI and wallet support): https://www.dropbox.com/s/2d44g4mnanb6jny/bitcoin-core-0.10.0-addrindex-for-windows.zip (`MD5: 102DD17EEEABEE1BE273C7057437DDBE bitcoin-core-0.10.0-addrindex-for-windows.zip`)
* Windows (with GUI, without wallet): https://www.dropbox.com/s/tjbzdyjrs76l90e/bitcoin-core-0.10.0-addrindex-for-windows-with-gui-without-wallet.zip (`MD5: 34FFF54206E3E166765B60B7F952AAD8 bitcoin-core-0.10.0-addrindex-for-windows-with-gui-without-wallet.zip`)
* Windows (without wallet and without GUI): https://www.dropbox.com/s/0aiyubdofbxd5vz/bitcoin-core-0.10.0-addrindex-for-windows-no-wallet-no-gui.zip (`MD5: 86F4FC13E68496FD7F760311794D110C bitcoin-core-0.10.0-addrindex-for-windows-no-wallet-no-gui.zip`)
* Ubuntu 14.04: TODO
* Rasbpian (Debian Jesse): TODO
