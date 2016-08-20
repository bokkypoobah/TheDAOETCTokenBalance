# TheDAOETCTokenBalance
Scripts and data to compute the balances of The DAO token holders on the Ethereum Classic chain

The Goodies plan to return the ethers (ETC) they retrieved from The DAO and it's child DAOs on the Ethereum Classic chain and distribute the ETCs to The DAO token holders (DTH) based on the DTH's holding just prior to the hard-fork at block 1,919,999 - see [The White Hats and DAO Wars: Behind the Scenes](https://blog.bity.com/2016/08/13/the-white-hats-and-dao-wars-behind-the-scenes/) and [ETC withdraw contract to be reviewed](https://www.reddit.com/r/ethereum/comments/4yhz8h/etc_withdraw_contract_to_be_reviewed/).

Some DTH have stated that they would prefer to have the ETCs distributed according to the current DTH's holding - see [Reddit - The White Hats and DAO Wars: Behind the Scenes](https://www.reddit.com/r/ethereum/comments/4xlxd3/the_white_hats_and_dao_wars_behind_the_scenes/). I've computed the change in the token balances both at block 1,919,999 and the latest block when I started my script running - block 2,074,689 . There has been some movement in the token balances. This could be due to the replaying of transfer events on the hard-forked blockchain, or through ETC executed transfers (although no exchanges have offered any trading of The DAO tokens on the Classic chain). The data is in the balances file if anyone wants to analyse these post-fork transfers on the Classic chain.

<br />

---

## Summary
The [getTheDAOTokenBalance](https://github.com/bokkypoobah/TheDAOETCTokenBalance/blob/master/getTheDAOTokenBalance) script extracts all `CreatedToken` events in [theDAOTokenBalance_20160819_155742UTC_creation.txt](https://github.com/bokkypoobah/TheDAOETCTokenBalance/blob/master/theDAOTokenBalance_20160819_155742UTC_creation.txt) and all `Transfer` events in [theDAOTokenBalance_20160819_155742UTC_transfer.txt](https://github.com/bokkypoobah/TheDAOETCTokenBalance/blob/master/theDAOTokenBalance_20160819_155742UTC_transfer.txt). All addresses from the `CreatedToken` and `Transfer` events then have their The DAO balance checked at block 1,919,999. The balances of all these addresses match the queried balances from Goodies' `DAOBalanceSnapShot` contract. The results are in [theDAOTokenBalance_20160819_155742UTC_balance.txt](https://github.com/bokkypoobah/TheDAOETCTokenBalance/blob/master/theDAOTokenBalance_20160819_155742UTC_balance.txt) with the spreadsheet version in [theDAOTokenBalance_20160815_144633UTC_balance.xlsx](https://github.com/bokkypoobah/TheDAOETCTokenBalance/blob/master/theDAOTokenBalance_20160815_144633UTC_balance.xlsx). 

The summary statistics at the bottom of the _balances text file follows:

    Stats	nonZeroAccounts	22092
    Stats	daosPreHardForkTotal	11538165987024671407837618.0000	1153816598.7024669647216797
    Stats	daosPreHardForkContractTotal	11538165987024671407837618.0000	1153816598.7024669647216797
    Stats	daosCurrentTotal	11538165381458511407837618.0000	1153816538.1458511352539062


The number of non-zero accounts and the `daosPreHardForkTotal` matches the 22092 figure and the 11538165987024671407837618 `totalSupply` from the Goodies' `DAOBalanceSnapShot` contract. The Goodies' `DAOBalanceSnapShot` accurately represent the balances of DTH's accounts at the pre-hard-fork block 1,919,999. 

<br />

---

## Goodies' `DAOBalanceSnapShot` Contract
From [ETC withdraw contract to be reviewed](https://www.reddit.com/r/ethereum/comments/4yhz8h/etc_withdraw_contract_to_be_reviewed/), the [DAOBalanceSnapShot](https://github.com/BitySA/whetcwithdraw/blob/master/daobalance/dao_balance_snapshot.sol) contract has been deployed to the ETC chain at [0x180826b05452ce96e157f0708c43381fee64a6b8](http://unforked.info/addr/0x180826b05452ce96e157f0708c43381fee64a6b8). This contract provides the balance for each DTH account just before the hard-fork block 1,919,999. The ABI for this contract is `[{"constant":true,"inputs":[],"name":"totalSupply","outputs":[{"name":"","type":"uint256"}],"type":"function"},{"constant":false,"inputs":[],"name":"seal","outputs":[],"type":"function"},{"constant":true,"inputs":[],"name":"totalAccounts","outputs":[{"name":"","type":"uint256"}],"type":"function"},{"constant":true,"inputs":[{"name":"","type":"address"}],"name":"balanceOf","outputs":[{"name":"","type":"uint256"}],"type":"function"},{"constant":false,"inputs":[{"name":"data","type":"uint256[]"}],"name":"fill","outputs":[],"type":"function"},{"constant":true,"inputs":[],"name":"owner","outputs":[{"name":"","type":"address"}],"type":"function"},{"constant":true,"inputs":[],"name":"sealed","outputs":[{"name":"","type":"bool"}],"type":"function"},{"inputs":[],"type":"constructor"}]`. 

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

<br />

---

## Require Full Classic Blockchain
Note that to run this script you will need the full ETC blockchain including intermediate states, not the version downloaded using the `geth --fast --oppose-dao-fork`. The reason for this is that the `debug.traceTransaction(...)` call is used to extract the owner in The DAO token creation process as the origin account could be an exchange (proxy) or an account instructing a wallet contract to purchase The DAO tokens.

If you have already downloaded a `--fast` version, you can `geth export chaindatafile`, rename `.ethereum/chaindata` and `geth import chaindatafile` to rebuild the intermediate states. Note that it may be faster to just re-sync the whole blockchain.

<br />

Enjoy. (c) BokkyPooBah 2016. The MIT licence.
