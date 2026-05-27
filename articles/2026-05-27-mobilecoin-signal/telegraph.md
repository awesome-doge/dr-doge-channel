# The Crypto That Nobody Uses

by Dr. Awesome Doge, 2026-05-27

---

Signal has had a cryptocurrency built in for five years. You probably did not know that.

In April 2021, Signal, the encrypted messenger used by journalists, activists, and security professionals around the world, added a feature called Signal Payments. The currency was called MobileCoin. The integration was the first of its kind. Send money through the same app you use to text. Privacy preserved end to end.

Signal had eighty-five million monthly active users. MobileCoin's price hit seventy-three dollars within days of the launch. It would later crash to sixteen cents and stay there.

I bring this up because Pavel Durov has just spent the month doing the exact thing that Signal's founder Moxie Marlinspike did in 2021, with an opposite outcome. Telegram is taking direct control of TON. Telegram is shipping the kind of vertically integrated messenger-plus-crypto experience that Signal pioneered. And it is going to work for reasons that have nothing to do with the technology and everything to do with how each company actually committed.

This is a story about commitment, not code.

## What MobileCoin was supposed to be

MobileCoin was founded in 2017 by Joshua Goldbard, with Moxie Marlinspike as a paid technical advisor. The pitch was simple. Cash for the internet age, with cryptographic privacy by default. Built on a protocol called CryptoNote, the same family of cryptography that Monero uses. Transactions confirm in seconds.

The integration into Signal in 2021 was the first time a privacy-focused messenger and a privacy-focused cryptocurrency met in public. Press treated it as a breakthrough. Cryptography journals wrote about it. Critics, including Jack Dorsey four years later, would call it a mistake.

The integration was the entire bet. Signal had eighty-five million users. If even one percent used Signal Payments, the network would have been one of the largest crypto user bases in the world. The math was on the wall.

## What actually happened

Five years later, Signal Payments still exists. The feature works. The cryptocurrency still trades. MobileCoin's foundation rebranded itself to Sentz in 2023 and now sells a savings product called Sentz Earn that pays eight percent yield on dollar stablecoins.

Almost nobody uses it.

The main blockchain has not received a single code update in 2026. The last meaningful release shipped in October 2025. The last commit to the core repo was November 5th. Signal continues to ship a hundred updates to its Android app every month. Its payment layer, the cryptocurrency at its heart, has been quiet for over six months.

If you open Signal today and scroll into the settings, the payments feature is still there. You can enable it. You can send MobileCoin. Most users do not enable it. Most users do not know it exists.

## Why it failed

The simplest answer is regulatory caution. Signal is a nonprofit. Its lawyers were nervous about adding a privacy coin in 2021. By 2022 the founder had resigned. By 2023 the project had rebranded away from its original name. By 2025 Jack Dorsey was publicly calling for Signal to abandon it for Bitcoin.

The more interesting answer is commitment.

Signal kept MobileCoin at arm's length. The feature was opt-in. The payments did not appear in the main app interface. The settings menu was where you went to find it. Signal's leadership stopped talking about it after a year. Moxie's resignation in early 2022 took with it the institutional energy that had pushed the integration forward.

MobileCoin became a thing you could do in Signal, not a thing Signal did.

Compare this to Telegram's approach. Telegram did not add TON as a settings-menu feature. Telegram embedded a wallet directly in the messenger. Made it work without leaving the chat window. Integrated Telegram Stars as the in-app currency. Encouraged Mini Apps to use it. Pavel Durov personally promoted it. The company is now the largest validator on the network. The founder is publicly the network's biggest advocate.

This is the difference between adding a feature and committing to a stack.

## What this means for TON

I have spent this week reading the testnet branch of TON's main repository. The next mandatory mainnet update is scheduled for Monday June 1. The configuration vote two days later proposes activating the next version of the network's virtual machine. Twelve thousand lines of obsolete consensus code have been deleted. New cryptographic primitives have been added that let TON contracts verify Ethereum signatures directly. Maximum block size goes up to ten megabytes. An old bridge gets decommissioned with new addresses replacing it.

None of this is happening on MobileCoin. There is no equivalent update. The team is on something else now.

The comparison is unfair to MobileCoin in some ways. The teams are different sizes. The user bases are different orders of magnitude. The commercial logic is different. Signal is a nonprofit. Telegram is a billion-user commercial platform that found a way to align its mission with a blockchain.

But the lesson holds. Crypto inside a messaging app only works when the messenger commits to it. Signal did not. Telegram did. The five-year results are now visible.

## What MobileCoin tells us about the next year

The pattern is clear. When a messenger adopts a cryptocurrency as a feature, the cryptocurrency dies. When a messenger adopts a cryptocurrency as its core, the cryptocurrency lives.

TON has crossed the second threshold. Telegram is no longer integrating TON. Telegram is now operating TON. The next twelve months will continue to ship updates because the same company that owns the messenger owns the chain.

If you are looking at this and worrying about centralization, that is the right concern to have. If you are looking at this and asking whether the project has stopped shipping, the answer is the opposite of MobileCoin's answer. The release schedule is public. The next update is Monday.

A year from now, Bitcoin will live in your Telegram chats. AI agents will run their own wallets. The messenger and the blockchain will be the same product.

That outcome is not promised. It is calendared.

The only reason it works is because Telegram chose what Signal chose not to. To commit.
