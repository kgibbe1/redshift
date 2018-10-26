# redshift

Stellar HD Accounts Generator - library.

[![npm version](https://img.shields.io/npm/v/@stellar-fox/redshift.svg)](https://www.npmjs.com/package/@stellar-fox/redshift)
[![npm license](https://img.shields.io/npm/l/@stellar-fox/redshift.svg)](https://www.npmjs.com/package/@stellar-fox/redshift)
[![GitHub top language](https://img.shields.io/github/languages/top/stellar-fox/redshift.svg)](https://github.com/stellar-fox/redshift)
[![GitHub code size](https://img.shields.io/github/languages/code-size/stellar-fox/redshift.svg)](https://github.com/stellar-fox/redshift)
[![GitHub tag](https://img.shields.io/github/tag/stellar-fox/redshift.svg)](https://github.com/stellar-fox/redshift)

```bash
$ npm i @stellar-fox/redshift
```

<br />




## [BIP39](https://github.com/bitcoin/bips/blob/master/bip-0039.mediawiki) hierarchical deterministic addresses

`BIP39` mnemonics can be used to generate deterministic series of account
addresses. The method implemented here uses `24` (or `12`) words to generate
series of deterministic _Stellar_ key pairs. `24` word mnemonics are also used
by many hardware wallets (such as _Ledger Nano S_). This library can also be
used to restore account based on the mnemonic that was generated earlier
by either the hardware wallet or any other wallets compatible with `BIP32`,
`BIP39` and `BIP44` specifications.

<br />




## [BIP44](https://github.com/bitcoin/bips/blob/master/bip-0044.mediawiki) multi-account hierarchy for deterministic wallets

_redshift library_ uses `BIP32` path with hardened derivation. One can choose
to derive key pair for default account (with `pathIndex` address equal to `0`)
or select `pathIndex` number of desired account hierarchy. The upper limit of
`pathIndex` number of child accounts is `2**31`.

<br />




## [SEP-0005](https://github.com/stellar/stellar-protocol/blob/master/ecosystem/sep-0005.md) key derivation methods for stellar accounts

_Stellar Ecosystem Proposal_ describes implementation of deterministic key
derivation for _Stellar_ accounts based on word mnemonic of different lengths.
_redshift library_ uses `24` (or `12`) word mnemonics.

<br />




> Tested with `SEP-0005` Vectors 3 and 4

```bash
$ npm run test

  Test Vector 3 (SEP-0005)
    BIP39 Seed
      ✓ should return: 937ae91...f567866
    Public Key (m/44'/148'/0')
      ✓ should return: GC3MM...BTPJQ (58ms)
    Public Key (m/44'/148'/1')
      ✓ should return: GB3MT...YYISO
    Public Key (m/44'/148'/2')
      ✓ should return: GDYF7...DGAKU
   ...
      ✓ should return: SBSHU...2ZEO2
    Secret Key (m/44'/148'/7')
      ✓ should return: SC2QO...YWC6E
    Secret Key (m/44'/148'/8')
      ✓ should return: SCGMC...MAQST
    Secret Key (m/44'/148'/9')
      ✓ should return: SCPA5...C37GF

  Test Vector 4 (SEP-0005)
    BIP39 Seed
      ✓ should return: d425d39...742a489
    Public Key (m/44'/148'/0')
      ✓ should return: GDAHP...B63EQ
    Public Key (m/44'/148'/1')
      ✓ should return: GDY47...4OJOC
    ...
    Secret Key (m/44'/148'/9')
      ✓ should return: SDXDY...UXNOS


  42 passing (567ms)
```

<br />




## experimenting in browser

> [RunKit with @stellar-fox/redshift](https://npm.runkit.com/@stellar-fox/redshift)

<br />




## experimenting in [node.js](https://nodejs.org/)

```bash
$ git clone git@github.com:stellar-fox/redshift.git
Cloning into 'redshift'...
$ cd redshift/src/lib/
$ npm i
$ npm start
🎉  Successfully compiled 1 file with Babel.
```

```javascript
redshift
```

> ```javascript
> { ENTROPY: { HIGH: 256, MEDIUM: 128 },
>   LANGUAGE:
>    { CN: 'chinese_simplified',
>      CT: 'chinese_traditional',
>      EN: 'english',
>      FR: 'french',
>      IT: 'italian',
>      JP: 'japanese',
>      KR: 'korean',
>      SP: 'spanish' },
>   genMnemonic: [Function: genMnemonic],
>   mnemonicToSeedHex: [Function: mnemonicToSeedHex],
>   genKeypair: [Function: genKeypair],
>   newAccount: [Function: newAccount] }
> ```

<br />




## examples

* Randomly generate `mnemonic` of a high entropy using english words.

    ```javascript
    redshift.genMnemonic().split(' ')
    ```

    > ```javascript
    > [ 'eye',
    > 'urge',
    > 'child',
    > 'before',
    > 'sudden',
    > 'this',
    > 'assault',
    > 'else',
    > 'brisk',
    > 'twelve',
    > 'hair',
    > 'topic',
    > 'divert',
    > 'raw',
    > 'onion',
    > 'cattle',
    > 'result',
    > 'birth',
    > 'catalog',
    > 'dice',
    > 'auction',
    > 'sibling',
    > 'goat',
    > 'initial' ]
    > ```


* Randomly generate `mnemonic` of a medium entropy in italian.

    ```javascript
    redshift.genMnemonic(redshift.LANGUAGE.IT, redshift.ENTROPY.medium)
    ```

    > ```javascript
    > 'desumere sogno cuculo stirpe sepolto salmone elfico giocare ...'
    > ```


* Generate hex `seed` from a given `mnemonic`.

    ```javascript
    mnemonic = redshift.genMnemonic()
    ```

    > ```javascript
    > 'hold awful slender tide arrange window burden erase bamboo ...'
    > ```

    ```javascript
    seed = redshift.mnemonicToSeedHex(mnemonic)
    ```

    > ```javascript
    > '016d98a5956955896613c59e277cf...318ca1e6d94f792316b8f5afa0d8f2dc6'
    > ```


* Generate hex `seed` from a given `mnemonic` and secret `passphrase`.

    ```javascript
    mnemonic = redshift.genMnemonic()
    ```

    > ```javascript
    > 'ride throw body pet abstract gossip few online acoustic scare ...'
    > ```

    ```javascript
    seed = redshift.mnemonicToSeedHex(mnemonic, 'my secret phrase')
    ```

    > ```javascript
    > '2b8b5c2a3bac1f54a5c716621e3c487...f54d68f7e14402ac9ff76f1fcf92096e'
    > ```


* Generate _stellar_ `keypair` object from a given `seed`.

    ```javascript
    kp = redshift.genKeypair(seed)
    ```

    > ```javascript
    > Keypair {
    > type: 'ed25519',
    > _secretSeed: <Buffer ... >,
    > _secretKey: <Buffer ... >,
    > _publicKey: <Buffer ... > }
    > ```

    ```javascript
    kp.<press-TAB-key>
    ```

    > ```javascript
    > kp.canSign  kp.publicKey     kp.rawPublicKey   kp.rawSecretKey
    > kp.secret   kp.sign          kp.signDecorated  kp.signatureHint
    > kp.verify   kp.xdrAccountId  kp.xdrPublicKey
    > ```

    ```javascript
    kp.publicKey()
    ```

    > ```javascript
    > 'GALJAMOTJC2OU6GRCGLOANONTMAVHI3ZD6PTTR7ED5NPSJV3D2VC37RL'
    > ```

    ```javascript
    kp.secret()
    ```

    > ```javascript
    > 'SCK2PGA6Q6YG6I77QLCW5ZENEWDDX4KHEBMT2AIV7FLAWCAGFCG2FZIT'
    > ```


* Generate _stellar_ `keypair` object from a given `seed` and `pathIndex`.

    ```javascript
    kp = redshift.genKeypair(seed, 27)
    ...
    kp.publicKey()
    ```

    > ```javascript
    > 'GBDKU27YWDIRYDFAZF5J2JNPI2CPICSML4VBREP3SC45MKV433NHRNCR'
    > ```

    ```javascript
    kp.secret()
    ```

    > ```javascript
    > 'SBNWL6JJ3Q5CS6U4JUVUTOWIU24NDCWEI5P7BINENF4K3PHRST3QDOZH'
    > ```


* Generate object with a new `mnemonic` of high entropy and resulting
    `seed` and _stellar_ `keypair`. Optionally `passphrase` and `pathIndex`
    can be passed as an arguments.

    ```javascript
    redshift.newAccount("strawberry fields forever", 27)
    ```

    > ```javascript
    > { mnemonic: 'spell crawl shiver swallow ecology mercy unaware ...',
    > passphrase: 'strawberry fields forever',
    > pathIndex: 27,
    > seed: 'd3dbb69cf5a538ef8594fafd364...2370a10a806b9a44be5157917',
    > keypair:
    > Keypair {
    >     type: 'ed25519',
    >     _secretSeed: <Buffer ... >,
    >     _publicKey: <Buffer ... > } }
    > ```

<br />




## support

```
GAUWLOIHFR2E52DYNEYDO6ZADIDVWZKK3U77V7PMFBNOIOBNREQBHBRR
```