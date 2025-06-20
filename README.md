# Decentralized Exchange with custom token – FIIT STU LS 2024/25

**Autori:**  
Martin Kvietok 
Juraj Černička  
**Predmet:** Digital Currencies and Blockchain  
**Fakulta:** Fakulta informatiky a informačných technológií, STU Bratislava  

---

## Popis projektu

Tento projekt predstavuje implementáciu decentralizovanej burzy pomocou smart kontraktov v jazyku Solidity, spolu s JavaScriptovou interakciou a jednoduchým frontendom. Cieľom bolo pochopiť fungovanie DeFi platforiem, výmenu tokenov a správu likvidity v Liquidity Pooloch.

---

## Projekt je rozdelený do častí:

### 1. Smart kontrakty

#### `token.sol` – Vlastný ERC-20 token
- Podpora štandardných funkcií `transfer`, `approve`, `transferFrom`
- `mint()` – len vlastník môže emitovať nové tokeny
- `allowance` systém pre správu práv

#### `exchange.sol` – Burzový smart kontrakt
- **Liquidity Pool:** správa tokenov a ETH, podiely pre poskytovateľov likvidity
- **Výmenný kurz:** zachováva konštantný pomer pomocou invariantu `k`
- **Funkcie:**
  - `createPool()`, `addLiquidity()`, `removeLiquidity()`, `removeAllLiquidity()`
  - `swapETHForTokens()`, `swapTokensForETH()`
  - `getProviderLiquidity()` – zobrazenie podielu

### 2. JavaScript (frontend)

#### `exchange.js`
- Interakcia s kontraktami cez Web3
- Pridanie/odobratie likvidity, výmena tokenov
- Dynamické zobrazovanie stavu účtu

#### `index.html`
- Select box na výber účtu
- Zobrazenie tokenov, ETH a LP podielu

---

##  Testovanie

Testy vytvorené pomocou **Hardhat** a **Solidity Coverage**:
- `Token.js`: testy pre token kontrakt
- `Exchange.js`: testy pre `createPool()`, `addLiquidity()`, `removeLiquidity()`
- Testujú aj edge casy – napr. zlé allowance, nesprávny výmenný kurz

**Spustenie testov:**
```bash
cd test
npx hardhat coverage
```

---

## Odpovede

**Prečo sa výmenný kurz nemení?**  
Pridávanie a odoberanie likvidity udržuje konštantný pomer medzi tokenmi a ETH, výpočet:
```solidity
amountTokens = (token.balanceOf(address(this)) * msg.value) / address(this).balance;
```

**Schéma odmeňovania:**  
Každý swap necháva 3 % v poole – tie zvyšujú hodnotu podielov LP poskytovateľov bez nutnosti prepočtu.

**Optimalizácia gasu:**  
- Použitie `constant` premenných
- Ukladanie medzi-výpočtov do lokálnych premenných

---

## Bonus funkcionality

- Dynamické zobrazovanie účtu v UI
- Automatické prepočty výmenného kurzu s ohľadom na `rateDecimals`
- Testy pokrývajú aj edge casy a chovanie kontraktov pri zlyhaniach

---

## Používateľská príručka

- **Jazyk:** Solidity, JavaScript  
- **Frameworky:** Hardhat, Web3.js  
- **Prostredie:** MetaMask, lokálna sieť Hardhat  
- **GUI:** Web frontend (`index.html`)  
- **Testy:** Spustiteľné cez `npx hardhat coverage`  

---
