# Install & Configure Node.Js

## download node.js

- open link  https://nodejs.org/en/

```shell
//安装node.js
sudo apt-get install nodejs
//安装npm
sudo apt-get install npm

sudo npm install npm -g
```

- get original address

```shell
npm get registry 

> https://registry.npmjs.org/
```

- set as the mirror image of taobao

```shell
npm config set registry http://registry.npm.taobao.org/
```

- set as the original mirror image

```shell
npm config set registry https://registry.npmjs.org/
```