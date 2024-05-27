
2024-05-27 18:31

Source: #youtube

Tags: [[win]] [[RDS]] 

The **AppData** system folder in Windows is the default location for application data, settings, and caches.
- **Local** 
	Device-specific settings and temporary files are stored in this folder
	Access it by typing this in the path area of the file explorer using the environment variable `%LOCALAPPDATA%`
- **LocalLow** – used for low-integrity application data (typically by apps with strict security settings)
- **Roaming** – it stores application settings and data that can be transferred from one computer to another (environment variable **%APPDATA%**). This folder can be used in [roaming profiles](https://theitbros.com/configuring-windows-roaming-user-profiles-in-active-directory/) or user profile discs in an RDS environment.

### References
[What is AppData Folder in Windows and How to Clean It Up? – TheITBros](https://theitbros.com/appdata-folder/#:~:text=The%20AppData%20folder%20on%20Windows%20has%20three%20subfolders%3A,one%20computer%20to%20another%20%28environment%20variable%20%25APPDATA%25%20%29.)
[Remote Desktop Services - Secure data storage | Microsoft Learn](https://learn.microsoft.com/en-us/windows-server/remote/remote-desktop-services/rds-plan-secure-data-storage)