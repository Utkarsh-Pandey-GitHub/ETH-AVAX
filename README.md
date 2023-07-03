# ETH-AVAX

In this assessment error handling funtions in solidity namely require, revert and assert are used make  a contract.

# About require() revert() and assert()

require(): The require statement is used to validate conditions within a smart contract. It checks for a specific condition and throws an exception, reverting the transaction if the condition evaluates to false. It is often used for input validation, preconditions, or enforcing certain requirements.

revert(): The revert statement allows you to explicitly revert the current transaction. It is typically used to handle exceptional cases or invalid conditions. You can provide a custom error message with revert to provide more information about the reason for the revert. When revert is executed, any state changes made up to that point in the transaction are discarded.

assert(): The assert statement is used to verify internal errors or invariants within the contract. It checks for conditions that should never evaluate to false. If the condition passed to assert evaluates to false, it indicates a critical error in the contract's code, and the contract execution is immediately halted and reverted. Unlike require, the error message for assert is not customizable

#implementation
Here, I have implemented these functions in a scenarion of a bank where one can open an account, deposit some money and get loans or can withdraw it. For every account there is a mapping between its address and balance left.
 mapping(address=>uint) public balance;


    modifier isAdult(uint age){
        require(age>=18,"minor");
        _;
    }
    modifier isBankrupt(address account){
        if(balance[account]<0){
            revert("You are already Bankrupt");
        }
        _;
    }

    function canOpen(uint age) public pure  isAdult(age) returns (bool){
        return true;
    }

For every transaction it is important that the transaction is mmade by an adult who is required to be an adult while in case of a loan the 
balance of that adddress must be more than 0 as the bank wont give the loan to bankrupt people.

    function takeLoan(address add,uint value,uint age) public isAdult(age) isBankrupt(add) returns(uint,string memory){
        balance[add]+=value;
        return (balance[add],"Loan sanctioned");
    }

A similar fact is asserted before any withdral is made i.e. one can only withdraw when they have sufficient balanc ein their account.

    function canWithdraw(address add,uint value,uint age) public isAdult(age) returns(uint,string memory){
        assert(balance[add]>=value);
        balance[add]-=value;
        return (balance[add],"Success");

    }
