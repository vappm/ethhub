# Eth 2.0 Economics

### Introduction

The Ethereum Serenity upgrade will bring with it a switch from Proof of Work to Proof of Stake. This means that rather than pay miners to secure the network, we will be paying validators to secure the network. It's vitally important to get the economics of staking right so that the network stays healthy and secure.   
  
If the incentive to stake is too low, the network will not get the minimum amount of validators needed to keep many shards going. If the incentive is too high, the network is overpaying for security and inflating at a rate that is detrimental to the economics of the network as a whole.

Currently, the latest spec is targeting 1024 shards. Ideally, each shard would have a committee size of 256. This means the network needs 262,144 validators. That equates to 8,388,608 total ETH at stake on the network. 

### Terms 

NOTE: Some of these are taken from [https://github.com/ethereum/eth2.0-specs/blob/master/specs/core/0\_beacon-chain.md](https://github.com/ethereum/eth2.0-specs/blob/master/specs/core/0_beacon-chain.md)

* **Validator** - a participant in the Casper/sharding consensus system. You can become one by depositing 32 ETH into the Casper mechanism.
* **Committee** - a \(pseudo-\) randomly sampled subset of active validators. When a committee is referred to collectively, as in "this committee attests to X", this is assumed to mean "some subset of that committee that contains enough validators that the protocol recognizes it as representing the committee".
* **Withdrawal period** - the number of slots between a validator exit and the validator balance being withdrawable.
* **Inflation** - The rate at which ETH supply grows year over year.
* **Interest** - The annualized rate at which validators are rewarded \(in ETH\).

### Validator Economic Incentive

There are many things that a user will consider when wanting to become a validator. In the base case, some users may believe in the Ethereum network so much that they would stake a loss if need be. A good example of this is the 12,000 Ethereum nodes running today. However, in the simplest case we can break down the thought process as follows:  
  
Total Incentive to Stake = Validator Rewards + Network Fees - Cost to run a Validator   
  
\*One factor discussed later that validators will consider as well is competition.

### Staking Rewards

In order to incentivize those that have ETH to stake in the network, there must be some type of incentive. It's unlikely that anyone would stake their ETH for no reward. Serenity accomplishes this by paying validators a reward for every block they successfully propose. In the [latest spec](https://github.com/ethereum/eth2.0-specs/blob/master/specs/core/0_beacon-chain.md) this is a sliding scale based on total network stake. So if total ETH stake is low, the interest rate goes up and as stake rises, it starts to fall.   
  
We can calculate this scale using the spec. There are a lot of variables in doing this. First up are the **constants**:

| **Constant** | Value |
| :--- | :--- |
| ETH stake | 32 |
| Shards | 1024 |
| Slot time | 6 |
| Epoch Length | 64 |
| Base Reward Quotient | 2048 |

From here we can start to calculate the **outputs** using a single **assumption** which is **total network stake.** \(Let's assume 10,000,000 in the example\)

| **Output** | Calculation |
| :--- | :--- |
| Network Validators | 10000000/32 = 312,500 |
| Validators/Shard | 10000000/\(32\*1024\) = 305 |
| Blocks/epoch | 31536000/\(6\*64\) = 82125 |
| Reward Quotient | 2048\*INT\(SQRT\(10000000\)\) = 6475776 |
| Reward/epoch | 10000000/6475776\*2 = 3088 |
| Generated ETH/Year | 82125\*3088 = 253638 |
| Validator Interest/Year | 253638/100000000 = 2.54% |
| Inflation/Year | 253638/104000000 = 0.24% |

So here we can see that with 10,000,000 total network stake, validators are gaining 2.54% a year and the network is inflating at 0.24% a year. We can now take these formulas and generate the sliding scale:

| Total Network Stake | Validator Interest | Network Inflation |
| :--- | :--- | :--- |
| 1,000,000 | 8.02% | 0.08% |
| 2,000,000 | 5.67% | 0.11% |
| 3,000,000 | 4.63% | 0.13% |
| 5,000,000 | 3.59% | 0.17% |
| 10,000,000 | 2.54% | 0.24% |
| 20,000,000 | 1.79% | 0.34% |
| 30,000,000 | 1.46% | 0.42% |
| 50,000,000 | 1.13% | 0.55% |
| 100,000,000 | 0.80% | 0.77% |

### Fees

One thing the above does not consider is staking fees. Validators will also be taking in a cut of the total fees that users are paying to use the network. This is one area that needs more research but currently, the Ethereum network is paying about 600 ETH a day in fees. At current rate that's 219,000 ETH a year. How this will scale up as we add shards and throughput to the network will be important because it goes into the reward calculation for validators. 

### Staking Costs and Risks

Validating and earning rewards is not a free lunch. There are many things to consider for one to become a validator. These considerations will be considered by every validator when considering if the staking rewards are "worth it". They are:

* Computing cost
  * Users will need to run validators clients at a minimum and likely a beacon node as well. This requires computing resources. The specifics around how much as far as HDD, bandwidth and more are still being figured out.
    * Beacon Node: similar to running geth/parity today
    * Validator client: lightweight and need one per 32 ETH stake
* Capital acquisition and lockup
  * The user must have acquiring their ETH capital by some means of work.
  * The user must lock up their capital for a set amount of time \(withdrawal period\). This brings volatility risk and lost opportunity cost elsewhere.
* Code Risk
  * With any smart contract on Ethereum, the user must trust that the contract cannot be compromised. Early on this will be a bigger concern for some.
* General uptime and maintenance cost
  * Users need to make sure their validator doesn't have downtown or they risk a quadratic leak on their stake.
  * If a user has multiple validators, maintenance cost and worry of the infrastructure comes into play.

### Competition

A very important factor in determining if staking ETH is worth it is comparing the net reward versus competition. In the case of ETH, the best competition to compare to are decentralized finance applications such as Compound Finance, Dharma, Maker, and many others.  
  
Just like staking, these applications offer ways for users to lock up ETH and gain a reward \(interest\).
