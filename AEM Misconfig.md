
2024-11-25 19:49

Tags: [[Bug Bounty]]

The vulnerability was **Sensitive Data Exposure due to Adobe Experience Manager Misconfiguration**. **Adobe Experience Manager (AEM)** is a software solution that is a combination of Content Management System (CMS) and Digital Asset Management (DAM) system that helps companies to manage digital assets across multiple channels, apps, and sites. 
The DAM system is called as **Adobe Experience Manager Assets**, and it is a cloud based system that stores and organizes all the digital files.
### Discovery 

How do we know that the given website is using AEM ?
Google Dorking can be used to find if our target is using AEM to manage content.

```
site:target.* inurl:'/content/dam' ext:txt
```

This lists all the webpages that uses AEM to render a txt file in the target site

While testing for this vulnerability I didn’t have a particular target site in mind, so I tweaked the Google dork a bit to see which all websites are using AEM and the dork is as follows

```
site:*.* inurl:'/content/dam' ext:pdf
```

This lists all the sites which uses AEM to render a PDF file. It took me no time to find websites that are using this software, but now I need to make sure that these websites have a vulnerability disclosure program before diving deeper into the same.

Use [YesWeHack VDP Finder](https://github.com/yeswehack/yeswehack_vdp_finder) to check if the visited site has a vulnerability disclosure program. 

### Exploitation



### References
https://abhishek-praveen.github.io/posts/Story-of-a-Simple-AEM-Misconfiguration-Bug/