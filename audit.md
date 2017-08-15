## Testability
1. One pending test case -  `should set allocations`
https://github.com/BlockchainLabsNZ/moeda-contracts/blob/master/test/moedatoken.js#L25


2. Although all tests could be passed in testrpc enviornment, most of them fail if ran against Parity dev Chain node.
During the investigation of failure, it seems that https://github.com/BlockchainLabsNZ/moeda-contracts/blob/master/test/utils.js#L102
`shouldThrowVmException` function is not throwing an error if ran in parity/geth mode. The fix will not be trivial and will require some research on how to better write tests for scenarious like `shouldThrowExpection` under prod-like ethereum nodes like parity/geth.

3. Setup instructions should explicitly tell a developer to have at least 4 eth accounts due to:
https://github.com/erkmos/moeda-contracts/blob/11d58c535ee8fc2954a388b0a8cb43ebf798f056/test/moedatoken.js#L122 `accounts[3]`

4. https://github.com/erkmos/moeda-contracts/blob/master/test/moedatoken.js#L95 - no need for an address to be passed in since the constructor doesn't use it.
5. Any good reason for using https://github.com/erkmos/moeda-contracts/blob/11d58c535ee8fc2954a388b0a8cb43ebf798f056/contracts/test/TestMintingToken.sol ? it's identical to the source with exception of renamed method `create` to `mint` 
6. https://github.com/erkmos/moeda-contracts/blob/11d58c535ee8fc2954a388b0a8cb43ebf798f056/test/utils.js#L102 shouldThrowVmException doesn't assert anything.
Proof: ![](audit_proof.gif)



