# mPEP

A program for running experiments where stimuli and timings can be decided in advance (i.e. not in the context of behavior). mPEP is written in MATLAB and Java. 

### History
mPEP descends from “pep” (physiology experiment program), which was used in the Movshon lab in the 1990s and was written in C to run in DOS, and from “zPEP”, (z stands for Zurich) which was used in the Carandini lab 1998-2012 and was written in Visual Basic to run in Windows. The m in mPEP stands for MATLAB.

### Getting started
To get mpep running all you need to do is
 - edit Paths.m to point to correct directories.
 - run "mpep" on the matlab command window

The default settings of...
```
    % directory where mpep saves experiment logs
    DIRS.data = fullfile(cd, 'test_data');
    % directory where mpep expects to find xfiles
    DIRS.xfiles = fullfile(cd, 'xfiles');
    % config path for mpep (currently in working directory)
    DIRS.config = 'config.mat';
```
... allow mPEP to run stand alone, but you'll want to change these to use mpep
properly. You can either ignore or delete the pfiles, xfiles, and test_data
directories within mPEP.

### File structure
- mPEP saves the following files in DIRS.Data.
- A text log file called ANIMALNAME.txt in ANIMALNAME
- information about the stimuli in a .p file and a Protocol.mat file in ANIMALNAME/SERIES/EXPERIMENT

### Experiment types

Experiments can be of 5 kinds...

- REGULAR: all stimuli 1..n are randomized 
- ADAPTATION: 1..n-2 are test stimuli, n-1 is top-up adaptor (shown before every test stimulus), n is fill-up adaptor (shown once before repeat 1).
- PRIMING: stimuli 1..n-1 are test stimuli, stimulus n is the priming stimulus (shown before every test stimulus)
- SEQUENCE: stimuli are shown in sequence 1..n
- UP/DOWN: stimuli are shown in sequence 1..n n..1 1..n and so on.

In the case of adaptation and priming:
- the "fill-up" stimulus (ADAPTATION protocol) is labeled as repeat 0, stimulus n+2
- the "top-up" stimulus (ADAPTATION protocol) or "priming" stimulus (PRIMING protocol) are labeled with actual repeat, stimulus `-istimulus` (a minus sign in front of the stimulus that it is preceding).

All variables have to be resolved to numbers before starting an experiment. Then zpep/mpep asks whether the stimulus maker should make all stimuli in advance, the user chooses, and the experiment starts. 

Exception: the character # is interpreted as “the number of the repeat”. In other words, mpep will tell the stimulus maker that this number is 1 in repeat 1, 2 in repeat 2, and so on. And 0 if no experiment is being run or if the experiment is of type ADAPTATION. Obviously in this case it is impossible to make all stimuli in advance of an experiment, so the user will not be asked whether to do this or not. (note: in Protocol.mat the entry for the “‘#” in the Protocol.pars matrix are NaNs).

### UDP ports for communication between mPEP and hosts

mpep 1103 --------> data acq host 1001
mpep 1001 <-------- data acq host 
mpep 1102 --------> stimulus host 1005 (was 1001 up to 12 Dec 2013)
mpep 1002 <-------- stimulus host 

### Communication protocol with data acquisition hosts

If bidirectional communication is enabled, mPEP expects an echo of each instruction back from the data acquisition hosts. It will continue the experiment only when they have all echoed back.

If ReceivedString is the string received on the UDP socket, then it can be parsed with:
`[instr, animal, iseries, iexp, irepeat, istim, dur] = strread( ReceivedString, '%s %s %d %d %d %d %d' );`

(dur is always in seconds*10, so 5 means 0.5 s)

where instr is one of the following:
- ExpStart - Called by mpep when an experiment is about to start
- BlockStart - Called by mpep when a block (repeat) is about to start
- StimStart - Called by mpep when a stimulus is about to start
- StimEnd - Called by mpep when a stimulus has ended
- BlockEnd - Called by mpep when a block (repeat) has ended
- ExpEnd - Called by mpep when an experiment has ended
- ExpInterrupt - Called by mpep when an experiment has been interrupted

(will add SeriesStart and SeriesEnd)

After StimStart, the sync pulse will come in from the monitor, go positive, and at the end of the stimulus it will go back to zero. (not necessarily true -- in some setups we now flicker the sync box)

If a data host times out (default timeout is 60 seconds) a dialog box pops up asking if you want to try again with increased timeout, try again with infinite timeout, or give up.
