# **Split pay contract**

```solidity
// SPDX-License-Identifier: MIT
pragma solidity ^0.8.18;


contract SplitPay {

    uint public minDeposit;
    uint public balance;
    address [] members;
    mapping (address => bool) paid;

    event Submit(
        address indexed sender,
        uint amount,
        uint timestamp
    );

    event PayInvoice(
        address indexed receiver,
        uint amount,
        uint timestamp
    );

    constructor(uint _minDeposit) {
        require(_minDeposit>=1);
        minDeposit = _minDeposit * 1 ether;
    }

    function getContractBalance() external view returns (uint) {
        return address(this).balance;
    }

    function getMembers() external view returns (address[] memory) {
        return members;
    }


    function submit() external payable {
        address _sender = msg.sender;
        uint _amount = msg.value;
        
        require(!paid[_sender],"You have already submitted a payment.");
        require(_amount>=minDeposit,"The submitted amount is less than the minimum required deposit.");
        members.push(_sender);
        paid[_sender]=true;
        balance+=_amount;
        emit Submit(_sender,_amount,block.timestamp);

    }

    function payInvoice(address _receiver,uint _amount) external  {          
        require(
            _amount<=balance,
            "The invoice amount exceeds the available balance."
        );
        require(
            paid[msg.sender],
            "You are not authorized to pay the invoice"
        );
        
        _amount *= 1 ether;
        balance -=_amount;
        (bool success,) = _receiver.call{ value: _amount }("");
        require(success,"Payment of the invoice failed.");
        emit PayInvoice(_receiver, _amount, block.timestamp);
        
        

    }
}

```
>📝 This smart contract in Solidity is called **`SplitPay`**. It allows payments to be split between several members. Each member must make a minimum deposit specified when the contract is created. 💰 When a member submits a payment, their address is recorded, and the amount is added to the total balance. Members can then use this balance to pay bills by entering the beneficiary's address and the amount to be paid. The balance is deducted from the amount paid and transferred to the beneficiary. 💸

<br>

>📝 Ce contrat intelligent en Solidity s'appelle **`SplitPay`**. Il permet de diviser les paiements entre plusieurs membres. Chaque membre doit effectuer un dépôt minimum spécifié lors de la création du contrat. 💰 Lorsqu'un membre soumet un paiement, son adresse est enregistrée, et le montant est ajouté au solde total. Les membres peuvent ensuite utiliser cette balance pour payer des factures en fournissant l'adresse du bénéficiaire et le montant à payer. La balance est déduite du montant payé et transférée au bénéficiaire. 💸


 [🔙](../README.md)