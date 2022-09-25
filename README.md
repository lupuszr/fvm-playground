Install lotus for m1
https://lotus.filecoin.io/developers/local-network/

export LOTUS_PATH=~/.lotus-local-net
export LOTUS_MINER_PATH=~/.lotus-miner-local-net
export LOTUS_SKIP_GENESIS_CHECK=_yes_
export CGO_CFLAGS_ALLOW="-D__BLST_PORTABLE__"
export CGO_CFLAGS="-D__BLST_PORTABLE__"
export LIBRARY_PATH=/opt/homebrew/lib
export FFI_BUILD_FROM_SOURCE=1
export PATH="$(brew --prefix coreutils)/libexec/gnubin:/usr/local/bin:$PATH"


za minera dodati 

export MINER_API_INFO=eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9.eyJBbGxvdyI6WyJyZWFkIiwid3JpdGUiLCJzaWduIiwiYWRtaW4iXX0.Ray1STHiaPpx9wlxIOFIE9WfiULJf1GeZX4KPpwcs2U:/ip4/127.0.0.1/tcp/2345/http



za workera 
./lotus-worker run --no-local-storage


// attach worker to miner:

./lotus-worker storage attach --init --seal ~/sealing


To add support for wasm.
rustup target add wasm32-unknown-unknown --toolchain nightly-2021-07-29-aarch64-apple-darwin

to build wasm:
cargo build

to install on network:
1. go where your lotus is located
2. ./lotus chain install-actor ROUTE_TO_ACTOR/target/debug/wbuild/fil_hello_world_actor/fil_hello_world_actor.compact.wasm

this will return a CID

3. ./lotus chain create-actor CID
 
 this will return an invokation ID code (e.g t01001)

then go:
``` 
./lotus chain invoke t01001 2 
```
/ \
 |_ num 2 means call method (see lib.rs)

 this should return the state as base64

call base64 -d to see the state

```
echo b0hlbGxvIHdvcmxkICMxIQ== | base64 -d
```
