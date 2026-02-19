# Strings

To be honest, strings might not need to exist, they can be replaced with `[]U8` and variants or be aliases/distincts of said types

However, for debate, here is the String types incase they are added

```rs
CString = [^]U8;
StringView = []U8;
StringBuilder = [..]U8;
StringFixed = [N]U8;
String = StringView | StringBuilder;//Pending
```