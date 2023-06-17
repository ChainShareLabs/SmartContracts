
# **Escrow contract**

```solidity

// SPDX-License-Identifier: MIT

pragma solidity ^0.8.18;

contract Escrow {

    address payable public buyer;
    address payable public seller;
    address payable public arbiter;


    enum State {
        await_payment,
        await_delivery,
        complete
     }
    State public state;

    event ConfirmPayement(uint balance,uint timestamp,State state);
    event ConfirmDelivery(uint balance,uint timestamp,State state);

    modifier inState(State _state) {
        require(state==_state,"Invalid contract state");
        _;
    }


    constructor(address payable _buyer,address payable _seller)  {        
        arbiter = payable(msg.sender);
        require(_buyer!=_seller && _buyer!=arbiter && _seller!=arbiter);
        buyer = _buyer;
        seller = _seller;
        state = State.await_payment;

    }


    function confirmPayement() external payable inState(State.await_payment)  {
        require(msg.value!=0);
        require(msg.sender == buyer, "Only buyer can send payments");
        state = State.await_delivery;
        emit ConfirmPayement(msg.value,block.timestamp, state);
    }

    function confirmDelivery() external inState(State.await_delivery) {
        require(msg.sender==arbiter || msg.sender==buyer,"Only buyer or arbiter can call this function");
        uint balance = address(this).balance;
        seller.transfer(balance);
        state = State.complete;
        emit ConfirmDelivery(balance,block.timestamp,state);
    }

    
    function getState() external view returns(State){
        return state;
    }

}

    

```
>📝This smart contract in Solidity is an **`escrow`** contract that facilitates a transaction between a 👤 buyer, a 👤 seller and a ⚖️ referee. The contract passes through three states: 🕓 awaiting payment, 🚚 awaiting delivery and ✅ complete. The buyer sends a payment 💰 to the contract, then once delivery has been confirmed by the buyer or referee, the seller receives payment. The contract allows you to check the current state of the transaction with the "getState()" function.

<br>

>📝Ce contrat intelligent en Solidity est un contrat d'entiercement **`escrow`** qui facilite une transaction entre un 👤 acheteur, un 👤 vendeur et un ⚖️ arbitre. Le contrat passe par trois états : 🕓 en attente de paiement, 🚚 en attente de livraison et ✅ terminé. L'acheteur envoie un 💰 paiement au contrat, puis une fois la livraison confirmée par l'acheteur ou l'arbitre, le vendeur reçoit le paiement. Le contrat permet de vérifier l'état actuel de la transaction avec la fonction "getState()".


 [🔙](../README.md)