
2025-01-25 09:35

Source: #Cloud #CTF 

A fake email serves as the prompt for each challenge.

### How S3 URL's work

Challenge description - Email 
```
Hey Bob,

I got that text file with the flag in it from Nancy and uploaded it to our S3. Does anybody know why she insists on using insane bucket names? Anyway, it’s in a700de6aeab6ef373e7d. WTF, right? Let me know if you have any problems accessing it.
```

The bucket name is given. The file (object) can be guessed from the given information,
```
root@kali ~# aws s3 ls s3://a700de6aeab6ef373e7d
2017-09-18 12:16:31         35 flag.txt
```

Visit either style URL, get the flag via [[curl]]
```
root@kali ~# curl https://s3.amazonaws.com/a700de6aeab6ef373e7d/flag.txt

root@kali ~# curl https://a700de6aeab6ef373e7d.s3.amazonaws.com/flag.txt
```

### S3-provided ‘access denied’ error.

```
OK, wow… ANYBODY could access that last bucket, what a fiasco. We’re using a new one now. Tell me if you can get to it using the method we discussed.

https://s3.amazonaws.com/b1f507894bee098d7e9d/flag.txt
```

the option exists to allow object access to _any_ authenticated AWS user using a ‘[canned ACL](http://docs.aws.amazon.com/AmazonS3/latest/dev/acl-overview.html#canned-acl).’

To get the flag, [install](http://docs.aws.amazon.com/cli/latest/userguide/installing.html) `awscli` and [configure](http://docs.aws.amazon.com/cli/latest/userguide/cli-config-files.html) it using any valid AWS credentials. Requests to read the flag with the `s3` [subcommand](http://docs.aws.amazon.com/cli/latest/reference/s3/index.html) will now succeed.

```
root@kali ~# aws s3 cp s3://b1f507894bee098d7e9d/flag.txt . 

root@kali ~# cat flag.txt 
```

### Region locked bucket

```
The security guys said they set up yet another bucket and this time it can only be accessed by our instances in the Oregon region. Should take care of these access problems once and for all.

https://s3.amazonaws.com/238e2d3768a54ec910/flag.txt
```

Create an instance, grab the flag.

```
[ec2-user@ip-10-90-10-169 ~]$ curl icanhazip.com
52.36.85.8

[ec2-user@ip-10-90-10-169 ~]$ curl https://s3.amazonaws.com/238e2d3768a54ec910/flag.txt
flag{ThisIsAnInstanceOfYouGettingAFlag}
```

### Pre signed URLs

```
Frank - we’ve set up an ultra-secure S3 solution using highly advanced authentication token schemas on bucket 3b089c7fc5845c792a4. From now on you’ll need to use Access Key AKIAIGVSTJJMGLVUPVYQ and Signature “P%2BnFfsUVDKShQjPYVMg%2BAjq%2B7Go%3D”

Do NOT share that information with anyone or anything except the in-house OpsSecDevDoubleSec group secure credentialing storage solution.

By the way, the access expires Tuesday, September 18, 2018 7:54:59 PM (GMT).
```

This challenge is to test familiarity with [pre-signed URLs](http://docs.aws.amazon.com/cli/latest/reference/s3/presign.html). Pre-signed URLs have five pieces of information; bucket, object, access key, signature, and expiration. `awscli` tends to present them in this form:

```
https://<bucket>.s3.amazonaws.com/<object>?AWSAccessKeyId=<key>&Expires=<expiration>&Signature=<signature>
```

All of the information is given (bucket, key, signature), known (object), or calculable (the expiration needs to be in unix time). Put it together, and grab the flag.

### References
https://n0j.github.io/2017/10/02/aws-s3-ctf.html