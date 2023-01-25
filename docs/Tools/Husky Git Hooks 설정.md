---  
category: Tools  
share: true  
title: Husky Git Hooks 설정  
date created: Wednesday, January 25th 2023, 2:44:00 pm  
date modified: Wednesday, January 25th 2023, 2:48:16 pm  
---  
  
  
  
  
## husky   
[Husky - Git hooks 문서](https://typicode.github.io/husky/#/)  
  
*yarn* 버전으로 설치  
```shell  
npx husky-init && yarn              # Yarn 1  
```  
  
## commit rule 강제하기  
[커밋 메시지 작성법](../%EC%9E%A1%EB%8F%99%EC%82%AC%EB%8B%88%20%F0%9F%A7%B8/%EC%BB%A4%EB%B0%8B%20%EB%A9%94%EC%8B%9C%EC%A7%80%20%EC%9E%91%EC%84%B1%EB%B2%95.md) 을 지키지 않으면 커밋 못하게 하는 설정법이다.  
  
[GitHub - conventional-changelog/commitlint: 📓 Lint commit messages](https://github.com/conventional-changelog/commitlint) 를 참고해서 세팅하면 된다.  
[commitlint - Lint commit messages](https://commitlint.js.org/#/guides-local-setup)  
  
### getting start  
```shell  
# Install commitlint cli and conventional config  
npm install --save-dev @commitlint/{config-conventional,cli}  
# For Windows:  
npm install --save-dev @commitlint/config-conventional @commitlint/cli  
  
# Configure commitlint to use conventional config  
echo "module.exports = {extends: ['@commitlint/config-conventional']}" > commitlint.config.js  
```  
    
    
To lint commits before they are created you can use Husky's `commit-msg` hook:  
```shell  
# Install Husky v6  
npm install husky --save-dev  
# or  
yarn add husky --dev  
  
# Activate hooks  
npx husky install  
# or  
yarn husky install  
```  
  
  
### Add hook  
```shell  
npx husky add .husky/commit-msg  'npx --no -- commitlint --edit ${1}'  
```  
  
## 참고 문서  
  
[Best way to baseline nestjs microservice – @tkssharma | Tarun Sharma | My Profile](https://tkssharma.com/nestjs-microservice-baseline-setup/)  
