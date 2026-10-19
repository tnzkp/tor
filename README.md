name: Build Tor
on: [push, workflow_dispatch]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - name: Install dependencies
        run: sudo apt-get update && sudo apt-get install -y git build-essential autoconf automake libevent-dev libssl-dev zlib1g-dev
      - name: Get Tor source
        run: git clone --depth 1 https://gitlab.torproject.org/tpo/core/tor.git
      - name: Build
        working-directory: tor
        run: |
          ./autogen.sh
          ./configure --disable-asciidoc
          make -j2
      - name: Check result
        working-directory: tor
        run: ./src/app/tor --version
