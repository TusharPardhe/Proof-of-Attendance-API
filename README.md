# Attendify XRPL API library

## Project information

Attendify is an API for PoA(proof of attendance system) built on the XRP ledger that can provide easy and secure way to mint and distribute NFTs or verify attendance at a wide range of events and activities happening in real life or online. Few potential uses for it are: 

⚫University students can use the system to quickly verify attendance at lectures, seminars or other educational events. 

⚫Large events like conferences etc where it can be difficult to keep track of who has attended and who didn't. 

⚫Verifying employee showing up at meetings, training sessions etc.

Project was firstly developed during [XRPL NFT hackathon.](https://devpost.com/software/xrp-nft-attendence)

## Features

⚫ Creating new events and batch minting NFTs for it

⚫ Uploading NFT metadata to IPFS

⚫ Checking if it's possible to claim NFTs for particular event

⚫ Requesting claim for NFT from particular event. The new sell offer for NFT that has to be accepted by the user is returned

⚫ Event attendees lookup

⚫ NFT ownership verification

## Documentation

### Attendify.js

⚫ Current version of documentation for Attendify.js is in the `docs` folder and was generated by jsdoc. Latest version of docs is hosted [here](https://justanotherdevv.github.io/AttendifyXRPL/Attendify.html).

### REST API endpoints

This API is built using the ExpressJS framework and the XRPL library, it connect to the local Attendify.js class to perform most of the actions. 

⚫ `GET /api/mint` - Creates new claimable event and premints NFTs for it. You must have at least X XRP in order to use this to handle the reserve requirements for the NFTs and the minting process which uses [Tickets](https://xrpl.org/tickets.html), where X is amount of tokens that should be minted multiplied by 2 + additional XRP for feees.

**@param {string} walletAddress** - Wallet address from user requesting claim

**@param {integer} tokenCount** - Amount of NFTs that should be create for the event

**@param {string} url** - Url to Image for event | Preferably this should be stored on IPFS

**@param {string} title** - Title for event

**@param {string} desc** - Description of event

**@param {string} loc** - Location of event

⚫ `GET /api/claim` - Checks if user is eligible for NFT claim or creates new offer that allows for claiming NFT for specific event. Endpoint returns response with sellOfferID that has to be accepted by user afterwards.

**@param {string} walletAddress** - The wallet address of the user trying to claim

**@param {integer} type** - The type of claim (1 for checking claim status and metadata, 2 for claiming)

**@param {string} minter** - The minter address of the event

**@param {string} eventId** - The ID of the event

**Possible responses**

    -`404` - Event with specified ID does not exist

    -`claimed` - The NFT for specified event was already claimed by this user

    -`empty` - All NFTs for this particular event were already claimed

    -`success` - If `onlyCheckStatus` parameter was true it indicated that current parameters for selected event were sent

    -`transferred` - Indicates that new sell offer for NFT related to selected event was created successfully and details were sent to user

⚫ `GET /api/verifyOwnership` - Verifies whether or not user owns NFT with provided id for particular user.

**@param {string} walletAddress** - Wallet address from user requesting verification

**@param {string} signature** - Signature from requesting user

**@param {string} minter** - The minter address of the event

**@param {string} eventId** - The ID of the event

⚫ `GET /api/attendees` - Looks up attendees for event with specific id with NFTs create by minter address.

**@param {string} minter** - The minter address of the event

**@param {string} eventId** - The ID of the event

## ToDo

⚫ Whitelist system

⚫ Configuration: e.g. choosing whether or not NFTs are soulbound

⚫ If called again by the same user the claim endpoint should check if sell offer that wasn't accepted exists and it should be returned instead of showing that NFT was claimed

⚫ Checking whether or not deadline for claiming NFTs has passed

⚫ Integration with updated UI and XUMM wallet

## How to setup

⚫execute `npm i` in CLI

⚫copy .env.example content to .env file and fill with your data and API keys

⚫execute `npm run start` in CLI
