# TheDAOETCTokenBalance
Scripts and data to compute the balances of The DAO token holders on the Ethereum Classic chain

There is a plan for the Goodies who have retrieved ethers from The DAO and it's child DAOs on the Ethereum Classic chain to distribute the ethers (ETC) to The DAO token holders (DTH) based on the DTH's holding just prior to the hard-fork at block 1,919,999 - see [The White Hats and DAO Wars: Behind the Scenes](https://blog.bity.com/2016/08/13/the-white-hats-and-dao-wars-behind-the-scenes/). Some DTH would prefer to have the ETCs distributed according to the current DTH's holding - see [Reddit - The White Hats and DAO Wars: Behind the Scenes](https://www.reddit.com/r/ethereum/comments/4xlxd3/the_white_hats_and_dao_wars_behind_the_scenes/).

## The Script
A script has been written to extract The DAO token and ether balances of DTHs just prior to the hard-fork and currently - see [getTheDAOTokenBalance](https://github.com/bokkypoobah/TheDAOETCTokenBalance/blob/master/getTheDAOTokenBalance).

## Require Full Blockchain
Note that to run this script you will need the full ETC blockchain, not the version downloaded using the `geth --fast --oppose-dao-fork`. The reason for this is that the `debug.traceTransaction(...)` call is used to extract the owner in The DAO token creation process as the origin account could be an exchange (proxy) or an account instruction a wallet contract to purchase The DAO tokens.

If you have already downloaded a `--fast` version, you can `geth export chaindatafile`, rename `.ethereum/chaindata` and `geth import chaindatafile` to rebuild the intermediate states.
