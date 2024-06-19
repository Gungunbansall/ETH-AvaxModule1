# CorruptionReporting Smart Contract

The CorruptionReporting smart contract is designed to facilitate the reporting and verification of corruption incidents in a decentralized manner. This contract allows users to submit corruption reports, which can then be verified and rewarded by an admin. The contract uses Solidity's require(), assert(), and revert() statements to ensure the integrity and correctness of its operations.


## Description
The CorruptionReporting smart contract provides a decentralized platform for reporting, verifying, and rewarding corruption reports. It ensures the integrity and reliability of the reporting process through the use of require(), assert(), and revert() statements. The contract includes functionalities for report submission, verification by an admin, and reward issuance for verified reports.
## Getting Started
## Executing Program
To run this program, you can use Remix, an online Solidity IDE. To get started, go to the Remix website at https://remix.ethereum.org/.

Once you are on the Remix website, create a new file by clicking on the "+" icon in the left-hand sidebar. Save the file with a .sol extension (e.g., avax.sol). Copy and paste the following code into the file:




```bash
 // SPDX-License-Identifier: MIT
pragma solidity ^0.8.0;

contract CorruptionReporting {
    struct Report {
        address reporter;
        string description;
        bool verified;
        bool rewarded;
    }

    address public admin;
    uint public reportCount;
    mapping(uint => Report) public reports;

    event ReportSubmitted(uint indexed id, address indexed reporter);
    event ReportVerified(uint indexed id);
    event RewardIssued(uint indexed id);

    modifier onlyAdmin() {
        require(msg.sender == admin, "Caller is not the admin");
        _;
    }

    constructor() {
        admin = msg.sender;
    }

    function submitReport(string memory _description) public {
        require(bytes(_description).length > 0, "Description is required");

        reports[++reportCount] = Report(msg.sender, _description, false, false);
        emit ReportSubmitted(reportCount, msg.sender);
    }

    function verifyReport(uint _reportId) public onlyAdmin {
        require(_reportId > 0 && _reportId <= reportCount, "Invalid report ID");

        Report storage report = reports[_reportId];
        require(!report.verified, "Report already verified");

        report.verified = true;
        emit ReportVerified(_reportId);
    }

    function issueReward(uint _reportId) public onlyAdmin {
        require(_reportId > 0 && _reportId <= reportCount, "Invalid report ID");

        Report storage report = reports[_reportId];
        require(report.verified && !report.rewarded, "Invalid reward state");

        report.rewarded = true;
        (bool success, ) = report.reporter.call{value: 1 ether}("");
        require(success, "Reward transfer failed");

        emit RewardIssued(_reportId);
    }

    function checkInvariant() public view {
        assert(reportCount >= 0);
    }

    function forceRevert(string memory _reason) public pure {
        revert(_reason);
    }

    receive() external payable{
	}
}
```

To compile the code, click on the "Solidity Compiler" tab in the left-hand sidebar. Make sure the "Compiler" option is set to "0.8.0" (or another compatible version), and then click on the "Compile Assesment.sol" button.

Once the code is compiled, you can deploy the contract by clicking on the "Deploy & Run Transactions" tab in the left-hand sidebar. Ensure "CorruptionReporting" is selected in the contract dropdown.
Click on the "Deploy" button.

In the "Deployed Contracts" section, expand the CorruptionReporting contract.Under the "submitReport" function, enter the report description.Click the "transact" button next to the submitReport function.

Under the "verifyReport" function, enter the report ID.Click the "transact" button next to the verifyReport function.Confirm the transaction in MetaMask.







## Authors

- Gungun
gungunbansal2604@gmail.com



## License

This project is licensed under [MIT](https://choosealicense.com/licenses/mit/) License - see the LICENSE.md file for details

