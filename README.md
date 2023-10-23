# Project Guide README
## Overview
This smart contract is specifically tailored to oversee non-fungible tokens (NFTs) on the [Blockchain Network] platform, providing crucial functionality for minting, burning, and transferring NFTs. NFTs represent unique digital or physical assets on the blockchain. This README is your definitive guide for deploying and interacting with the NFT contract

## Functionalities
### Mint
-**Purpose**: Generate new NFTs.
-**Usage**: Mint fresh NFTs by designating the connected content or asset along with the recipient's address.

### Burn

-**Purpose**: Eliminate NFTs from the ecosystem.
-**Usage**: Irreversibly erase particular NFTs.

### Transfer

-**Purpose**: Exchange ownership of NFTs.
-**Usage**: Shift NFTs from one address to another, allowing for purchases, sales, gifts, and various other methods of transferring ownership.

# Deployment

## Código Platform
Código is an AI-Powered Code Generation Platform for blockchain developers and web3 teams that saves development time and increases the code's security across various blockchains.

## Getting started
- You can immediately start using [Código Studio](https://studio.codigo.ai). Código Studio is a web-based IDE environment that comes with all the tools and programs to develop Solana programs using the CIDL.
- You can work from your local environment by downloading the latest version of the [Código CLI](https://github.com/Codigo-io/platform/releases) that targets your operating system. After downloading, put the Código CLI in your preferred location and add this location to your environment PATH.

## CIDL Quickstart
In this Quickstart guide, you will get acquainted with Código's Interface Description Language (CIDL) as you embark on the journey to create a straightforward Solana counter program.

>If you're joining us from your local environment, this guide assumes that you've already installed and set up the Solana tool suite. If you're using Código Studio, there's no need to be concerned – the Solana tool suite is pre-installed and configured.

### 1. NFT Contract
Create a `nft.yaml` file and copy and paste the following CIDL.
cidl: "0.8"
info:
  name: nft
  title: RiseIn NFT
  version: 0.0.1
  license:
    name: Unlicense
    identifier: Unlicense
types:
  GemMetadata:
    solana:
      seeds:
        - name: "gem"
        - name: mint
          type: sol:pubkey
    fields:
      - name: color
        type: string
        solana:
          attributes: [ cap:16 ]
      - name: rarity
        type: string
        solana:
          attributes: [ cap:16 ]
      - name: short_description
        type: string
        solana:
          attributes: [ cap:255 ]
      - name: mint
        type: sol:pubkey
      - name: assoc_account
        type: rs:option<sol:pubkey>
methods:
  - name: mint
    uses:
      - csl_spl_token.initialize_mint2
      - csl_spl_assoc_token.create
      - csl_spl_token.mint_to
      - csl_spl_token.set_authority
    inputs:
      - name: mint
        type: csl_spl_token.Mint
        solana:
          attributes: [ init ]
      - name: gem
        type: GemMetadata
        solana:
          attributes: [ init ]
          seeds:
            mint: mint
      - name: color
        type: string
      - name: rarity
        type: string
      - name: short_description
        type: string
  - name: transfer
    uses:
      - csl_spl_assoc_token.create
      - csl_spl_token.transfer_checked
    inputs:
      - name: mint
        type: csl_spl_token.Mint
      - name: gem
        type: GemMetadata
        solana:
          attributes: [ mut ]
          seeds:
            mint: mint
  - name: burn
    uses:
      - csl_spl_token.burn
    inputs:
      - name: mint
        type: csl_spl_token.Mint
      - name: gem
        type: GemMetadata
        solana:
          attributes: [ mut ]
          seeds:
            mint: mint
```
### 2. Generate the Solana program and client library
Open the terminal and type the following command

```shell
codigo solana generate nft.yaml
```








