- [English](#simple-nft-example-on-bttc-donau-testnet)
- [Chinese 中文版](#在-bttc-测试网发布-nft-的简单教程)
# Simple NFT Example on BTTC Donau Testnet
Build, mint, and send around your own ERC721 with BTTC Donau testnet!

Ref: [🏗 Scaffold-ETH - 🎟 Simple NFT Example][simple-nft-example]


# 🏃‍♀️ Quick Start
Required: [Node](https://nodejs.org/dist/latest-v12.x/) plus [Yarn](https://classic.yarnpkg.com/en/docs/install/#mac-stable) and [Git](https://git-scm.com/downloads)  
Optional: [IPFS][IPFS]

## 🛠 App Configuration
Open `Terminal` and build the project, please install node in advance
``` 
git clone url simple-nft-example
```

```
cd simple-nft-example
git checkout simple-nft-example
yarn install
yarn start
```
in a second terminal window, start your frontend
```
cd simple-nft-example
yarn chain
```

in a third terminal window, 🛰 deploy your contract, use `localhost` as your network
```
cd simple-nft-example
yarn deploy --network NETWORK_OF_CHOICE (localhost, bttc_donau)
```

Open http://localhost:3000 to see the app.
![localhost][pic_localhost]

## 🗳 Config IPFS
If you don't have an Infura IPFS project, please use desktop or command-line version of [**IPFS**][IPFS]. You'll need to edit below scripts to fit your needs.

IPFS API & port -> `packages/react-app/scripts/ipfs.js`
```
const localhost = {
   host: 'localhost',
   port: '5001',
   protocol: 'http',
   path: 'api/v0'
};
```
Mint the token will require uploading to IPFS -> `packages/hardhat/scripts/mint.js`
```
const ipfs = ipfsAPI({
  host: '127.0.0.1', 
  port: '5001', 
  protocol: 'http', 
  path: 'api/v0'
});
```
The frontend's test upload will use IPFS as well -> `packages/react-app/src/App.jsx`
```
const ipfs = ipfsAPI({ 
  host: "localhost", 
  port: "5001", 
  protocol: "http"
 });
 ```
There might be CORS problem，change the config file of IPFS
 ```
ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["your whitelist address", "http://localhost:3000", "http://127.0.0.1:5001"]'
ipfs config --json API.HTTPHeaders.Access-Control-Allow-Methods '["PUT", "POST"]'
 ```
Desktop version please go to `Settings` and change the `IPFS CONFIG` section, don't forget to save and restart the service.
![ipfs config][pic_ipfs_config]

For Infura IPFS project，change the project ID and secret in `mint.js` and `App.jsx`
 ```
const projectId = 'YOUR INFURA PROJECT ID';
const projectSecret = 'YOUR INFURA PROJECT SECRET';
 ```

 ## 💎 Mint NFT
 After the IPFS configuration is done, go the the frontend and test upload a file to IPFS, make sure the service is up and running. 
![ipfs upload tab][pic_ipfs_upload]

Before minting, go the `mint.js` script and change `toAddress` linked in the `localhost:3000`, you could use MetaMask or other wallets. Then we go back to the terminal and start minting NFT.
```
yarn mint
```
![mint][pic_mint]

After the NFTs are minted, try to send around the token (in congnitive window).
![MFT][pic_NFT]


# 🏦 Deploy the Contract!
## ✏️ Configuration for BTTC Donau testnet
Let's move from the `localhost` to the real world, take BTTC Donau testnet as an example.

Firtst edit `packages/hardhat/hardhat.config.js`
```
donau_bttc:{
   url: "https://pre-rpc.bt.io/",
   gasPrice: 300000000000000,
   accounts:{
      mnemonic: mnemonic(),
   },
}
// Change default network，from localhost to BTTC testnet
const defaultNetwork = "donau_bttc";
```

Edit `packages/react-app/src/constants.js` as well
```
donau_bttc: {
   name: "donau_bttc",
   color: "#e8a094",
   chainId: 1029,
   rpcUrl: `https://pre-rpc.bt.io/`,
   faucet: "https://testfaucet.bt.io/#/",
   blockExplorer: "https://testscan.bt.io/",
}
```
Modify `App.jsx` to change the default deloying network
```
const targetNetwork = NETWORKS.donau_bttc; 
```

## 🔑 Generate Account to Deploy Contract
First run `yarn generate` and get a new address. View account info & QR code with `yarn account`, you'll need to fund this account.
![yarn generate][pic_generate]

Go to the [**Faucet**][faucet] and get some test BTTs.
![bttc faucet][pic_faucet]

Deploy the contract on Donau testnet, if there's any warning related to insufficient gas, please follow the instruction and increase gasPrice or gasLimit.
```
yarn deploy --network bttc_donau
```

Mint NFT
```
yarn mint
```
Go the chain explorer to view your transaction or follow the [**Original tutorial**][simple-nft-example] and ship your app.

# 📋 Notes
🕵🏻‍♂️ Inspect the `Debug Contracts` tab to figure out what address is the `owner` of `YourCollectible`?

💼 Edit your deployment script `deploy.js` in `packages/hardhat/scripts`

🔏 Edit your smart contract `YourCollectible.sol` in `packages/hardhat/contracts`

📝 Edit your frontend `App.jsx` in `packages/react-app/src`


# 在 BTTC 测试网发布 NFT 的简单教程
构建、铸造并发送自己的 ERC721 到 BTTC Donau 测试链。 

参考: [🏗 Scaffold-ETH - 🎟 Simple NFT Example][simple-nft-example]

# 🏃‍♀️快速启动
准备工作：[Node](https://nodejs.org/dist/latest-v12.x/), [Yarn](https://classic.yarnpkg.com/en/docs/install/#mac-stable) 以及 [Git](https://git-scm.com/downloads) 

可能需要：[IPFS][IPFS]

## 🛠 配置应用
打开第一个终端 (Terminal) 窗口构建项目，此过程需要 node，请提前安装
``` 
git clone url simple-nft-example
```

```
cd simple-nft-example
git checkout simple-nft-example
yarn install
yarn start
```
打开第二个终端窗口，启动前端
```
cd simple-nft-example
yarn chain
```

第三个终端窗口用于发布合约，可以先在本地进行测试 (NETWORK_OF_CHOICE 填写 localhost)
```
cd simple-nft-example
yarn deploy --network NETWORK_OF_CHOICE (localhost, bttc_donau)
```

打开 http://localhost:3000 可以看到运行中的应用。
![localhost][pic_localhost]


## 🗳 配置 IPFS
如果没有 Infura 的 IPFS 项目，可以根据需要可以[下载 IPFS][IPFS] 的命令行版本或者更简单的桌面版本。并修改下列文件，进行 IPFS 的配置。

配置 IPFS 端口 -> `packages/react-app/scripts/ipfs.js`
```
const localhost = {
   host: 'localhost',
   port: '5001',
   protocol: 'http',
   path: 'api/v0'
};
```
铸造 NFT 时需要上传到 IPFS -> `packages/hardhat/scripts/mint.js`
```
const ipfs = ipfsAPI({
  host: '127.0.0.1', 
  port: '5001', 
  protocol: 'http', 
  path: 'api/v0'
});
```
以及前端测试上传到 IPFS 也需要修改配置 -> `packages/react-app/src/App.jsx`
```
const ipfs = ipfsAPI({ 
  host: "localhost", 
  port: "5001", 
  protocol: "http"
 });
 ```
可能会有跨域 (CORS) 问题，此时可以修改 IPFS 本身的配置文件
 ```
ipfs config --json API.HTTPHeaders.Access-Control-Allow-Origin '["your whitelist address", "http://localhost:3000", "http://127.0.0.1:5001"]'
ipfs config --json API.HTTPHeaders.Access-Control-Allow-Methods '["PUT", "POST"]'
 ```
桌面端需要到`配置`中修改上述内容并保存。
![ipfs config][pic_ipfs_config]


如果有 Infura 的 IPFS 项目，则仅需要修改 `mint.js` 以及 `App.jsx` 中的项目 ID 及 secret
 ```
const projectId = 'YOUR INFURA PROJECT ID';
const projectSecret = 'YOUR INFURA PROJECT SECRET';
 ```

 ## 💎 铸造 NFT
 完成 IPFS 配置后启动服务，可以先在前端进行上传测试，确保 IPFS 没有问题。
![ipfs upload tab][pic_ipfs_upload]

 正式开始前修改 `mint.js` 中的发送地址 `toAddress`，可以连接 MetaMask 等钱包，将连接的地址填入。回到终端/命令行中开始铸造。铸造成功后页面中就会出现6个 NFT，可以发送给其他地址。
```
yarn mint
```
![mint][pic_mint]

铸造到指定的地址后可以在前端查看：
![MFT][pic_NFT]


# 🏦 在正式环境中进行铸造
## ✏️ BTTC Donau 测试网的配置
在本地环境测试完成就可以在其他公链进行测试。此处选择 BTTC 的 donau 测试网，也可以换成其他链。

首先修改 `packages/hardhat/hardhat.config.js`
```
donau_bttc:{
   url: "https://pre-rpc.bt.io/",
   gasPrice: 300000000000000,
   accounts:{
      mnemonic: mnemonic(),
   },
}
// 修改默认的网络，从 localhost 修改为 BTTC 测试链
const defaultNetwork = "donau_bttc";
```

还需要修改 `packages/react-app/src/constants.js`
```
donau_bttc: {
   name: "donau_bttc",
   color: "#e8a094",
   chainId: 1029,
   rpcUrl: `https://pre-rpc.bt.io/`,
   faucet: "https://testfaucet.bt.io/#/",
   blockExplorer: "https://testscan.bt.io/",
}
```
修改 `App.jsx`，将发布合约的默认网络改为 BTTC 测试链
```
const targetNetwork = NETWORKS.donau_bttc; 
```

## 🔑 用于发布合约的账户配置
首先执行 `yarn generate` 创建一个新的地址，创建后可以通过`yarn account` 查看账户信息以及 QR 码
![yarn generate][pic_generate]

拿到地址后需要进行充值，测试网需要到[**水管**][faucet]获取 BTT。
![bttc faucet][pic_faucet]

在测试网发布合约，如果遇到 gas 不足的情况请按提示执行增加 gasPrice
```
yarn deploy --network bttc_donau
```

进行 NFT 铸造 
```
yarn mint
```
铸造成功后可以进行转账并在链上查询交易，也可按照[**原教程**][simple-nft-example]发布网站。

# 📋 其他事项
🕵🏻‍♂️ 合约发布后可以在 `Debug Contracts` 页面中查看合约 `YourCollectible` 的所有者  

💼 部署脚本 `deploy.js` 位于 `packages/hardhat/scripts`  

🔏 智能合约 `YourCollectible.sol` 位于 `packages/hardhat/contracts`  

📝 如果需要编辑前端，在 `packages/react-app/src` 中找到 `App.jsx`

[IPFS]: https://ipfs.tech/#install
[faucet]: https://testfaucet.bt.io
[simple-nft-example]: https://github.com/scaffold-eth/scaffold-eth/tree/simple-nft-example


[pic_localhost]: https://i.imgur.com/PyWMkaI.png
[pic_ipfs_config]: https://i.imgur.com/cPrl2Me.png
[pic_mint]: https://i.imgur.com/8FmgZ0L.png
[pic_ipfs_upload]: https://i.imgur.com/SjvifHl.png
[pic_generate]: https://i.imgur.com/Rx4sqCW.png
[pic_faucet]: https://i.imgur.com/vEUn2s3.png
[pic_NFT]: https://i.imgur.com/rgXDn6r.png