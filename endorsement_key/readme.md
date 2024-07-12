```bash
# 生成 rsa 对应的 ek，存储在 ek.pub 中

tpm2_createek -G rsa -u ek.pub -c key.ctx

# 持久化到NV RAM中

tpm2_evictcontrol -c key.ctx 0x81000008

# 使用该命令读取nv handle, 确认里面存了ek

tpm2_readpublic -c 0x81000008

# 将 ek.pub 转换成 openssl 能读取的 pem 格式

tpm2_readpublic -c key.ctx -f pem -o rsa_ek_pub.pem
```



