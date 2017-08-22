# Moeda Token Audit

## Preamble
This audit report was undertaken by BlockchainLabs.nz for the purpose of providing feedback to Moeda. It has subsequently been shared publicly without any express or implied warranty.

Solidity contracts were sourced from the public Github repo [erkmos/moeda-contracts](https://github.com/erkmos/moeda-contracts/) prior to commit [35b9af7aa25384b191eb71a11d9b4499ec9c48c2](https://github.com/erkmos/moeda-contracts/commit/35b9af7aa25384b191eb71a11d9b4499ec9c48c2) - we would encourage all community members and token holders to make their own assessment of the contracts.

## Scope
All Solidity code contained in [/contracts](https://github.com/erkmos/moeda-contracts/tree/master/contracts) was considered in scope along with the tests contained in [/test](https://github.com/erkmos/moeda-contracts/tree/master/test) as a basis for static and dynamic analysis.

## Focus Areas
The audit report is focused on the following key areas - though this is *not an exhaustive list*.

### Correctness
* No correctness defects uncovered during static analysis?
* No implemented contract violations uncovered during execution?
* No other generic incorrect behavior detected during execution?
* Adherence to adopted standards such as ERC20?

### Testability
* Test coverage across all functions and events?
* Test cases for both expected behavior and failure modes?
* Settings for easy testing of a range of parameters?
* No reliance on nested callback functions or console logs?
* Avoidance of test scenarios calling other test scenarios?

### Security
* No presence of known security weaknesses?
* No funds at risk of malicious attempts to withdraw/transfer?
* No funds at risk of control fraud?
* Prevention of Integer Overflow or Underflow?

### Best Practice
* Explicit labeling for the visibility of functions and state variables?
* Proper management of gas limits and nested execution?
* Latest version of the Solidity compiler?

## Classification

### Defect Severity
* **Minor** - A defect that does not have a material impact on the contract execution and is likely to be subjective.
* **Moderate** - A defect that could impact the desired outcome of the contract execution in a specific scenario.
* **Major** - A defect that impacts the desired outcome of the contract execution or introduces a weakness that may be exploited.
* **Critical** - A defect that presents a significant security vulnerability or failure of the contract across a range of scenarios.

## Findings
### Minor

**setMigrationAgent** - While testing the correctness of SGTExchanger, we found that it was possible for an owner to call the `setMigrationAgent`() function with regular address passed in. Subsequently, with the regular address being set, it wouldn't be possible to call `migrate()` function or to reset migration agent.

However, this is a minor issue because the owner of the contract should know how to call `setMigrationAgent`. Since commit [7fe531a2ab0b3fc217f29eec01061bb3160a7b5e](https://github.com/erkmos/moeda-contracts/commit/7fe531a2ab0b3fc217f29eec01061bb3160a7b5e), an exception is now thrown.


**Testability** - 
All functions provided by MoedaToken contract is covered by Moeda team's test suite. The test suite was well developed. No major issues were found.

### Moderate
_No moderate defects were found during this audit._

### Major
_No major defects were found during this audit._

### Critical
_No critical defects were found during this audit._

## Conclusion
Overall we have been satisfied with the quality of the code and responsiveness of the developers in fixing defects. There was good test coverage for all components.

The developers have followed common best practices and demonstrated an awareness of security weaknesses that could put funds at risk.

We have tested all functions of the contract, and it works as expected.

### Successful deployments on Kovan testnet:
https://kovan.etherscan.io/address/0x17edcc8eb8f2a2a6afe7af8cb8df1b56dd127b8b#code
https://kovan.etherscan.io/address/0x4a070634836ad332207230e6d5227997964ec0c9#code

1. Called setMigrationAgent fails if account address provided:  https://kovan.etherscan.io/tx/0x5ba889d6bb5525a5c6b7023996446dbf3a00fd372f97ad0a1c77ab4eca992989 
 Failed as expected

2. MigrationAgent deployed:
 0x887858B5Ca4d144E4F973954e8a5D66fb3e265c2
 `setMigrationAgent` called with MigrationAgent Contract:
 https://kovan.etherscan.io/tx/0x1a567840e28aa6e690b95b21d82eb4d098939bb1d40855c208ab1425216ee2ca
 Passed as expected

3. Can’t create BonusTokens twice:
 https://kovan.etherscan.io/tx/0xb7c75ecb9b3f938ac0e975d5b3e8b92c59012916660826725952dbe7e9c603c1
 Failed as expected

4. Can’t call unlock twice:
 https://kovan.etherscan.io/tx/0x88d4a2f63dac8ed321d3fb358b1b22959018541633b90e19478d40927228b6a0
 Failed as expected

5. Call migrate without MigrationAgent set:
 https://kovan.etherscan.io/tx/0x288d8b9bf47fd724f45fadaf3885d00291fcd6ccf247928f5d4ca9bc3c491114 (failed as expected)
 Failed as expected

6. Successfully migrated tokens :
 https://kovan.etherscan.io/tx/0x420702cd91d3bc08ed1bc5969645732339a7956ff8beeeabc296245905e4f997
 Passed as expected