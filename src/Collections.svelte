<script>
	import { onMount } from 'svelte';
  import { connected, web3, selectedAccount, chainId, chainData } from 'svelte-web3'

  let marketplaceContract = '0x594CDa36362037C4bC3068aDD61f780bD0870Dfc';
  let marketplaceABI = [
    {
      "inputs": [],
      "stateMutability": "nonpayable",
      "type": "constructor"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "collectionAddress",
          "type": "address"
        }
      ],
      "name": "CollectionDisabled",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "collectionAddress",
          "type": "address"
        }
      ],
      "name": "CollectionUpdated",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "previousOwner",
          "type": "address"
        },
        {
          "indexed": true,
          "internalType": "address",
          "name": "newOwner",
          "type": "address"
        }
      ],
      "name": "OwnershipTransferred",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "collectionAddress",
          "type": "address"
        },
        {
          "indexed": true,
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        },
        {
          "indexed": false,
          "internalType": "uint256",
          "name": "value",
          "type": "uint256"
        },
        {
          "indexed": true,
          "internalType": "address",
          "name": "fromAddress",
          "type": "address"
        }
      ],
      "name": "TokenBidEntered",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "collectionAddress",
          "type": "address"
        },
        {
          "indexed": true,
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        },
        {
          "indexed": false,
          "internalType": "uint256",
          "name": "value",
          "type": "uint256"
        },
        {
          "indexed": true,
          "internalType": "address",
          "name": "fromAddress",
          "type": "address"
        }
      ],
      "name": "TokenBidWithdrawn",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "collectionAddress",
          "type": "address"
        },
        {
          "indexed": true,
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        },
        {
          "indexed": false,
          "internalType": "uint256",
          "name": "value",
          "type": "uint256"
        },
        {
          "indexed": false,
          "internalType": "address",
          "name": "fromAddress",
          "type": "address"
        },
        {
          "indexed": false,
          "internalType": "address",
          "name": "toAddress",
          "type": "address"
        }
      ],
      "name": "TokenBought",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "collectionAddress",
          "type": "address"
        },
        {
          "indexed": true,
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        }
      ],
      "name": "TokenNoLongerForSale",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "collectionAddress",
          "type": "address"
        },
        {
          "indexed": true,
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        },
        {
          "indexed": false,
          "internalType": "uint256",
          "name": "minValue",
          "type": "uint256"
        },
        {
          "indexed": true,
          "internalType": "address",
          "name": "toAddress",
          "type": "address"
        }
      ],
      "name": "TokenOffered",
      "type": "event"
    },
    {
      "anonymous": false,
      "inputs": [
        {
          "indexed": true,
          "internalType": "address",
          "name": "collectionAddress",
          "type": "address"
        },
        {
          "indexed": true,
          "internalType": "address",
          "name": "from",
          "type": "address"
        },
        {
          "indexed": true,
          "internalType": "address",
          "name": "to",
          "type": "address"
        },
        {
          "indexed": false,
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        }
      ],
      "name": "TokenTransfer",
      "type": "event"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "",
          "type": "address"
        }
      ],
      "name": "collectionState",
      "outputs": [
        {
          "internalType": "bool",
          "name": "status",
          "type": "bool"
        },
        {
          "internalType": "uint256",
          "name": "royaltyPercent",
          "type": "uint256"
        },
        {
          "internalType": "string",
          "name": "metadataURL",
          "type": "string"
        }
      ],
      "stateMutability": "view",
      "type": "function",
      "constant": true
    },
    {
      "inputs": [],
      "name": "owner",
      "outputs": [
        {
          "internalType": "address",
          "name": "",
          "type": "address"
        }
      ],
      "stateMutability": "view",
      "type": "function",
      "constant": true
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "",
          "type": "address"
        }
      ],
      "name": "pendingBalance",
      "outputs": [
        {
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
        }
      ],
      "stateMutability": "view",
      "type": "function",
      "constant": true
    },
    {
      "inputs": [],
      "name": "renounceOwnership",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "tokenBids",
      "outputs": [
        {
          "internalType": "bool",
          "name": "hasBid",
          "type": "bool"
        },
        {
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        },
        {
          "internalType": "address",
          "name": "bidder",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "value",
          "type": "uint256"
        }
      ],
      "stateMutability": "view",
      "type": "function",
      "constant": true
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "",
          "type": "uint256"
        }
      ],
      "name": "tokenOffers",
      "outputs": [
        {
          "internalType": "bool",
          "name": "isForSale",
          "type": "bool"
        },
        {
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        },
        {
          "internalType": "address",
          "name": "seller",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "minValue",
          "type": "uint256"
        },
        {
          "internalType": "address",
          "name": "onlySellTo",
          "type": "address"
        }
      ],
      "stateMutability": "view",
      "type": "function",
      "constant": true
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "newOwner",
          "type": "address"
        }
      ],
      "name": "transferOwnership",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "contractAddress",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "royaltyPercent",
          "type": "uint256"
        },
        {
          "internalType": "string",
          "name": "metadataURL",
          "type": "string"
        }
      ],
      "name": "updateCollection",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "contractAddress",
          "type": "address"
        }
      ],
      "name": "disableCollection",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "contractAddress",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        },
        {
          "internalType": "uint256",
          "name": "minSalePriceInWei",
          "type": "uint256"
        }
      ],
      "name": "offerTokenForSale",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "contractAddress",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        },
        {
          "internalType": "uint256",
          "name": "minSalePriceInWei",
          "type": "uint256"
        },
        {
          "internalType": "address",
          "name": "toAddress",
          "type": "address"
        }
      ],
      "name": "offerTokenForSaleToAddress",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "contractAddress",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        }
      ],
      "name": "tokenNoLongerForSale",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "contractAddress",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        }
      ],
      "name": "enterBidForToken",
      "outputs": [],
      "stateMutability": "payable",
      "type": "function",
      "payable": true
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "contractAddress",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        }
      ],
      "name": "withdrawBidForToken",
      "outputs": [],
      "stateMutability": "payable",
      "type": "function",
      "payable": true
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "contractAddress",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        }
      ],
      "name": "acceptOfferForToken",
      "outputs": [],
      "stateMutability": "payable",
      "type": "function",
      "payable": true
    },
    {
      "inputs": [
        {
          "internalType": "address",
          "name": "contractAddress",
          "type": "address"
        },
        {
          "internalType": "uint256",
          "name": "tokenIndex",
          "type": "uint256"
        },
        {
          "internalType": "uint256",
          "name": "minPrice",
          "type": "uint256"
        }
      ],
      "name": "acceptBidForToken",
      "outputs": [],
      "stateMutability": "payable",
      "type": "function",
      "payable": true
    },
    {
      "inputs": [],
      "name": "withdraw",
      "outputs": [],
      "stateMutability": "nonpayable",
      "type": "function"
    }
  ]

	let collections = [
    '0x1Ca311D37D3130C4C8Ff8686745178Ff4Dbdbb09',
    '0xBFC0a6468b8F462a6F6AE619Ff49bA05C99029f5',
    '0xC6597f7609b3dDF95a86e4B1291eFC9E03C786A4',
  ];

  async function getMMAccount() {
    try {
      const accounts = await window.ethereum.request({
        method: 'eth_requestAccounts',
      });
      const account = accounts[0];
      return account;
    } catch(e) {
      updateMintMessage(`Something went wrong. Refresh and try again.`);
    }
  }

  async function switchNetwork(){
    // don't do this if no metamask (errors on coinbase wallet)
    if (!MetaMaskOnboarding.isMetaMaskInstalled()) {
      return false;
    }
    await ethereum.request({
      method: 'wallet_switchEthereumChain',
      params: [{ chainId: '0x4' }],
    });
  }



	onMount(async () => {
    const w3 = new Web3(Web3.givenProvider || "http://127.0.0.1:7545");
    const walletAddress = await getMMAccount();
    await switchNetwork();
    const gasPrice = await w3.eth.getGasPrice();
    const contract = new w3.eth.Contract(marketplaceABI, marketplaceContract, {from: walletAddress});
    let c = await contract.methods.collectionState('0x1Ca311D37D3130C4C8Ff8686745178Ff4Dbdbb09').call()
    // let c = await contract.methods.collectionState('0x1Ca311D37D3130C4C8Ff8686745178Ff4Dbdbb09').call()
    console.log(c)
		// const res = await fetch(c.metadataURL);
		// details = await res.json();
	});
</script>

<div class="collections">
	{#each collections as collection}
		<figure>
			<!-- <img src={photo.thumbnailUrl} alt={photo.title}> -->
			<figcaption>{collection}</figcaption>
		</figure>
	{:else}
		<p>loading...</p>
	{/each}
</div>

<style>
	.collections {
		width: 100%;
		display: grid;
		grid-template-columns: repeat(5, 1fr);
		grid-gap: 8px;
	}

	figure, img {
		width: 100%;
		margin: 0;
	}
</style>
