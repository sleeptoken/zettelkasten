
2024-05-25 13:04

Source:  #bandit 

Tags: [[git]] [[ssh]] [[linux]]

A git repository at `ssh://bandit30-git@localhost/home/bandit30-git/repo` via the port `2220`
to clone the repo use git clone `ssh://bandit30-git@localhost:2220/home/bandit30-git/repo`
notice how and where port number is added

`git log` - to check all the commits made (use this first)
`git branch -r` -  to check all the branches available 
`git checkout commit-id` - to checkout a particular commit 
`git tag` - to check the tags 
`git show tag-name` - to see specific tags

To push to a remote repo 
	- Check if .gitignore file exits and if it does then check what it is set to ignore 
	 - if you have to push something that is mentioned in gitignore then delete(`rm`) gitignore 
- `git add file-name`
- `git commit -m "commit-message"`
- `git push`

### References

https://mayadevbe.me/posts/overthewire/bandit/level28/
https://mayadevbe.me/posts/overthewire/bandit/level29/
https://mayadevbe.me/posts/overthewire/bandit/level30/
https://mayadevbe.me/posts/overthewire/bandit/level31/
https://mayadevbe.me/posts/overthewire/bandit/level32/