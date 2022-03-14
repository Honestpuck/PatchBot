# PatchBot - Zero Touch Packaging and Patch Management

PatchBot is a software system for providing up to date applications across a Mac fleet. It leverages AutoPkg, the JAMF patch management system, and Jamf API to build a total solution where applications are provided to the fleet without human intervention.

It is described in a number of blog posts:

  - [PatchBot - Zero-Touch Packaging and Patch Management](https://macintoshguy.wordpress.com/patchbot/)
  - [PatchBot - Zero Touch Patch Management #2](https://macintoshguy.wordpress.com/patchbot-2/)
  - [PatchBot #3](https://macintoshguy.wordpress.com/patchbot-3/)
  - [PatchBot #4](https://macintoshguy.wordpress.com/patchbot-4)
  
You no longer need to alter the `.pkg` recipe override. Details on running JPCImporter as an AutoPkg post processor are at https://macintoshguy.wordpress.com/2020/07/31/patchbot-update/
  
 You can find the components in three GitHub repositories
 
  - [PatchBotProcessors](https://github.com/Honestpuck/PatchBotProcessors) Three custom processors for AutoPkg
  - [PatchBotTools](https://github.com/Honestpuck/PatchBotTools) The other components
  - [PatchBotExamples](https://github.com/Honestpuck/PatchBotExamples) Three example recipes
  
 In this repo you can see, above, the presentation and notes from my JNUC2020 presentation about PatchBot. You can see the presentation at https://www.youtube.com/watch?v=m4casr7nXIw
 
If you would like help implementing this in your own environment feel free to reach out. The best place to do that is in the MacAdmins Slack channel #patchbot

v3. has now been released to production.

Changes can be summarised:

 - Replaced the need for Move.py. All the checking to see if there is a test patch to move into production is now done in the Production processor.
 - There is a new constant in the Production code, `DEFAULT_DELTA` to set the default number of days between test and production.
 - There is a new constant in the Production code, `DEFAULT_DEADLINE` The Production processor sets the Self Service deadline to this value every time it
 updates a "Stable" patch policy.
 - There is a new optional variable in Production `.prod` recipes called `delta` to set the number of days between test and production for that package.
- There is a new optional variable in Production `.prod` recipes called `deadline` to set the Self
 Service deadline for that package.

The code *should* run, it has been vigorously tested. There are still things to be done. Certainly the Production processor could be cleaned up as it it grabs information to check the delta then throws it all away so the process to move a package from test into production has to find it all again, that's less than optimal and makes unnecessary API calls.

Now that `delta` can be defined in a `.prod` recipe it is now possible to move a package from test into production from the command line. `autopkg run GoogleChrome.prod -k 'delta=-1'` will immediately move Google Chrome from testing into production, for example. You can do the same with `deadline`. 
`autopkg run GoogleChrome.prod -k 'delta=-1' -k 'deadline=2` will move Google Chrome into production
with a short Self Service deadline. (You need to use '-1' instead of 0 as the code will see 0 as unset.)


![visitors](https://visitor-badge.glitch.me/badge?page_id=honestpuck.patchbot.page.id)
