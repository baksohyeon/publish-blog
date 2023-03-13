---  
category: Blog/Tools  
share: true  
title: Husky Git Hooks 설정  
date created: Wednesday, January 25th 2023, 2:44:00 pm  
date modified: Wednesday, January 25th 2023, 7:57:03 pm  
---  
  
[Using lint-staged, husky, and pre-commit hooks to fail fast and early | by Dominic Fraser | codeburst](https://codeburst.io/continuous-integration-lint-staged-husky-pre-commit-hook-test-setup-47f8172924fc)  
## husky   
[Husky - Git hooks 문서](https://typicode.github.io/husky/#/)  
  
- 설치  
```shell  
# npm  
npm i husky --save-dev  
​  
# yarn  
yarn add --dev husky  
```  
  
  
- 옵션: 설정 파일을 지정하여 저장  
    
폴더이름을 따로 지정하지 않을 경우 `.husky`가 기본값으로 사용된다.  
```shell  
# `.husky/husky-config` 에 설정 파일 저장  
npx husky install ".husky/husky-config"  
```  
  
  
- 사용 방법  
```shell  
# pre-commit  
npx husky add .husky/husky-config/pre-commit "npm test"  
git add .husky/husky-config/pre-commit  
​  
# pre-push  
npx husky add .husky/husky-config/pre-push "npm test"  
git add .husky/husky-config/pre-push  
```  
  
## commit rule 강제하기  
[커밋 메시지 작성법](./%EC%BB%A4%EB%B0%8B%20%EB%A9%94%EC%8B%9C%EC%A7%80%20%EC%9E%91%EC%84%B1%EB%B2%95.md) 을 지키지 않으면 커밋 못하게 하는 설정법이다.  
  
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
  
### Test  
#### Test simple usage  
For a first simple usage test of commitlint you can do the following:  
```shell  
npx commitlint --from HEAD~1 --to HEAD --verbose  
```  
This will check your last commit and return an error if invalid or a positive output if valid.  
  
#### Test the hook  
```bash  
git commit -m "foo: this will fail"  
husky > commit-msg (node v10.1.0)  
No staged files match any of provided globs.  
⧗   input: foo: this will fail  
✖   type must be one of [build, chore, ci, docs, feat, fix, perf, refactor, revert, style, test] [type-enum]  
  
✖   found 1 problems, 0 warnings  
ⓘ   Get help: https://github.com/conventional-changelog/commitlint/#what-is-commitlint  
  
husky > commit-msg hook failed (add --no-verify to bypass)  
```  
  
![Pasted image 20230125175637.png](../attachments/Pasted%20image%2020230125175637.png)  
  
테스트 통과 확인 및 앵귤러 커밋 규칙을 지켰는지 확인 후 커밋한다.  
  
## 참고 문서  
[Best way to baseline nestjs microservice – @tkssharma | Tarun Sharma | My Profile](https://tkssharma.com/nestjs-microservice-baseline-setup/)  
[husky 로 git hook 하자 | 가비아 라이브러리](https://library.gabia.com/contents/8492/)  
[husky 적용 방법 | Dan DevLog](https://dantechblog.gatsbyjs.io/posts/husky/)