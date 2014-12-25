Bitcoin Core v0.10.0 with addrindex (Binaries)
==============================================

##Q&A

**Q: What's this?** A: Various binaries of Bitcoin Core 0.10.0 with jmcorgan addrindex patch. It's very similar to the other page with binaries I built [here](https://github.com/rippler/btc-jmcorgan-addrindex-v0.9.2.0-fca268c-beta), but the only difference is that Bitcoin Core is v0.10.0. 

Like those binaries, these can be used with [counterpartyd](https://github.com/CounterpartyXCP/counterpartyd) for Windows, Debian and Raspbian, but also stand-alone (however if you wanted to do that you'd probably want to download Bitcoin Core from the usual place like bitcoin.org rather than have a patched version).

**Q: Where did source code come from and how did you build the binaries?** A: It's the current (as of Dec 25, 2014) bitcoin master branch with jmcorgan's addrindex patch. You can get it this way here:
`git clone --branch=addrindex https://github.com/btcdrak/bitcoin bitcoin-core-0.10.0-addrindex`.
* Windows binaries were built without wallet support, using a 32-bit version of MinGW
* Ubuntu and Raspbian bianaries were built without wallet support, using the same approach that Counterparty Federated Node setup script uses

**Q: Is it stable?**  A: No clue, I just finished building them. Counterparty server (`counterpartyd`) cannot use this version yet, so I have not tried as of now (Dec 25, 2014) and probably won't know until 2-3 weeks later.

**Q: Any gotchas?** A: Yes, vs. 0.9 there are several important differences. Read the release notes [here](https://github.com/bitcoin/bitcoin/blob/0.10/doc/release-notes.md). Addrindex is added via a patch, but all it does is it allows `bitcoind` to build another index (of all seen addresses (`addrindex`)) so you can have another index in addition to transaction index (`txindex`) which Bitcoin Core has been supporting for a while.

##Installation and Removal

###Windows

* Install Bitcoin Core 0.10.0 (x86) and choose **not** to start it.
* Make a new directory and move the original binaries to it (e.g. in `C:\Program Files (x86)\Bitcoin\original`, with `bitcoin-qt.exe`, `bitcoind.exe`, and `bitcoin-cli.exe`)
* Make a new directory and copy patched binaries to it (e.g. in `C:\Program Files (x86)\Bitcoin\patched`, with a **patched** `bitcoin-qt.exe`, `bitcoind.exe`, and `bitcoin-cli.exe` (these files you get here))
* Now overwrite the original files in their original locations (copy `bitcoind.exe` and `bitcoin-cli.exe` to `C:\Program Files (x86)\Bitcoin\daemon` directory, and copy `bitcoin-qt.exe` to `C:\Program Files (x86)\Bitcoin\`)
* Start Bitcoin Core as you normally would. If you want to enable `addrindex` make sure your bitcoin.conf has `addrindex=1` in it before you fire it up. If you have parts of the blockchain already downloaded using v0.10.0, add that parameter to your config file first and run bitcoind from CLI with `--reindex` once to let it index addresses from whatever transactions you had downloaded before.

How to switch back or uninstall:

* In case you decide to switch or uninstall, stop the program, find your original files in `C:\Program Files (x86)\Bitcoin\patched`, copy them over the patched files and that's it.

###Linux
TODO

###Raspbian
TODO

##Downloads

* Windows: https://www.dropbox.com/s/2d44g4mnanb6jny/bitcoin-core-0.10.0-addrindex-for-windows.zip (`MD5 checksum: 102DD17EEEABEE1BE273C7057437DDBE bitcoin-core-0.10.0-addrindex-for-windows.zip`)
* Ubuntu 14.04: TODO
* Rasbpian (Debian Jesse): TODO
