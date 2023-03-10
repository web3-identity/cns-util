# Why use it

This project is to simplify the use of conflux cns, without paying attention to the specific function details of different contracts, and will also process parameters and return values, making it easier to call and view results.

In addition, it supports the multicall method, which can call multiple different functions with apply different parameters in one RPC call

# How to use

## Install Dependence

`npm i @web3identity/cns-util`

Note: This project is depending on [js-conflux-sdk](https://www.npmjs.com/package/js-conflux-sdk), [buffer](https://www.npmjs.com/package/buffer) and [bignumber.js](https://www.npmjs.com/package/bignumber.js)\
if your project does not use it, run `npm i -D js-conflux-sdk buffer bignumber.js` to install first.

## Code Example

```javascript
const ADDRESS1 = "cfxtest:aargwwstcp4axhxgkxfuy9pent1vtmaskjwr12xfsj";
// registered 88888888.web3, xiaokonglong.web3
const ADDRESS2 = "cfxtest:aap7yfv4bhh5db8xrnu3w27v8dcjzwavty234a1hjz";
// registered 666666.web3
const NAME1 = "666666.web3";
const NAME2 = "88888888.web3";
const NAME2_SUBDOMAIN = "subdomain.88888888.web3";
const DNS_NOTATION = "\x05hello\x04web3\x00";

// Get CNSUtil instance
const cnsutil = new CNSUtil();
// Get CNSUtil instance with optional config, all options are individually configurable:
// const cnsutilWithConfig = new CNSUtil({
//   networkId: 1029,
//   rpc: "https://main.confluxrpc.com",
//   contracts: { // note: contract name should be uppercase
//     REVERSE_REGISTRAR: "",
//     BASE_REGISTRAR: "",
//     NAME_WRAPPER: "",
//     WEB3_CONTROLLER: "",
//     PUBLIC_RESOLVER: "",
//     REVERSE_RECORDS: "",
//     MULTICALL: "",
//   }
// });

// Get owner of name
const owner = await cnsutil.ownerOf(NAME1);

// Get owned names of address
const userDomains = await cnsutil.userDomains(ADDRESS1);

// Get forward resolved address of name
const forwardResolvedAddress = await cnsutil.address(NAME2);

// Get parent name and forward resolved address of subdomain name
const parent = await cns.parent(NAME2_SUBDOMAIN);

// Get expiration time
const nameExpires = await cnsutil.nameExpires(NAME2);

// Get reverse resolved name of address
// Note:
//   For security reasons, when the forward resolved record and reverse resolved record configs are inconsistent, the reverse resolved name will return empty. For detail reference:
//   https://web3-username.gitbook.io/.web3-username-docs/contract-api-reference/publicresolver#xu-yao-zhu-yi-de-shi
const name = await cnsutil.name(ADDRESS1);

// Get reverse resolved names of multiple addresses
// Note:
//   For security reasons, when the forward resolved record and reverse resolved record configs are inconsistent, the reverse resolved name will return empty. For detail reference:
//   https://web3-username.gitbook.io/.web3-username-docs/contract-api-reference/publicresolver#xu-yao-zhu-yi-de-shi
const names = await cnsutil.names([ADDRESS1, ADDRESS1]);

// Get status of name, results in: Valid, TooShort, Reserved, IllegalChar, Locked, Registered, SoldOut
const status = await cnsutil.status(NAME1);

// Get registrant contract address of name
const registrant = await cns.registrant(NAME1);

// Get controller contract address of name
const controller = await cns.controller(NAME1);

// Call multiple different functions with apply different parameters in one RPC call
const MULTICALL_PARAMS = [
  {
    method: "ownerOf",
    args: [NAME1],
  },
  {
    method: "userDomains",
    args: [ADDRESS1],
  },
  {
    method: "address",
    args: [NAME2],
  },
  {
    method: "parent",
    args: [NAME2_SUBDOMAIN],
  },
  {
    method: "nameExpires",
    args: [NAME2],
  },
  {
    method: "name",
    args: [ADDRESS1],
  },
  {
    method: "names",
    args: [[ADDRESS1, ADDRESS2]],
  },
  {
    method: "status",
    args: [NAME1],
  },
  {
    method: "registrant",
    args: [NAME1],
  },
  {
    method: "controller",
    args: [NAME1],
  },
];
const MULTICALL_RESULT = await cns.multicall(MULTICALL_PARAMS);

// utils
const { namehash, dnsNameNotationDecode, labelhash } = CNSUtil.utils;
namehash(NAME1);
labelhash(NAME1.split(".")[0]);
dnsNameNotationDecode(DNS_NOTATION);
```

# CNS related resources

- SDK: [web3ns.js](https://github.com/web3-identity/web3ns.js)
- Docs:
  - [Github](https://github.com/web3-identity/cns-contracts)
  - [Developer Docs](https://web3-username.gitbook.io)
  - [Contracts Docs](https://github.com/web3-identity/cns-contracts/tree/master/docs)
  - [Demo](https://github.com/zctocm/cns-demo)
  - [Contracts API](https://github.com/web3-identity/cns-contracts/blob/master/docs/index.md#solidity-api)

# CNS util project default config:

```javascript
const NETWORK = {
  mainnet: 1029,
  testnet: 1,
};

const DEFAULT_NETWORK = "testnet";

const RPC = {
  mainnet: "https://main.confluxrpc.com",
  testnet: "https://test.confluxrpc.com",
};

const ROOT = ".web3";

const RPC = {
  mainnet: "https://main.confluxrpc.com",
  testnet: "https://test.confluxrpc.com",
};

const CONTRACTS = {
  // come from: https://web3-username.gitbook.io/.web3-username-docs/deployment#conflux-core-mainnet
  mainnet: {
    CNS_REGISTRY: "cfx:acemru7fu1u8brtyn3hrtae17kbcd4pd9uwbspvnnm",
    REVERSE_REGISTRAR: "cfx:acfarpzehntpre0thg8x7dp0ajw4ms328ps634v1zk",
    BASE_REGISTRAR: "cfx:acg08bujp0kmsup1zk11c9mad7zd6648eybmv2kbha",
    NAME_WRAPPER: "cfx:acdpx5pyc9xkry6x84bdstvt52grxpj69uadprjs7p",
    WEB3_CONTROLLER: "cfx:ace0bgf408jt5kmw34k3mxx03tpsfpt8by010ma8ww",
    REVERSE_RECORDS: "cfx:achsgpgs5dgpmgpj2zd87apj6js33c07pjth6k33mj",
    UTIL: "cfx:ace8hzgt9rcwejnh7dw1861r881g9rcgyy04r298hn",
    MULTICALL: "cfx:acf0y8g4x1r8he6xhv9z0m3j3y5rxjhs5j4mr83808",
  },
  // come from: https://web3-username.gitbook.io/.web3-username-docs/deployment#testnet
  testnet: {
    CNS_REGISTRY: "cfxtest:acemru7fu1u8brtyn3hrtae17kbcd4pd9u2m761bta",
    REVERSE_REGISTRAR: "cfxtest:acfarpzehntpre0thg8x7dp0ajw4ms328pe1mm17vd",
    BASE_REGISTRAR: "cfxtest:acg08bujp0kmsup1zk11c9mad7zd6648eynbcjtndm",
    NAME_WRAPPER: "cfxtest:acapc3y2j7atme3bawvaex18hs36tn40uu5h6j3mtu",
    WEB3_CONTROLLER: "cfxtest:aca1858y5a9fnyx9rxd1c9knr517cd0e6afzzhgj01",
    REVERSE_RECORDS: "cfxtest:acgddsj3kah2f4f4c6959bvc4732f4juyj90h0zmg2",
    UTIL: "cfxtest:aca4w63ypgup8tryphprzfcrh5kh0hpbgasb2z3s0j",
    MULTICALL: "cfxtest:acexk57dcp2gcaydnyyz615b1993c319uup08gwwzs",
  },
};
```

# Getting Started

This project was bootstrapped with [Vite](https://cn.vitejs.dev/guide/#scaffolding-your-first-vite-project).

## Available Scripts

In the project directory, you can run:

### `npm i`

Install project dependence

### `npm run dev`

Runs the project in the development mode.

The page will reload if you make edits.\
You will also see any lint errors in the console.

### `npm run build`

Builds the project for production to the `dist` folder.\
It correctly bundles project in production mode and optimizes the build for the best performance.

The build is minified.
