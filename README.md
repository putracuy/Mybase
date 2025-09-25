# Mybase
require("@nomicfoundation/hardhat-toolbox");

const PRIVATE_KEY = process.env.PRIVATE_KEY; // Kunci dompet Anda

module.exports = {
  solidity: "0.8.20",
  networks: {
    base: {
      url: "https://mainnet.base.org", // RPC Base mainnet
      accounts: [PRIVATE_KEY],
    },
    baseTestnet: {
      url: "https://goerli.base.org", // RPC Base testnet
      accounts: [PRIVATE_KEY],
    },
  },
};
name: Deploy to Base

on:
  push:
    branches:
      - main

jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - name: Checkout code
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v3
        with:
          node-version: 18

      - name: Install dependencies
        run: npm install

      - name: Deploy to Base
        run: npx hardhat run scripts/deploy.js --network base
        env:
          PRIVATE_KEY: ${{ secrets.PRIVATE_KEY }}
const hre = require("hardhat");

async function main() {
  const [deployer] = await hre.ethers.getSigners();
  console.log("Deploying from:", deployer.address);

  const HelloBase = await hre.ethers.getContractFactory("HelloBase");
  const helloBase = await HelloBase.deploy("Hello, Base!");

  await helloBase.deployed();
  console.log("Contract deployed to:", helloBase.address);
}

main()
  .then(() => process.exit(0))
  .catch((error) => {
    console.error(error);
    process.exit(1);
  });
