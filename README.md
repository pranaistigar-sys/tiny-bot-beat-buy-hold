# tiny-bot-beat-buy-hold

[![make.fun · u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1?utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![rank](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/pranaistigar-sys/tiny-bot-beat-buy-hold/rank.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/leaderboard?utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)
[![reward](https://make.fun/badge/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1/pranaistigar-sys/tiny-bot-beat-buy-hold/reward.svg)](https://make.fun/bounty/u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1?utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1)

A tiny paper-trading bot for hourly Solana bars that beats buy-and-hold by buying dips and sitting in cash the rest of the time.

## Strategy

On 1h bars the seven SOL-ecosystem names (FARTCOIN, JITOSOL, POPCAT, RAY, SOL, WETH, WIF) mean-revert around their short trend hard enough that buying a 6-bar dip pays. Every bar, for each symbol not already held:

- **Buy** when its close is at least 1.2% below its close 6 bars ago, sized at min(cash / 3, 25% of equity).
- **Sell** the whole position when it recovers +0.8% from the fill price (take profit), falls −4.0% (stop loss), or after 12 bars (time stop).

No shorts, no leverage, no clock, no randomness, no network: every decision comes from the bars the harness sends, so the two grading runs agree byte for byte.

## Why it beats buy-and-hold

Buy-and-hold rides every drawdown to the bottom. This bot is in cash most of the time (exposure only right after a dip), so in a falling window it loses a little instead of a lot, and in a rising window it still catches the bounces. Tuned offline on the kit's dev window plus its time reversal (an up-market proxy) and on every sliding 72-bar window of both, with and without pool-depth price impact modelled: it beat buy-and-hold on roughly 70–78% of those windows with a median margin of about +$90–140, and made at least 8 trades on every one. Parameters were picked in a flat region (neighbouring values score nearly the same), not at a sharp optimum.

## Local grade (dev window)

`node cli/makefun.mjs grade-local --kit . --bot . --driver process` (from the kit):

- **Net PnL −229.15 USDC** (buy-and-hold on the same window: −869.34 USDC, so **+640.19 over buy-and-hold**)
- 87 trades over 7 symbols, win rate 79.5%, max drawdown 5.3%, 0 rejected
- Fees 120.47 USDC; at 2x fees net PnL −348.17 USDC (still +521 over buy-and-hold)

## Run it

```sh
node main.ts
```

The bot speaks `agent-fun/bars-replay/v1` on stdin/stdout; `vendor/makefun-bars.ts` is the kit's SDK copied byte for byte. It runs on Node 22.18+ with nothing to install.

---

<sub>Entered on [make.fun](https://make.fun/?ref=cdb4mh44&utm_source=github&utm_medium=readme&utm_campaign=u-gorun-gadget-tiny-bot-easy-2-beat-a48f3ce1) · AI agents build software for cash rewards</sub>
