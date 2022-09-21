---
layout: post
title: "Learn PowerShell"
tags: [powershell]
excerpt: "PowerShell was released on November 14, 2006 by Microsoft. Nowadays, it has become an incredibly powerful and popular command-line shell and scripting language, for system administration and automation, among IT professionals and system administrators.  "
--- 
  
PowerShell was released on November 14, 2006 by Microsoft. Nowadays, it has become an incredibly powerful and popular command-line shell and scripting language, for system administration and automation, among IT professionals and system administrators.  
  
Starting with version 6, the tool was open sourced by Microsoft. However, it's now PowerShell Core which is conceived to work cross-platform on all major operating systems such as Windows, macOS and Linux.  
    
Powershell Core is built on top of .NET Core, the open source version of .NET which is also cross-platform and runs under macOS and Linux in addition to Windows.  

PowerShell was released in November 14, 2006 by Microsoft. Nowadays, it has became an incredibly powerful and popular command-line shell and scripting language, for system administration and automation, among IT professionals and system administrators.  
  
Starting with version 6, the tool was open sourced by Microsoft. However, it's now PowerShell Core which is conceived to work cross-platform on all major operating systems such as Windows, macOS and Linux.  
  
> Note: The latest stable release is 7.1.1 which was released on January 14, 2021  
  
PowerShell is now not a Windows-only management tool but a shell that can be installed on multiple operating systems to enhance the management features across to all [supported](https://docs.microsoft.com/en-us/powershell/scripting/powershell-support-lifecycle?view=powershell-6) environments.  

You can still use the older version (5.1) which is available only on Windows devices, and based on the .NET Framework but if you want to be updated with the latest features or be able to manage other Linux-based systems and macOS with Powershell, you'll need to use Powershell Core, which is based on the new .NET Core runtime. 

> Note: Usesr can either use PowerShell (5.1) on WIndows only or PowerShell Core 6+ on Windows, Linux and macOS  
  
Powershell Core is built on top of .NET Core, the open source version of .NET which is also cross-platform and runs under macOS and Linux in addition to Windows.  
  
Because it's cross platform, it has many differences over the original PowerShell that runs only on Windows.  
  
The core features of PowerShell Core are:  
  
- It's available on the major operating systems used nowadays  
- It offers an unified experience across all supported platforms.  
  
PowerShell is similar to other shell environments -- it makes use of commands but these commands are named cmdlets.  
  
These are the key points and differences between Powershell and Powershell core that you need to know:
    
- PowerShell Core is the new, open-source version of PowerShell that can be installed and used under Linux, macOS, and Windows machines.  
- PowerShell is now available in two separate versions : PowerShell v5.1 and PowerShell Core v6+ which is the version that will be developed and updated with the new features. PowerShell 5.1 will only receive updates for critical bugs.
- Windows-only Powersell is based on .NET Framework, while PowerShell Core is built on top of .NET Core.  
- PowerShell Core have some missing modules and cmdlets which can be only used under Windows.  
  

### Launching PowerShell

>In Windows 10, the search field is one of the fastest ways to launch PowerShell. From the taskbar, in the search text field, type powershell. Then, click or tap the ‘Windows PowerShell’ result.

>To run PowerShell as administrator, right-click (touchscreen users: tap and hold) on the Windows PowerShell search result, then click or tap ‘Run as administrator’.

>There are also [_many_](https://www.digitalcitizen.life/ways-launch-powershell-windows-admin) other ways to start a PowerShell console, but this is a good method to begin with.

  
## Understanding Powershell  
  
In this section, we'll understand why we need to use a shell and why using PowerShell over the Command prompt and batch files on Windows.  
  
A shell allows casual users and system administrators alike to use the services offered by the underlying operating system. It's presented as a command-line interface for controlling your system using commands entered via a keyboard.  
  
PowerShell has various advantages over Command prompt and batch files. For example, let's cite some of them:  

In batch scripts, you can only use the commands that you can run on Command prompt and it's actually a series of CMD commands.
A batch script can be only used to automate simple tasks but if you need to make advanced tasks you'll have to use a more powerdull script language like PowerShell which is a fully-featured programming language that allows you to write complex logic to execute and automate advanced tasks with support for variables, functions, classes, loops, error handling, and more.  

PowerShell comes with an Integrated Scripting Environment or ISE which is more features and easy to use than Command Prompt with features like syntax highlighting, autocompletion, tabbed editing, and context-sensitive help.  
  
PowerShell is more powerful that CMD because it's built-it on top of the .NET Framework. PowerShell commands which are refreed to as a cmdlets are .NET classes that get called at runtime. This also means that cmdlets can be written in .NET languages such as C#.   

> PowerShell uses the notion of cmdlet, which is a simply a command but behind the curtains it's a .NET class that gets called at runtime. Cmdlets can be written in .NET languages such as C#.

PowerShell makes use of aliases which are simply alternative names for the existing cmdlets designed for allowing users that are familiar with the ather shells, such as Command Prompt or Unix Bash, to be quickly productive when using the new shell, by simply aliasing the commonly used commands with their equivalents in PowerShell.

For example, you can use `cd` in PowerShell to navigate into directories, just like it can be used in CMD and Bash but you're simply invloking the `Set-Location` cmdlet that does the same thing in PowerShell. 

> In this case, "cd" is just an alias for "Set-Location" and this makes life easier for you. Similarly, when you use **rename** in PowerShell, it's secretly running the **Rename-Item** cmdlet.  
  
  
If you are a system administrator or IT expert, you need to use PowerShell instead of CMD since all the commands in CMD are still available in addition to more powerfull cmdlets designed for versatile administration tasks. 

Since cmdlets are .NET classes, PowerShell can be extended by developers familar with .NET programming languages such as C# Third-party software vendors are extending PowerShell with custom cmdlets, like the [NetApp PowerShell Toolkit]([https://community.netapp.com/t5/Microsoft-Virtualization-Discussions/NetApp-PowerShell-Toolkit-4-5P1-released/td-p/138566](https://community.netapp.com/t5/Microsoft-Virtualization-Discussions/NetApp-PowerShell-Toolkit-4-5P1-released/td-p/138566)) that manages Data ONTAP.  
>PowerShell knowledge can be a differentiator for employment or even a job requirement, so it’s a worthwhile skill to invest in.  
  
  
>They are completely different, despite the illusion that the ‘dir’ command works the same way in both interfaces.  
>PowerShell uses cmdlets, which are self-contained programming objects that expose the underlying administration options inside of Windows.  
>PowerShell uses [pipes]([https://en.wikipedia.org/wiki/Pipeline_(Unix)](https://en.wikipedia.org/wiki/Pipeline_(Unix))) to chain together cmdlets and share input/output data the same way as other shells, like bash in linux. Pipes allow users to create complex scripts that pass parameters and data from one cmdlet to another. Users can create reusable scripts to automate or make mass changes with variable data – a list of servers, for example.  
>PowerShell is the ability to create aliases for different cmdlets. Aliases allow a user to configure their own names for different cmdlets or scripts, which makes it more straightforward for a user to switch back and forth between different shells: ‘ls’ is a linux bash command that displays directory objects, like the ‘dir’ command. In PowerShell, both ‘ls’ and ‘dir’ are an alias for the cmdlet ‘Get-ChildItem.’ 

    
## Understanding PowerShell  
  
A shell allows casual users and system administrators alike to use the services offered by the underlying operating system.
  
PowerShell has various advantages over Command prompt and batch files. Let's cite some of them:  

In batch scripts, you can only use the commands that you can run on Command Prompt. This makes batch scripts really limited when it comes to automating advanced tasks. But thanks to PowerShell, you have a more advanced script language which is actually a fully-featured programming language that allows you to write complex logic with support for variables, functions, classes, error handling, and more.  

PowerShell is way more powerful that CMD because it's built on top of the .NET Framework. PowerShell commands which are referred to as cmdlets are .NET classes that get called at runtime. 

     
## What You'll Learn  

Acquiring the necessary PowerShell knowledge can be very worthwhile for employment. In this course, you'll learn:  
  
- PowerShell Core to manage multiple systems such as Windows, Linux and macOS,  
- PowerShell fundamentals to write scripts that will save you time when managing your systems,  
- Automate repetitive and tedious tasks using scripts.
- How to build the kinds of scripts that you can use in your day-to-day work.

Manage multiple systems with PowerShell such as Windows, Linux, and macOS
Write PowerShell scripts that will save you time when managing your systems by automating repetitive and tedious tasks
Use PowerShell as a command-line shell for navigating the file system, creating, deleting, copying and renaming files and folders, then working with processes, and the pipeline, etc.
Use PowerShell as a scripting environment for writing and running scripts that accept parameters and return results
Work with scalar variables and arrays for storing data in scripts
Implement some common administration tasks such as backing up folders, sending an email if the CPU usage reaches a threshold, downloading URLs and uncompressing archives, and finally deleting older files 
Use functions and classes in PowerShell
  
## PowerShell as a command line shell  

In this section we will see how to use PowerShell as a command line shell.
  
### Navigating the filesystem  
    
Let's start our journey with PowerShell by learning how to navigate the filesystem using commands like `Get-ChildItem`, and `Get-Location` and their aliases.  
   
#### Using `ls`  

The `ls` command is an alias for the [`Get-ChildItem`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-childitem?view=powershell-7.1) cmdlet which is used to list the contents of a folder. 

You can use `Get-ChildItem`  and its aliases to perform many tasks related to file system such as:

-   Listing and searching recursively for files (including hidden files),
-   Accessing files and directory objects,
-   Passing files to other cmdlets, functions, or scripts.

> Note: Another historic alias for  `Get-ChildItem`  is `dir`.
  
Head back to your PowerShell console and run the following command:  
  
```powershell
PS> ls  
```  
By default, when running the PowerShell console, it uses your home location as the current location so the previous command without any parameters will list the files of your home folder. 

You can also provide a folder's path to the command to list the items in the specified location:

```powershell
PS> ls C:\Windows
```   

This command accepts many parameters, such as `Recurse` to get items in all child containers and `Depth` to limit the number of levels to recurse, that you can find from the official docs.

#### Using `pwd`   

The `pwd` command stands for print working directory and is a commonly used command in Linux Bash.

PowerShell provides the same command but it's actually an alias for the [`Get-Location`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/get-location?view=powershell-7.1) cmdlet. 
  
Head back to the PowerShell console and run the following command:  

```powershell
PS> pwd  
```
This will output the current directory you are working on like the screenshot below
  
#### Using `cd` 
 
In Linux Bash, the `cd` command stands for Change Directory. Powershell provides the same command but it's simply an alias for the [`Set-Location`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/set-location?view=powershell-7.1) cmdlet. It's used to navigate into directories and subdirectories.

Head back to the PowerShell console and run the command as follows:

```powershell
PS> cd -Path C:\Windows
```

You can also use the `Set-Location` cmdlet directly as follows:

```powershell
PS C:\> Set-Location -Path C:\Windows
PS C:\Windows> 
```

Make sure to enclose the path in quotes if it contains spaces.

Unlike the `cd` command in CMD which requires you to be already in the drive of the location you want to navigate to, the `Set-Location` cmdlet and its aliases in PowerShell can change both the drive and directory. 

This also means, you can use these commands to switch between the file system drives.

In addition to `cd`, there are also some other aliases for `Set-Location` such as `chdir`, and `sl`.

These are some ways you can use the `cd` command to navigate to special folders in your system. For example; you can navigate to one folder higher than the current folder using:

```powershell
PS>  cd  ..  
```

Navigate to the home directory using:

```powershell
PS> cd ~
```

You can read more information about the available commands for managing your location from this [article](https://docs.microsoft.com/en-us/powershell/scripting/samples/managing-current-location?view=powershell-7.1).
 
### Basic file operations  

Now that we have seen how to navigate into directories, we'll learn how to do basic operations such as renaming, removing, copying, and creating directories. 
  
The [`New-Item`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/new-item?view=powershell-7.1) cmdlet enables users to create new directories and files while [`Remove-Item`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/remove-item?view=powershell-7.1) could be used to recursively remove a directory and all of its files and also single files. 

You can use the `-Force` parameter to instruct the cmdlet to skip prompting the user for confirmation.

Head back to your PowerShell console and start by running the following command:

```powershell
PS> mkdir C:\dummyfolder
```

The `mkdir` command is a function which simply invokes the `New-Item` cmdlet to create directories specifically. 

If you want to use the original `New-Item` cmdlet to create a directory, you need to run the following full command:

```powershell
PS> New-Item -Path C:\dummyfolder -Type Directory
``` 

You can also use  `New-Item`  to create new files as follows. 

Let's first navigate to our previously created `dummyfolder` using the following command:

```powershell
PS> cd C:\dummyfolder
```
Next, run:

```powershell
PS>  New-Item  "dummy file.txt"  -ItemType  File
```

If you need to add some text to your text file, you can use  `-Value`, otherwise an empty file will be created.

You can rename the folder, you just created using the following command:

```powershell
PS> Rename-Item C:\dummyfolder D:\dummyfolder1
```

This will rename `dummyfolder` to `dummyfolder1`.
The command will fail if another folder named `dummyfolder1` exists.

You can remove a directory by running the following command:

```powershell
PS> Remove-Item –path C:\dummyfolder1  –recurse
```

We use the `–recurse` parameter to instruct PowerShell to remove any child items without asking for permission. 

We can use the `-Filter` parameter to provide a filter for selecting the items that will be removed. For example the following command will remove any item in the C drive with the word `test` in its name:
 
```powershell
PS> Remove-Item –path c:\* -Filter *test* -whatif
```

We also added the `–whatif` parameter, which simply let you test the command and visualize the results before actually affecting your system, so you can be sure you don't accidently delete important files.

> Note: The  `Remove-Item`  cmdlet has an alias called  `del` that you can use instead. 

You can copy items from a source folder to a destination using the [`Copy-Item`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/copy-item?view=powershell-7.1) cmdlet. For example, let's suppose we have a file named `test.txt` in `C:\`, we can copy it to our `dummyfolder1` folder using the following command: 

```powershell
PS> Copy-Item -Path C:\test.txt -Destination C:\dummyfolder1
```

The `Path` parameter takes the path of the source file and the `Destination` parameter takes the path of the destination folder.

This cmdlet can also be used to copy all the contents of a folder by providing wildcard characters, such as the asterisk to match one or more characters or the question mark to only match a single character,  to the `Path` parameter.

For example:

```powershell
PS> Copy-Item -Path C:\dummyfolder1\* -Destination C:\dummyfolder2
PS> Copy-Item -Path 'C:\tes?.txt' -Destination C:\dummyfolder1
```

You can also use  `Copy-Item`  to copy multiple folders at the same time while merging their contents. You simply need to provide multiple paths to the  `Path`  parameter,  `Copy-Item`  will then copy either the folder or file(s) depending on the provided path and place the contents in the destination folder. For example:

```powershell
PS> Copy-Item -Path C:\dummyfolder1\*,C:\dummyfolder2\*,C:\dummyfolder3\* -Destination C:\dummyfolder4
```

### Basic processes 

PowerShell provides many cmdlets for working with processes. The  [`Get-Process`](https://docs.microsoft.com/en-us/powershell/module/Microsoft.PowerShell.Management/Get-Process?view=powershell-7.1)  cmdlet can be used for getting the processes on a local or remote computer. Using the command without parameters, will retrieve all of the processes on the local computer but you can also provide a process name or ID (PID) to get a particular process.

The [`Start-Process`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/start-process?view=powershell-7.1#:~:text=By%20default%2C%20Start%2DProcess%20creates,a%20program%20on%20the%20computer) and  [`Stop-Process`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/stop-process?view=powershell-7.1) cmdlets respectively starts and stops one or more processes on the local computer.

Head back to your PowerShell console and run the following to get a the Idle process in your system:

```powershell
PS> Get-Process -id 0
```

Next, run the following command to start Notepad:

```powershell
PS> Start-Process -FilePath  "notepad"
```

We use the `-FilePath` parameter to specify an executable file or script file, or a file that can be opened by using a program on the computer. For a non-executable file, the cmdlet starts the program that is associated with the file.

You can then stop the process using the `Stop-Process`  cmdlet that takes a Name or Id to specify the process you need to stop:

```powershell
PS> Stop-Process -Name "notepad"  
```

PowerShell provides advanced features that you can find on the other popular shells, such as pipes and redirection.

Pipes is a feature that enables you to pass the output from one command to another command while redirection allows the output from a command to be redirected to a file.

You can connect the output of a command to the input of another command using the pipeline via the `|`pipeline character.
 
For example, we can provide the output from the  `Get-ChildItem`  cmdlet to the [`Format-List`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/format-list?view=powershell-7.1)  command using the following syntax:

```powershell
PS> Get-ChildItem ./dummyfile.txt | Format-List 
```

This will output the information of the `dummyfile.txt` file in a list format.

We can also redirect this output to a file of the command using the following command:

```powershell
PS> Get-ChildItem ./dummyfile.txt | Format-List > output.txt
```

If the `output.txt` file already exists, its current contents will be overwritten. If you want the new contents to be appended without overwriting the old contents of the file, you need to use the `>>` symbol instead.

## Powershell as a scripting environment  

The language used in PowerShell is a high-level [object-oriented programming](https://en.wikipedia.org/wiki/Object-oriented_programming) developed by Microsoft to help system administrators automate their tasks. You can use it to write powerful scripts which are basically just text files with the `.ps1` extension.

Let's see the basics of scripting with PowerShell but before that make sure to create a `Script.ps1` file in your working directory where we'll add commands as we progress. You can use Notepad or simply run the following cmdlet from the console:

```powershell
PS> New-Item -ItemType File -Name Script.ps1
```
 
### Parameters and results

Parameters are an important element of PowerShell as they allow you to avoid hard-coded values in your scripts and thus making them useful for multiple cases.
 
You can pass parameters to a PowerShell script in two ways — by position or by name. 

We can pass parameters by position to `Script.ps1` by running the following command in the console:

```powershell
PS> .\Script.ps1 foo bar
```

Both `foo` and `bar` are parameters passed by position — `foo` has a position of zero while `bar` has a position of one.

We can pass the two parameters by name using something like the following:

```powershell
PS> .\Script.ps1 -Foo foo -Bar bar
``` 

`Foo` and `Bar` are parameters' names while `foo` and `bar` are their values. In our example we used the same word for the parameters and values but you can provide any names for your parameters as long as they are meaningful. 

Next, we need to add the logic for retrieving the values of the passed parameters.

For parameters passed by position, we can access them using the `$args` array which contains the arguments that are passed to the script at the time you run it.  

Open the `Script.ps1` file and add the following code:

```powershell
echo "At position 0: " $args[0] 
echo "At position 1: " $args[1] 
```

So the parameters' values are stored in the `$args` array by the position you feed them to the script with index zero being the argument at the most left just after the script name. 

> Note: `echo` is used for displaying the text to the screen and it's actually an alias for the [`Write-Output`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/write-output?view=powershell-7.1) cmdlet for sending the specified objects to the next command in the pipeline.

Now, let's modify our script to tell PowerShell that it expects two named parameters. We need to use a `param` statement which should be the first line in your script:

```powershell
param($Foo, $Bar)

echo "Foo: $Foo"  
echo "Bar: $Bar" 
```

We can also specify the types of the expected values using the following syntax:

```powershell
param([String] $Foo, [String]  $Bar)
```

Named parameters can also have default values:
 
```powershell
param([String] $Foo = "Foo", [String]  $Bar = "Bar")
```

Now, how to return and get result values from a script?

In PowerShell, you can return values using the `return` keyword plus anything you write to stdout.

Let's modify our previous script as follows:

```powershell
param([String] $Foo = "Foo", [String]  $Bar = "Bar")

"A script"
1 - 1
Get-Date
echo "Foo: $Foo"  
echo "Bar: $Bar" 

return 0
```

Next, we need to run the script as follows:

```powershell
powershell .\Script.ps1 
```

We'll get a similar output:

```powershell
A script
0

Saturday, January 23, 2021 10:32:11 PM
Foo: Foo
Bar: Bar
0
```
  
### Variables

You can think of [variables](https://en.wikipedia.org/wiki/Variable_(computer_science)) as containers that store information and can be seen also as labels for locations in the computer's memory.

You can store different types of values in variables such as strings and numbers. In PowerShell, you can declare a variable by simply prepending a name with the dollar (`$`)  sign then assign a value using the equal (`=`) symbol.

For example, let's declare the following variables:
 
```powershell
$firstName = 'Ahmed'
$job = 'Developer'
```
  
### Advanced Variables

You can use arrays in PowerShell to store collections of data and you can access each element using an index that starts at zero.

An element can be a string, integer, object, or  another array, and one array can contain any combination of these elements.
 
The easiest way to create an array in PowerShell is by using the following syntax:

```powershell
$arr1 = @()
```

This is an empty array, let's create another array with five values:

```powershell
$arr2 = @('A','B','C', 1, 2)
```
 
We can access individual elements inside an array using their indexes in brackets. For example to get the first element we use the following syntax:

```powershell
$arr2[0]
```

We can also get multiple elements (slices) by using multiple indexes:

```powershell
$arr2[0,1]
```

We can change the values of elements using indexes as follows:

```powershell
$arr2[0] = 'D'
```
 
 We can add an element to the end of the array using the `=+` operator as follows:

```powershell
$arr2 += 'E'
```

PowerShell provides multiple ways for looping through arrays and performing the same action on each element.

For example, we can use the regular `for` loop as follows:

```powershell
for ($i=0; $i -lt $arr2.length; $i++) {
	$arr2[$i]
}
```

We can use the `foreach` loop:

```powershell
foreach ($elm in $arr2) {
	$elm
}
```

Finally, we can also use `foreach` with the pipline:

```powershell
$arr2 |foreach {
	$_
}
```
 
When you use `foreach` through the pipeline, you have a special variable ( `$_`) that references the current element in each iteration.
  
### Practical Powershell: Some simple tasks  
  
After learning about the basics, let's see how to implement some common tasks.
   
#### Backing up the home directory  

We can back up a folder by first looking for all the files that have changed and copy them to the backup location.

Create a `backupHomeDirectory.ps` file and add the following code:

```powershell
$files = ls $home -recurse | Where-Object { !($_.psiscontainer) -and $_.lastwritetime -gt (get-date).date }  

$files | foreach { 
    Copy-Item -path $_.fullname -destination  \\server\backup\  
}
```

We find the files inside the home folder and its subfolders using the `ls` command with `-recurse` and the `$home` variable which is a builtin variable that points to the home folder. We pipe the results to the [`Where-Object`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/where-object?view=powershell-7.1) cmdlet which selects files that have particular property values. We select only files, not folders using the builtin `psiscontainer` variable, that were modified in the last day using  `lastwritetime`.

Next, we copy the modified files to a network backup folder. We pipe the `$files` array, which contains all the modified files, to a [`Foreach-Object`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/foreach-object?view=powershell-7.1) cmdlet (or its `foreach` alias), and we use the `Copy-Item` cmdlet to copy the files (the `$_` references the current file in the iteration) to the network folder.

This script is best used with the Windows Task Scheduler to schedule it to run periodically at specified times. See [Use Scheduled Tasks to Run PowerShell Commands on Windows](https://devblogs.microsoft.com/scripting/use-scheduled-tasks-to-run-powershell-commands-on-windows/).


#### Downloading an url and uncompressing it if it’s an archive  

Next, let's see how to create a script that you can use to download a `.zip` archive from the internet, and extract the files.

Create a `downloadAndUncompressFile.ps1` file and start by adding the following code:
 
```powershell
$Url = Read-Host -Prompt "Please, enter a file URL"
$Filename = Split-Path -Path $Url -Leaf
$Basename = Split-Path -Path $Url -LeafBase
```

We use the [`Read-Host`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/read-host?view=powershell-7.1) cmdlet to prompt users for input with a  `Please, enter a file URL` message, and we place the user's input inside an `$Url` variable. Next, we use [`Split-Path`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.management/split-path?view=powershell-7.1) cmdlet to get the filename and basename from the URL.

Next, add the next part of the code:

```powershell
Write-Host "Start download.."
Invoke-WebRequest -Uri $Url -OutFile $Filename 
Write-Host "Download finished, stored as $Filename"
```

We invole the `Write-Host` cmdlet to write `Start download..` on the console; next we invoke the [`Invoke-WebRequest`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/invoke-webrequest?view=powershell-7.1) cmdlet to download the file and save it using the name extracted from the URL. Finally, we tell users that the download has finished. 

Next, write the following part:

```powershell
If ($FileName.ToLower().EndsWith(".zip"))
{
   New-Item -ItemType Directory -Path $Basename
   Expand-Archive -Path $Filename -DestinationPath $Basename
}
```

This will use the `If` statement to check if the filename ends with `.zip` extension (i.e if it's an archive). If that's the case, we create a folder with the same name as the filename but without the extension. Finally, we invoke the [`Expand-Archive`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.archive/expand-archive?view=powershell-7.1) cmdlet to extract the files of the archive into the destination folder.

#### Sending an email if the [CPU usage] reaches a threshold 

Next, let's write a script to send an email to a certain address (to yourself) if the CPU usage of the server reaches a threshold using [Send-MailMessage](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/send-mailmessage?view=powershell-7.1)

If you don't have an email server for testing the script, you can use [mailtrap](https://mailtrap.io) which is a fake SMTP server for development.

Now, create a `monitorCPUUsage.ps1` file and start by adding the following variables:

```powershell
$From = "user@domain.com"
$To = "admin@domain.com"
$Subject = "Alert"
$Body = "Your CPU Usage Has Reached the Threshold!!!!"

$SMTPServer = "smtp.mailtrap.io"
$SMTPPort = 2525

[decimal]$CPUThreshold = 80
```

Next,  we need to use `Get-CimInstance` cmdlet with `Win32_Processor` class to get CPU usage as follows:

```powershell
$LoadPercentage = (Get-CimInstance -Class Win32_Processor).LoadPercentage 
```

Finally, we check if the returned value is greater than our specified threshold and send an email if that's the case:
 
```powershell
if( $LoadPercentage -gt $CPUThreshold )
{
    $User = "858fc9e630c58c"
    $PWord = ConvertTo-SecureString -String "a9f1750584db31" -AsPlainText -Force
    $Credential = New-Object -TypeName System.Management.Automation.PSCredential -ArgumentList $User, $PWord
    Send-MailMessage -From $From -to $To -Subject $Subject -Body $Body  -SmtpServer $SMTPServer -port $SMTPPort -Credential $Credential  
}
```

We the `-gt` operator, which stands for greater than, for comparing the CPU usage to the specified threshold, next we call the [`ConvertTo-SecureString`](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/convertto-securestring?view=powershell-7.1) cmdlet to convert our password string into a secure string.

Next, we invoke `New-Object`  cmdlet with `System.Management.Automation.PSCredential` to create a [credential object](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.security/get-credential?view=powershell-7.1) from the username and password. This credential object then can be used by `Send-MailMessage` along with the other parameters to send the email. 

See [About Comparison Operators](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_comparison_operators?view=powershell-7.1)
  
  
### Creating a real-world script  

Let's now create a realistic script that deletes all files older than 30 days in the current directory.
  
First, we need to define a variable named `$currentFolder` that holds the current folder where the script is running:

```powershell
$currentFolder = $PSScriptRoot
$dateAt30DaysFromNow = (Get-Date).AddDays(-30)

$logsFolder = "$home\logs" 
New-Item -ItemType Directory -Path $logsFolder -Force
```

`$PSScriptRoot` is a builtin PowerShell variable that holds the folder's name of the running script.

Next, we need to get the files inside the current folder that are older than 30 days using the `Get-ChildItem` cmdlet or its `ls` alias

```powershell
$olderFiles = ls $currentFolder -Recurse -Force |
Where-Object {!$_.PsIsContainer -and $_.LastWriteTime -lt $dateAt30DaysFromNow } 
```

The `PsIsContainer` returns $True if the item is a folder and `LastWriteTime` contains the date of the latest write operation on the file.

Next, we loop over the array of files and call the `del` command to delete the file then we add its name to the `deletedFiles.txt` file for logging purpose:

```powershell
$olderFiles | foreach {
   $_ | del -Force  
   $_.FullName | Out-File "$logsFolder\deletedFiles.txt"  -Append 
}
```

This is a script for creating files with random creation date and content:

```powershell
$Folder= ".\demo"
New-Item -ItemType Directory -Path $Folder

for($count=0; $count -lt 100; $count++){ 
    $filename = "$($Folder)\$((Get-Random).tostring()).txt"
    $randomDate = (Get-Date).AddDays(-1 * (Get-Random -Minimum 0 -Maximum 60))
    $dateAt30DaysFromNow = (Get-Date).AddDays(-30)
    $fileContent = "Dummy file content"    
    if ($randomDate -lt $dateAt30DaysFromNow){
          $fileContent = "This file is older than 30 days"
    }
    else {
          $fileContent = "This file is newer than 30 days"          
    }
    $file = New-Item -ItemType File -Path $filename -Value $fileContent
    $file.CreationTime = $randomDate
    $file.LastWriteTime = $randomDate
}
```
  
## Deeper Powershell  
 
After getting the basics of scripting with PowerShell, let's now learn about some advanced concepts such as functions and classes and then using these features to improve our previous script.  
  
### Functions 

If you need to use the same code in more than one place in your script or even across multiple scripts, it's best to use a function which can be defined as a set of statements that together have a name and can be invoked using that name. It may also accept one or more parameters and return values using the `return` keyword plus any outputs produced by statements in the function. 

You can declare a function using the `function` keyword then the block of the statements, inside the curly braces (`{}`), that will be executed when the function is invoked. 

For example, let's declare a function called `add`

```powershell
function Add-Two-Numbers {
    $Result = $args[0] + $args[1]
	return $Result
}  
```

We used the `$args` to retrieve the arguments that will be passed to the function.
 
You can then call the function as follows:

```powershell
Add-Two-Numbers 1 2
```

When calling functions, the passed parameters are space-separated without parentheses.

We can also modify the function to expect named parameters as follows:

```powershell
function Add-Two-Numbers {
    param( [Int]$A , [Int]$B)
    $Result = $A + $B
	return $Result
}  
```
 
Next, we can call the function as follows:

```powershell
Add-Two-Numbers -A 1 -B 2
``` 
Please refer to the **Parameters and results** section for more details parameters and returned results, the same instructions are valid for scripts and functions.
  
### Classes

A class is a template for creating objects that contains member variables for defining state and methods for defining behavior. 

For example, we can define a `Car` class with properties such as `Color` and `Brand` and methods such as `Drive`:
 
```powershell
class Car
{
    
    [String]$Color 
    [String]$Brand

    Car([String]$Color, [String]$Brand)
    {
        $this.Color = $Color
        $this.Brand = $Brand
        
    }
    [String] Drive() {
	   return "Driving.."
    }

}
```

Inside class methods, the `$this` variable is  used to reference the current instance.

The method with the same name as the class is called a constructor which is a special method used to initialize objects that we create from the class.

We can create objects from our `Car` class using `New-Object` as follows:

```powershell
$car = New-Object Car("Red","Fiat")
```

Read more details, read [About Classes](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_classes?view=powershell-7.1)

### Creating a more advanced script  
  
Using the new features covered in the previous section, let's enhance the script we previously built as follows:

```powershell
class DeleteOlderFiles
{
    
    [String]$TargetFolder 
    [String]$LogsFolder
    [DateTime]$DateAtXDaysFromNow
    $OlderFiles
    
    DeleteOlderFiles([String]$LogsFolder, [Int]$Days)
    {
        $this.OlderFiles = @()
        $this.GetDateAtXDaysFromNow($Days)
        $this.LogsFolder = $LogsFolder
        $this.CreateLogsFolder() 
    }
    [Void] GetDateAtXDaysFromNow($Days){
        $this.DateAtXDaysFromNow = (Get-Date).AddDays($Days)
    }
    [Void] CreateLogsFolder(){
        New-Item -ItemType Directory -Path $this.LogsFolder -Force       
    }
    [Void] SetTargetFolder([String]$Folder) {
        $this.TargetFolder = $Folder    
	return 
    }
    [Void] FindOlderFiles() {
        $this.OlderFiles = ls $this.CurrentFolder -Recurse -Force | 
        Where-Object {!$_.PsIsContainer -and $_.LastWriteTime -lt $this.DateAtXDaysFromNow }       
	return 
    }
    DeleteFiles(){
        $this.OlderFiles | foreach {
             $_.FullName | Out-File "$($this.LogsFolder)\deletedFiles.txt"  -Append 
        }
    }
}

$d = New-Object DeleteOlderFiles("$home\logs",-30)
$d.SetTargetFolder($PSScriptRoot)
$d.FindOlderFiles()
$d.DeleteFiles()
```  
We created a `DeleteOlderFiles` class with four member variables:   `$TargetFolder` , `$LogsFolder`, `$DateAtXDaysFromNow`, `$OlderFiles`. Next, we added a constructor for initializing those variables and defined a few methods for creating the logs folder, setting the target folder, finding the older files and deleting them.
 
## Using Habitat Hooks with Powershell  

Chef Habitat allows you to automate building, deploying and managing your applications and services.  A [plan](https://docs.chef.io/habitat/plan_writing/)  is the file where you define how you will build, deploy, and manage your application. In Windows, a plan is defined inside a `plan.ps1`  file, which is as you can see, just a PowerShell script file. The plan file exists in the  `habitat`  directory, which you install at the root of your project with the `hab plan init` command.

This is an example of plan file for downloading, installing and running MySQL server v5.7:

```powershell
$pkg_name="mysql"
$pkg_origin="core"
$pkg_version="5.7.17"
$pkg_license=('GPL-2.0')
$pkg_upstream_url="https://www.mysql.com/"
$pkg_description="mysql, a database"
$pkg_maintainer="The Habitat Maintainers <humans@habitat.sh>"
$pkg_source="https://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-$pkg_version-winx64.zip"
$pkg_shasum="53b2e9eec6d7c986444926dd59ae264d156cea21a1566d37547f3e444c0b80c8"
$pkg_bin_dirs=@("bin")
$pkg_lib_dirs=@("lib")
$pkg_include_dirs=@("include")
$pkg_exports=@{
   "port"="port"
   "password"="app_password"
   "username"="app_username"
}

function Invoke-Install {
    Copy-Item "mysql-$pkg_version-winx64/bin/*" "$pkg_prefix/bin" -Recurse -Force
    Copy-Item "mysql-$pkg_version-winx64/lib/*" "$pkg_prefix/lib" -Recurse -Force
    Copy-Item "mysql-$pkg_version-winx64/include/*" "$pkg_prefix/include" -Recurse -Force
    Copy-Item "mysql-$pkg_version-winx64/share" "$pkg_prefix" -Recurse -Force
}
```

The file makes use of PowerShell variables, functions and cmdlets. These variables have special meanings in Habitat and by just setting some of them you can trigger many default actions. For example by setting `$pkg_source`, habitat will download the specified file, try to unpack it and install it.     

You can override the default behavior of Chef Habitat in each build phase using [callbacks](https://docs.chef.io/habitat/build_phase_callbacks/). We simply  create a PowerShell function of the same name (prefixed with `Invoke-` and using a PascalCase convention) in your plan file and provide your own custom logic or no implementation at all if not needed. 

     
**Note:** If you are not familiar with Habitat, take a look at Chef’s series on why they created it:

-   [Why Habitat? – Plans and Packages, Part 1](https://blog.chef.io/2016/11/14/why-habitat-plans-and-packages-part-1/)
-   [Why Habitat? – Plans and Packages, Part 2](https://blog.chef.io/2016/11/28/why-habitat-plans-and-packages-part-2/)
-   [Why Habitat? – The Supervisor and Run Lifecycle](https://blog.chef.io/2016/12/14/why-habitat-the-supervisor-and-run-lifecyle/)

Habitat provides hooks for the install/init/run/uninstall lifecycle events that occur during the life of the application. They are simply PowerShell scripts that reside in a `hooks/` folder and have predefined file names.
  
### The install hook

The `install` hook is invoked when you run `hab pkg install` to install the package. In our example, we could use this hook to check if our system possess enough memory,  by [calling CIM to see how much RAM is installed](https://www.nextofwindows.com/getting-ram-info-on-local-or-remote-computer-in-powershell)  on the machine and if it's less than 1GB, refuse to proceed with installation. You can make a hook fail by returning a non-zero exit code:

```powershell
$ram_gb = Get-CimInstance Win32_PhysicalMemory | Measure-Object -Property capacity -Sum | Foreach {"{0:N2}" -f ([math]::round(($_.Sum / 1GB),2))}

if ($ram_gb < 1.0) {
    exit 1
}
```

Please note that an exit code other than zero will tell Habitat the hook failed and it will refuse to proceed with installing the application.
 
### The init hook

The init hook can be used to initialize the application. In our MySQL example, we initialize MySQL server as follows:
 
```powershell
mkdir "{{pkg.svc_var_path}}/logs" -ErrorAction SilentlyContinue

if (!(Test-Path "{{pkg.svc_data_path}}/mysql")) {
  mysqld --defaults-file={{pkg.svc_path}}/config/my.cnf `
         --explicit-defaults-for-timestamp `
         --datadir={{pkg.svc_data_path}} `
         --init-file={{pkg.svc_config_path}}/init.sql `
         --initialize
}
```

We use the `Test-Path` cmdlet to check if the `mysql` installation directory exists under `pkg.svc_data_path` which points to the location of the data files for the Habitat service, and then we call `mysqld` to initialize the data directory, including the `mysql` database containing the initial MySQL grant tables. We also created the `logs/` folder.
  
For more details check out [Initializing the Data Directory](https://dev.mysql.com/doc/refman/5.7/en/data-directory-initialization.html)
  
### The run event 

The run hook can be used to start the service. In our example, this starts the MySQL server as follows:

```powershell
mysqld --defaults-file={{pkg.svc_config_path}}/my.cnf `
            --datadir={{pkg.svc_data_path}} 2>&1
```
Check out [Starting the Server for the First Time on Windows](https://dev.mysql.com/doc/mysql-startstop-excerpt/5.7/en/windows-server-first-start.html)
  
### The uninstall event  

The `uninstall` hook is invoked at uninstall time (when executing the `hab pkg uninstall` command) to uninstall anything that was installed during the `install` hook. 

In the MySQL example, we could use the uninstall hook to delete any temporary files MySQL has created.  So in the uninstall hook file, we could add PowerShell code to clean up any remaining MySQL temporary files (The `*.MYD` files):

```powershell
$TempDir = "{{pkg.svc_var_path}}/logs"
if (Test-Path -Path $TempDir -PathType Container) {
    Get-ChildItem -Path $TempDir -Recurse | Remove-Item -Force -ErrorAction SilentlyContinue
}
```
  
The temporary folder was created in the init hook and set as a temprary folder for MySQL in the [`my.cnf`](https://github.com/mwrock/habitat-aspnet-sample/blob/master/hab-mysql/config/my.cnf) file.   
    
## Wrap Up  
  
 As a summary we've covered how to use PowerShell Core as a command-shell and a scripting language for automating administration tasks. We've seen how to navigate the filesystem using `ls`, `cd` and `pwd` commands, how to create, rename, copy and remove files and directories, and how to start and stop processes.

Next, we've learned about simple variables, arrays, functions and classes in PowerShell and used that knowledge to create scripts that can be used to automate real-life administration tasks.

Finally, we learned that Chef Habitat provides hooks, which are simply scripts that contain PowerShell commands, for the install/init/run/uninstall lifecycle events that occur during the life of an application.


