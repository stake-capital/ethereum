Implementation of the Ethereum Constantinople Fork Timer (ECFT)

## Live 

https://www.stake.capital

## Example 
```
Time Until Ethereum Constantinople: 02 days 07 hours 39 minutes 51 seconds
Thu Feb 28 2019 21:44:08 GMT+0100 (Central European Standard Time)
View Research
```

## Research 

Ethereum Fork Timer 2.0 - Stake Capital

https://docs.google.com/spreadsheets/d/1c1GMS8sLSAorMblsXvg_iPzNfAg_Y5F-y5N4D-IDgj4/edit?usp=drive_web&ouid=113559707563988463959

Medium's post: https://medium.com/stakecapital/ethereum-constantinople-fork-timer-ecft-d0edfd34c8f0

## How

In order to provide a smooth transition for the upcoming Ethereum PoS algorithm, an « Ice age » has been introduced by the EIP1234 leading to an exponential overall difficulty increase, whence mining reward less profitable. Furthermore, the client will calculate the difficulty based on a fake block number « suggesting the difficulty bomb is adjusting around 5 million blocks later than previously » which represent a 1100 days delay. 

As implemented, the difficulty is under the aegis of 3 parameters: 

```« current_block_difficulty = int(2**((fake_block_number // 100000) — 2)) + parent_block_difficulty + (parent_block_difficulty // 2048) * max(1 — (block_time// 10), -99) »```

1. The bomb’s ticks, increasing the difficulty with an exponential rate. 

2. The parent block time difficulty, evolving regarding the bomb’s ticks.

3. The leverage, making the network able to re-estimate the difficulty regarding congestion or mining popularity.

The timestamps and the difficulty represent the sensitives inputs of the counter, it must be more precise than just an average around last block timestamp.
For the counter accuracy, regarding the magnitude order, we agreed that a 2000blocks average is the network safeguard snapshot (as the last 100block stats aren’t the absolute law, it’s the bigger range which is not impacting more than 1% the global growth of the block difficulty).
As for the global exponential difficulty, the global uncertainty of the network is also growing exponentially, leading the statistics of an expected congestion/mining or average timestamp less reliable. 


### Technical Note

An average of both block difficulty and block time from the past `x` blocks is used to calculate the estimate. The more consistent / accurate this average is, the stronger the overall estimate. These block difficulty / time averages can either be created on the fly (e.g. past 100 blocks) or generated before and stored as static data in the component. We decided to go with generating the averages using the past 2000 blocks and storing it as a static result in the component. This is because generating the average for more than about the last 100 blocks begins to create browser lag (the average is generated at page load on the client-side). Obviously, one solution would be to generate the averages on the backend. However, we decided that, for now, using the last 2000 blocks (< ~1% change in exponential growth of block difficulty) would provide a sufficient estimate—and yield far more accuracy than creating the averages from 100 blocks, even though this estimate would be on the fly (100 most recent blocks vs older static data for 2000 blocks).
