## 一、Git

### 1. 用户                

#### 1.1 用户切换（https方式）

需要用其他git用户登陆clone代码的时候，可以通过修改git配置中的用户和邮箱来切换用户：
```js
git config --global user.name "YOURUSERNAME" 
git config --global user.email "YOUREMAIL"
```
尝试切换之后却一直没能成功，一直用的是原来的git用户来访问，猜测应该是有其他的默认验证方式，查阅了一下原来windows上有凭据工具叫做**windows凭据**，git使用这个凭据对应的是credential.helper，我们设置这个值为：
```js
git config credential.helper manager
```
manager对应的就是windows凭据工具，在git需要账户密码的时候，会先从指定的凭据管理器中查找凭据，如果查找不到，会弹框要求输入并记录，第二次访问的时候就可以使用相应的凭据, 所以在切换账户之后要记得清理本地缓存（这里对应的就是缓存凭据）。


