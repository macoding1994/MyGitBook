# requests

>`requests` 是一个Python第三方库，在使用前需`pip install requests`
>
>在使用 `requests` 发送网络请求非常简单，而且也比Python自带的`urllib` 效率上高上一点点

## 导包

```python
import requests
```

## 创建

```python
import requests

url = 'www.baidu.com'
headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36"
}

# 发送一个get请求
# headers=headers 设置请求头，还是比较终于的
# User-Agent 相当于这个请求的身份证，如果服务器检测到你是什么妖魔鬼怪，
# 可能就会禁止你访问或者发一些假数据过来
res = requests.get(url=url,headers=headers)
```

> 对应一些 restfull api 也是非常方便的

```python
r = requests.put('http://httpbin.org/put', data = {'key':'value'})
r = requests.delete('http://httpbin.org/delete')
r = requests.head('http://httpbin.org/get')
r = requests.options('http://httpbin.org/get')
```

> 关于URL传参

```python
payload = {'key1': 'value1', 'key2': ['value2', 'value3']}
# requests可以将一个list进行传参 还是非常方便的
r = requests.get('http://httpbin.org/get', params=payload)
print(r.url)
# http://httpbin.org/get?key1=value1&key2=value2&key2=value3
```

> 关于POST请求

```python
import requests
import json

url = 'https://api.github.com/some/endpoint'
payload = {'some': 'data'}

r = requests.post(url, data=json.dumps(payload))
```

## 响应

> 上面的案例说明了如何各种请求
>
> 接下来最重要的情况如何获取返回的响应（数据）

```python
import request

url = 'www.baidu.com'
headers = {
        "User-Agent": "Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/73.0.3683.86 Safari/537.36"
}

res = requests.get(url=url,headers=headers)

res = requests.get(url=url,headers=headers)
# 如果请求成功200
if res.status_code == requests.codes.ok:
        # 返回 str 响应
        print(res.text)
        # 返回 btys 响应
        print(res.content)
```

todo cookies 使用