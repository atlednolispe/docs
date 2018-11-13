## Fiddler

### 断点

```
设置request拦截: bpu https://www.baidu.com
取消拦截: bpu

设置response拦截: bpafter https://www.baidu.com
取消拦截: bpa
```

### 请求重定向

```
1. 选择session
2. 点击请求部分的AutoResponder
3. Enable rules + Unmatched requests passthrough
4. 将请求拖入到规则中
5. 编辑重定向的结果
```

### 手机配置

```
1. 桥接访问Fiddler: ip:port下载证书
2. 开启decode和stream模式
```

## MITMPROXY

## 手机配置

```
1. mitm.it下载证书
```

### 简单过滤

```
## 过滤 f

## status_code != 200
!(~c 200)

## 域名qq.com
~d qq.com

## method post 且 url包含baidu
~m post & ~u baidu
```

### 断点

```
## 断点 i 配置规则

## e 编辑请求

## a 继续请求
```

### 联动python

```
mitmdump -s test.py

# test.py
from mitmproxy import ctx


def request(flow):
    headers = flow.request.headers
    headers = str(headers)
    ctx.log.info(headers)
    ctx.log.warn(headers)
    ctx.log.error(headers)

```