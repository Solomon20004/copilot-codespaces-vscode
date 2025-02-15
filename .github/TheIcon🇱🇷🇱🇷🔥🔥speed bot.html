import websocket
import json
import time
import pandas as pd

# API Configuration
API_TOKEN = "YOUR_DERIV_API_TOKEN"
SYMBOL = "R_100"  # Choose a synthetic index (e.g., R_100, R_50)
STOP_LOSS = -50  # Maximum loss before stopping
TAKE_PROFIT = 100  # Profit target before stopping
MACD_FAST = 12
MACD_SLOW = 26
MACD_SIGNAL = 9

# Staking Sequence
stakes = [3, 5, 7]  # Stake progression: Over 3 → Over 5 → Over 7
stake_index = 0  # Start with Over 3
balance = 1000  # Simulated starting balance

ws = None

# WebSocket Connection
def on_open(ws):
    print("Connected to Deriv API")
    auth_data = {"authorize": API_TOKEN}
    ws.send(json.dumps(auth_data))

# Handle Messages from API
def on_message(ws, message):
    global stake_index, balance

    data = json.loads(message)

    if "authorize" in data:
        print("Authorization successful!")
        request_candles()

    elif "history" in data:
        df = process_candles(data)
        macd, signal = calculate_macd(df)

        # Trading Signal: Buy when MACD crosses above Signal, Sell when it crosses below
        if macd.iloc[-1] > signal.iloc[-1] and macd.iloc[-2] <= signal.iloc[-2]:  
            place_trade("CALL")
        elif macd.iloc[-1] < signal.iloc[-1] and macd.iloc[-2] >= signal.iloc[-2]:  
            place_trade("PUT")

# Request Market Data
def request_candles():
    ws.send(json.dumps({
        "ticks_history": SYMBOL, 
        "count": 100, 
        "end": "latest", 
        "style": "candles", 
        "granularity": 60
    }))

# Process Candle Data
def process_candles(data):
    candles = data["history"]["candles"]
    df = pd.DataFrame(candles)
    df["time"] = pd.to_datetime(df["epoch"], unit="s")
    df.set_index("time", inplace=True)
    df["close"] = df["close"].astype(float)
    return df

# Calculate MACD Indicator
def calculate_macd(df):
    exp1 = df["close"].ewm(span=MACD_FAST, adjust=False).mean()
    exp2 = df["close"].ewm(span=MACD_SLOW, adjust=False).mean()
    macd = exp1 - exp2
    signal = macd.ewm(span=MACD_SIGNAL, adjust=False).mean()
    return macd, signal

# Place Trade
def place_trade(direction):
    global stake_index, balance

    if balance < STOP_LOSS:
        print("Stop Loss Reached. Stopping trading.")
        return
    if balance >= TAKE_PROFIT:
        print("Take Profit Reached. Stopping trading.")
        return

    stake_amount = stakes[stake_index]  # Get stake amount based on current index

    trade_request = {
        "buy": 1,
        "price": 0,
        "parameters": {
            "amount": stake_amount,
            "basis": "stake",
            "contract_type": direction,
            "currency": "USD",
            "duration": 5,
            "duration_unit": "t",
            "symbol": SYMBOL
        }
    }
    
    ws.send(json.dumps(trade_request))
    print(f"Placed {direction} trade with stake {stake_amount} USD")

    # Simulating trade results (replace this with actual API trade response)
    trade_result = simulated_trade_result()  

    if trade_result == "win":
        print(f"Trade WON! Profit: {stake_amount} USD")
        balance += stake_amount
        stake_index = 0  # Reset to Over 3
    else:
        print(f"Trade LOST! Loss: {stake_amount} USD")
        balance -= stake_amount
        stake_index = min(stake_index + 1, len(stakes) - 1)  # Move to next stake level

# Simulated Trade Result (Replace with API Response Handling)
def simulated_trade_result():
    import random
    return random.choice(["win", "loss"])  # Randomly decide trade outcome

# Start WebSocket
ws = websocket.WebSocketApp("wss://ws.binaryws.com/websockets/v3?app_id=1089", on_open=on_open, on_message=on_message)
ws.run_forever()
