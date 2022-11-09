# Sample Hardhat Project

This project demonstrates a basic Hardhat use case. It comes with a sample contract, a test for that contract, and a script that deploys that contract.

Try running some of the following tasks:

처음
```shell
yarn add --dev hardhat
yarn hardhat
```

다운로드
```shell
yarn add --dev prettier prettier-plugin-solidity
yarn add --dev dotenv
yarn add --dev @chainlink/contracts
yarn add --dev hardhat-deploy
yarn add --dev @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers ethers
```

테스트
```
yarn hardhat test
yarn hardhat --network goerli //staging test
```

컴파일
```
 yarn hardhat compile
```
배포
```
yarn hardhat deploy
//mocks만 deploy
yarn hardhat deploy --tags mocks 
//fundme만
yarn hardhat deploy --tags fundme
//goerli testnet
yarn hardhat deploy --network goerli
```

로컬 노드 환경 실행
```
yarn hardhat node
```

참고</br>
chainlink price feeds => https://docs.chain.link/docs/data-feeds/price-feeds/</br>
오라클 문제 
https://couplewith.tistory.com/entry/%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8-%EB%B8%94%EB%A1%9D%EC%B2%B4%EC%9D%B8%EC%9D%98-%EC%98%A4%EB%9D%BC%ED%81%B4-%EB%AC%B8%EC%A0%9C-Oracle-Problem
</br>https://blog.chain.link/what-is-the-blockchain-oracle-problem-korean/

기타
```
1. unchecked를사용하면 gas efficient하다. (overflow가 안난다는 확신이 있다면)
2. s_funders = new address[](0); //새로운 빈(blank) address배열을 할당하는 문장
3. constant랑 immutable을 사용하면 gas efficient하다. (배포할때와 읽을때) 왜냐하면 storage slot을 차지하지 않고 contract의 bytecodes 안으로 들어간다고 한다. wow
4. constant variable은 CAP_WORDS, immutable variable은 변수앞에 prefix i_ 붙인다.
5. require에서 string 에러메세지가 gas를 많이 쓴다. 그래서 컨트랙트 밖에 error NotOwner()를 선언하고 custum 에러를 사용. if(msg.sender!= i_owner) {revert NotOwner();}
6. storage slot 하나는 32 byte 길이 slot임. 그리고 array이면 storage slot에 array length가 저장됨. eg.0x00...01 그리고 값은 해시를 사용해서 다른곳에 저장됨. 그리고 mapping은 storage slot을 차지하긴 하지만 blank임. constant와 immutable은 storage slot을 차지하지 않는다.
7. private이랑 internal variables 들이 좀더 싸다. 그래서 custom getter function으로 뺀다. 또한 storage variable의 네이밍에서 prefix로 s_ 이것을 붙였는데, 이 코드와 interact하는 사람이 헷갈리지 않도록 custom getter function을 쓰기도 한다.
```

test환경
```
unit 
 - 그냥 유닛테스트. 코드 다 돌려보기 coverage 채우기
 - local hardhat 환경 (forked hardhat도 가능)   
staging - testnet환경 테스트 (LAST STOP before deploy to mainnet)
```

배포환경
```
scripts 모듈을 사용해서 deploy하는 것(simple-storage-hardhat에서 한것)은 배포됐던것들을 keeping track하기 어렵다. 배포 했던것들을 어떤 형태의 파일로도 저장하지 않는다. 그리고 test와 deploy scripts가 연관되게 작동하지 않는다.

그래서 hardhat-deploy 를 사용함. https://github.com/wighawag/hardhat-deploy
A Hardhat Plugin For Replicable Deployments And Easy Testing
yarn add --dev hardhat-deploy
yarn add --dev @nomiclabs/hardhat-ethers@npm:hardhat-deploy-ethers ethers
harthat-ethers가 ethers를 override하고, hardhat-deploy-ethers가 또 harthat-ethers를 override함.
그리고 scripts폴더의 deploy파일이 아닌, deploy폴더에 deploy파일(코드)을 만든다.

yarn hardhat node를 하게 되면 로컬환경에서, 배포하는 컨트랙트가 노드에 추가되어있는 것을 볼 수 있다.
```

Mocking
```
unit testing을 위한 것. real object를 시뮬레이팅하는 object.
그래서 fake price feed contracts를 만들 수 있음. (로컬 환경에서 테스트하기 위해)
when going for localhost or hardhat network we want to use a mock
```

documentation 생성
```
solc --usderdoc --devdoc FundMe.sol
안함. solidity docs의 natspec 참고
```