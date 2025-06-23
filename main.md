  // SPDX-License-Identifier: MIT
  pragma solidity ^0.8.17;

  contract childerns_poket_money{

      struct childerns_info {
        string child_name;
          uint _Amount;
          uint Date;
      }

  mapping (address => childerns_info ) public childerns;

  address[] childerns_Addr;

  address father = msg.sender;

  modifier onlyfather(){
      require(father == msg.sender,"only father can call this");
      _;
  }
  uint public balance;

  function distribute(address _child_addr,string memory _name) external payable    onlyfather {
      require(msg.value > 0,"poket money is not equal to zero");
      childerns_Addr.push(_child_addr);
      childerns_info storage temp;
      temp = childerns[_child_addr];
      temp.child_name = _name;
      temp._Amount = msg.value;
      temp.Date = block.timestamp;
      balance += msg.value;
  }
  function collect_poket_Money() external payable {

    require(msg.sender != father,"only child collect poket money");
    require(block.timestamp > childerns[msg.sender].Date,"wait for your poket_money date");
    require(childerns[msg.sender]._Amount > 0,"you already collect your poket money");

    payable (msg.sender).transfer(childerns[msg.sender]._Amount);
    childerns[msg.sender].Date =block.timestamp + 5 minutes;
    balance -= childerns[msg.sender]._Amount
  }

  }

