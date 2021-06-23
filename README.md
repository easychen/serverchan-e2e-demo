# serverchan-e2e-demo
Server酱端对端加密函数和DEMO

## 端对端加密
新版Server酱支持消息的端对端加密。其流程为：

1. 在调用接口之前，先将 desp 内容通过加密函数（下文会讲算法）进行加密
1. 在调用接口时，通过 desp 传递加密后的内容，同时传递 encoded 参数 = 1
1. 在详情页面输入加密函数中传递的密码，通过JS在客户端界面查看内容

加密函数（ PHP版 ）的参数为：

- content : 需要加密的内容
- key : 阅读时输入的密码
- iv : 固定字串，由 SCT 和 UID 拼接而成。比如 UID 为1，那么 iv 即为 SCT1。

```php
function sc_encode($content, $key, $iv)
{
    $key = substr(md5($key), 0, 16);
    $iv = substr(md5($iv), 0, 16);
    return openssl_encrypt(base64_encode($content), 'AES-128-CBC', $key, 0, $iv);
}
```

欢迎 PR 贡献其他语言的加密函数或库
