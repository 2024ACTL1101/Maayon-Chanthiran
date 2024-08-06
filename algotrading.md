
## Algorithmic Trading Strategy

## Introduction

In this assignment, you will develop an algorithmic trading strategy by incorporating financial metrics to evaluate its profitability. This exercise simulates a real-world scenario where you, as part of a financial technology team, need to present an improved version of a trading algorithm that not only executes trades but also calculates and reports on the financial performance of those trades.

## Background

Following a successful presentation to the Board of Directors, you have been tasked by the Trading Strategies Team to modify your trading algorithm. This modification should include tracking the costs and proceeds of trades to facilitate a deeper evaluation of the algorithm’s profitability, including calculating the Return on Investment (ROI).

After meeting with the Trading Strategies Team, you were asked to include costs, proceeds, and return on investments metrics to assess the profitability of your trading algorithm.

## Objectives

1. **Load and Prepare Data:** Open and run the starter code to create a DataFrame with stock closing data.

2. **Implement Trading Algorithm:** Create a simple trading algorithm based on daily price changes.

3. **Customize Trading Period:** Choose your entry and exit dates.

4. **Report Financial Performance:** Analyze and report the total profit or loss (P/L) and the ROI of the trading strategy.

5. **Implement a Trading Strategy:** Implement a trading strategy and analyze the total updated P/L and ROI. 

6. **Discussion:** Summarise your finding.


## Instructions

### Step 1: Data Loading

Start by running the provided code cells in the "Data Loading" section to generate a DataFrame containing AMD stock closing data. This will serve as the basis for your trading decisions. First, create a data frame named `amd_df` with the given closing prices and corresponding dates. 

```r
# Load data from CSV file
amd_df <- read.csv("AMD.csv")
# Convert the date column to Date type and Adjusted Close as numeric
amd_df$date <- as.Date(amd_df$Date)
amd_df$close <- as.numeric(amd_df$Adj.Close)
amd_df <- amd_df[, c("date", "close")]
```

#### Plotting the Data
Plot the closing prices over time to visualize the price movement.
```r
plot(amd_df$date, amd_df$close,'l')
```

### Step 2: Trading Algorithm
Implement the trading algorithm as per the instructions. You should initialize necessary variables, and loop through the dataframe to execute trades based on the set conditions.

- Initialize Columns: Start by ensuring dataframe has columns 'trade_type', 'costs_proceeds' and 'accumulated_shares'.
- Change the algorithm by modifying the loop to include the cost and proceeds metrics for buys of 100 shares. Make sure that the algorithm checks the following conditions and executes the strategy for each one:
  - If the previous price = 0, set 'trade_type' to 'buy', and set the 'costs_proceeds' column to the current share price multiplied by a `share_size` value of 100. Make sure to take the negative value of the expression so that the cost reflects money leaving an account. Finally, make sure to add the bought shares to an `accumulated_shares` variable.
  - Otherwise, if the price of the current day is less than that of the previous day, set the 'trade_type' to 'buy'. Set the 'costs_proceeds' to the current share price multiplied by a `share_size` value of 100.
  - You will not modify the algorithm for instances where the current day’s price is greater than the previous day’s price or when it is equal to the previous day’s price.
  - If this is the last day of trading, set the 'trade_type' to 'sell'. In this case, also set the 'costs_proceeds' column to the total number in the `accumulated_shares` variable multiplied by the price of the last day.



```r
# Initialize columns for trade type, cost/proceeds, and accumulated shares in amd_df
amd_df$trade_type <- NA
amd_df$costs_proceeds <- NA  # Corrected column name
amd_df$accumulated_shares <- 0  # Initialize if needed for tracking

# Initialize variables for trading logic
previous_price <- 0
share_size <- 100
accumulated_shares <- 0

for (i in 1:nrow(amd_df)) {
current_price <- amd_df$close[i]
 if (prev_price == 0) {
 #First day, buy 100 shares
 amd_df$trade_type[i] <- "buy"
 amd_df$costs_proceeds[i] <- -current_price * share_size
 accumulated_shares <- accumulated_shares + share_size
 total_cost_of_shares <- total_cost_of_shares - amd_df$costs_proceeds[i]
 }else if (i == nrow(amd_df)) {
 # Sell all shares on the last day
 amd_df$trade_type[i] <- "sell"
 amd_df$costs_proceeds[i] <- current_price * accumulated_shares
 accumulated_shares <- 0
 }else if (current_price < prev_price) {
 # Buy if current price is less than previous day's price
 amd_df$trade_type[i] <- "buy"
 amd_df$costs_proceeds[i] <- -current_price * share_size
 accumulated_shares <- accumulated_shares + share_size
 total_cost_of_shares <- total_cost_of_shares - amd_df$costs_proceeds[i]
 }
 # Update accumulated shares
 amd_df$accumulated_shares[i] <- accumulated_shares

 # Update previous price
 prev_price <- current_price
}
}
```


### Step 3: Customize Trading Period
- Define a trading period you wanted in the past five years 


```r
start_date <- as.Date("2020-07-01")
end_date <- as.Date("2021-06-30")
customised_trading_period_df <- amd_df[amd_df$date >= start_date & amd_df$date <= end_date, ]
# Initialize columns for trade type, cost/proceeds, and accumulated shares in amd_df
amd_df$trade_type <- NA
amd_df$costs_proceeds <- 0 # Corrected column name
amd_df$accumulated_shares <- 0 # Initialize if needed for tracking
# Initialize variables for trading logic
prev_price <- 0
share_size <- 100
accumulated_shares <- 0
total_cost_of_shares <-0
for (i in 1:nrow(customised_trading_period_df)) {
 current_price <- customised_trading_period_df$close[i]
 if (prev_price == 0) {
 # First day, buy 100 shares
 customised_trading_period_df$trade_type[i] <- "buy"
 customised_trading_period_df$costs_proceeds[i] <- -current_price * share_size
 accumulated_shares <- accumulated_shares + share_size
 total_cost_of_shares <- total_cost_of_shares - customised_trading_period_df$costs_proceeds[i]
 } else if (i == nrow(customised_trading_period_df)) {
 # Sell all shares on the last day
 customised_trading_period_df$trade_type[i] <- "sell"
 customised_trading_period_df$costs_proceeds[i] <- current_price * accumulated_shares
 accumulated_shares <- 0
 } else if (current_price < prev_price) {
 # Buy if current price is less than previous day's price
 customised_trading_period_df$trade_type[i] <- "buy"
 customised_trading_period_df$costs_proceeds[i] <- -current_price * share_size
 accumulated_shares <- accumulated_shares + share_size
 total_cost_of_shares <- total_cost_of_shares - customised_trading_period_df$costs_proceeds[i]
 }

 # Update accumulated shares
 customised_trading_period_df$accumulated_shares[i] <- accumulated_shares

 # Update previous price
 prev_price <- current_price
}
```


### Step 4: Run Your Algorithm and Analyze Results
After running your algorithm, check if the trades were executed as expected. Calculate the total profit or loss and ROI from the trades.

- Total Profit/Loss Calculation: Calculate the total profit or loss from your trades. This should be the sum of all entries in the 'costs_proceeds' column of your dataframe. This column records the financial impact of each trade, reflecting money spent on buys as negative values and money gained from sells as positive values.
- Invested Capital: Calculate the total capital invested. This is equal to the sum of the 'costs_proceeds' values for all 'buy' transactions. Since these entries are negative (representing money spent), you should take the negative sum of these values to reflect the total amount invested.
- ROI Formula: $$\text{ROI} = \left( \frac{\text{Total Profit or Loss}}{\text{Total Capital Invested}} \right) \times 100$$

```r
# Fill your code here
total_profit_loss <- 0
invested_cap <- 0
# Loop through each row of the customised_trading_period_df dataframe
for (i in 1:nrow(customised_trading_period_df)) {
 if (!is.na(customised_trading_period_df$trade_type[i])) {
 if (customised_trading_period_df$trade_type[i] == 'buy') {
 invested_cap <- invested_cap - customised_trading_period_df$costs_proceeds[i]
 }
 # Update the total profit/loss by adding the costs/proceeds of the current transaction
 total_profit_loss <- total_profit_loss + customised_trading_period_df$costs_proceeds[i]
 }
}
ROI <- (total_profit_loss / invested_cap) * 100
# Print the results
cat("ROI:", ROI, "\n")
cat("Total investment:", invested_cap, "\n")
cat("Total profit/loss:", total_profit_loss, "\n")
```

### Step 5: Profit-Taking Strategy or Stop-Loss Mechanisum (Choose 1)
- Option 1: Implement a profit-taking strategy that you sell half of your holdings if the price has increased by a certain percentage (e.g., 20%) from the average purchase price.
- Option 2: Implement a stop-loss mechanism in the trading strategy that you sell half of your holdings if the stock falls by a certain percentage (e.g., 20%) from the average purchase price. You don't need to buy 100 stocks on the days that the stop-loss mechanism is triggered.


```r
# Fill your code here
# Initialize columns for trade type, cost/proceeds, and accumulated shares in amd_df
customised_trading_period_df$trade_type <- NA
customised_trading_period_df$costs_proceeds <- 0 # Corrected column name
customised_trading_period_df$accumulated_shares <- 0 # Initialize if needed for tracking
customised_trading_period_df$avg_price <- 0
# Initialize variables for trading logic
prev_price <- 0
share_size <- 100
accumulated_shares <- 0
total_cost_of_shares <-0
total_shares_bought <- 0
for (i in 1:nrow(customised_trading_period_df)) {
 current_price <- customised_trading_period_df$close[i]
 if (i == nrow(customised_trading_period_df)) {
 # Sell all shares on the last day
 customised_trading_period_df$trade_type[i] <- "sell"
 customised_trading_period_df$costs_proceeds[i] <- current_price * accumulated_shares
 accumulated_shares <- 0
 #average_purchase_price <- total_cost_of_shares/accumulated_shares
 }
 else if (prev_price == 0) {
 # First day, buy 100 shares
 customised_trading_period_df$trade_type[i] <- "buy"
 customised_trading_period_df$costs_proceeds[i] <- -current_price * share_size
 accumulated_shares <- accumulated_shares + share_size
 total_cost_of_shares <- total_cost_of_shares - customised_trading_period_df$costs_proceeds[i]
 average_purchase_price <- total_cost_of_shares/accumulated_shares
 }else if (current_price < prev_price) {
 # Buy if current price is less than previous day's price
 customised_trading_period_df$trade_type[i] <- "buy"
 customised_trading_period_df$costs_proceeds[i] <- -current_price * share_size
 accumulated_shares <- accumulated_shares + share_size
 total_cost_of_shares <- total_cost_of_shares - customised_trading_period_df$costs_proceeds[i]
 average_purchase_price <- total_cost_of_shares/accumulated_shares
 }
 if(current_price>=average_purchase_price*1.25 && is.na(customised_trading_period_df$trade_type[i])){
 customised_trading_period_df$trade_type[i] <- "sell"
 customised_trading_period_df$costs_proceeds[i] <- current_price*accumulated_shares*1/2
 accumulated_shares <- accumulated_shares/2
 total_cost_of_shares <- average_purchase_price*accumulated_shares
 }
 # Update accumulated shares
 customised_trading_period_df$accumulated_shares[i] <- accumulated_shares

 # Update previous price
 prev_price <- current_price
}


# Update accumulated shares
 customised_trading_period_df$accumulated_shares[i] <- accumulated_shares

```


### Step 6: Summarize Your Findings
- Did your P/L and ROI improve over your chosen period?
- Relate your results to a relevant market event and explain why these outcomes may have occurred.


Discussion: Over the trading period between 2020-07-01 and 2021-06-30, the initial strategy yielded an ROI of 15.71%. However, when the profittaking strategy was applied, the ROI decreased to 13.80%. This reduction in ROI can be attributed to the global COVID-19 pandemic, which
caused a recession and a continual decline in AMD’s stock price, reaching a low of $52.34 on 2020-07-02.

Several factors contributed to the initial drop in AMD’s stock price, including panic selling by investors due to market volatility and restrictions placed on businesses during this period. However, by August 2020, the market began to rebound. This recovery was driven by a growing demand for technology products due to the shift to remote work and online education. 

AMD, known for its high-performance graphics cards, benefited from the surge in demand for gaming as more people spent time at home.
The automated nature of the profit-taking strategy meant it could not account for the broader market recovery and demand surge that occurred
later in 2020. Selling portions of the holdings during intermediate price increases led to missed opportunities for greater gains as the stock
continued to rise. The ongoing economic uncertainty and fluctuations caused by the pandemic created a challenging environment for any trading
strategy to optimize fully. This was evident between 2021-04-27 and 2021-05-03, when the strategy executed four separate buy transactions while
the stock price dropped from $85.21 to $78.55.

Thus, although the profit-taking strategy was implemented correctly, it was not as successful due to the specific market context during this period.




