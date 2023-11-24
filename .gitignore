from tkinter import *
import requests
from tkinter import ttk

root = Tk()
root.title('Crypto price-checker')        # Nane
root.geometry("300x250+100+300")          # Size (optionally)
root.resizable(False, False)              # Program size (optionally)

# Functions
import time

last_request_time = 0
cached_prices = None
update_enabled = True  # Flag to monitor the status of the timer

def get_crypto_prices():
    global last_request_time, cached_prices

    try:
        current_time = time.time()
        if current_time - last_request_time > 5:
            btc_url = 'https://api.coinbase.com/v2/exchange-rates?currency=BTC'     #All API'S
            eth_url = 'https://api.coinbase.com/v2/exchange-rates?currency=ETH'
            bnb_url = 'https://api.coinbase.com/v2/exchange-rates?currency=BNB'

            btc_response = requests.get(btc_url)
            eth_response = requests.get(eth_url)
            bnb_response = requests.get(bnb_url)

            btc_response.raise_for_status()
            eth_response.raise_for_status()
            bnb_response.raise_for_status()

            btc_data = btc_response.json()
            eth_data = eth_response.json()
            bnb_data = bnb_response.json()

            btc_price = btc_data['data']['rates']['USD']
            eth_price = eth_data['data']['rates']['USD']
            bnb_price = bnb_data['data']['rates']['USD']

            # Save results and request time
            cached_prices = btc_price, eth_price, bnb_price
            last_request_time = current_time

        return cached_prices
    except requests.exceptions.RequestException as e:
        print(f"Error fetching crypto prices: {e}")
        return None, None, None
    
def update_prices():
    global update_enabled
    if not update_enabled:
        return
    
    print("Updating prices...")
    btc_price, eth_price, bnb_price = get_crypto_prices()

    if btc_price is not None and eth_price is not None and bnb_price is not None:
        btc_buy.config(text=f"{btc_price}")
        eth_buy.config(text=f"{eth_price}")
        usd_buy.config(text=f"{round(float(bnb_price), 2)}")
        root.update()  # Update the main tkinter window
    else:
        print("Unable to update prices.")

    # Condition for calling the function again after 5 seconds
    if update_enabled:
        root.after(5000, update_prices)

# Function Stop/Resume
def toggle_update():
    global update_enabled
    update_enabled = not update_enabled
    if update_enabled:
        update_prices()

# Header Frame
header_frame = Frame(root)
header_frame.pack(fill=X)
header_frame.grid_columnconfigure(0, weight=1)
header_frame.grid_columnconfigure(1, weight=1)

# Header
h_currency = Label(header_frame, text="Token: ", bg="#ccc", font="Arial 12 bold")
h_currency.grid(row=0, column=0, sticky=EW)
h_buy = Label(header_frame, text="Actual Price: ", bg="#ccc", font="Arial 12 bold")
h_buy.grid(row=0, column=1, sticky=EW)

# BNB course
usd_currency = Label(header_frame, text="BNB", font="Arial 10")
usd_currency.grid(row=4, column=0, sticky=EW)
usd_buy = Label(header_frame, text="", font="Arial 10")
usd_buy.grid(row=4, column=1, sticky=EW)

# BTC Course
btc_currency = Label(header_frame, text="BTC", font="Arial 10")
btc_currency.grid(row=2, column=0, sticky=EW)
btc_buy = Label(header_frame, text="", font="Arial 10")
btc_buy.grid(row=2, column=1, sticky=EW)

# ETH Course
eth_currency = Label(header_frame, text="ETH", font="Arial 10")
eth_currency.grid(row=3, column=0, sticky=EW)
eth_buy = Label(header_frame, text="", font="Arial 10")
eth_buy.grid(row=3, column=1, sticky=EW)

# Calc Frame
calc_frame = Frame(root, bg="#fff")
calc_frame.pack(expand=1, fill=BOTH)
calc_frame.grid_columnconfigure(1, weight=1)

# Button "Stop/Resume"
btn_stop_resume = ttk.Button(calc_frame, text="Stop/Resume", command=toggle_update)
btn_stop_resume.grid(row=2, column=1, columnspan=2, sticky=EW, padx=0)

# Initial update
update_prices()

# Timer every 5 seconds
root.after(5000, update_prices)

root.mainloop()
