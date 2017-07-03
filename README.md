# Mist Classic Browser

Mist Classic browser and wallet is a modified version of the [Mist](https://github.com/ethereum/mist) client restored to work with the original/classic chain. This client is maintained by the Ethereum Classic Project, the project that does not hardcode exceptions to the premise of unstoppable code. Ethereum Classic Project ensures unstoppable, censorship-resistant applications operating on a neutral world computer.

**LOOKING FOR MAINTAINERS**

**Project is compatible with ETC, but is not maintained. Mist has some issues with Windows #151 and doesn't have a build for Linux #35. If you're a JS developer with Windows or Linux, please take a look at this issues**

**NOTICE The project is going to be replaced with Emerald Wallet, which is currently under development.**


[![Join the chat at https://gitter.im/ethereumproject/mist](https://badges.gitter.im/Join%20Chat.svg)](#TBD)

The Mist(classic) browser is the tool of choice to browse and use Ðapps.

For the Mist API see the [MISTAPI.md](MISTAPI.md).

## Installation

If you want install the app from a pre-built version on the [release page](https://github.com/ethereumproject/mist/releases),
you can simply run the executeable after download.

For updating simply download the new version and copy it over the old one (keep a backup of the old one if you want to be sure).
The data folder for Mist is stored in other places:

- Windows `%APPDATA%\Mist`
- MacOSX `~/Library/Application Support/Mist`
- Linux `~/.config/Mist`


## Development

For development, a Meteor server will to be started to assist with live reload and CSS injection.
Once a Mist version is released the Meteor frontend part is bundled using `meteor-build-client` npm package to create pure static files.

### Dependencies

Requirements:

* Electron v1.2.5
* Node v4.3.0 or above

To run mist in development you need [Node.js NPM](https://nodejs.org) and [Meteor](https://www.meteor.com/install) and electron installed:

    $ curl https://install.meteor.com/ | sh
    $ npm install -g electron-prebuilt@1.2.5
    $ npm install -g gulp

### Installation

Now you're ready to install Mist:

    $ git clone https://github.com/ethereumproject/mist.git
    $ cd mist
    $ git submodule update --init
    $ npm install
    $ gulp update-nodes

To update Mist in the future, run:

    $ cd mist
    $ git pull && git submodule update
    $ npm install
    $ gulp update-nodes


### Run Mist

For development we start the interface with a Meteor server for autoreload etc.
*Start the interface in a separate terminal window:*

    $ cd mist/interface && meteor

In the original window you can then start Mist with:

    $ cd mist
    $ electron .


### Run the Wallet

Start the wallet app for development, *in a separate terminal window:*

    $ cd mist/interface && meteor

    // and in another terminal

    $ cd my/path/meteor-dapp-wallet/app && meteor --port 3050

In the original window you can then start Mist using wallet mode:

    $ cd mist
    $ electron . --mode wallet


### Passing options to Geth/Eth

You can pass command-line options directly to Geth/Eth by prefixing them
with `--node-`:

```bash
$ electron . --mode mist --node-rpcport 19343 --node-networkid 2
```


### Using Mist with a privatenet

To run a private network you will need to set the `networkdid`, `ipcpath` and
`datadir` flags:

```bash
$ electron . --ipcpath ~/Library/Ethereum/geth.ipc --node-networkid 1234  --node-datadir ~/Library/Ethereum/privatenet
```

_NOTE: since `ipcpath` is also a Mist option you do not need to also include a
`--node-ipcpath` option._

You can also run `geth` separately yourself with the same options prior to start
Mist normally.


### Deployment


To create a binaries you need to install the following tools:

    // tools for the windows binaries
    $ brew install Caskroom/cask/xquartz
    $ brew install wine
    $ npm install -g meteor-build-client

To generate the binaries simply run:

    $ cd mist
    $ gulp update-nodes

    // to generate mist
    $ gulp mist

    // Or to generate the wallet (using the https://github.com/ethereumproject/meteor-dapp-wallet -> master)
    $ gulp wallet

This will generate the binaries inside the `dist_mist` or `dist_wallet` folder.

#### Options

##### platform

Additional you can only build the windows, linux or mac binary by using the `platform` option:

    $ gulp update-nodes --platform darwin

    // And
    $ gulp mist --platform darwin

    // Or
    $ gulp mist --platform "darwin win32"


Options are:

- `darwin` (Mac OSX)
- `win32` (Windows)
- `linux` (Linux)


##### walletSource

With the `walletSource` you can specify the branch to use, default ist `master`:

    $ gulp mist --walletSource develop


Options are:

- `master`
- `develop`
- `local` Will try to build the wallet from [mist/]../meteor-dapp-wallet/app

##### mist-checksums | wallet-checksums

Spits out the SHA256 checksums of zip files. The zip files need to be generated manually for now!
It expects zip files to be named as the generated folders e.g. `dist_wallet/Ethereum-Wallet-macosx-0-5-0.zip`

    $ gulp mist-checksums

    > SHA256 Ethereum-Wallet-linux32-0-5-0.zip: 983dc9f1bc14a17a46f1e34d46f1bfdc01dc0868
    > SHA256 Ethereum-Wallet-win32-0-5-0.zip: 1f8e56c198545c235d47921888e5ede76ce42dcf
    > SHA256 Ethereum-Wallet-macosx-0-5-0.zip: dba5a13d6114b2abf1d4beca8bde25f1869feb45
    > SHA256 Ethereum-Wallet-linux64-0-5-0.zip: 2104b0fe75109681a70f9bf4e844d83a38796311
    > SHA256 Ethereum-Wallet-win64-0-5-0.zip: fc20b746eb37686edb04aee3e442492956adb546

### Code signing for production

After the bundle run:

    $ codesign --deep --force --verbose --sign "5F515C07CEB5A1EC3EEB39C100C06A8C5ACAE5F4" Ethereum-Wallet.app

Verify

    $ codesign --verify -vvvv Ethereum-Wallet.app
    $ spctl -a -vvvv Ethereum-Wallet.app
