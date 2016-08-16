# TheDAOETCTokenBalance
Scripts and data to compute the balances of The DAO token holders on the Ethereum Classic chain

There is a plan for the Goodies who have retrieved ethers from The DAO and it's child DAOs on the Ethereum Classic chain to distribute the ethers (ETC) to The DAO token holders (DTH) based on the DTH's holding just prior to the hard-fork at block 1,919,999 - see [The White Hats and DAO Wars: Behind the Scenes](https://blog.bity.com/2016/08/13/the-white-hats-and-dao-wars-behind-the-scenes/). However some DTH would prefer to have the ETCs distributed according to the current DTH's holding - see [Reddit - The White Hats and DAO Wars: Behind the Scenes](https://www.reddit.com/r/ethereum/comments/4xlxd3/the_white_hats_and_dao_wars_behind_the_scenes/).

<br />

---

## Summary
The script extracts all `CreatedToken` events in [theDAOTokenBalance_20160815_144633UTC_creation.zip](https://github.com/bokkypoobah/TheDAOETCTokenBalance/blob/master/theDAOTokenBalance_20160815_144633UTC_creation.zip) and all `Transfer` events in [theDAOTokenBalance_20160815_144633UTC_transfer.zip](https://github.com/bokkypoobah/TheDAOETCTokenBalance/blob/master/theDAOTokenBalance_20160815_144633UTC_transfer.zip). All addresses involved are stored and The DAO balance is check at block 1,919,999. The balances match the figures in **The DAO ETC Token Balance Contract** listed below, when comparing the differences to 9 decimal places in a spreadsheet - the `PreHardForkDAODiff` column is all 0. And from the summary statistics at the bottom of the _balances text file:

    Stats	nonZeroAccounts	23012
    Stats	daosPreHardForkTotal	1153816598.702436
    Stats	daosPreHardForkContractTotal	1153816598.702436

The number of non-zero accounts is slightly different to the 22092 figure from **The DAO ETC Token Balance Contract** below, but to 9 decimal places, the total calculated by evaluating each addresss at block 1,919,999 is the same as the total calculated using the data in **The DAO ETC Token Balance Contract** below.

## The DAO ETC Token Balance Contract
A contract providing The DAO ETC Token Balance has been deployed to the ETC chain at [0x180826b05452ce96e157f0708c43381fee64a6b8](http://unforked.info/addr/0x180826b05452ce96e157f0708c43381fee64a6b8) by @jbaylina. This contract provides the balance for each DTH account just before the hard-fork block 1,919,999. The ABI for this contract is `[{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"name":"","type":"uint256"}],"type":"function"},{"constant":false,"inputs":[],"name":"seal","outputs":[],"type":"function"},{"constant":true,"inputs":[],"name":"totalAccounts","outputs":[{"name":"","type":"uint256"}],"type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"balanceOf","outputs":[{"name":"","type":"uint256"}],"type":"function"},{"constant":false,"inputs":[{"name":"data","type":"uint256[]"}],"name":"fill","outputs":[],"type":"function"},{"constant":true,"inputs":[],"name":"owner","outputs":[{"name":"","type":"address"}],"type":"function"},{"constant":true,"inputs":[],"name":"sealed","outputs":[{"name":"","type":"bool"}],"type":"function"},{"inputs":[],"type":"constructor"}]`. 

At 01:23 15 Aug 2016 UTC, this contract has the attributes Total supply = 11538165987024671407837618, Total accounts = 22092, Sealed = yes. 

<br />

---

## The Script
A script has been written to extract The DAO token and ether balances of DTHs just prior to the hard-fork and currently - see [getTheDAOTokenBalance](https://github.com/bokkypoobah/TheDAOETCTokenBalance/blob/master/getTheDAOTokenBalance). 

The script:
* Extracts all `TheDAO.CreatedToken` events from the creation of The DAO contract in block 1,428,757 Apr-30-2016 01:42:58 AM +UTC until the end of the creation phase in block 1,599,205 May-28-2016 08:59:47 AM +UTC
* Extracts all `TheDAO.Transfer` events from block 1,599,207 May-28-2016 09:00:07 AM +UTC to the current block
* Save a hashmap of the addresses involved in the `TheDAO.CreatedToken` and `TheDAO.Transfer` events
* Query the blockchain at block 1,919,999 just prior to the hard-fork and currently for all the collected addresses for The DAO token balances and ether balances.

I'm now running the data extraction process.

<br />

---

## Require Full Blockchain
Note that to run this script you will need the full ETC blockchain including intermediate states, not the version downloaded using the `geth --fast --oppose-dao-fork`. The reason for this is that the `debug.traceTransaction(...)` call is used to extract the owner in The DAO token creation process as the origin account could be an exchange (proxy) or an account instructing a wallet contract to purchase The DAO tokens.

If you have already downloaded a `--fast` version, you can `geth export chaindatafile`, rename `.ethereum/chaindata` and `geth import chaindatafile` to rebuild the intermediate states. Note that it may be faster to just re-sync the whole blockchain.

<br />

Enjoy. (c) BokkyPooBah 2016. The MIT licence.
