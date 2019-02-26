Implementation of the Ethereum Constantinople Fork Timer (ECFT)

## Research 

Ethereum Fork Timer 2.0 - Stake Capital

https://docs.google.com/spreadsheets/d/1c1GMS8sLSAorMblsXvg_iPzNfAg_Y5F-y5N4D-IDgj4/edit?usp=drive_web&ouid=113559707563988463959

## How

### Technical Note

An average of both block difficulty and block time from the past `x` blocks is used to calculate the estimate. The more consistent / accurate this average is, the stronger the overall estimate. These block difficulty / time averages can either be created on the fly (e.g. past 100 blocks) or generated before and stored as static data in the component. Iimbo and I discussed the merits of both options, and decided to go with generating the averages using the past 2000 blocks and storing it as a static result in the component. This is because generating the average for more than about the last 100 blocks begins to create browser lag (the average is generated at page load on the client-side). Obviously, one solution would be to generate the averages on the backend. However, we decided that, for now, using the last 2000 blocks (< ~1% change in exponential growth of block difficulty) would provide a sufficient estimate—and yield far more accuracy than creating the averages from 100 blocks, even though this estimate would be on the fly (100 most recent blocks vs older static data for 2000 blocks).
