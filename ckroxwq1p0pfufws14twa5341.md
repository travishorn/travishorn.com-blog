---
title: "Combine Multiple PowerShell Commands into One"
datePublished: Thu Aug 22 2019 23:01:09 GMT+0000 (Coordinated Universal Time)
cuid: ckroxwq1p0pfufws14twa5341
slug: combine-multiple-powershell-commands-into-one-f096e7bcaf7
cover: https://cdn.hashnode.com/res/hashnode/image/upload/v1627409473467/R2Nce3YwXP.png
tags: windows, powershell

---


There are some sequences of commands I use over and over again. I decided to make commands that will execute these sequences together.

![](https://cdn.hashnode.com/res/hashnode/image/upload/v1627409471436/Y0i8CWn3N.png)

At first, I thought an [alias](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.utility/set-alias?view=powershell-6) would be perfect for this job. As it turns out, PowerShell aliases can only execute one command. [Functions](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_functions?view=powershell-6), however, can execute multiple commands.

For example, say I just fixed a bug in a Node.js package I am writing. Now I want to bump the version number of my package, publish it through npm, and push the changes to a remote repository.

Normally, I’d execute three commands.

```
npm version patch
npm publish
git push --follow-tags
```


This gets tedious when you’re constantly fixing bugs. If I stuff the commands inside a function…

```
function Publish-Patch {
>>   npm version patch
>>   npm publish
>>   git push --follow-tags
>> }
```


I can reuse the function any time I fix a bug!

```
Publish-Patch
```


This works great. Except it doesn’t persist across sessions.

In order to write persistent functions, you’ll have to store them in a [PowerShell profile](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles?view=powershell-6). A profile is a script that gets run at the beginning of every session.

PowerShell supports multiple profiles depending on your environment. You can set a profile to run for only a specific user or only inside a specific host program. See the [documentation](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_profiles) for more information.

For my purpose, I want to create profile that will run for the current user (me) in any host (command line, VS Code, etc).

Edit the profile in Notepad.

```
notepad $PROFILE.CurrentUserAllHosts
```


Pro tip: replace `notepad` with `code` if you have [VS Code](https://code.visualstudio.com/) installed for a better editing experience.

If no profile exists, Notepad will ask if you want to create it. Accept the prompt.

In Notepad, write the function.

```
function Publish-Patch {
  npm version patch
  npm publish
  git push --follow-tags
}
```


In fact, I want to create functions for [each type of update](https://semver.org/) — not just patches. That’s easy, too. Just write the other functions below the first.

```
function Publish-Patch {
  npm version patch
  npm publish
  git push --follow-tags
}

function Publish-Minor {
  npm version minor
  npm publish
  git push --follow-tags
}

function Publish-Major {
  npm version major
  npm publish
  git push --follow-tags
}
```


There’s one last step before we’re done. By default, PowerShell blocks scripts from running. Which in turn will block your profile from running. You can easily change this setting, though.

```
Set-ExecutionPolicy -ExecutionPolicy RemoteSigned -Scope CurrentUser
```


I chose the `RemoteSigned` execution policy. It allows scripts to be run, but those from remote sources must be signed by a trusted publisher. While this works for me, there are [other execution policies](https://docs.microsoft.com/en-us/powershell/module/microsoft.powershell.core/about/about_execution_policies?view=powershell-6) to choose from.

That’s it! Restart your PowerShell session. Now those functions will always be available to you.