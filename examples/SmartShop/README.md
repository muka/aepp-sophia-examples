# SmartShop Sophia smart contract

## Sophia SmartShop smart contract overview
This project implements basic **Seller**, **Transport** and **Buyer** behaviors. 
1. **SellerContract** implements the following functions: 
- `send_item()` - check if the buyer deposit the price needed to the sellers contract and passes to the transport courier.
- `received_item()` - the function has to be called from Buyer contract when the item is received.

2. **TransportContract** implements following functions:
- `change_location(city : string)` - updates last location of the item and the timestamp.
- `delivered_item(city : string)` - finishes the delivery process of the item and updates the timestamp.

3. **BuyerContract** implements following functions: 
- `deposit_to_seller_contract()` - will deposite item price to the seller contract.

There are functions implemented in Transport and Seller contracts, that we can call from Buyer contract, to check status of the item:
- `received_item()` - will change the item status to _delivered_ and send the deposited price to the seller.
- `seller_contract_balance()` - will return the seller contract balance.
- `check_item_status()` - will return the item status of the order.
- `check_courier_status()` - will return the courier delivery status of the order.
- `check_courier_location()` - will return the current delivery location of the order

## Workflow
1. The Seller shoud deploy the SellerContract first by passing the buyer public address and item price as arguments.
2. The Transport Courier should deploy the TransportContract passing `location` as an argument.  
3. The Buyer shoud pass seller contract address and transport contract address as arguments when deploying BuyerContract.
4. The Buyer should deposit the needed amount, by calling `deposit_to_seller_contract()`, which takes the price of the item as `Call.value`.
5. The seller now should send the item, which will be redirected to transport courier. The function is `send_item()`. It checks if buyer had deposited the price of the item to the seller's contract.
6. Buyer can track the status of the item:
- `check_courier_status()` - returns current delivery status 
- `check_courier_location()` - returns current location status
7. Once the item is delivered, the courier should call `delivered_item(city : string)` function, with current `location`.
8. To finalize the order, buyer should call `received_item()` function, with SellerContract address.
9. The amount of tokens, payed by buyer, will be sent to seller's account.