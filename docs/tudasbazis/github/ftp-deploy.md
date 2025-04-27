---
title: FTP Deploy
description: A weboldal statikus tárhelyre történő deployolása
sidebar_label: FTP Deploy
---

# Deploy FTP-n keresztül

## Secret adatok beállítása
A GitHub repository beállításaiban a `/settings/secrets/actions` oldalon 3 repository secret-et kell létrehozni:
1. `FTP_PASSWORD`
2. `FTP_USERNAME`
3. `FTP_SERVER`

## deploy.yml

A repositoryban `.github/workflows/deploy.yml` néven kell létrehozni a fájlt, amit futtatni szeretnénk:
```
on: push
name: 🚀 Deploy website on push
jobs:
  web-deploy:
    name: 🎉 Deploy
    runs-on: ubuntu-latest
    steps:
    - name: 🚚 Get latest code
      uses: actions/checkout@v4
    
    - name: 📂 Sync files
      uses: SamKirkland/FTP-Deploy-Action@v4.3.5
      with:
        server: ${{ secrets.FTP_SERVER }}
        username: ${{ secrets.FTP_USERNAME }}
        password: ${{ secrets.FTP_PASSWORD }}
```

## Teljes példa egy Hugo projektre
```
name: Deploy Hugo to cPanel

on:
  push:
    branches:
      - main

jobs:
  build-and-deploy:
    runs-on: ubuntu-22.04

    steps:

    # 1. Repo klónozása
    - name: Checkout repository
      uses: actions/checkout@v4.2.2

    # 2. Yarn telepítése és függőségek telepítése
    - name: Setup Node.js
      uses: actions/setup-node@v3
      with:
        node-version: '22.13.1'

    - name: Install dependencies with Yarn
      run: yarn install

    # 2. Hugo telepítése
    - name: Setup Hugo
      uses: peaceiris/actions-hugo@v3.0.0
      with:
          hugo-version: '0.141.0'
          extended: true

    # 5. Build Hugo
    - name: Build site with Hugo 
      run: hugo --minify --gc

    # 6. Fájlok feltöltése FTP-n keresztül
    - name: Deploy to cPanel
      uses: SamKirkland/FTP-Deploy-Action@v4.3.5
      with:
        server: ${{ secrets.FTP_SERVER }}
        username: ${{ secrets.FTP_USERNAME }}
        password: ${{ secrets.FTP_PASSWORD }}
        local-dir: public/
        server-dir: /
        dangerous-clean-slate: true
```
