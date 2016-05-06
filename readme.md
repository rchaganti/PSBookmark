#PowerShell Location Bookmark for easy and faster navigation
I work at the commandline quite a bit and at times I find moving to certain folders very tedious and requires lot of typing (even with tab-completion). For example, one of the locations (may not be very frequent) that I move to when working at PowerShell console or ISE is my local Github folder where all my repositories. Another example could be the configuration folder in C:\Windows\System32. At times, this is frustrating. So, I wanted something simple but useful for me to just type as few characters as possible but still let me navigate to the place I want to. 

So, this is when I started writing out something quick and added a few functions to my Profile script. And, then I tweeted that! :)

[https://t.co/je278bAWy4](https://t.co/je278bAWy4)

I saw a few suggestions as a response to this. 

##PSDrives
One suggestion was to use the New-PSDrive cmdlet and add one for each location I want to quickly move to. This is not very intuitive to me. I can map something like Conf: to go to C:\Windows\System32\Configuration but I lose on tab-completion. I always have to type the full drive name like *cd conf:*. You can tab-complete within the drive but not the drive itself.

##Jump.Location or ZLocation modules
This module is very good. It learns your usage and then auto-completes the path based on the history! This is good if you are frequently accessing a few folders. However, this is not my case, exactly. I wanted an easier way to navigate to a longer path and of course, at the same time make it easy for me to get into some of the folders that I use frequently. 

So, here it is: PSBookmark!

##How do you get this?
Simple! Either clone my Github repo or get it from PowerShell Gallery!

```PowerShell
Install-Module -Name PSBookmark
```

##How to use this?
```PowerShell
PS C:\> Get-Command -Module PSBookmark

CommandType     Name                                               Version    Source                                           
-----------     ----                                               -------    ------                                           
Function        Get-LocationBookmark                               1.0.0      PSBookmark                                       
Function        Remove-LocationBookmark                            1.0.0      PSBookmark                                       
Function        Save-LocationBookmark                              1.0.0      PSBookmark                                       
Function        Set-LocationBookmarkAsPWD                          1.0.0      PSBookmark                                       



PS C:\> Get-Command -Module PSBookmark -CommandType Alias

CommandType     Name                                               Version    Source                                           
-----------     ----                                               -------    ------                                           
Alias           glb -> Get-LocationBookmark                        1.0.0      PSBookmark                                       
Alias           goto -> Set-LocationBookmarkAsPWD                  1.0.0      PSBookmark                                       
Alias           rlb -> Remove-LocationBookmark                     1.0.0      PSBookmark                                       
Alias           save -> Save-LocationBookmark                      1.0.0      PSBookmark 
```

Since the idea is to make it easy to navigate and avoid lot of typing, I will use aliases instead of the full function names. I had a hard time coming up with the function names but aliases was very easy.

##Create a new location alias
You can use the *save* command to save alias for either the PWD or a specific path.

```PowerShell
#This will save $PWD as scripts
save scripts 

#This will save C:\Documents as docs
save docs C:\Documents
```

##jump or goto a saved location alias
```PowerShell
#You don't have to type the alias name. Instead, you can just tab complete. This function uses dynamic parameters.
goto docs
```

##Get all saved alias locations
```PowerShell
PS C:\> glb

Name                           Value                                                                                           
----                           -----                                                                                           
docs                           C:\Documents                                                                                                                                          
oss                            C:\Documents\Github
```

##Removed a saved alias location
```PowerShell
#You don't have to type the alias name. Instead, you can just tab complete. This function uses dynamic parameters.
rlb docs
```

#TODO
- This was written in just a few minutes and did not spend any time polishing it. So, there is certainly scope for improvement.
- At the moment, the aliases are stored as a hash table in a .PS1 file in $env:UserProfile. This may change in future. 