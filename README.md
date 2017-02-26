# CoinTK
## Bitcoin Trading Algorithm Backtesting and Analysis Toolkit

CoinTK -- An open-sourced framework for rapid prototyping and testing of Bitcoin trading strategies. Also check out BitBox, a webserver and iOS app (remote control coming soon!) for backtesting and dry running prototype strategies.

---

# Getting Started

1. Make sure `python3` and `python3-pip` are installed:
    ```
    sudo apt install python3 python3-pip
    ```

2. Clone and install `cointk`
    ```
    cd && git clone https://github.com/cointk/cointk.git
    cd cointk
    sudo pip3 install .
    ```

3. Initialize `cointk`
    ```
    cd && python3 -c 'import cointk.init'
    ```

4. Start writing strategies!  As an example, try backtesting the naive
strategy included in cointk
    ```
    cd && mkdir -p plots histories
    ```

    Create `~/naive.py` with the following contents:

    ```
    # ~/naive.py
    from cointk.backtest import backtest
    from cointk.strategies import NaiveStrategy

    strategy = NaiveStrategy()
    backtest(strategy)
    ```

    Run the script:

    ```
    python3 naive.py
    ```

    You should see something like this pop up in a browser window:

    ![Naive Backtest Output](plots/naive_plot.png)

    From here, you can play around with different strategies and testing parameters via scripts in ```backtests```, or start thinking about making your own [strategy](#creating-your-own-strategies).

    Happy developing!


# File structures

`cointk/` contains most of the algorithmic work

* `strategies/` contains different buying/selling strategies, which is just a decision framework based on the given state of price/quantity and past histories

* `prescient` contains strategies that have access to perfect information, i.e. all historical and future data. These are only useful for a Machine Learning extension we will build in the future, which we hope to train to model such a prescient strategy *without* having perfect informaiton.

* `backtests/` tests strategies running on historical data, so you can evaluate performance had you ran this strategy since the beginning

* `plots/` contain plots generated locally by `plotly` -- such as when you run [backtest.py](cointk/backtest.py).

* `trainings/` contain support files for `cointk/strategies/prescient`, which will be flushed out with


# Creating your own strategies

To create your own strategy, create a class similar to one of the sample strategies given: [Naive](cointk/strategies/hp_naive.py), [Reverse Naive](cointk/strategies/naive_reverse.py), [Random](cointk/strategies/simple_random.py), and [Exponential Moving Averages](cointk/strategies/ema.py). It should inherit the `Strategy` class (defined [here](cointk/strategies/core.py)) and have a
```
	gen_order(self, ts, price, qty, funds, balance):
```
function that decides, given the tuple (ts, price, qty) and any past histories stored in the `Strategy` class, whether to buy or sell.
