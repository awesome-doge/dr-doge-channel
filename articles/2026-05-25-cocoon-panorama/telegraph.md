# What Telegram Quietly Built on TON, and What You Can Do With It

by Dr. Awesome Doge, 2026-05-25

---

Six months ago Pavel Durov stood on stage at Blockchain Life in Dubai and announced something most of the crypto press fumbled.

Telegram was going to do AI. Not by paying OpenAI. Not by building a giant data center. Instead, anyone with a powerful GPU could plug in and earn TON for serving the AI. Nobody, not even Telegram itself, would be able to see what users asked.

People clapped. Headlines wrote themselves. Then nothing happened publicly for six months.

Last month it went live. The thing is called COCOON. I run a dashboard called cocoon.doge.tg that decodes every transaction on the network. I've been watching it wake up since November.

Almost nobody is writing about what you can actually do with this. So let's start there.

## What COCOON does

Strip the buzzwords. COCOON lets you run an AI model on someone else's GPU without that GPU's owner being able to read what you asked it.

That sentence sounds small. The applications are not.

**A private AI you can ask anything.** Today, every question you send to ChatGPT or Claude sits in their logs for 30 days, sometimes longer. Medical questions, legal questions, the things you'd never tell your spouse. With COCOON, the prompt goes into a sealed compartment on the chip, gets answered, comes back. No log exists outside your screen. The GPU owner cannot see it. The middleman cannot see it. The chip is built so they cannot see.

**An OpenAI-compatible API, paid in TON, with no account.** No signup, no KYC, no monthly bill, no credit card on file. Send the same request format you send to OpenAI, pay per token in TON, get back the same response shape. For developers in countries where US AI providers are blocked or unwelcoming, this is the cleanest version of "private AI access" that has ever existed.

**Earning TON with a GPU you already own.** If you have an H100 or newer with a TDX or SEV-SNP capable CPU, you can run a worker. Install the image, point it at your TON wallet, and start serving inference. Workers earn 95% of what users pay. The setup is rough today, which is why I'm building cocoon-gpu-pool, so smaller GPU owners can join the network without becoming sysadmins.

## What the Telegram ecosystem can do with COCOON

Telegram has a billion users. Half of them sit in countries where the government would love a transcript of every AI conversation. If Telegram pays OpenAI to do its AI, the privacy promise of the product collapses.

So Telegram built something else. COCOON is one part of that answer.

There is an interesting wrinkle here. The April outage thread on the cocoon repo accidentally revealed that Telegram's mobile app AI features (translation, text correction) kept working while the public COCOON network was down. Mira's Private Mode, which routes through public COCOON, did not. The implication is that Telegram runs a private inference path for its own product features, and treats the public COCOON network as something more useful to outside developers and third-party bots than to its own first-party app.

That reframes the use cases below. Some of what follows is what Telegram likely runs on its private path. Some is what becomes buildable, today, by third-party developers on the public COCOON network.

**Third-party translation for Telegram content.** Issue #72 on the cocoon repo, opened May 9, is the first public example. The request comes from Stas, founder of margeleT, an external platform that aggregates public Telegram channels across 25 countries. He wants on-demand translation of channel content without piping it through Google or OpenAI. This is the kind of request COCOON exists to serve.

**AI bots that do not leak prompts.** Mira's Private Mode is the working production example today. Mira is Telegram's AI assistant, built by The Open Platform team and reportedly used by over two million people. By default it uses ordinary providers (ChatGPT for text, Nano Banana for images, Google Veo 3 for video). When the user turns on Private Mode, the request routes through public COCOON instead. The prompt sits inside a confidential VM and never reaches the regular providers. Anyone can build a bot with this pattern today.

**Voice transcription that does not leave the trust boundary.** Voice messages turned into text by AI, with the audio sealed at every hop. Useful for accessibility bots, for transcribing voice memos privately, for letting groups search past voice notes.

**AI moderation for private channels where admins do not see flagged content.** A bot scans messages for spam or threats, raises an alert, but the message body never reaches a human moderator's inbox. Compliance teams have wanted this for years.

**Mini Apps that handle private uploads.** Medical records, contracts, financial statements, tax forms. A Mini App today cannot responsibly process any of these because the upload passes through ordinary AI providers. With COCOON, the upload sits inside a sealed worker. The handling is provable on chain.

This is the moment Telegram stops being a chat app and starts being a platform stack. Stars is the payment layer. TON is the chain. COCOON is one of the compute layers, the public one, for developers who want to build privacy-grade AI features on top of Telegram.

## What TON + COCOON unlocks

This is the section where the use cases get speculative. None of these are shipped products. All of them are buildable today on the infrastructure that exists. I am writing about design space, not announced features.

**Autonomous AI agents on TON that hold their own funds and act on their own decisions.** A smart contract that holds TON, calls an AI to make a decision, and acts on the result. Trading bot, rebalancer, yield optimizer, oracle. The agent's strategy stays private from the AI provider. The provider cannot front-run the agent because the provider cannot read the prompt. This was impossible before. With OpenAI in the middle, OpenAI can see every move your agent considers.

**AI oracles for smart contracts.** A contract needs to know whether a piece of news is real. It calls COCOON, gets back a signed inference result, acts on it. Because the worker proves its identity on chain through the root contract, the result is verifiable. No need to trust an off-chain API operator.

**TON-native AI marketplaces with multiple providers competing.** Anyone can become a worker. Multiple workers serving the same model can compete on price and reliability. Reputation lives on chain. This is the closest the industry has come to a real free market for AI compute.

**Confidential DeFi using AI.** Sentiment analysis on private trade flow. Risk scoring on private portfolios. Fraud detection without exposing transactions. Anything where the model needs to see sensitive data and the user needs to know that data does not leak.

**Pay-as-you-go AI for anyone with a TON wallet.** No account creation. No credit card. No region check. Open a TON wallet, send TON, get inference. For the parts of the world where US AI providers are inaccessible by design, this is real access.

## How to actually use it today

There are three developer paths, plus a fourth that requires nothing.

The first developer path is the OpenAI-compatible endpoint. The COCOON team runs proxy infrastructure, and you can request API access by opening an issue on the cocoon repo. The proxies accept standard chat completion requests, you sign with your TON wallet, and the request gets routed to a sealed worker. The response shape matches OpenAI, so existing SDKs work.

The second is direct integration from a TON smart contract or Mini App. The contracts on chain (CocoonRoot, CocoonProxy, CocoonWorker, CocoonClient, CocoonWallet) are public. You can call them from your own contract. The client library handles the attestation handshake automatically.

The third is operating a worker. Buy an H100 or newer NVIDIA GPU. Pair it with an Intel TDX or AMD SEV-SNP capable CPU. Run the worker image. Wait for the proxy to route requests to you. Get paid in TON for every token of output. The setup guide is on cocoon.org, but as of today it expects a sysadmin who is comfortable with TEE attestation debugging.

The fourth path is the one most people will take. Turn on Mira's Private Mode. Use a third-party Telegram bot that has plugged into COCOON. Open a Mini App that processes a private upload. You do not need to know the network exists for it to do its job. That is the point.

## How it works under the hood

The trick is a feature in modern Intel and AMD chips called confidential computing. The chip builds a sealed room inside itself. Anything that runs in that room is invisible to the rest of the computer. The owner of the physical server cannot peek inside. Not with root access. Not with the keyboard in their hands.

When a worker boots up, the chip stamps the running software with a cryptographic seal. The client checks that seal against a list of approved seals stored in a smart contract on TON. If the seal does not match, the connection fails. There is no way to fake the check, because the seal comes from the chip itself, not from a software setting.

For this to work, you trust one thing: that Intel or AMD built the chip the way they said they did. Smaller trust footprint than every AI company that exists today.

The team supports both Intel and AMD chips, which matters more than it sounds. Open the code history. On January 12 of this year, the team renamed a folder from `tdx` to `tee`. Two months later they merged AMD support. A network only running on Intel is hostage to Intel. They killed SGX in consumer chips a few years ago. They can do it again to TDX. By supporting AMD too, COCOON has a backup supply chain.

Payments use a running-tab trick borrowed from Bitcoin's Lightning Network. Every payout is signed cryptographically but stays off chain until either side has enough on the tab to justify a settlement transaction. Sending a TON transaction costs about one cent. An AI token costs a fraction of that. Without the running tab, fees would eat the whole economy.

The economics, as reported in early community coverage, are roughly 95% to workers and 5% to proxies. The cocoon-contracts code treats both numbers as parameters that can be changed by the proxy or the root contract, and the official site does not publish a fixed schedule. Either way the proxy take is small. Apple takes 30%. Steam takes 30%. AWS takes everything. The proxy is a load balancer, not a gatekeeper.

## What broke, and what it accidentally revealed

On April 9 at 07:23 UTC, the public COCOON network stopped working.

I know the exact minute because that is when the worker logs in issue #58 froze mid-request. The operator opened a GitHub issue with their full stack trace. Within 24 hours, fifteen other worker operators piled in with the same complaint. The proxy could not reach a working TON liteserver. No requests were getting through.

The popular story was that TON shipped a main blockchain upgrade the day before, and the upgrade broke COCOON. That is half wrong. TON's release went out fine. What actually broke was COCOON's bundled TON dependency, which had not been updated to match the new mainnet. The COCOON workers were running on an older TON client. The network would not talk to it.

On April 14, five days into the outage, a community member named @erokhinav published a patch. It disabled an optional block-proof check so worker operators could keep running while the official fix was being prepared. The community kept the network alive while the team caught up.

On the same day, another user opened issue #60, asking whether Telegram's AI features were also broken. The answer was surprising.

Telegram's mobile app AI kept working through the outage. Mira, the AI assistant built by The Open Platform team and used inside Telegram, went down. Mira's Private Mode runs on public COCOON, so that part of the bot would have broken with the network. The user who tested both reached the conclusion that Telegram is running more than one inference path. The public COCOON network is one of them. There appears to be at least one other path, probably a private COCOON instance, that runs inside Telegram's own infrastructure and does not depend on the public network.

That is the part nobody has written about. The public network is the launch product. The private network is what Telegram uses for its own user-facing features. Both can run the same cryptographic guarantees. One can stay up while the other is down.

The public network came back on April 22 when the official TON dependency update merged. Issue #60 closed. Issue #58 is still open as I write this, because the underlying worker connectivity problem never got cleaned up. So is issue #69, opened April 27, which reports that the latest worker image cannot be reproducibly rebuilt from source. That last one matters more than it sounds. If you cannot reproduce the binary, you cannot verify the chip seal points at the right code. New GPU operators are blocked from joining until it gets fixed.

Hype projects do not have public outages because hype projects do not have public infrastructure. COCOON has it. The dependency issues are visible on GitHub. The community patches are visible. The fact that public bots break while a private Telegram path keeps working is visible. All of that tells you more than a press release ever would.

## The honest part

I am not going to pretend this is fully decentralized.

The COCOON team controls the whitelist of approved software. They run all the proxies. They can update the smart contracts. If they decided to push a malicious worker tomorrow, they could.

The transparency you have is that the whitelist lives on chain. Any change is visible. Workers and clients could refuse the upgrade. The network would split along human lines.

That is not nothing. That is not everything either.

Today the privacy is real. Your prompts are cryptographically sealed inside the chip. Tomorrow's governance is a roadmap promise about a DAO. Most crypto projects get this backwards. Theatrical governance, wishful privacy. COCOON has team-controlled governance and actual privacy. I will take that.

## Where I fit in

I built cocoon.doge.tg because the network was opaque from the outside and I wanted to see what was happening. Other people in the COCOON developer community use it now.

I am building cocoon-gpu-pool so small GPU owners can join the network without becoming sysadmins.

I have been in crypto since 2013. I translated Mastering Bitcoin, Mastering Lightning, Programming Bitcoin, and The Blocksize War into Chinese. I have watched enough cycles to know when something is going to matter.

What matters is when a real product needs the crypto. The crypto is never the product. The product is.

The product here is a billion Telegram users getting AI features without giving up privacy, plus a new kind of AI agent that can hold TON, think privately, and act on chain. The TON token is the settlement. The GPU workers are the supply. The TEE chips are the trust anchor. It holds together.

---

Repo: [github.com/TelegramMessenger/cocoon](https://github.com/TelegramMessenger/cocoon)

Contracts: [github.com/TelegramMessenger/cocoon-contracts](https://github.com/TelegramMessenger/cocoon-contracts)

Dashboard: [cocoon.doge.tg](https://cocoon.doge.tg)

The outage thread: [issue #60](https://github.com/TelegramMessenger/cocoon/issues/60)
