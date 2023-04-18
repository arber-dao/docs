---
description: COMING SOON!
---

# White Paper

## Abstract

As the popularity of decentralized finance grows, we seek methods to employ these digital currencies within distributed systems that can generate real world transformations. A significant challenge in decentralized systems is their lack of a central authority to manage disputes that emerge between involved parties. Decentralized dispute resolution paves the way for many peer-to-peer applications involving parties that may not be inherently trustworthy. In this paper, we present Arber, a decentralized crowdfunding platform for donations, incorporating milestones and enabling donors to raise disputes when they perceive the project's mission statement is not being fulfilled. We discuss the motivation behind creating such a platform, provide an overview of relevant terminology, and describe the core smart contract interface.

## Motivation

There are already many existing cloud solutions for facilitating donations. GoFundMe and Patreon are two examples that most people would be familiar with, but there are also web3 donation platforms emerging such as [Gitcoin grants](https://gitcoin.co/grants), [Giveth](https://giveth.io/) and [Juicebox.money](https://juicebox.money/) to name a few. A big drawback of these platforms is that they don't offer dispute resolution for projects that don't complete their mission statement they set out to complete. If they do offer dispute resolution, it is often very centralized, and the decision is made by the organization, or the DAO's governance token holders.

By having a foundation for decentralized dispute resolution integrated into Arber, it allows donors to trust that the project they are donating to will complete their mission statement, otherwise, the donor can withdraw their donation from the project. Furthermore, it affirms creators will receive the donations upon completing the mission since donors cannot withdraw proceeds from the project unless there is a dispute ruled in favour of the donors.&#x20;

Our goal is to bring secure, transparent, cheap, and decentralized donations to the mainstream environment. With the use of Ethereum smart contracts, there is no server maintenance overhead, or requirement for staff to handle business operations, which reduces donation fees. All transactions with the Arber smart contract, are completely transparent and can be viewed on a block explorer (creating projects, donating to projects, disputes, etc.). Finally, there is no one entity that can censor projects, and any protocol improvements must go through the Arber DAO.

We hope that the implementation of Arber will help bring greater support to new creative ideas, reputable causes, and events. Anybody will be able to create a project for a mission they hope to complete, which they are held accountable for completing. By bringing trust between creators and donors, creators will be able to receive larger donations for their project thereby allowing greater success rate for their mission statement.

## Introduction

Arber is a decentralized autonomous organization (DAO) that helps facilitate donations for creative ideas, reputable causes and events on the Ethereum blockchain. Creators initialize projects and specify milestones which lay out a mission statement that they must complete, otherwise they lose the donations for their project. This is made possible with the use of 3rd party decentralized dispute resolution protocols. Below we define some terminology that will be used throughout the course of this paper.

### Terminology

* **Project -** A project is a request for donations which is initialized by the creator. Details about creating a project can be found in [Creating a Project](white-paper.md#creating-a-project).
* **Creator -** The creator is the account that initializes the project, and can be either an EOA or a Contract Account. Any proceeds that the project generates from donors can be withdrawn only by the creator upon milestone completion.
* **Donor -** A donor is an account that sends ERC20 tokens to a project. If donors succeed in disputing a projects milestone, this is the account that can withdraw their donation from the project. Like with the creator, the donor can be an EOA or a Contract Account.
* **Milestone -** Upon initialization of a project, one or many milestones can be embedded in the project. Each milestone specifies the percentage of donations that will be released from the project upon successful completion of the milestone.
* **MetaEvidence -** The meta evidence document is the document that the creator submits when initializing a project. It outlines very specific details about the mission statement to be completed for each milestone.
* **Evidence -** When the creator believes they have satisfied all the requirements for a milestone, as laid out by the meta evidence document, they can request to withdraw proceeds for that milestone by submitting an evidence document which represents the evidence that they have completed said milestone.
* **Dispute -** A dispute is created by the donors in the event they believe the creators evidence document does not satisfy the requirements as laid out by the meta evidence document. If the dispute succeeds in favour of the donors, all donors can withdraw their donation from that project. If the creator wins the dispute, the milestone is marked as completed, and they are able to withdraw proceeds for that milestone. By allowing to crowdfund dispute fees, as well as allowing to crowdfund the appeal for a disputes ruling, we reduce the possibility of collusion and the "rich" always winning disputes.
* **Arbitrator -** The arbitrator is the smart contract which handles disputes. Currently Arber uses [Kleros](https://kleros.io/) for dispute resolution, but in the future could support alternative arbitrators. Kleros uses game theory to incentivize jurors that are ruling on a dispute to act honestly. By ensuring that there is an economic benefit to acting honestly, and a corresponding economic loss for acting dishonestly, donors and creators can trust that the ruling on a dispute will be the truth. Visit the [Kleros docs](https://kleros.gitbook.io/docs) for more information on how they facilitate decentralized dispute resolution.

TODO: Keep writing intro

By holding Arber's _ARBR_ token, users can stake it to boost the visibility of a project. This reflects their level of trust and support for the goals and mission of the project. It also allows the staker to generate passive income when a milestone for that project is completed, which is calculated as a percentage of total amount staked, amount of time staked, and the amount claimed by the creator for that milestone. Additionally, _ARBR_ holders can vote on protocol improvements, and the rules for which project creators must abide by (ie. cannot create a project that is clearly inciting violence, where disputes for this are handled by [Kleros Curated Lists](https://curate.kleros.io/)).&#x20;

## Creating a Project

TODO: create detailed outline of creating a project. This is left for now as the function is subject to change (still unsure if \_creatorWithdrawTimeout should be decided by the creator or not as well as some other uncertainties about some parameters). Additionally should include very detailed outline of how MetaEvidence document is created as well as an example MetaEvidence document.

```solidity
function createProject(
  uint64[] memory _milestoneAmountUnlockablePercentage,
  bytes[] memory _milestoneArbitratorExtraData,
  uint64 _creatorWithdrawTimeout,
  address _crowdfundToken,
  string memory _metaEvidenceUri
) external payable returns (uint32 projectId);
```

## Donating to a Project

Donating to a project is quite straight forward, and consistent with how you would allow any contract to take ownership of funds.

```solidity
function approve(address _spender, uint256 _value) public;
```

The _approve_ function must be called on the ERC20 token that is being donated. The spender is the Arber core smart contract, with the value being the amount you wish to donate.

```solidity
function donateProject(uint32 _projectId, uint256 _value) external;
```

Once the Arber core contract has been approved to spend your ERC20, you can call the _donateProject_ function, by passing the ID of the project you wish to donate to, as well as the value you wish to donate (the same value that was approved).

In order to donate ETH, a WETH contract must be used since the protocol currently only supports donating ERC20 compatible tokens.&#x20;

## Claiming Donations from a Milestone

When the creator believes they have completed the necessary requirements for their milestone, as laid out by the MetaEvidence document they can call the _requestClaimMilestone_ function.&#x20;

```solidity
function requestClaimMilestone(
  uint32 _projectId, 
  string memory _evidenceUri
) external;
```

The _requestClaimMilestone_ function is only callable by the creator of the project, and the ID of the project, as well as the URI for the evidence document must be passed to the function. Since milestones must be completed in sequential order, there is no need to pass the ID of the milestone to this function.



TODO: add necessary info for evidence document. This should include a detailed outline of how an evidence document is created, and an example of an evidence document



Once _requestClaimMilestone_ is called, the _claimMilestone_ function can be called after _creatorWithdrawTimeout_ time has exceeded (see [Creating a Project](white-paper.md#creating-a-project)). This timeout is to allow sufficient time for the donors to raise a dispute. If a dispute is raised, the _claimMilestone_ function is blocked until the dispute has been addressed.

```solidity
function claimMilestone(uint32 _projectId) external;
```

If no dispute is raised, and the timeout has exceeded, anyone can call _claimMilestone_ and the appropriate amount of funds will be awarded to the creator which can immediately be withdrawn (see [Withdrawing Funds](white-paper.md#withdrawing-funds)).

## Disputing a Project Milestone

Currently, creators do not have to pay an arbitration fee when a dispute is created. The reason for this is that the creator is already potentially sacrificing a lot of donation funds in order to go through the dispute process. This is to prevent spamming a project with disputes. If creators had to pay arbitration fee every time some malicious actor attempted to raise a dispute, this could get cumbersome for the creator if they, for example missed the deadline to pay a dispute fee. In the event the donors win the dispute, but the creator wishes to request an appeal, both parties must fund the appeal fee.

```solidity
function createDispute(uint32 _projectId) external payable;
```

In order to create a dispute anyone can call the _createDispute_ function and pass the project ID. They also must send ETH to pay for the dispute fee. It is important to note that they do not need to cover the entire dispute fee, as the dispute fee can be crowdfunded. Every time the function is called, the amount contributed to the dispute is tracked, and the function can be called until the entire dispute fee is covered.

```solidity
function rule(uint256 _disputeId, uint256 _ruling) external override(IArbitrable);
```

Arber exposes a _rule_ function. This is intended only for the arbitrator contract to call. When a dispute is finally ruled upon (and all appeals have been addressed), the arbitrator will call this function which will either be in favour of the creator, or the donors. The function will mark a milestone as completed and allow the creator to withdraw donations for that milestone, or will allow donors to withdraw their funds from the project depending on the ruling.



TODO: Add necessary API interface for creating appeals.



The Arber team will diligently monitor any disputes that arise. In order to minimize the risk of missed deadlines for dispute or appeal fees, the team will cover such fees if they believe that the party in question has a strong case. It should be noted, however, that this should be viewed as a precautionary measure rather than a failsafe, and all parties are advised to pay their own fees to ensure their case makes it to arbitration.&#x20;

It is important to emphasize that the Arber team is not vested with ultimate authority to determine the outcome of disputes, rather the arbitrator retains this responsibility. Additionally, the team does not have any additional capabilities beyond those available to others, as anyone is permitted to pay dispute or appeal fees for either party.

## Withdrawing Funds

This is the global _withdraw_ function that allows EOA's and contract accounts to withdraw funds that are stored in the Arber core contract for that account. This includes claiming funds earned by creators for completing milestones, donors claiming funds from disputed projects, any overpaid dispute fees, as well as reclaiming appeal fees when a dispute is won.

```solidity
function withdraw(address tokenAddress) external;
```

In order to claim funds, the token address that you are claiming for must be passed to the _withdraw_ function. Since dispute and appeal fees must be paid in ETH, in order to claim ETH, the 0 address is passed to the withdraw function. Note that if a project is using a WETH contract to raise funds, in order to claim those funds, it is the address of the WETH contract that is passed to the withdraw function. Then the WETH tokens can appropriately be traded for ETH.&#x20;

## Roadmap

1. Implement MVP. This will include the full set of functions required for creating projects, donating to projects, creating disputes, and withdrawing funds as defined above. The governor will initially be set as a multi-sig wallet controlled by numerous interested parties in Arber to allow for rapid development of the protocol.
2. Give creators the option to provide rewards for donating to their project. This could be exclusive NFT's or any desired ERC20 token.
3. Implement support for [Reality.eth](https://reality.eth.link/) to allow project creators to specify alternative arbitration mechanisms. Reality.eth will provide greater incentives for telling the truth, and greater cost for being dishonest. For projects which have a higher level of ambiguity of what a completed mission statement looks like, Kleros is a better arbitration option. On the contrary, projects which have a very clear completion criteria which can easily be proved upon milestone completion, would benefit from using Reality.eth.
4. Give donors the ability to create projects, which creators can stake on to take ownership of the project. This will give more power to donors to specify what sort of mission statement they want to be completed.
5. Transition to a DAO governance solution. This will make the protocol much more decentralized, and allow anyone who holds the _ARBR_ token to stake it to generate passive income, as well as contribute to protocol improvements
6. Integrate a fiat onramp. This is to open our audience to the regular person, allowing people who do not own crypto or know how to purchase crypto to become a user.

## Conclusion
