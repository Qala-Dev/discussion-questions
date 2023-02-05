# Mastering the Lightning Network - Discussion Questions

https://github.com/lnbook/lnbook

## 1. Introduction

1. Provide 3 fairness protocols. Feel free to draw from bitcoin (besides proof of work), but also from your other experiences. In your experience, where have you seen fairness protocols breakdown?
1. "Increasing block size shifts the cost to node operators and requires them to expend more resources to validate and store the blockchain" - if that is evident why were there blocksize wars in the first place?
    Wouldn't everyone agree this is a bad thing?
1. "Lightning is not a separate token or coin, it is Bitcoin." One of the loudest critiques of lightning is that this is NOT true.
    Can you articulate why they think that lightning is not bitcoin?
1. Lightning Network has already required a major softfork (SegWit) to allow it to function. It may require (or strongly benefit from) additional softforks (e.g. ANYPREVOUT) in the future. Since Lightning Network is only one of multiple approaches to scaling Bitcoin, is it reasonable to modify the L1 Bitcoin protocol?
1. What are some use cases where Lightning Network can outperform existing payment networks? What are use cases where it (currently) underperforms? Can you think of some more relevant personas, in addition to the ones listed in the book?

## 2. Getting Started

1. Open up one of the explorers as suggested in the reading. Note a few observations based on your immediate impression.
1. Does the term wallet accurately describe the set of functions users needs? How could using this term be helpful or confusing for a new lightning user?
1. If you are running your wallet off of a third-party Lightning node connected to a third-party Bitcoin node, with keys that are keys held by a third-party custodian, is this still a trustless system?
1. "The more well-connected a node is, the more people Alice can reach." Given this statement, what is the fastest way to become well connected to the network?
1. In which ways is a Lightning wallet similar to an on-chain Bitcoin wallet? In which ways is it different?

## 3. How the Lightning Network Works

1. Describe the Lightning Network in terms of channels. What is needed to set up a "Lightning Network?" Is there only one Lightning Network?
1. Which parts of the Lightning Network are public? Which parts are private? How could we improve the privacy?
1. What data must a Lightning Network node keep in order to route payments and protect itself against loss of funds? As a node operator, how does this scale?
1. Why is revoking a transaction in bitcoin tricky? Why can't we simply invalidate older channel states?
1. Describe the differences between a bitcoin address and a Lightning Network payment invoice. What are the security assumptions of the invoice?
1. Why can't we just increase the channel capacity by sending more funds to the 2-of-2 multisignature address of the funding transaction?
    What would need to change for this to even have a chance of working?
1. What are some disadvantages of using the Lightning Network compared to using on-chain bitcoin?
1. Why are privacy and payment reliability said to be at odds with each other on the Lightning Network? If there is a trade-off to be made, where would you prefer the equilibrium to be?
1. Should we be okay with more experimentation on the Lightning Network by different node implementations?
    Should we hold the stability of the network to the same standard as the Bitcoin network? Why or why not?

## 4. Lightning Node Software

1. What other full lightning node projects are there?
    Do you think it is beneficial to have more or less of these in a non-consensus network?
1. Mastering Lightning demonstrates setting up nodes in docker for testing.
    Is it safe to then use these docker nodes in production later?
1. Do you think all companies developing on the Lightning Network should be required to open-source their Lightning apps?
1. Does it help or hurt the network to have different Lightning Network node implementations, and why?
1. What are the ways to check the integrity of all the software used in this demo? Which software have you verified it for?


## 6. Architecture

1. How important are each of the different network protocol layers?
    Is it possible to choose to implement some of them differently, whilst remaining compatible with the wider lightning network?
1. How does the Lightning Network layered architecture approach compare to that of the internet?
    What are some of the benefits of a layered architecture in general networks?
1. Looking at the network connection layer in the diagram, why would we need all the different types of network I/O protocols? Which of the ones listed in the diagram is actually deprecated and unusable now?
1. With your prior knowledge from Mastering Bitcoin, what is the purpose of DNS bootstrap? How does it help us connect to the lightning network and how what can you say about its trustlessness?
1. Do you think it is ideal that the Lightning Network uses the internet protocol? Do you consider this an avenue for government censorship?


## 7. Payment Channels

1. What happens if one node in the channel tries to unilaterally close the channel whilst there is a payment (HTLC) in-flight which has not been settled?
1. Is there a way to "guess" that a transaction as seen by a third party Bitcoin node might be a channel funding transaction?
    How about when the channel is closed cooperatively, is there a way for a third party node to "guess" that it was a channel closing transaction?
    What can we say about the anonymity in both cases?
1. What are some things you need to have backed up to ensure that you do not lose your funds associated with your lightning node and its channel in case of some failure or disaster. How is this different from backing up a Bitcoin wallet?
1. Why is it okay that the local node sends its `revoke_and_ack` at the end of the message exchange to advance channel state?
    How do incentives ensure that the local node won't want to publish its prior commitment transaction before revoking it while the remote node only has its own version revoked?
1. The commitment transactions are "asymmetric". What does that mean, and why is that the case?
1. Which signatures are exchanged in the `funding_created` and `funding_signed` messages? How is the funding transaction different from a commitment transaction?
1. Why can't we keep the funding transaction off-chain until we close the channel?
1. Do you think the `to_self_delay` value should be static or negotiated for every new channel and why?
1. How does one verify that the signature provided by a channel partner for the first commitment transaction is valid?

## 8. Routing

1. What is the difference between routing and pathfinding?
    Who is responsible for these actions? Which one is part of LN's scaling model?
1. Can a route involve multiple paths? How is this useful and is it used in the Lightning Network today?
1. When is it dangerous to reuse a BOLT-11 invoice, and why?
1. Alice pays Dylan through Bob and Carole. (A -> B -> C -> D). What happens if Carole reaches out to Alice and tells her the payment preimage that she received from Dylan, before telling Bob? In fact, why would she even tell Bob the preimage at all?
1. What are the benefits and disadvantages of increasing/reducing the duration of the timelock in an HTLC?
    Why should you always ensure the timelock of an incoming HTLC is longer than the one of an outoing HTLC?
1. If one node in the route decides to settle an HTLC on-chain, how does that affect the other nodes (both up and downstream) to keep settling their HTLCs off-chain?
1. What is payment atomicity, and why is it so important and how is it achieved?
1. How can an HTLC be revoked by the issuer before the timeout expires?
    What is the rationale behind that design decision (hint: think about how it affects other nodes in the routing path)?
1. “...This refers to the use of a cryptographic hash algorithm to commit to a randomly generated  secret.”
    Does the pre-image have to be random? Can you draw parallels to a component of on-chain Bitcoin usage where we frequently require randomness?
1. Which hash function is used to generate the payment hash? Could another function be used too, and if so - are there any limitations to which function(s) are possible? Do all the nodes along the routing path have to agree on the used hashing function?

## 9. Channel operation and payment forwarding

1. Can users of the lightning network configure their nodes to accept more or less than 483 in-flight HTLCs on a channel if they have more/less powerful hardware available? Why or why not?
1. Alice and Bob have an in-flight HTLC which has timed out (expired). Alice would like to remove the HTLC from the channel, however Bob has gone offline.
    Is Alice in any danger? What action if any should she take?
1. Why aren't the `update_add_htlc` and `commitment_signed` messages combined into a single message?
    What is the purpose of the `payment_hash` field in the `update_add_htlc` message?
1. What are all the ways in which an HTLC output script can be spent? If Alice sent the HTLC to Bob, who is able to spend which paths?
1. Alice sends a payment to Carole through Bob. After receiving the `update_fulfill_htlc` message from Carole, what prevents Bob from lying to Alice and claiming the payment failed, keeping the funds to himself?
1. If Alice funded the channel between her and Bob, is Alice able to send HTLCs to Bob, and is Bob able to send HTLCs to Alice?
1. HTLCs contain both a hash and a timelock. Is the timelock absolute or relative? Which opcode is used to express this? Why do we use an opcode, instead of the `nLockTime` or `nSequence` fields?
1. The `update_add_HTLC` message contains a `channel_id` field. Is this necessary? Isn't just having a channel available together enough, without necessarily specifying which one to use for the HTLC?

## 12. Pathfinding and payment delivery

1. Not only successful, but also failed payment attempts can be used to gather information on the liquidity ranges of channels. Can this be abused to surveil channel balances, and what cost does the attacker incur?
1. What are the benefits and downsides of using source-based onion routing in LN, as compared to destination-based routing?
1. What happens when different nodes use different pathfinding algorithms? To what extent do they remain interoperable?
1. Reduced uncertainty about channel balances could improve pathfinding efficiency. What are the downsides, and do you think we should further make that trade-off?
1. Maintaining a channel graph and performing pathfinding are computationally quite expensive. Could we offload that work to third parties? How would that impact an individual's privacy, security and ability to receive payments?
1. In which ways does pathfinding rely on messages from the gossip protocol?
1. Could a malicious actor lie about his channel's fees? How would that affect the network, and how can we assume it's not in his best interest to do so?

## 15. Lightning payment requests

1. Alice (A) paid Carole (C\) through Bob (B) (A->B->C). What happens when she makes the same payment again, with the same route and the same payment hash? What happens when instead of routing through Bob, she routes through Dylan (A->D->C)?
1. What are the privacy implications of sharing a BOLT11 invoice with someone? Can they be shared over the gossip protocol?
1. Could BOLT11 invoices be encoded with base58 instead of bech32? Do we even need to encode them at all, and why?
1. What is the maximum size of the payment description, and why?
1. BOLT11 invoices cannot be safely reused. Why is this not true for Bitcoin (L1) addresses, ignoring the privacy implications?
1. Each invoice is signed with "[a] signature [that] allows the sender to verify that the payment request was indeed created by the destination of the payment." Why is this necessary? Why would anyone create an invoice that does *not* pay to their own node?
1. As per BOLT8, all communication on the Lightning Network is encrypted. Could BOLT11 invoices also be encrypted, and why are they not? Is that a security threat?
