# hexo

_config.yml

```js
url: https://github.com/JacobHsu/vue
root: /vue

deploy:
    type: git
    repository: https://github.com/JacobHsu/vue
    branch: gh-pages
```

Settings > GitHub Pages > Source > gh-pages branch  

## hexo-theme-doc

[hexo-theme-doc](https://github.com/zalando-incubator/hexo-theme-doc)  

`$ git clone https://github.com/zalando-incubator/hexo-theme-doc-seed.git`  

vue  dir  `doc`  

`$ cd doc && npm install`  

_config.yaml

```js
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
root: /javascript

# Deployment
## Docs: https://hexo.io/docs/deployment.html
deploy:
    type: git
    repository: https://github.com/JacobHsu/javascript
    branch: gh-pages
```
ERROR Deployer not found: git
`npm install hexo-deployer-git --save`  

`$doc> hexo g`
`$doc> hexo d`  


### Visual Studio Code 

格式化 `alt+shift+F`  
extension  [Preview on Web Server](https://marketplace.visualstudio.com/items?itemName=yuichinukiyama.vscode-preview-server) `Launch on browser (ctrl+shift+l)`