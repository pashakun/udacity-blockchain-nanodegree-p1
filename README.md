# udacity-blockchain-nanodegree-p1
Create Your Own Private Blockchain

Project Requirements
Your project will be evaluated using the Project Rubric. For a project to pass, all criteria must Meet Specifications. Please thoroughly read through the rubric before beginning to build your project.

What tools or technologies will you use to create this application?
This application will be created using Node.js and Javascript programming language. The architecture will use ES6 classes because it will help us to organize the code and facilitate the maintenance of the code.
We suggest you use Visual Studio Code as an IDE because it will help easily debug your code, but you can choose any code editor you feel comfortable with.
Some of the libraries or npm modules you will use are:
"bitcoinjs-lib": "^4.0.3",
"bitcoinjs-message": "^2.0.0",
"body-parser": "^1.18.3",
"crypto-js": "^3.1.9-1",
"express": "^4.16.4",
"hex2ascii": "0.0.3",
"morgan": "^1.9.1"
Remember if you think you need install any other library you will use npm install <npm_module_name>

Libraries purpose:

bitcoinjs-lib and bitcoinjs-message will help us verify wallet address ownership and signatures. Note: Make sure to always use Legacy Wallet addresses.
express is a node framework used to create The REST Api used in this project
body-parser is used as a middleware module for Express and will help us to read the json data submitted in a POST request.
crypto-js is a module containing some of the most important cryptographic methods and will help us create the block hash.
hex2ascii will help us decode the data saved in the body of a Block.
Remember all this libraries can be found in the package.json file in your project folder.

Understanding the boilerplate code
The Boilerplate code is a simple architecture for a Blockchain application, it includes a REST APIs application to expose your Blockchain application methods to your client applications or users.

app.js contains the configuration and initialization of the REST Api, the team who provide this boilerplate code suggest do not change this code because it is already tested and works as expected.
BlockchainController.js contains the routes of the REST Api. Those are the methods that expose the urls you will need to call when make a request to the application.
src/ In here we are going to have the main two classes we need to create our Blockchain application, we are going to create a block.js file and a blockchain.js file that will contain the Block and BlockChain classes.
Starting with the boilerplate code:
First thing first, we are going to download our boilerplate code

Then we need to install all the libraries and module dependencies, to do that: open a terminal and run the command npm install

( Remember to be able to work on this project you will need to have installed in your computer Node.js and npm )

At this point we are ready to run our project for first time, use the command: node app.js

You can check in your terminal the the Express application is listening in the PORT 8000

What do I need to implement to construct my block?
block.js file. In the Block class we are going to implement the method: validate().

The validate() method will validate if the block has been tampered or not.
Been tampered means that someone from outside the application tried to change values in the block data as a consequence the hash of the block should be different.
Steps:
Return a new promise to allow the method be called asynchronous.
Create an auxiliary variable and store the current hash of the block in it (this represent the block object)
Recalculate the hash of the entire block (Use SHA256 from crypto-js library)
Compare if the auxiliary hash value is different from the calculated one.
Resolve true or false depending if it is valid or not.
Note: to access the class values inside a Promise code you need to create an auxiliary value let self = this;

block.js file. In the Block class we are going to implement the method: getBData().

Auxiliary Method to return the data stored in the body of the block but decoded.
Steps:
Use hex2ascii module to decode the data
Because data is a javascript object use JSON.parse(string) to get the Javascript Object
Resolve with the data and make sure that you don't need to return the data for the genesis block or Reject with an error.

What do I need to implement to construct my blockchain class?
blockchain.js file. In the Blockchain class we are going to implement the method: _addBlock(block).

_addBlock(block) will add a block in the chain
The method will return a Promise that will resolve with the block added or reject if an error happen during the execution.
You will need to check for the height to assign the previousBlockHash, assign the timestamp and the correct height...
At the end you need to create the block hash and push the block into the chain array. Don't forget to update the this.height class variable.
Note: the symbol _ in the method name indicates in the javascript convention this method is a private method.
blockchain.js file. In the Blockchain class we are going to implement the method: requestMessageOwnershipVerification(address)

The requestMessageOwnershipVerification(address) method will allow you to request a message that you will use to sign it with your Bitcoin Wallet (Electrum or Bitcoin Core)
This is the first step before submit your Block.
The method return a Promise that will resolve with the message to be signed

<WALLET_ADDRESS>:${new Date().getTime().toString().slice(0,-3)}:starRegistry;

You will need to replace <WALLET_ADDRESS> with the wallet address submitted by the requestor and the time in your message will allow you to validate the 5 minutes time window.

blockchain.js file. In the Blockchain class we are going to implement the method: submitStar(address, message, signature, star)

The submitStar(address, message, signature, star) method will allow users to register a new Block with the star object into the chain. This method will resolve with the Block added or reject with an error.
Algorithm steps:
Get the time from the message sent as a parameter example: parseInt(message.split(':')[1])
Get the current time: let currentTime = parseInt(new Date().getTime().toString().slice(0, -3));
Check if the time elapsed is less than 5 minutes (compare the time in the message and currentTime)
Verify the message with wallet address and signature: bitcoinMessage.verify(message, address, signature)
Create the block and add it to the chain
Resolve with the block added.
blockchain.js file. In the Blockchain class we are going to implement the method: getBlockByHash(hash)

This method will return a Promise that will resolve with the Block with the hash passed as a parameter.
Search on the chain array for the block that has the hash for example investigate about filter in the array object in javascript.
blockchain.js file. In the Blockchain class we are going to implement the method: getStarsByWalletAddress (address)

This method will return a Promise that will resolve with an array of Stars on the chain that belong to the owner with the wallet address passed in as parameter.
This method will always return an array because a person can register more than one Star.
blockchain.js file. In the Blockchain class we are going to implement the method: validateChain()

This method will return a Promise that will resolve with the list of errors when validating the chain.
Steps to validate:
You should validate each block using validate() method from each of the blocks in the chain.
Each Block should check the with the previousBlockHash to make sure the chain isn't broken.
