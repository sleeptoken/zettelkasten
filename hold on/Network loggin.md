A Syslog process can run on a network device and send event messages to a logging server. And that logging server is usually known as a Syslog server or a Syslog viewer. And this will provide information about the device and the network to a central location.

> The protocol is defined by RF 5424
Syslog is a standardized mechanism for logging on computer systems.

First, it is used to describe the actual log messages that you get either in transit on a network or stored on a computer.

The tools used to route, process, transmit, and store log messages, such as RSyslog, and Syslog-ng can also just be called Syslog.

RSyslog, is an open source software implementation of the basic Syslog protocol as defined in RF 5424,but it extends it with content based filtering, flexible configuration options, and adds features such as using TCP for transport.

The Syslog server often listens on UDP port 514, but also, TCP port 514 if configured to do so.

The default port for Syslog over TLS, which you should use if at all possible, is TCP port 6514.

Syslog has eight severity levels,

These severity levels are standardized as part of the RFC 

Syslog also uses a concept of what it calls facilities, which loosely relate to processes, and it's a way of categorizing messages. When devices send a message to a Syslog server, it includes one of the standard facility values, along with the severity level.
 
And these are all fully configurable per device.

Generally, you configure Syslog via a conf file

where that conf file is and what conf file that is

will depend on the Syslog that you have got.

Try running man Syslog to see where it might be.

Here, we are on Cali,

and the conf file is ETC RSyslog.conf,

but that won't necessarily be the case

depending on the version that you are running.

And you get a file like this,

and it's actually quite easy to understand.

So if you think about the facilities and the severity level,

if we look here, the facility is daemon,

and the log level or severity is everything.

And that's getting sent to the daemon.log.

And if we look at this one,

we have the facility of everything

and the severity of everything.

So basically everything is going to Syslog here.

And we can see the other facilities,

and they're logging all severities there.

And when we come down a little bit,

we can see the mail facility.

And for this one, it's just info.

This one warning, this one error,

and it's going through to the info warning and error.

And of course, you can configure where the Syslog server is

and the port and all the rest of those other things.

And that will all be slightly different

depending on the version you are using,

but it's all very similar.

And to help you think about

what you're actually going to get here,

if you set it to debug,

you're going to get absolutely everything.

It's going to send you every little noise

that that device wishes to make.

And all the way on the other side, emergency,

this is only going to send you very rare messages

of extreme situations for the device.

So it's a spectrum of how much information it will send you.

And just a word of warning here,

if you do set it to high levels,

it will send lots of information.

I've actually worked for some financial institutions

where they have set the log level incorrectly

and you had things like credit card information

in the log files, which is a big no-no.

Thankfully that was spotted and removed.

So just bear that in mind.

If you set the log level to debug informational,

just be careful about what it's sending.

Let's look at DDWRT as a custom router example for Syslog.

If we go to services here and we scroll down,

we have the simple ability to enable

and disable the Syslog and provide the remote server.

So not much there in the GUI in the way of configuration.

But you can go via SSH and configure it via command line,

and that obviously will give you more options via the GUI.

If we go to security and log management, we can enable this.

We have the log level.

This is essentially the severity level,

so it's a shorthand for the ones that are in Syslog.

It's only giving you the option of low, medium, and high.

And then you can log each of these chains if you choose to.

So this is essentially the firewall facility for Syslog.

You can choose which one you want,

and then this is the severity level.

If you enable that in DDWRT,

then Syslog will be logging the drops,

the rejects, the accepts, whatever you configure it to be.

So you'll be able to see that in your Syslog server.

Also locally on DDWRT, if you go to Syslog.asp,

you can actually see the log itself.

If we SSH into DDWRT, we can play around with Syslog.

DDWRT does use the standard Syslog,

and we can just, for example, tail the Syslog,

which is here on DDWRT.

Minus F1 tail, by the way, means it will constantly refresh.

And there we see some kernel facility severity info,

and you can see the network interfaces

been put into a promiscuous mode and out promiscuous mode.

And that was doing the TCP dump.

A little trick you can do,

if you create a symbolic link,

which I'm doing here with LN minus S to the Syslog file,

and then map it to your web server route.

In this case, it has a web server route, it's DDWRT,

then you're gonna be able to see your Syslog file

via the web server.

Just a little tip there.

And then if I go here, user and then the file name,

whatever symbolic link I create, then I can see the Syslog.

As I've said, depending on the sort of Syslog you have,

it can work in slightly different ways.

So on this one, I'll do a minus H.

I can actually see that this version of Syslog

ignores the conflg file.

So how does it facilitate configuration?

Well, what it does is it uses switches at run time

to set the Syslog configuration.

And you can see that if you do,

and we can see here it's setting the parameters here,

and that's setting the remote service.

And what's happened is we've made the changes in the GUI,

it's restarted the process and using these switches

to configure it instead of the config file.

So that's Syslog on DDWRT.

Let me show you another example of how you would configure

your device to speak to your Syslog ser