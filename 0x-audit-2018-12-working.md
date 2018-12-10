# 0x MultiAssetProxy Audit


<img height="100px" Hspace="30" Vspace="10" align="right" src="static-content/diligence.png"/>

<!-- Don't remove this -->
<!--59bcc3ad6775562f845953cf01624225-->
<!-- Don't remove this -->

## 1 Summary

ConsenSys Diligence conducted a security audit on the 0x MultiAssetProxy contract. This contract is part of 0x's decentralized exchange. This new component makes it possible to trade bundles of assets in a single trade.

### 1.1 Audit Dashboard

________________

<img height="120px" Hspace="30" Vspace="10" align="right" src="static-content/dashboard.png"/>

#### Audit Details
* **Project Name:** 0x MultiAssetProxy
* **Client Name:** 0x
* **Client Contact:** Amir Bandeali
* **Auditors:** Steve Marx, John Mardlin
* **GitHub :** https://github.com/ConsenSys/0x-audit-2018-12/
* **Languages:** Solidity assembly
* **Date:** 2018-12-10

#### Number of issues per severity

<%= issue_overview %>

________________

### 1.2 Audit Goals

The focus of the audit was to verify that the smart contract system is secure, resilient and working according to its specifications. The audit activities can be grouped in the following three categories:  

**Security:** Identifying security related issues within each contract and within the system of contracts.

**Sound Architecture:** Evaluation of the architecture of this system through the lens of established smart contract best practices and general software best practices.

**Code Correctness and Quality:** A full review of the contract source code. The primary areas of focus include:

* Correctness
* Readability
* Sections of code with high complexity
* Improving scalability
* Quantity and quality of test coverage

### 1.3 System Overview

#### Documentation

The following documentation was available to the audit team:
* [MultiAssetProxy specification](https://github.com/0xProject/0x-protocol-specification/blob/ad13141d9a2c6d93e06658d18c53e9f3d99442d4/v2/v2-specification.md#multiassetproxy)
* [MultiAssetProxy ZEIP](https://github.com/0xProject/ZEIPs/issues/23)

#### Scope

This audit was limited in scope to just the `MultiAssetProxy` contract.

#### Design

The `MultiAssetProxy` is a somewhat straightforward contract written in Solidity assembly for gas efficiency reasons. It accepts data which specifies a list of assets and corresponding amounts. It loops through those lists and transfers the requested amount of each asset by delegating to the appropriate asset proxy.

Only authorized callers may invoke the `MultiAssetProxy`, and only registered asset proxies may be delegated to.

### 1.4 Key Observations/Recommendations  

* The code is clear and contains excellent comments.
* Because the `MultiAssetProxy` is backward compatible with existing asset proxies, the code changes to add this feature are minimal.
* Most of the data consumed by the `MultiAssetProxy` is not validated in the rest of the contract system. Either more validation should be added to the contract, or a high burden for validation is being placed on the tools used by individuals to participate in trades.

## 2 Issue Overview  

The following table contains all the issues discovered during the audit. The issues are ordered based on their severity. More detailed description on the  levels of severity can be found in Appendix 1. The table also contains the GitHub status of any discovered issue.

<%= issue_list %>

## 3 Issue Detail  

<%= issues_markdown %>

## 4 Threat Model

The creation of a threat model is beneficial when building smart contract systems as it helps to understand the potential security threats, assess risk, and identify appropriate mitigation strategies. This is especially useful during the design and development of a contract system as it allows to create a more resilient design which is more difficult to change post-development.

A threat model was created during the audit process in order to analyze the attack surface of the contract system and to focus review and testing efforts on key areas that a malicious actor would likely also attack. It consists of two parts a high level design diagram that help to understand the attack surface and a list of threats that exist for the contract system.

### 4.1 Overview

Although the `MultiAssetProxy` itself does not directly own any assets, it has the ability to transfer arbitrary assets from traders to any destination by calling out to the appropriate asset proxy. This makes it a valuable target for an attacker. Robust mechanisms are in place to prevent unauthorized callers from directly invoking the contract, so any exploit would have to pass through the `Exchange` contract layer. This minimal surface area, combined with the low branching factor of the contract itself, results in a low risk of security vulnerabilities.

### 4.2 Detail

#### Assets
* All tokens authorized by traders

#### Potential attackers
* Malicious traders: maker or taker
* Malicious forwarders: third parties submitting trades
* Other actors: people directly trying to exploit the contract

#### Surface area
* Traders can manipulate data passed through the `Exchange` to the `MultiAssetProxy`
* Traders and relayers can manipulate other aspects of transactions (gas, stack depth, origin)
* Anyone can directly attempt calls to the `MultiAssetProxy`, including targeting the fallback function

The most promising avenues of attack are probably on the trader side. The data passed around is complex and dictates what assets are transferred where, and this data is largely controllable by traders. Makers are particularly interesting attackers because they establish the initial transaction inputs.

## 5 Tool based analysis

The issues from the tool based analysis have been reviewed and the relevant issues have been listed in chapter 3 - Issues.

### 5.1 Mythril

<img height="120px" align="right" src="static-content/mythril.png"/>

Mythril is a security analysis tool for Ethereum smart contracts. It uses concolic analysis to detect various types of issues. The tool was used for automated vulnerability discovery for all audited contracts and libraries. More details on Mythril's current vulnerability coverage can be found [here](https://github.com/ConsenSys/mythril/wiki).

The raw output of the Mythril vulnerability scan can be found [here](./tool-output/mythril/mythril_report.md).

### 5.2 Solhint

<img height="120px" align="right" src="static-content/solhint.png"/>

This is an open source project for linting Solidity code. The project provides both Security and Style Guide validations. The issues of Solhint were analyzed for security relevant issues only. It is still recommended to use Solhint during development to improve code quality while writing smart contracts.

The raw output of the Solhint vulnerability scan can be found [here](./tool-output/solhint/solhint_report.md).

### 5.3 Surya
Surya is an utility tool for smart contract systems. It provides a number of visual outputs and information about structure of smart contracts. It also supports querying the function call graph in multiple ways to aid in the manual inspection and control flow analysis of contracts.

A complete list of functions with their visibility and modifiers can be found [here](./tool-output/surya/surya_report.md).

### 5.4 Odyssey

<img height="120px" align="right" src="static-content/odyssey.png"/>

Odyssey is an audit tool that acts as the glue between developers, auditors and tools. It leverages Github as the platform for building software and aligns to the approach that quality needs to be addressed as early as possible in the development life cycle and small iterative security activities spread out through development help to produce a more secure smart contract system.
In its current version Odyssey helps to better communicate audit issues to development teams and to successfully close them.



## 6 Test Coverage Measurement

Testing is implemented using the Mocha. 6244 tests are included in the test suite (for the entire contract system) and they all pass.

We were unable to generate test code coverage reports working in the provided repo. Based on a manual review of the tests, and given the low branching structure of the `MultiAssetProxy`, we believe that test coverage is likely to be very close to 100%.

## Appendix 1 - Severity


### A.1.1 - Minor

Minor issues are generally subjective in nature, or potentially deal with topics like "best practices" or "readability".  Minor issues in general will not indicate an actual problem or bug in code.

The maintainers should use their own judgment as to whether addressing these issues improves the codebase.

### A.1.2 - Medium

Medium issues are generally objective in nature but do not represent actual bugs or security problems.

These issues should be addressed unless there is a clear reason not to.

### A.1.3 - Major

Major issues will be things like bugs or security vulnerabilities.  These issues may not be directly exploitable, or may require a certain condition to arise in order to be exploited.

Left unaddressed these issues are highly likely to cause problems with the operation of the contract or lead to a situation which allows the system to be exploited in some way.

### A.1.4 - Critical

Critical issues are directly exploitable bugs or security vulnerabilities.

Left unaddressed these issues are highly likely or guaranteed to cause major problems or potentially a full failure in the operations of the contract.

## Appendix 2 - Disclosure

ConsenSys Diligence (“CD”) typically receives compensation from one or more clients (the “Clients”) for performing the analysis contained in these reports (the “Reports”). The Reports may be distributed through other means, including via ConsenSys publications and other distributions.

The Reports are not an endorsement or indictment of any particular project or team, and the Reports do not guarantee the security of any particular project. This Report does not consider, and should not be interpreted as considering or having any bearing on, the potential economics of a token, token sale or any other product, service or other asset. Cryptographic tokens are emergent technologies and carry with them high levels of technical risk and uncertainty. No Report provides any warranty or representation to any Third-Party in any respect, including regarding the bugfree nature of code, the business model or proprietors of any such business model, and the legal compliance of any such business. No third party should rely on the Reports in any way, including for the purpose of making any decisions to buy or sell any token, product, service or other asset. Specifically, for the avoidance of doubt, this Report does not constitute investment advice, is not intended to be relied upon as investment advice, is not an endorsement of this project or team, and it is not a guarantee as to the absolute security of the project. CD owes no duty to any Third-Party by virtue of publishing these Reports.

PURPOSE OF REPORTS The Reports and the analysis described therein are created solely for Clients and published with their consent. The scope of our review is limited to a review of Solidity code and only the Solidity code we note as being within the scope of our review within this report. The Solidity language itself remains under development and is subject to unknown risks and flaws. The review does not extend to the compiler layer, or any other areas beyond Solidity that could present security risks. Cryptographic tokens are emergent technologies and carry with them high levels of technical risk and uncertainty.

CD makes the Reports available to parties other than the Clients (i.e., “third parties”) -- on its GitHub account (https://github.com/ConsenSys). CD hopes that by making these analyses publicly available, it can help the blockchain ecosystem develop technical best practices in this rapidly evolving area of innovation.

LINKS TO OTHER WEB SITES FROM THIS WEB SITE You may, through hypertext or other computer links, gain access to web sites operated by persons other than ConsenSys and CD. Such hyperlinks are provided for your reference and convenience only, and are the exclusive responsibility of such web sites' owners. You agree that ConsenSys and CD are not responsible for the content or operation of such Web sites, and that ConsenSys and CD shall have no liability to you or any other person or entity for the use of third party Web sites. Except as described below, a hyperlink from this web Site to another web site does not imply or mean that ConsenSys and CD endorses the content on that Web site or the operator or operations of that site. You are solely responsible for determining the extent to which you may use any content at any other web sites to which you link from the Reports. ConsenSys and CD assumes no responsibility for the use of third party software on the Web Site and shall have no liability whatsoever to any person or entity for the accuracy or completeness of any outcome generated by such software.

TIMELINESS OF CONTENT The content contained in the Reports is current as of the date appearing on the Report and is subject to change without notice. Unless indicated otherwise, by ConsenSys and CD.

