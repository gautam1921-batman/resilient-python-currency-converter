import requests

def convert_currency_robust():
    print("=== Live Python Currency Converter ===")
    url = "https://er-api.com"
    
    # Hardcoded test values for environment safety
    target_currency = "INR"
    amount = 100.0
    
    try:
        # Attempt to request live data with a strict 5-second timeout limit
        response = requests.get(url, timeout=5)
        data = response.json()
        rates = data.get("rates", {})
        exchange_rate = rates[target_currency]
        print(f"\n[ONLINE MODE] Fetched live data from API server.")
        
    except (requests.exceptions.RequestException, ValueError, KeyError):
        # Fallback activation if a firewall, timeout, or network crash happens
        print(f"\n[OFFLINE SANDBOX MODE] Network blocked. Using local database fallback.")
        fallback_rates = {"EUR": 0.92, "GBP": 0.78, "INR": 83.50}
        exchange_rate = fallback_rates[target_currency]

    # Execute operations and print results
    converted_amount = amount * exchange_rate
    print(f"Target: {target_currency} | Amount: ${amount} USD")
    print(f"Exchange Rate applied: {exchange_rate}")
    print(f"${amount:,} USD is equal to {converted_amount:,.2f} {target_currency}")

if __name__ == "__main__":
    convert_currency_robust()
