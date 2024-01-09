# 왜 HD Key에 ' 가 뒤에 붙는가?

An "HD path" is an instruction as to how to derive a keypair from a root secret. BIP44 specifies a schema for such paths as follows:

```
m / 44' / coin_type' / account' / change / address_index
```

where `m` is a constant symbol at the beginning of the path, `/` is the separator of path components, 44 is the the purpose value from BIP43, `'`denotes hardened derivation and the remaining four symbols are variable names for integers. A BIP44 path always has those five components where the first three are always hardened and the last two are always unhardened.

참조: https://github.com/confio/cosmos-hd-key-derivation-spec?tab=readme-ov-file#bip44
