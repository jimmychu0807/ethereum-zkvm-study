2025-12-11 22:04:04 From Raúl Kripalani to Everyone:
	p2p team released a first batch of daily notebooks: https://ethp2p.github.io/notebooks/ more coming soon
	
2025-12-11 22:05:03 From Łukasz Rozmej to Everyone:
	@Potuz
	why not save in async?
	
2025-12-11 22:05:10 From Raúl Kripalani to Everyone:
	Should be fine to attest and store async
	
2025-12-11 22:05:26 From Raúl Kripalani to Everyone:
	Attestation duty != custody duty
	
2025-12-11 22:05:51 From Pawan Dhananjay to Everyone:
	Haven’t heard any attestation degradation reports on LH. But I doubt any of our users don’t use decently fast ssds
	
2025-12-11 22:05:54 From Łukasz Rozmej to Everyone:
	if you validated it you DO want to be async
	
2025-12-11 22:06:00 From Dustin to Everyone:
	It's more that until the columns are processed, the block isn't valid
	Cayman Nava:👍
	
2025-12-11 22:08:19 From Manu to Everyone:
	It’s more than serving.
	What about if you attest for a block and save columns async, and the client has a bug preventing it to save columns into disk?
	
	In this case, the data is unavailable, while we voted for the given block
	Satyajit:👍
	
2025-12-11 22:08:39 From Dustin to Everyone:
	then you don't have them
	Manu:👍
	
2025-12-11 22:08:46 From Dustin to Everyone:
	until they have been persisted
	
2025-12-11 22:09:18 From Manu to Everyone:
	Exactly. That’s why IMO we should not declare these columns as available before clearly persisting them to storage
	
2025-12-11 22:09:36 From Manu to Everyone:
	(And so, declare the block as available, and vote for this block)
	
2025-12-11 22:09:37 From Dustin to Everyone:
	It's not about the status message, only
	Manu:👍
	
2025-12-11 22:10:23 From Potuz to Everyone:
	+1 Dustin
	
2025-12-11 22:10:43 From Carl Beekhuizen to Everyone:
	do we have an idea of what is meant when we say these nodes have a slower disk? what kind of disk? any idea of how many nodes?
	
2025-12-11 22:11:08 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	I know of one user’s disk, just a sec
	Carl Beekhuizen:🙏
	
2025-12-11 22:11:50 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	SAMSUNG 870 QVO SATA III SSD 2TB
	
2025-12-11 22:11:52 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	This failed
	
2025-12-11 22:11:53 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	And
	
2025-12-11 22:12:01 From Barnabas to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	QVO’s are very slow.
	
2025-12-11 22:12:00 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	Moving to this fixed
	
2025-12-11 22:12:01 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	SAMSUNG 970 EVO Plus SSD 2TB NVMe M.2
	
2025-12-11 22:12:12 From Barnabas to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	QVOs can be slower than HDDs
	
2025-12-11 22:12:17 From stokes to Everyone:
	https://ethereum-magicians.org/t/2025-upgrade-process-retrospective/27082?u=nixo
	
2025-12-11 22:12:18 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	The point is that this node was performing perfectly since genesis
	
2025-12-11 22:12:23 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	And started failing at Fulu
	
2025-12-11 22:12:33 From Łukasz Rozmej to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	SATA SSD considered to slow? 😱
	
2025-12-11 22:12:40 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	Regardless of having a slower disk, the requirements clearly changed for this node
	
2025-12-11 22:12:49 From Potuz to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	And I have a few others impacted by the same
	
2025-12-11 22:13:10 From Barnabas to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	QVO SSD’s are considered to be slow. Not all SATA SSD is created equal.
	
2025-12-11 22:13:52 From Łukasz Rozmej to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	Max SATA speed, 100k IOPS, looks good to me: https://www.techpowerup.com/ssd-specs/samsung-870-qvo-2-tb.d17
	
2025-12-11 22:14:51 From Barnabas to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	As soon as your SLC cache is exhasted your writes speeds will go down to sub 70MBps
	Łukasz Rozmej:👍
	
2025-12-11 22:15:10 From Carl Beekhuizen to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	also we require nvme drives in 7870. but in general we should have better resolution on the ssd requirements here
	Kalo | Obol:👍
	
2025-12-11 22:15:12 From Barnabas to Everyone:
	Replying to "do we have an idea of what is meant when we say th...":
	max IOPS advertisments are a scam when we are talking about QLC type storage
	Carl Beekhuizen:👍
	
2025-12-11 22:15:13 From alex (ultra sound) to Everyone:
	good summary 👍
	
2025-12-11 22:15:46 From Fredrik to Everyone:
	I propose we include trustless payments
	Justin Florentine (Besu), Barnabas, donnoh | L2BEAT, Poulav, Yassine Ferhane, terence, Fredrik, Stefan Starflinger, Justin Traglia, brunoflashbots, James He, Satyajit, christophschlegel, Milos, kingy_sigp, soispoke, Pawan Dhananjay, Enrico Del Fante (tbenr), hangleang, Mehdi Aouadi, Caleb, Mario Havel, saulius, DA | Flashbots, Bharath:➕
	
2025-12-11 22:18:48 From Justin Florentine (Besu) to Everyone:
	alex makes a good point, warrants addition to the retro thread on eth mag
	alex (ultra sound):❤️
	jochem-brouwer:➕
	
2025-12-11 22:18:53 From Francesco to Everyone:
	Q: there are still proposed modifications to this part of the spec. How long do we expect we need to agree on a final version?
	Carl Beekhuizen, hangleang:👍
	
2025-12-11 22:18:54 From Barnabas to Everyone:
	I think we have already slowed down, and cycled back to this topic 2/3 weeks after we have made a decision.
	
2025-12-11 22:19:23 From alex (ultra sound) to Everyone:
	Replying to "Q: there are still proposed modifications to this ...":
	can i find them somewhere?
	
2025-12-11 22:19:51 From Ansgar Dietrichs to Everyone:
	I think at this point it is likely too late to revisit the decision, at some point it needs to be final.
	
	I will say though, as a point of criticism of the EIP champions, given how important a headliner is, I found remarkably little willingness to consider adjustments earlier into the process. That was a failure in my opinion.
	
2025-12-11 22:20:10 From nflaig to Everyone:
	if we wanna ship the fork fast I think we should split it out into H* and also allow more feedback on the design, if we wanna ship in Glamsterdam we should consider all feedback and take into account that shipping the fork might take longer
	
2025-12-11 22:20:25 From alex (ultra sound) to Everyone:
	existing ecosystem will be disrupted to some extent i think. still researching what it’ll mean exactly but i foresee some real work to be done before we settle in a new equilibrium for how most bids are sourced.
	
2025-12-11 22:20:31 From hangleang to Everyone:
	Replying to "Q: there are still proposed modifications to this ...":
	the sooner we can freeze the spec, the better
	
2025-12-11 22:20:52 From Justin Florentine (Besu) to Everyone:
	Replying to "I think at this point it is likely too late to rev...":
	thats a tough assertion to make. I saw the same, but didn't feel that adjustments were not fairly considered.
	alex (ultra sound):👍
	
2025-12-11 22:21:48 From Justin Traglia to Everyone:
	Replying to "Q: there are still proposed modifications to this ...":
	At least a few weeks from today, given the holidays. There are some things we’d like to explore like staked builders without validator duties so they can make faster deposits. But I would consider these to be optimizations/improvements.
	gd, alex (ultra sound):👍
	
2025-12-11 22:21:51 From Dustin to Everyone:
	Replying to "I think at this point it is likely too late to rev...":
	I thought Potuz was remarkably open to contributions/changes
	kasey:❤️
	Caleb, James He, brunoflashbots, DA | Flashbots, Pawan Dhananjay:➕
	
2025-12-11 22:22:07 From Dustin to Everyone:
	Replying to "I think at this point it is likely too late to rev...":
	It's been ongoing for literally years now
	
2025-12-11 22:22:31 From Potuz to Everyone:
	Replying to "Q: there are still proposed modifications to this ...":
	These are big changes though
	Justin Traglia, donnoh | L2BEAT:👍
	
2025-12-11 22:22:44 From terence to Everyone:
	Replying to "Q: there are still proposed modifications to this ...":
	what changes here?
	
2025-12-11 22:23:09 From christophschlegel to Everyone:
	Replying to "I think at this point it is likely too late to rev...":
	Potuz presented at Flashbots and was very open to our opinion. Seems just that MEV ecosystems and devs operate on different timelines.
	
2025-12-11 22:23:21 From Francesco to Everyone:
	Replying to "Q: there are still proposed modifications to this ...":
	It doesn’t feel like we should be exploring things at this stage though?
	
2025-12-11 22:23:33 From Francesco to Everyone:
	Replying to "Q: there are still proposed modifications to this ...":
	This is exactly why this part of the EIP should have been split up btw...
	donnoh | L2BEAT, alex (ultra sound):😅
	
2025-12-11 22:24:50 From Justin Traglia to Everyone:
	Replying to "Q: there are still proposed modifications to this ...":
	We’re trying to work with builders to make things better for them. Compromises.
	
2025-12-11 22:25:13 From Justin Florentine (Besu) to Everyone:
	i think this is a fair case, but seems like it is an iterable impovement
	Trent, Potuz, Justin Traglia, donnoh | L2BEAT:👍
	
2025-12-11 22:25:34 From Justin Florentine (Besu) to Everyone:
	i.e. ePBS 1.1 in a later release
	
2025-12-11 22:25:48 From Justin Traglia to Everyone:
	Call out for the next ePBS breakout call: https://github.com/ethereum/pm/issues/1835
	
2025-12-11 22:26:17 From Francesco to Everyone:
	In Fusaka we spent ~a month discussing getBlobsv2 versus v3, which was a tiny decision. I am worried that this will drag on, especially since it’s not an isolated decision, it affects builders, it touches a few things in the spec etc...
	Trent:👍
	
2025-12-11 22:27:00 From Quintus Kilbourn to Everyone:
	Taking away the 32 eth minimum and allowing deposits to go faster keep capital requirements down which is good for keeping the market easy to participate in
	
	The only other major thing to say is that we really should do as much as possible to avoid using p2p for bidding (great for WW3 but undesirable for everything else)
	alex (ultra sound), christophschlegel, gd:👍
	
2025-12-11 22:27:00 From Francesco to Everyone:
	I am sure we’ll sort it out eventually, just doesn’t seem like a small thing
	
2025-12-11 22:27:08 From Potuz to Everyone:
	Replying to "In Fusaka we spent ~a month discussing getBlobsv2 ...":
	My general take on these things is to by default not change things like this big when they are already in motion, but at least we want to be exploring how bad of a change it would be
	
2025-12-11 22:27:18 From Potuz to Everyone:
	Replying to "In Fusaka we spent ~a month discussing getBlobsv2 ...":
	It could be this fork or the next one to iterate over this
	
2025-12-11 22:27:19 From Justin Traglia to Everyone:
	Replying to "In Fusaka we spent ~a month discussing getBlobsv2 ...":
	There was also the new transaction type, with cell proofs instead of blob proofs. That happened pretty late into the process, no?
	
2025-12-11 22:27:41 From Francesco to Everyone:
	Replying to "In Fusaka we spent ~a month discussing getBlobsv2 ...":
	That happened a few months before Pectra went live
	Justin Traglia:👍
	
2025-12-11 22:28:33 From Trent to Everyone:
	Replying to "I think at this point it is likely too late to rev...":
	Bump for @Ansgar Dietrichs to reaccess after dropping
	
2025-12-11 22:28:53 From Potuz to Everyone:
	Replying to "Taking away the 32 eth minimum and allowing deposi...":
	I think we want to have a decentralized set of p2p bids, they aren’t really a problem for the proposer cause they’ll get just a few of them already heavily filtered
	Justin Traglia:👍
	
2025-12-11 22:31:06 From gd to Everyone:
	Replying to "Taking away the 32 eth minimum and allowing deposi...":
	I think as long as there is an alternative path to validators p2p is fine. p2p becomes a bit more of a concern if it is the only option to reach a large enough portion of the stake
	Bharath:👍
	
2025-12-11 22:31:34 From Justin Florentine (Besu) to Everyone:
	so that framing is assuming that the door is closed to FOCIL in Glamsterdam.
	
2025-12-11 22:33:16 From Toni Wahrstätter to Everyone:
	Replying to "so that framing is assuming that the door is close...":
	Which is reasonable, imo. ePBS and BALs are both big features and we shouldn't do 3 big features in one fork.
	
2025-12-11 22:33:44 From nixo to Everyone:
	it’s not ‘process for the sake of process’ - client teams have indicated support for focil in glamsterdam but the rest of the ecosystem hasn’t really had the opportunity. they need a ‘process’ to know how to participate
	Trent:👍
	
2025-12-11 22:34:00 From Justin Florentine (Besu) to Everyone:
	Replying to "so that framing is assuming that the door is close...":
	i agree, but disagree that we can't take more time on a fork. i'm not sold on the 6th month cadence being inviolable.
	
2025-12-11 22:34:40 From nixo to Everyone:
	imo FOCIL should be CFI’d and then we should set this heka(?) timeline definitively so there’s clarity
	
2025-12-11 22:35:03 From Francesco to Everyone:
	Replying to "I think at this point it is likely too late to rev...":
	Two things are true imo:
	Potuz (and Terence and others) did a great job at engaging with all kinds of stakeholders repeatedly, over a long period of time.
	Earlier in the decision process, the discussion became more charged than it should have been because people felt that trustless payments would either get in now (on the wave of epbs’ scalability features) or never (similar dynamics to FOCIL). People saying that trustless payments could be split from the other features and raising concerns around complexity were ignored or treated as lacking Ethereum values
	alex (ultra sound):👀
	Lorenzo, Cayman Nava:👍
	
2025-12-11 22:35:22 From Ansgar Dietrichs to Everyone:
	I personally would be open to FOCIL sfi now - but I don’t think it would be appropriate to assign it headliner status before starting the headliner process. That would be unfair to other potential headliner proposals.
	
	In practice ofc good chance we then decide there is no room for a(nother) CL headliner, but that should be during a proper headliner scoping process.
	Greg K |Lido, soispoke, Cayman Nava:👍
	
2025-12-11 22:35:45 From soispoke to Everyone:
	Replying to "I personally would be open to FOCIL sfi now - but ...":
	So SFI FOCIL as a non headliner?
	
2025-12-11 22:35:49 From Trent to Everyone:
	I dont know if we need to argue the benefits of FOCIL (or any other specific EIP) right now, we’re talking about the process
	Potuz, terence, Toni Wahrstätter, Vukasin, Mehdi Aouadi:👍
	
2025-12-11 22:36:06 From Ansgar Dietrichs to Everyone:
	Replying to "I personally would be open to FOCIL sfi now - but ...":
	SFI without assessment of whether headliner
	soispoke, Greg K |Lido:👍
	
2025-12-11 22:36:21 From Justin Florentine (Besu) to Everyone:
	one users censorhip is another users whitehat rescue
	
2025-12-11 22:36:33 From Francesco to Everyone:
	Replying to "I think at this point it is likely too late to rev...":
	(And to be clear I am not saying that this was all on Potuz or someone, it’s partially just the governance dynamics that can turn unhealthy when something is contentious)
	alex (ultra sound):👍
	
2025-12-11 22:36:36 From Potuz to Everyone:
	FWIW, that paper doesn’t talk about FOCIL but MCP
	
2025-12-11 22:36:48 From Potuz to Everyone:
	The assumptions on FOCIL are different 🙂
	donnoh | L2BEAT:👍
	
2025-12-11 22:36:51 From terence to Everyone:
	Yes
	
2025-12-11 22:36:54 From terence to Everyone:
	It's very different
	
2025-12-11 22:37:02 From nixo to Everyone:
	Replying to "I personally would be open to FOCIL sfi now - but ...":
	the first SFI is the headliner status
	
2025-12-11 22:37:03 From Dustin to Everyone:
	Yeah, I don't think right now the argument was, is FOCIL good or not?
	
2025-12-11 22:37:18 From Dustin to Everyone:
	There wasn't really disagreement about that part
	
2025-12-11 22:37:22 From Potuz to Everyone:
	Except Matt 🙂
	
2025-12-11 22:38:05 From Ansgar Dietrichs to Everyone:
	Replying to "I personally would be open to FOCIL sfi now - but ...":
	It’s less of a bending of process to change that than to decide to skip an entire headliner process
	
2025-12-11 22:38:11 From Toni Wahrstätter to Everyone:
	Feels like the majority agrees that focil has merits. 
	This doesn't change the fact that glamsterdam is already big and that we should stick to the process to give other eips the same chance as focil in heka.
	Martin | ethrex, Stefan Starflinger, felipe:👍
	
2025-12-11 22:38:48 From nixo to Everyone:
	Replying to "I personally would be open to FOCIL sfi now - but ...":
	i don’t understand your message
	
2025-12-11 22:39:03 From Francesco to Everyone:
	Replying to "I personally would be open to FOCIL sfi now - but ...":
	The process is a few months old, so not exactly sacred, and at the end of the day the process serves Ethereum goals, not itself. Still, SFIng something before the headliner process makes the headliner process fairly meaningless
	nixo:👍
	
2025-12-11 22:40:17 From donnoh | L2BEAT to Everyone:
	the takeway i wanted to give is that i dont think we should SFI until we properly understand the benefits and right now we dont 😅
	
2025-12-11 22:40:28 From Dustin to Everyone:
	Yeah, also I think we'd already agreed that it's nto in Glamsterdam, that's not the point of contention anymore
	
2025-12-11 22:40:35 From Barnabas to Everyone:
	I have really bad deja vu about FOCIL discussion.
	soispoke:👍
	
2025-12-11 22:40:51 From Dustin to Everyone:
	It's literally, is it CFI or SFI
	
2025-12-11 22:40:51 From Toni Wahrstätter to Everyone:
	Replying to "There wasn't really disagreement about that part":
	We should also involve rollups in the discussion who will not get CR from focil, or at least flag it publicly that blobs won't be supported.
	Same for starks that are too big to fit into ILs.
	Talking to people, this isn't clear to many.
	donnoh | L2BEAT:💯
	
2025-12-11 22:40:52 From Dustin to Everyone:
	that's t
	
2025-12-11 22:41:32 From Potuz to Everyone:
	Do we have a timeline set for the headliner process? Tim had posted for Gloas a list of dates up until when the headliner would be accepted for being proposed, when discussions started, etc.
	
2025-12-11 22:41:50 From Caspar Schwarz-Schilling to Everyone:
	To me the clear take-away from last ACDC was that clients should decide whether to SFI FOCIL for Heka on this ACDC. Why change this now? Let‘s just do this.
	kingy_sigp, Potuz, Ladislaus, Stefan Starflinger, donnoh | L2BEAT:👍
	
2025-12-11 22:41:50 From Toni Wahrstätter to Everyone:
	Replying to "Do we have a timeline set for the headliner proces...":
	Process-wise, january?
	
2025-12-11 22:41:58 From Ansgar Dietrichs to Everyone:
	But we considered it very seriously as a non headliner for Glamsterdam as of 2 calls ago
	
2025-12-11 22:42:15 From Potuz to Everyone:
	Replying to "Do we have a timeline set for the headliner proces...":
	If we’re talking about a couple of weeks then it’s probably a waste of time anyway to decide anything now?
	Cayman Nava:👍
	
2025-12-11 22:42:21 From nixo to Everyone:
	Replying to "Do we have a timeline set for the headliner proces...":
	https://ethereum-magicians.org/t/eip-8081-heka-bogota-network-upgrade-meta-thread/26876
	
2025-12-11 22:42:28 From Ansgar Dietrichs to Everyone:
	Replying to "But we considered it very seriously as a non headl...":
	I do agree it will likely end up as a headliner. That’s fine. No need to make that decision today
	
2025-12-11 22:42:47 From nflaig to Everyone:
	FOCIL is orders of magnitude simpler than peerdas or epbs so I think it can definitely be a non headliner
	Ansgar Dietrichs, Mehdi Aouadi, soispoke, Justin Florentine (Besu), Francesco, Vukasin:👍
	
2025-12-11 22:42:50 From Justin Florentine (Besu) to Everyone:
	Replying to "But we considered it very seriously as a non headl...":
	sorta. i think that consideration hinges very much on a non 6-month cadence.
	
2025-12-11 22:42:51 From Francesco to Everyone:
	That is something I agree with though, the headliner process is a bit weird with things like FOCIL
	
2025-12-11 22:43:15 From Potuz to Everyone:
	Replying to "Do we have a timeline set for the headliner proces...":
	Oh thanks Nixo, I looked in forkcast instead 😅
	
2025-12-11 22:43:22 From sean to Everyone:
	can we do FOCIL + shorter slots?
	Justin Florentine (Besu):👎
	
2025-12-11 22:43:26 From Ansgar Dietrichs to Everyone:
	Replying to "But we considered it very seriously as a non headl...":
	Right, but as of 2 calls ago that seemingly was a price we were potentially willing to pay
	soispoke:👍
	
2025-12-11 22:43:51 From Potuz to Everyone:
	Replying to "can we do FOCIL + shorter slots?":
	I honestly disagreed with Pari here, I think FOCIL+shorter slots is even harder than FOCIL+ePBS
	
2025-12-11 22:44:06 From NC to Everyone:
	Replying to "FOCIL is orders of magnitude simpler than peerdas ...":
	I don’t think a headliner candidate is weighted by its technical complexity but the influence it brings
	
2025-12-11 22:44:06 From soispoke to Everyone:
	FOCIL is definitely not as big as ePBS though
	Potuz:👍
	
2025-12-11 22:44:25 From Toni Wahrstätter to Everyone:
	Replying to "can we do FOCIL + shorter slots?":
	I feel like we shouldn't, with regards to complexity
	
2025-12-11 22:44:35 From Justin Florentine (Besu) to Everyone:
	Replying to "But we considered it very seriously as a non headl...":
	yes, and so that would be the process point we should be discussing
	
2025-12-11 22:44:37 From donnoh | L2BEAT to Everyone:
	Replying to "There wasn't really disagreement about that part":
	it’s not only starks but some fraud proof system bisection steps can also be very big
	
2025-12-11 22:44:58 From sean to Everyone:
	basically SFI'ing FOCIL without it being explicit about what we're giving up is basically the problem with doing it early
	
	people def want shorter slots and we would be avoiding that discussion
	Chris Haug, Justin Florentine (Besu), Martin | ethrex:👍
	
2025-12-11 22:45:36 From Toni Wahrstätter to Everyone:
	Replying to "There wasn't really disagreement about that part":
	Right, should we sfi anything before those affected (by not being able to use it) are at least aware?
	
2025-12-11 22:45:47 From Mehdi Aouadi to Everyone:
	SFI it now and do the headliner process. If FOCIL is selected as headliner , fine, otherwise let it be just SFI’ed with another headliner. At least that way we will be carefull while selecting the hadliner if not FOCIL
	soispoke, Mario Havel, Caspar Schwarz-Schilling, nflaig, Ladislaus, Marc, Ansgar Dietrichs:👍
	
2025-12-11 22:45:55 From Fredrik to Everyone:
	How much time are we talking of between now and being able to SFI according to the process?
	
2025-12-11 22:46:05 From stokes to Everyone:
	Replying to "How much time are we talking of between now and be...":
	A few weeks
	
2025-12-11 22:46:17 From soispoke to Everyone:
	Replying to "SFI it now and do the headliner process. If FOCIL ...":
	Yeah exactly
	
2025-12-11 22:46:24 From Potuz to Everyone:
	Replying to "How much time are we talking of between now and be...":
	Yeah that was my question above, I think this all may be a waste of time if in a couple of weeks we’ll be discussing anyway
	Christine Kim, donnoh | L2BEAT:👍
	
2025-12-11 22:46:42 From Ansgar Dietrichs to Everyone:
	Replying to "But we considered it very seriously as a non headl...":
	Well yes, but I would argue not today as part of Glamsterdam scoping. We should keep the process deviation minimal. CFI or SFI FOCIL for H* today, then have the conversation about headliner status and 6-month cadence during the H* headliner process we will start soon
	
2025-12-11 22:47:21 From sean to Everyone:
	Replying to "SFI it now and do the headliner process. If FOCIL ...":
	I was just trying to get at, you can't do this with some EIPs if another one is complicated enough, for example shorter slots people don't think is possible
	
2025-12-11 22:47:31 From sean to Everyone:
	Replying to "SFI it now and do the headliner process. If FOCIL ...":
	you can't really make the decision in isolation
	
2025-12-11 22:47:34 From Francesco to Everyone:
	Given that most people do want FOCIL to be included and there’s this history etc… and the headliner process is soon, why not just follow the process and select it in a few weeks that way though, rather than bending the process?
	terence, Christine Kim, Toni Wahrstätter, Cayman Nava, Martin | ethrex, donnoh | L2BEAT:👍
	
2025-12-11 22:47:58 From Francesco to Everyone:
	Replying to "Given that most people do want FOCIL to be include...":
	(At the moment it also seems clear that it is much farther than 6SS in maturity, so it seems hard to imagine that this is not what we end up with in a few weeks)
	
2025-12-11 22:48:00 From Barnabas to Everyone:
	Can we timebox FOCIL pls?
	Trent, Stefan Starflinger, Martin | ethrex:👍
	
2025-12-11 22:48:03 From Trent to Everyone:
	We are again edging into retrospective discussions about process over the last few weeks instead of moving forward
	
	Can we agree to just start the broader process ASAP?
	Will Corcoran:👍
	
2025-12-11 22:48:05 From Potuz to Everyone:
	To me the bulliest case of FOCIL I have heard is that the current censors want it so that they can stop censoring
	soispoke, Subhasish, Mario Havel, Milos:👍
	
2025-12-11 22:48:33 From Mario Havel to Everyone:
	FOCIL is something really worth committing to and should not become a victim of the process we are still discovering
	soispoke, Milos, Vukasin:👍
	
2025-12-11 22:49:16 From Toni Wahrstätter to Everyone:
	It has the size of a headliner and we've closed glamsterdam. 
	The next step would be doing the headliner process in a few weeks and see where we land by then.
	Chris Haug:👍
	
2025-12-11 22:49:46 From terence to Everyone:
	TIL:  txns from the public mempool that are included in some blocks, 17% are not in the winning block. (https://x.com/soispoke/status/1999095871435137396 )
	
2025-12-11 22:49:54 From Toni Wahrstätter to Everyone:
	Replying to "FOCIL is something really worth committing to and ...":
	That's your take. Others will say differently or have different priorities
	Carl Beekhuizen:👍
	
2025-12-11 22:50:15 From soispoke to Everyone:
	Replying to "TIL:  txns from the public mempool that are includ...":
	Caveat dataset is small but yeah was surprised by this as well
	
2025-12-11 22:50:15 From Mehdi Aouadi to Everyone:
	Replying to "SFI it now and do the headliner process. If FOCIL ...":
	that reasoning doesn’t take into account that FOCIL already got more support than any new EIP, has a working prototype and has already been discussed… It’s unfait to just forget everything and reset the process
	soispoke:👍
	
2025-12-11 22:50:43 From Francesco to Everyone:
	Replying to "SFI it now and do the headliner process. If FOCIL ...":
	These things are still true though, and why very likely FOCIL will be selected
	
2025-12-11 22:50:44 From sean to Everyone:
	We can put resources into it regardless of SFI status
	
2025-12-11 22:50:47 From Cayman Nava to Everyone:
	imo focil can't make additional engineering progress until its rebased on ePBS (likely lines up with headliner process?)
	Justin Florentine (Besu):➕
	
2025-12-11 22:51:02 From Francesco to Everyone:
	Replying to "SFI it now and do the headliner process. If FOCIL ...":
	It’s not that in a few weeks we forget that it has a lot of support and mature specs etc…
	
2025-12-11 22:51:15 From Toni Wahrstätter to Everyone:
	Replying to "To me the bulliest case of FOCIL I have heard is t...":
	Yeah, bc we shift responsibility to validators. You should talk to them too.
	donnoh | L2BEAT:👍
	
2025-12-11 22:51:20 From Justin Florentine (Besu) to Everyone:
	Replying to "We can put resources into it regardless of SFI sta...":
	thats not generally realistic, existing SFI'd features tax teams.
	
2025-12-11 22:52:16 From Ansgar Dietrichs to Everyone:
	So seems like we have 3 options:
	​CFI
	SFI without deciding whether headliner
	SFI as headliner
	alex (ultra sound), soispoke, Hsiao-Wei Wang:👍
	
2025-12-11 22:52:18 From soispoke to Everyone:
	I think the same arguments were said for Glamsterdam: we all like FOCIL but we don’t want to include it now
	
2025-12-11 22:52:20 From terence to Everyone:
	Side comment: FOCIL on top of epbs may not be as simple as ppl think. The forkchoice part is the biggest complexiity. Epbs add the notion of empty payload slot so when focil reorg, it needs to consider parent hash as well
	Cayman Nava:👍
	
2025-12-11 22:52:34 From sean to Everyone:
	Replying to "We can put resources into it regardless of SFI sta...":
	Usually SFI'd stuff is further along tbf.. but we've already had Eitan on our team implement it and I'm sure he'd love to keep pushing it
	
2025-12-11 22:53:05 From nflaig to Everyone:
	Replying to "It has the size of a headliner and we've closed gl...":
	how is "size" defined here, in terms of LoC it's tiny (~2,5k) compared  to other headliners like peerdas which was 20k+ LoC
	
2025-12-11 22:53:27 From Francesco to Everyone:
	Replying to "I think the same arguments were said for Glamsterd...":
	I think the clear message is that we do want to include it now (for H*, as a headliner) though
	
2025-12-11 22:53:28 From Pawan Dhananjay to Everyone:
	Replying to "We can put resources into it regardless of SFI sta...":
	Tbf, 6 second slot is heavier on the testing side than implementation side
	
2025-12-11 22:53:36 From Trent to Everyone:
	Can we please timebox
	
2025-12-11 22:53:37 From Justin Florentine (Besu) to Everyone:
	Replying to "We can put resources into it regardless of SFI sta...":
	huh, see i see the fact that multiple clients have focil impls means it should be SFId to reflect reality
	
2025-12-11 22:53:38 From DA | Flashbots to Everyone:
	Replying to "TIL:  txns from the public mempool that are includ...":
	I don’t trust those stats. Certainly not reflective of what I’ve seen. Unless these are just late tx or 0 fee spam.
	
2025-12-11 22:53:46 From stokes to Everyone:
	Replying to "Can we please timebox":
	Yes, Matt and then that’s that
	
2025-12-11 22:54:40 From soispoke to Everyone:
	We’ve been discussing for 5 months though
	Mehdi Aouadi:😫
	
2025-12-11 22:54:53 From DA | Flashbots to Everyone:
	Replying to "TIL:  txns from the public mempool that are includ...":
	Would be curious to look at our internal stats.
	
2025-12-11 22:55:00 From Toni Wahrstätter to Everyone:
	Replying to "It has the size of a headliner and we've closed gl...":
	Touches both layers, adds extra deadlines, complex, many small "hacks"  (no blobs, no fraud proof support (too large, no stark support). Those things add up.
	
	donnoh | L2BEAT:👍
	
2025-12-11 22:55:07 From sean to Everyone:
	Replying to "We can put resources into it regardless of SFI sta...":
	there's a 6s impl too though
	
2025-12-11 22:55:24 From Ansgar Dietrichs to Everyone:
	So how are we going to make the decision today?
	soispoke:👍
	
2025-12-11 22:55:30 From Jihoon to Everyone:
	Replying to "Side comment: FOCIL on top of epbs may not be as s...":
	Good point, it's something we want to keep in mind. But still it doesn't change that FOCIL is probably the most mature and well-researched EIP among headliner candidates
	
2025-12-11 22:56:22 From nixo to Everyone:
	+1, ‘process’ should be independent of what we want, that’s the only way we have a good process. we can’t adjust the process based on what specific features we want in the next fork, that creates a bad process
	
