# NodeJS

nvm & npm



## nvm

### install

brew install

```
brew install nvm
```

profile setting

```
mkdir ~/.nvm
vi ~/.nvm/.profile

===.profile
# NVM
export NVM_DIR=~/.nvm
source $(brew --prefix nvm)/nvm.sh
===
```



### command

#### version

```
nvm --version
```

#### remote list

```
nvm ls-remote
```

#### install

```
nvm install --lts='Dubnium'
```

#### list

```
nvm ls
```

#### use

```
nvm use default
```



## npm

### package.json



#### package-lock.json

npm을 사용하여 의존성에 변화가 생겼을 경우 자동으로 생성되는 파일입니다.

이 파일은 파일이 생성되는 시점의 의존성 트리에 대한 정확한 정보를 가지고 있음





### command

#### list & list -g

```
npm list
npm list -g
npm list -g --depth=0
```



```
npm list <module>
npm list <module> -g

# 버전확인
npm view <module> version
```



#### npm install

```
npm install

# 취약점 확인 하지 않기 (^v6)
npm install --no-audit
```



#### npm audit

 취약점 확인

```
npm audit
```



#### npm fund

Running `npm fund ` will open the url listed for that given package right in your browser.

```
npm fund
```

