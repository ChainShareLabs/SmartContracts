# **Buy Me a Cup of Coffee Contract**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;


contract BuyMeacoffe {

    uint public numberOfCoffees;
    uint public balance;
    address payable public owner;
    

    event BuyCoffee(
        address indexed sender,
        string message,
        uint amount,
        uint timestamp
    );

    constructor() payable {
        owner = payable(msg.sender);
    }

    struct Coffee {
        address sender;
        string message;
        uint timestamp;
        
    }

    Coffee [] coffee;

    function getCoffee() external view returns (Coffee [] memory) {
        return coffee;
    }

    function getTotalCoffee() external view returns(uint){ 
        return numberOfCoffees;
    }

    function getBalance()external view returns (uint) {
        return balance;
    }

    function buyCoffee(string memory _message) external payable {
        
        uint minimumCost = 0.001 ether;
        uint _timestamp = block.timestamp;
        uint _amount = msg.value;
        require(bytes(_message).length !=0,"The message cannot be empty");
        require(msg.value>=minimumCost, "You must send at least 0.001 ether");

        numberOfCoffees += 1;
        balance+=_amount;
        coffee.push(Coffee(msg.sender,_message,_timestamp));
 
        (bool sent,) = owner.call{value: _amount}("");
        require(sent, "Failure to send coffee");

        emit BuyCoffee(msg.sender,_message,_amount,_timestamp);

    }


   
}



```


>📝The "BuyMeacoffe" smart contract is written in Solidity and allows users to buy ☕️ coffee in exchange for a certain amount of Ether 💰. The contract records the total number of coffees purchased, the available Ether balance and the owner's address. Users can buy coffee by sending a message and respecting a minimum cost. Upon purchase, the contract stores the coffee information in a data structure and transfers the corresponding amount to the contract owner. Once the purchase has been made, an event is issued to inform the participants. 😊

>📝Le contrat intelligent "`BuyMeacoffe`" est écrit en Solidity et permet aux utilisateurs d'acheter du café ☕️ en échange d'une certaine quantité d'Ether 💰. Le contrat enregistre le nombre total de cafés achetés, le solde Ether disponible et l'adresse du propriétaire. Les utilisateurs peuvent acheter du café en envoyant un message et en respectant un coût minimum. Lors de l'achat, le contrat enregistre les informations sur le café dans une structure de données et transfère le montant correspondant au propriétaire du contrat. Une fois l'achat effectué, un événement est émis pour notifier les participants. 😊


