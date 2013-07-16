# Moneyclip

(Requires Python 2.7.x.)

Most personal investors need several accounts, some of which at
different financial institutions:

1. Checking account: For paycheck deposits, bill payments, and ATM
   withdrawls.

2. Savings account: For building an emergency fund or saving for an
   upcoming big-ticket purchase.

3. Treasury Direct account: For purchasing and holding I Savings
   Bonds.

4. Brokerage account: For purchasing and holding individual stocks.

5. Taxable mutual fund account: For long-term, diversified investing.

6. Traditional IRA account: For long-term, tax-deferred investing.

7. Roth IRA: For long-term, tax-free investing.

Over time, you want to measure your **total asset allocation across
all accounts** to make sure it stays in balance. That is, you want to
know what percentage of your money is invested in each of:

* US stocks

* International stocks

* Fixed income

* Cash

Moneyclip takes as input a JSON file describing **how many shares of
each investment you own**, and the **composition of each investment**.
For example:

    $ cat config-example.json
    {
      "accounts": [
        {"name": "Chase - Checking", "holdings": {"cash": 563.21}},
        {"name": "Chase - Savings", "holdings": {"cash": 1042.14}},
        {"name": "Treasury Direct", "holdings": {"ibnd": 1000.00}},
        {"name": "Schwab - Brokerage", "holdings": {"AMZN": 2, "TSLA": 3, "YHOO": 3}},
        {"name": "Vanguard - Mutual Funds", "holdings": {"VTSMX": 7.760, "VGTSX": 47.096, "VWLTX": 4.014}},
        {"name": "Vanguard - Trad IRA", "holdings": {"VASGX": 121.760}},
        {"name": "Vanguard - Roth IRA", "holdings": {"VASGX": 125.861}}
      ],
      "investments": {
        "cash": {"mer": 0.0, "composition": [0.0, 0.0, 0.0, 100.0], "price": 1.0},
        "ibnd": {"mer": 0.0, "composition": [0.0, 0.0, 100.0, 0.0], "price": 1.0},
        "AMZN": {"mer": 0.0, "composition": [100.0, 0.0, 0.0, 0.0]},
        "TSLA": {"mer": 0.0, "composition": [100.0, 0.0, 0.0, 0.0]},
        "VASGX": {"mer": 0.17, "composition": [56.6, 23.5, 19.9, 0.0]},
        "VGTSX": {"mer": 0.22, "composition": [0.0, 100.0, 0.0, 0.0]},
        "VTSMX": {"mer": 0.17, "composition": [100.0, 0.0, 0.0, 0.0]},
        "VWLTX": {"mer": 0.20, "composition": [0.0, 0.0, 100.0, 0.0]},
        "YHOO": {"mer": 0.0, "composition": [100.0, 0.0, 0.0, 0.0]}
      }
    }

For any investment that does not have a price, moneyclip will fetch
the price from Yahoo Finance using [YQL](http://developer.yahoo.com/yql/).
It will also use any [MERs](http://en.wikipedia.org/wiki/Expense_ratio)
given to compute the total amount of annual fees you are paying for
your investments.

Run moneyclip like this:

    $ ./bin/moneyclip < config-example.json 
    ACCOUNTS                         VALUE      US  INTL  FIXD  CASH     FEES
    -----------------------    -----------    ----  ----  ----  ----    -----
    Chase - Checking           $       563      0%    0%    0%  100%    $   0
    Chase - Savings            $     1,042      0%    0%    0%  100%    $   0
    Treasury Direct            $     1,000      0%    0%  100%    0%    $   0
    Schwab - Brokerage         $     1,076    100%    0%    0%    0%    $   0
    Vanguard - Mutual Funds    $     1,095     30%   66%    4%    0%    $   2
    Vanguard - Trad IRA        $     3,124     57%   24%   20%    0%    $   5
    Vanguard - Roth IRA        $     3,229     57%   24%   20%    0%    $   5
    -----------------------    -----------    ----  ----  ----  ----    -----
    TOTAL                      $    11,131     45%   20%   21%   14%    $  13
