name: Build GBA ROM

on:
  workflow_dispatch:

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Repository
        uses: actions/checkout@v4
        with:
          ref: 'expansion/1.17.0'
          fetch-depth: 0

      - name: Install Build Dependencies
        run: |
          sudo apt-get update
          sudo apt-get install -y build-essential gcc-arm-none-eabi libpng-dev libsqlite3-dev

      - name: Build agbcc Compiler
        run: |
          ./build_agbcc.sh

      - name: Build ROM
        run: |
          make -j$(nproc)

      - name: Upload Built ROM Artifact
        uses: actions/upload-artifact@v4
        with:
          name: pokeemerald-expansion-1.17.0
          path: pokeemerald.gba
