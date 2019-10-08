# mPEP: Bugs and requests for new functionality

## Bugs / missing features 

ND: I loaded a p-file and the command window filled up with a ton of the same error message something about “control isn’t going to be given back until all variables are within an appropriate range” or something like that. This is kind of weird because i just loaded it up. Is it because of the test stimulus? It would also be better if it told me which variable was in the wrong range.
Happens after changing p-file directory.
ND: At the same time that the above happened, I couldn’t see anymore what were the p-files that I could select from the same p-file directory. That box had just disappeared. It wasn’t that the box was empty. The box was missing from the window altogether. Well, it also happens without the above error. I went to select a p-file from a different directory and now the box is gone. Normally, I would expect to see the p-files that are in that same directory.
ND: Also, the ability to see what I’m typing in the variables window disappears at some point. I can see the variable after I type it and hit return. 
MC: A funny bug that I can't quite replicate consistently: there are some pfiles that once loaded generate an asterisk in the title bar (and hence a request to save upon leaving) even though they were not touched by the user. An example is 3bars1noise.p. 
LD: This will happen by design on pfiles that have old zpep format - that is they don’t contain the experiment flag that we’ve added. But in the case of 3bars1noise.p this should NOT be happening.
MC:After "Test all" the list of pfiles disappears! (and possibly the list of variables as well) 
LD: I’ve not seen this, any help to reproduce? I’ll keep a lookout.
AB: If one restarts vs while mpep is open it is not possible to run a stimulus. One needs first to re-confirm the host list then mpep will work again.

## Bells & Whistles requests 
ND : Would be nice to have a button that hides variables that are not being used by current p-file. That is, I’m not sure they need to be unresolved, just not displayed. After running many p-files, many variables accumulate in the left hand column but not all are used. One then has to wade through and decide which ones are actually used. 
Would be nice to lose the unnecessary folders in mpep
When selecting animal name, in the place where one puts the date, it would be nice to have as a default today's date
perhaps streamline the GUI by renaming the button "Select..." as "Animal" and moving it to replace the label "Animal")
In addition to ctrl-T (which works a bit erratically) it would be great to add the space bar. Hitting space bar when the whole stimulus is selected would play the stimulus.
[Also, a button called "Test" next to "Paste" could do the same]

AP: if someone does not specify a tag after the date, there should be no hanging underscore 
mpep won't let me not add a tag after the date.
ND - Fine. but then the naming must be more flexible. Why can’t we type in the name of the animal that we want - even if we are making a new one. 

## Have these issues been resolved?
MC: Ctrl-T now works, but not at the very beginning, after launching mPEP. If I launch mPEP, then load a pfile, then select a row, then hit Ctrl-T, nothing happens. If go to the Stimulus menu and select Test, I get the stimulus. After having done that, Ctrl-T works fine, for the rest of the mPEP session.
MC: Something similar happens with Ctrl-C and Ctrl-V. In some conditions they are ignored, but if one clicks Copy or Paste first, then the subsequent times they become functional (though not for the entire mPEP session).
MC: I can't get Ctrl-X to work at all.
So all the shortcuts work for me without having to display the menus first. But I definitely have seen this be an issue: It seems to me like the Accelerator property of uimenu items sometimes only works after displaying the menu, but not always - I reproduced this on a minimal figure (without the rest of the mpep code) and the problem still happened. I'll figure out a work around if this is a persistent problem.
MC: Now that we have decided to save "Interstimulus delay" and "Dead time" in the pfile, they should be fields of Experiment->Properties, not their own menu entries.
MC: resizing from the left still has occasional problems. To see them, it helps to choose a wide pfile (one with many columns). Start shrinking and enlarging the window working from the left. Every once in a while, the resizing is incorrect (the spreadsheet appears narrower than it could be).
MC: It is indeed a problem to have a class called Protocol. Please do give call that class some other name. 
I'll move all of the code into it's own package to avoid any namespace problems. (This'll take a little time since for some reason classes in packages do not automatically import their own package!)
ND - Has this been done?
If I try to run on the same computer as stimulus host I get the following "Network Error" window, Java exception. This is understandable (ports are busy), but it should refuse in a more graceful way. 
MC: the version number should be in Contents.m so it appears when one types "ver" in Matlab. (If you put the version number in Contents.m you can read that through something like ver('mPEP');)
MC: Not sure this is easy to replicate, but: I clicked run for a spreadsheet with two unresolved variables, was prompted to resolve the first variable and did so, was prompted to resolve the second one and did so, and then was prompted again to resolve the first one (which at that point was unnecessary).  
AP: got this error on camera_control on zimage when using mpep:
“Warning: Error occurred while executing callback:
Operands to the || and && operators must be convertible to logical scalar values.“
The error doesn’t occur when running zpep.