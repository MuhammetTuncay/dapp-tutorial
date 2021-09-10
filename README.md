Uygulama geliştirilirken kullanılanlar:
---
______

```bash
- React => Client Framework
- Ether.js => Ethereum Web Client
- Solidity
- Hardhat => Ethereum Dev Environment
```

##Ethereum Blockchain üzerinde akıllı kontrakt işlemli basit fullstack bir projedir.

## Projenin Yapım Aşamaları

**React projenin oluşturulması**

```bash
npx create-react-app project-name
```

**Proje için gerekli kurulumların yapılması**

```bash
npm install --save-dev @nomiclabs/hardhat-waffle ethereum-waffle chai @nomiclabs/hardhat-ethers ethers
```

**Hardhat projesinin oluşturulması**

```bash
npx hardhat
```

**Hardhat projesinin react projesi ile bağlantısının yapılması**

```bash
- Ana dizinde bulunan hardhat.config.js dosyasının içerisinde;
module.exports = {
  solidity: "0.8.4",
  paths: {
    artifacts: "./src/artifacts",
  },
  networks: {
    hardhat: {
      chainId: 1337,
    },
  },
};
  Bu düzenleme yapılmalıdır. 
```

```bash
npx hardhat compile
```

**Localhost üzerinde test ortamında çalışma yapılabilmesi için oluşturulacak test hesapları**

```bash
npx hardhat node
```

**Localhost üzerinde deploy işlemi**

```bash
npx hardhat run scripts/deploy.js --network localhost
```

**React projesini başlatarak aşağıdaki adımları gerçekleştir.**

```bash
npm run start
- src içerisinde ki App.js dosyasını aşağıda verilen şekilde değiştir. 
- Metamask cüzdanını oluştur ve test hesaplarından ilk hesabın secret keyini import ederek hesaba erişim sağla.
- Web üzerinden console açarak işlemlerini gerçekleştir.

import React, { useState } from 'react';
import { ethers } from 'ethers'
import Greeter from './artifacts/contracts/Greeter.sol/Greeter.json'
import './App.css';
const greeterAddress = "your_address"

function App() {
  const [greeting, setGreetingValue] = useState()
  async function requestAccount() {
    await window.ethereum.request({ method: 'eth_requestAccounts' });
  }
  async function fetchGreeting() {
    if (typeof window.ethereum !== 'undefined') {
      const provider = new ethers.providers.Web3Provider(window.ethereum)
      const contract = new ethers.Contract(greeterAddress, Greeter.abi, provider)
      try {
        const data = await contract.greet()
        console.log('data: ', data)
      } catch (err) {
        console.log("Error: ", err)
      }
    }
  }
  async function setGreeting() {
    if (!greeting) return
    if (typeof window.ethereum !== 'undefined') {
      await requestAccount()
      const provider = new ethers.providers.Web3Provider(window.ethereum);
      const signer = provider.getSigner()
      const contract = new ethers.Contract(greeterAddress, Greeter.abi, signer)
      const transaction = await contract.setGreeting(greeting)
      await transaction.wait()
      fetchGreeting()
    }
  }
  return (
      <div className="App">
        <header className="App-header">
          <button onClick={fetchGreeting}>Fetch</button>
          <button onClick={setGreeting}>Set</button>
          <input onChange={e => setGreetingValue(e.target.value)} placeholder="Set greeting" />
        </header>
      </div>
  );
}

export default App;
```


