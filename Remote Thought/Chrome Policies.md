
2025-03-04 18:42

Source: #web 

Tags: [[browser]]

jist of looking at the source code - 
we need to set the `token` value to our token (`2123feca33c077329b773d226cbdf1b5`) and it should work. That seems pretty simple. We can send it to `/giveflag?token=2123feca33c077329b773d226cbdf1b5` and we will be done?

### Chrome Policies

I figured out that the reason this wasn’t working was due to a browser policy. It is stored in the `policies.json` file and added to the browser files during build time (see Dockerfile).

```
{
"URLBlocklist": ["*/giveflag", "*?token=*"]
}
```

From what I read about Chrome policies, these rules are globs. To interpret this; `*/giveflag` blocks a URL which contains `/giveflag` literally and anything preceding it. This is pretty simple, however `*?token=*` is deceiving. In this case the `?` means is globed as a single character. This means that if you tried to bypass this filter by doing something like `?abc=xyz&token=...` it wouldn’t work as it would still match this rule!

In the end, I realized that URL encoding worked for both cases. `https://political-web.chal.irisc.tf/%67iveflag?%74oken=2123feca33c077329b773d226cbdf1b5`

### References

https://ireland.re/posts/irisctf_2025/