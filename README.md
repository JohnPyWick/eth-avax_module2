## Smart Contract Front End Integration

## Description
A simple smart contract which has 4 functions and it has been integrated with the front end.
## Steps to run this project

1. Clone the starter project template in your pc.

2. Create functions in your smart contract
with some functionality.
For example - In this project, I have added one to show contract address and other function to buy nft.

3. Change the index.js file accordingly to show them on front end.

4. Open three separate terminals and run following commands in each one of them.

This installs the required dependencies of the project.
```
npm i

```
Run this in 2nd terminal to connect to the hardhat network and run a node on the network.
```
npx hardhat node

```
In 3rd terminal run the below commands

```
npx hardhat run --network localhost scripts/deploy.js

```
Run the below command in the first terminal again

```
npm run dev

```
5. Open the url of localhost to launch the app.



### Contract
```javascript
// SPDX-License-Identifier: UNLICENSED
pragma solidity ^0.8.9;

// import "hardhat/console.sol";

contract ethavaxmod2 {
    address payable public owner;
    uint256 public balance;

    event Deposit(uint256 amount);
    event Withdraw(uint256 amount);
    event buyNFT(uint256 _number);

    constructor(uint initBalance) payable {
        owner = payable(msg.sender);
        balance = initBalance;
    }

    function getBalance() public view returns (uint256) {
        return balance;
    }

    function deposit(uint256 _amount) public payable {
        uint256 _previousBalance = balance;

        // make sure this is the owner
        require(msg.sender == owner, "You are not the owner of this account");

        // perform transaction
        balance += _amount;

        // assert transaction completed successfully
        assert(balance == _previousBalance + _amount);

        // emit the event
        emit Deposit(_amount);

    }

    // custom error
    error InsufficientBalance(uint256 balance, uint256 withdrawAmount);

    function withdraw(uint256 _withdrawAmount) public {
        require(msg.sender == owner, "You are not the owner of this account");
        uint256 _previousBalance = balance;
        if (balance < _withdrawAmount) {
            revert InsufficientBalance({
                balance: balance,
                withdrawAmount: _withdrawAmount
            });
        }

        // withdraw the given amount
        balance -= _withdrawAmount;

        // assert the balance is correct
        assert(balance == (_previousBalance - _withdrawAmount));

        // emit the event
        emit Withdraw(_withdrawAmount);

        
    }

    function getContractAddress() public view returns (address) {
        return address(this);
    }


    function getContractBalance() public view returns (uint256) {
        return address(this).balance;
    }

    function BuyNFT(uint256 _number) public {
        withdraw(_number);

        emit buyNFT(_number);
    }
}

```

### Contract Address Function


```javascript
  function getContractAddress() public view returns (address) {
        return address(this);
    }
```

### Buy NFT function

This function uses the withdraw function also in it to work.

```javascript
function BuyNFT(uint256 _number) public {
        withdraw(_number);

        emit buyNFT(_number);
    }

```
### Buy NFT function in the index.js file

```javascript
 const BuyNFT = async () => {
    if (atm) {
      let tx = await atm.BuyNFT(1);
      await tx.wait();
      getBalance();
    }
  };
```

### Toggle Button for Contract Address


```css
<div className="button-group">
              <button onClick={toggleContractAddress}>
                {showContractAddress ? "Hide Contract Address" : "Show Contract Address"}
              </button>
              {showContractAddress && (
                <div>
                  <p>Contract Address: {contractAddress}</p>
                </div>
              )}

```
## Authors

- [@JohnPyWick](https://github.com/JohnPyWick)


## License

This project is licensed under the MIT License - see the LICENSE.md file for details


