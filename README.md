# ROI-SmartContract
A Smart Contract written in solidity on ROI problem


Problem :-

This will be a ROI contract. i.e, it'll pay registered users some % on their deposited capital.

Code a smart contract with following:

1. Register users. A user will be registered upon depositing a registration fee of 0.0169 ether. Not less, not more. Registered user can deposit more capital to earn reward after this.



2. It give each registered user a reward of 0.0171% on their deposited capital per day. Interest can be compounding or simple.



3. There will be a variable called "liquifyLimit = 1 ether". At any point in time if the contract balance reaches a value equals to this liquifyLimit variable then 0.67% of contract balance will be distributed among all the registered users.(Added to their account.)



4. Calculated/Pending reward will be transferred to user's wallet when user clicks/calls withdraw button/function.

Solution :-
ROI.sol contract

```
// SPDX-License-Identifier: GPL-3.0

pragma solidity ^0.6.0;
contract ROI {

    //Types or Local variables

    uint liquifyLimit = 5 ether;

    struct User{
        bool registered;
        uint reward;
        uint balance;
        uint time;
    }

    address payable[] registeredUsers;
    mapping(address => User) private Users;



    //Modifiers

    modifier condition(bool _condition) {
        require(_condition, "ERROR");
        _;
    }

    modifier registeredUser(){
        require(Users[msg.sender].registered == true, "You are not a registered User");
        _;
    }



    //Functions

    function sendMoney() public payable returns(bool){
        return true;
    }

    function getContractBalance() public view returns(uint){
        return address(this).balance;
    }

    function register() public payable condition(Users[msg.sender].registered==false) condition(msg.value == 0.0169 ether) returns(bool){
        registeredUsers.push(payable(msg.sender));
        Users[msg.sender].balance = 0;
        Users[msg.sender].reward = 0;
        Users[msg.sender].registered = true;
        Users[msg.sender].time = now ;
        return true;
    }

    function getDepositedBalance() public view registeredUser returns(uint){
        return Users[msg.sender].balance;
    }

    function getRewardCollected() public view registeredUser returns(uint){
        return Users[msg.sender].reward;
    }

    function getLastUpdateTime() public view registeredUser returns(uint){
        return Users[msg.sender].time;
    }



    function distribution() private {
        uint noOfUsers = registeredUsers.length;
        uint bonus = ((address(this).balance * 67) / 100 ) / noOfUsers;
        if(bonus > 0){
            for(uint i=0;i<noOfUsers;i++){
                Users[registeredUsers[i]].reward += bonus;
            }
        } 
    }

    function calculateReward() private {
        uint noOfDays = now - Users[msg.sender].time;
        noOfDays = noOfDays / (30*30*24);
        Users[msg.sender].reward += ((Users[msg.sender].balance * 171) / 10000 ) * noOfDays;
        Users[msg.sender].time = now;
    }

    function deposite() public payable registeredUser {
        if(address(this).balance>=liquifyLimit) distribution();
        calculateReward();
        Users[msg.sender].balance += msg.value;
    }

    function collectReward() public registeredUser returns(bool){
        calculateReward();
        if(address(this).balance >= Users[msg.sender].reward){
            payable(msg.sender).transfer(Users[msg.sender].reward);
            Users[msg.sender].reward = 0;
            return true;
        }
        return false;
    }

    function withdraw() public registeredUser condition(Users[msg.sender].balance <= address(this).balance){
        collectReward();
        payable(msg.sender).transfer(Users[msg.sender].balance);
        Users[msg.sender].balance = 0;
    }

}

```
