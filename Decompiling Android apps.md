
2024-09-16 20:44

Source: #android 
### WHY 

Because of features such as `reflection`, Java applications generally retain all their symbols. Also, because the Dalvik bytecode is relatively high-level, it contains a lot of information on the structure and control-flow of classes. This means that we can get very good results when decompiling Android applications.
### JADX usage 

- Double click the name of the activity and it will automatically bring you to that class
- **Add a comment** -  semicolon or right click then `comment`

and we can also already see that if the password is correct, the second password activity will be started. Let's try to double click secret two.

00:18

Unfortunately, this just brings us to a class where the variable is defined with a number assigned to it. These strings being retrieved from the compiled in resources

00:26

and so we need to somehow find those. Luckily, jet X has a very powerful search function and so we can just right click secret two

00:35

and say Global search. By default, the global search will only search through the code, and so we also want

00:43

to enable resource here, and after this search has finished, we can see that we have four findings including one

00:49

that is in rest values strings that xml, and so let's double click that result and we can see that we found the string.

Nobody will ever find this seems to be the password. Let's give it a try. And yes, we are in. Awesome.

And now It's your turn. Can you find the second password for the application in the IPK

### References
[Reverse Engineering Android Apps (hextree.io)](https://app.hextree.io/courses/reverse-android-apps/decompiling-android-applications)

To install jadx, follow the instructions on the [jadx Github Repository](https://github.com/skylot/jadx?tab=readme-ov-file#download).