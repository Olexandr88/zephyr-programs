# zephyr-programs

## Setup

0.- Get Your Mercury JWT Tokens.
For this repo youll need 3 accounts and 6 tokens, as each account can manage only one program per network. 
So you will create 1 account for `soroswap`, 1 account for `phoenix` and 1 account for `aqua`.
Then, in https://main.mercurydata.app/ Dashboard click on "Get Access Token" to get yout JWT token.
Make sure to change from Mainnet to Testnet (Menu in the left) in order to get your token for the correct network

1.- Clone this Repo
`git clone https://github.com/soroswap/zephyr-programs.git`

`cd zephyr-programs`

2.- Fill with 3 Mercury JWT Tokens.
Because we will be deploying 3 Zephyr programs, we will need 6 different JWT tokens.

`cp .env.example .env`

Your `.env` should look like this

```bash
JWT_soroswap_mainnet=
JWT_soroswap_testnet=

JWT_phoenix_mainnet=
JWT_phoenix_testnet=

JWT_aqua_mainnet=
JWT_aqua_testnet=

```
3.- Run the Docker Container
Be sure to do this after setting your .env, if you do some changes into your .env, you`ll need to re do this again

`docker compose up -d`

4.- Enter to the Docker Terminal

`bash run.sh`

## Deploy a Zephyr Program
1.- Enter to the program folder
```bash
cd phoenix
```
2.- Verify tests are running ok. 
Zephyr programs need that you define the NETWORK environmental variable so they can be build with the correct Contract Addredss
```bash
NETWORK=testnet cargo test  -- --nocapture
```
3.- Deploy the zephyr program

For Mainnet youll do
```bash
mercury-cli --jwt $JWT_phoenix_mainnet --local false --mainnet true deploy
```

For Testnet youll do
```bash
mercury-cli --jwt $JWT_phoenix_testnet --local false --mainnet false deploy
```


This will build the program into `./target/wasm32-unknown-unknown/release/[YOUR_PROGRAM].wasm` and it will deploy it into the Zephyr VM.

The outout will be something like this
```bash
Parsing project configuration ...
Building binary ...
Deploying tables ...
[+] Table "zephyr_af0e4a6a909cc9ea0185197f8cfefac3" created successfully
Registering indexes (if any) ...
Registering dashboard (if any) ...
Deploying wasm ...
Reading wasm ./target/wasm32-unknown-unknown/release/zephyr_phoenix.wasm
(Size of program is 318015)
[+] Deployed was successful!
Successfully deployed Zephyr program.
```

Where `zephyr_af0e4a6a909cc9ea0185197f8cfefac3` is the address of your program.


4.- Check your program in https://main.mercurydata.app/
In Dashboard > Manage Program 
Or
https://main.mercurydata.app/custom-ingestion
Start Streaming the logs and check that the Program is INdexing the correct contract address
be aware to use a different JWT token for every program
TODO: Complete readme and enviroment to work with multiple zephyr programs, create a JWT for each program, automate scripts 

