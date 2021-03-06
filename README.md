# SimpleIRCLibMono for Csharp Mono
**THIS LIBRARY IS STILL IN DEVELOPMENT**

This library should work for both windows and linux (Mono), windows git: [Windows](https://github.com/EldinZenderink/SimpleIRCLib). As you might have noticed, these are basically the same. I thought having two seperate repo's makes operating system specific changes (when necesary) easier. 
This library is designed to make communication through IRC easier to implement in your application. In comparison to other C# IRC libraries, this library also enables you to download using the DCC (XDCC) protocol, used by IRC. 

It's main features are:

  - Is simple to use.
  - Is simple to implement
  - It's lightweight
  - Multiple channel support
  - DCC Download support

### NuGet
[NuGet Package](https://www.nuget.org/packages/SimpleIRCLibMono)

### Version
1.0.1:
- Initial version

1.0.2:
- update to quit procedure
- shows quit messages in debug
- fixed pinger not quiting

1.0.2:
- update to quit procedure
- shows quit messages in debug
- fixed pinger not quiting

1.0.3:
- bug fix: channels with uppercases caused chat messages not to be parsed
### Usage - Console Application


This is a list with the most important methods available to you:

    (void)        setupIrc(ip, port, username, password, channel, chatOutputCallback);
    (void)        setDebugCallback(debugOutputCallback);
    (string)      downloadDir;            //is now string, can be changed while running instance 
    (void)        setDownloadStatus(downloadStatusCallback);
    (void)        startClient();
    (void)        stopClient();
    (bool)        isClientRunning();
    (object)      getDownloadProgress(string whichdownloaddetail) //see below
    (void)        sendMessage(message); 
    (string)      newUsername;            //is now string field instead of method
    (string)      newChannel;             //is now string field instead of method


Before you start programming, you need to get the package on the NuGet page for this library, or you need to download the dll file manually and reference it in your c# solution/project. Afterwards, you need to do the following:

`using SimpleIrcLib;`

After doing that, add the following code to start your irc client:

    SimpleIRC irc = new SimpleIRC();
    irc.setupIrc(ip, port, username, password, channel, chatOutputCallback);
    irc.setDebugCallback(debugOutputCallback);
    irc.setDownloadStatusChangedCallback(downloadStatusCallback);
    irc.startClient();
    
Your callbacks should/could look like this:

**chatOutputCallback:**

    void chatOutputCallback(string user, string message)
    {
        Console.WriteLine(user + ":" + message);
    }


**debugOutputCallback:**

    void debugOutputCallback(string debug)
    {
        Console.WriteLine("===============DEBUG MESSAGE===============");
        Console.WriteLine(debug);
        Console.WriteLine("===============END DEBUG MESSAGE===============");
    }
    
**downloadStatusCallback:**

    void downloadStatusCallback() //see below for definition of each index in this array
    {
         Object information = getDownloadProgress("progress");
         Object speedkbps = getDownloadProgress("kbps");
         Object status = getDownloadProgress("status");
         Object filename = getDownloadProgress("filename");
    }
    

And here is a bit of gibrish code for sending messages to the irc server:
    
    while (true)  //irc output and such are handled in different threads
    {
        string Input = Console.ReadLine();
        if (Input != null || Input != "" || Input != String.Empty)
        {
            irc.sendMessage(Input);
        }
    }


For getting information about the download in progress (in `downloadStatusChangeCallback()`), you can use this function:

`getDownloadProgress(string whichdownloaddetail)`

This will return an array of strings when a download is running, but if there is no download while you are requesting information, it will return a empty array, except for the first index which will contain the string "NULL".

Array that will be returned when downloading:

| Whichdownloaddetail  | What it is    | Explanation |
| ------------- |:-------------:| ----- |
| dccstring     | DCC receive string     | The bot sends you a string with connection details.|
| filename      | Filename               | Filename of the file that you currently are downloading. |
| size          | Filesize               | Size of the file that you currently  are downloading. (In bytes)|
| ip            | Server IP              | IP from the file server to which you have connected. |
| port          | Server Port            | Port from the file server to which you have connected. |
| pack          | Pack Number            | Packnumer corresponding to the file you have requested through XDCC.|
| bot           | Bot Name               | Bot from which you requested the file. |
| progress      | Progress               | Progress of completion is %.  |
| status        | status of download/connection| Gives status about download and connection, such as, waiting, downloading, failed | etc
| bps           | Bytes Per Second       | |
| kbps          | KBytes Per Second      | |
| mbps          | MBytes Per Second      | |



### Full Example 
An (quick and dirty) example can be found here **this works on my RPI, including (X)DCC :D **: 
[Example](https://github.com/EldinZenderink/SimpleIRCLibMono/blob/master/SimpleIRCLibMonoConsole/Program.cs)


A winform example including video tutorial (v1.1.2) (windows): 
[YouTube](https://www.youtube.com/watch?v=Y5JPdwFwoSI)

### Development
I will try to fix (significant) bugs as quick as possible, but due to my study taking a rollercoaster dive in a few days it might take a while before an actual update will appear. It is very barebone and does need some refinement. So, progress in development will come down to how much free time I have and how much of it I want to spend working on this library.

### Todos

- Some DCC fixes, most things seem to work, but there are some odd cases where it might not work.
- More readable code (getting better)
- Renaming some stupidly named names 


### Disclaimer
This library is still in alpha stadium, many things might go wrong and therefore I am not 
responsible for whatever happens while you use this application.

License
----

MIT


**Free Software, Hell Yeah!**

