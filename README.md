# Our-Block-
OUR-BLOCK is a Mult-level Subscription to the XDALE Directory. The first subscriber can choose to "Do Work"; or not. Every subscriber is automatically linked to a unique WALLET with public and private KEYS. "Doing Work" means (1) nominating THREE or more personal acquaintances to become subscribers to OUR BLOCK. (2) Explaining the features and benefits of membership. Once this is achieved the "Work is Done". The first subscriber then becomes "An Ancestor". All transactions (to 10 generations of DESCENDANTS) that is done via the XDALE Directory automatically returns an AMOUNT to the wallet of the subscriber as well as to his Ancestors' wallet.The AMOUNT is calculated in fractions of bitcoin. 
 gistfile1.txt
pragma solidity ^0.4.10;

contract Pyramid {
    mapping (address => address) parent;
    mapping (address => uint) balance;
    mapping (address => address) invite;
    mapping (address => uint) invites_left;
    mapping (address => bool) joined;
    function calc(uint level) returns (uint) {
        return 10;
    }
    function join() payable {
        if (msg.value < 1 ether) throw;
        if (joined[msg.sender]) throw;
        if (invite[msg.sender] != 0 && invites_left[invite[msg.sender]] > 0) {
            address par = invite[msg.sender];
            parent[msg.sender] = par;
            joined[msg.sender] = true;
            invites_left[msg.sender] = 3;
            for (var i = 0; i < 10; i++) {
                balance[par] += calc(i);
                par = parent[par];
            }
        }
    }
    function doInvite(address addr) {
        if (invite[addr] != 0) throw;
        invite[addr] = msg.sender;
        invites_left[msg.sender]--;
    }
    function cancelInvite(address addr) {
        if (joined[addr] || invite[addr] != msg.sender) throw;
        invite[addr] = 0;
        invites_left[msg.sender]++;
    }
    function pull() {
        uint sum = balance[msg.sender];
        balance[msg.sender] = 0;
        msg.sender.transfer(sum);
    }
}
