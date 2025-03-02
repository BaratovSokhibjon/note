#bittensor #programming #redteam #concepts
## Communication
- **Synapse**: A message format for sending requests and replies between neurons.
- **Dendrite**: The "sender" part of a neuron, asks miners for work (mostly used by validators).
- **Axon**: The "receiver" part of a neuron, answers requests (mostly used by miners).
- **Metagraph**: A big table showing the current state of a subnet, like a snapshot of all its nodes (validators and miners). It lists details like stakes, weights, and rewards to help calculate who gets TAO emissions.

![[Pasted image 20250223174954.png]]

## Biology vs Client-Server

- **Biological Terms**: Bittensor uses "neuron," "dendrite," and "axon" from biology—where dendrites receive signals and axons send them.
- **Client-Server Twist**: In Bittensor, it’s flipped—**Dendrite** acts as a client (sends requests), and **Axon** acts as a server (receives and replies), matching a network model instead of biology.****

## Token Stuff
- **TAO**: The main token of Bittensor, used for staking, rewards, and fees.
- **Dynamic TAO**: TAO supply changes—burned (removed) or minted (added)—based on network needs.
- **Alpha Token**: A token for a specific subnet, priced by TAO in Reserve ÷ Alpha in Reserve.
- **Alpha Outstanding**: Alpha tokens held by participants, not in the reserve.
- **Burning**: Removing TAO (e.g., to create a subnet) to keep supply in check.
- **Minting**: Adding new TAO as rewards for good work.

## Time & Blockchain
- **Block**: A 1-minute record on the blockchain, tracking network actions (720 blocks ≈ 12 hours).
- **Epoch**: A work cycle (e.g., 12 hours) where tasks happen and rewards are given. It is dependent on subnet's settings.
- **Tempo**: A subnet’s setting for how many blocks make an epoch (e.g., 720 blocks = 12 hours). Controls how fast a subnet updates rewards and weights—each subnet can pick its own.
## Rewards System
- **Yuma Consensus**: Decides how TAO rewards are split using validator scores and stake.
  - Validators score miners → weights.
  - Stake adjusts influence → consensus weights.
  - TAO split: miners get most, validators get a dividend (e.g., 18%).
- **Yuma Rao**: Mystery person (or name) behind Bittensor’s ideas and whitepaper.
- **Weights**: Scores validators give miners for their work.
- **Stake**: TAO locked by validators or miners to join in and gain influence.
- **Consensus:** a general agreement.
-  **Root Network**: Subnet 0, the “boss” subnet that manages subnet creation and overall network rules. - **Role**: Validators here decide which subnets get TAO emissions, using Yuma Consensus.
## Subnet Details
- **Reserve**: Holds TAO and Alpha tokens; their ratio sets Alpha price.
- **Hotkey**: A node’s ID, holds TAO or Alpha for participating.
- **Coldkey**: A secure key in your Bittensor wallet, used for big actions like holding TAO, transferring funds, or staking.

## Dynamic TAO

- [White Paper](https://drive.google.com/file/d/1vkuxOFPJyUyoY6dQzfIWwZm2_XL3AEOx/view)
 - Each subnet has two liquidity reserves
	 1.  TAO (τ)
	 2.  Alpha token (α) 
- Subnet's economy consists of three pools of currency
	1. TAO reserves - the amount of TAO that has been staked into subnet
	2. Alpha token reserves - the amount of alpha available for purchase
	3. Alpha outstanding - amount of alpha held in hotkeys of the members of the subnet 

> The price of subnet's alpha token is determined by
$$
  \text{Price of Alpha Token (in TAO)} = \frac{\text{TAO in Reserve}}{\text{Alpha in Reserve}} 
$$


## Alpha Tokens Workflow Example

- Setup
	- **Subnet 5**: Starts with 1,000 TAO and 500 Alpha in reserve (price = 2 TAO/Alpha).
	- **You**: Have 100 TAO, stake 50 TAO.

- Step 1: Earn Alpha
	- Stake 50 TAO → Get 25 Alpha (50 ÷ 2).
	- Reserve: 1,050 TAO, 475 Alpha (price → 2.21 TAO/Alpha).

- Step 2: Subnet Grows
	- Others stake 450 TAO → 203 Alpha out.
	- Reserve: 1,500 TAO, 272 Alpha (price → 5.51 TAO/Alpha).

- Step 3: Redeem TAO
	- Unstake 25 Alpha → Get 137.75 TAO (25 × 5.51).
	- Reserve: 1,362.25 TAO, 297 Alpha (price → 4.59 TAO/Alpha).

- Step 4: Cash Out
	- Sell 137.75 TAO at $500/TAO = $68,875 USD.
	- After fees (~$137), net $68,737 USD.

- Takeaway
	- Started with 100 TAO → Ended with 50 TAO + $68,737 USD by staking smart!