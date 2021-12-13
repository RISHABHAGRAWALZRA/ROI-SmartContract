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
