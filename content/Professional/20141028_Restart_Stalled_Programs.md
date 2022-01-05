Title: Restart Stalled Programs
Date: 2014-10-28
Tags: HPC, Migration

Page **edited** by [Camille Archer](https://wiki.hpcc.msu.edu/display/~archerc5@msu.edu) \- "Migration of unmigrated content due to installation of a new plugin"

[ ![User icon: colbrydi@msu.edu](/images/icons/profilepics/default.svg) ](/display/~colbrydi@msu.edu) [Hack to automatically restart programs that stall during
inicialization](/display/~colbrydi@msu.edu/2015/01/16/Hack+to+automatically+restart+programs+that+stall+during+inicialization)

Unknown User (colbrydi@msu.edu) posted on Jan 16, 2015

Sometimes we get jobs that stall out right at the beginning but do not error
out until the walltime for the job has been exceeded. Users get an email
saying their job "exceeds walltime" but when they check the output nothing (or
very little) seems to have happened. The cause of this problem is highly
dependent on what the job is doing. However, in some cases a simple resubmit
of the job gets it working. The following scripts check to see if the program
is running and automatically re-submits the job if their seems to be a
problem.

**file_flag_example.qsub**



    #!/bin/bash -l
    #PBS -l mem=100gb,nodes=100:ppn=1,walltime=03:59:59
    #PBS -m a
    #PBS -j oe

    # This method checks to see if a file is created at the beginning of a run. If the file is not created then the job is killed and restarted.
    # This method assumes that the program stalls before it has a chance to generate a file. This method is useful if the code does self checkpointing and the failure state is a busy wait.
    cd ${PBS_O_WORKDIR}
     
    #Run the main program
    ( 
    	mpirun mycommand 
   ) &
    PID=$!

    # Sleep for enough time to see if the job is running
    sleep 300
    if [ ! -f testfile.flag ]
    then
             echo "Job Seems to have stalled. Killing and restarting"
             kill $PID
             qsub $0
             echo "Job stats for debugging"
             qstat -f ${PBS_JOBID}
             exit 1
    fi

    wait $PID
    RET=$?
    qstat -f ${PBS_JOBID}


    #return the output of the main program
    exit $RET

**output_monitor_example.qsub**



    #!/bin/bash -l
    #PBS -l mem=100gb,nodes=100:ppn=1,walltime=03:59:59
    #PBS -m a
    #PBS -j oe
    # This method monitors job output and stops if the output doesn't change. 
    # This method assumes that the program continuously generates output at regular intervals. 
    cd ${PBS_O_WORKDIR}

    testfile=`testfile.flag`
     
    #Run the main program
    ( 
    	mpirun mycommand > $testfile
   ) &
    PID=$!

    # Sleep for enough time to start generating output
    sleep 300
    linecount1=`cat $testfile | wc -l`
     
    # Sleep enough for more output
    sleep 100
    linecount2=`cat $testfile | wc -l`
    if [ "$linecount1" == "$linecount2" ]
    then
             echo "Job Seems to have stalled. Killing and restarting"
             kill $PID
             qsub $0
             echo "Job stats for debugging"
             qstat -f ${PBS_JOBID}
             exit 1
    fi

    wait $PID
    RET=$?
    qstat -f ${PBS_JOBID}


    #return the output of the main program
    exit $RET

**qstat_monitor_example.qsub**



    #!/bin/bash -l
    #PBS -l mem=100gb,nodes=100:ppn=1,walltime=03:59:59
    #PBS -m a
    #PBS -j oe
    # This method uses the same idea as the previous but instead of relying on output it uses the cput stat generated by qstat.
    # This solution will not work if the job is in a busy wait state.
    cd ${PBS_O_WORKDIR}

    #Run the main program
    ( 
    	mpirun mycommand 
   ) &
    PID=$!

    # Sleep for enough time to start generating output
    sleep 300
    cpu1=`qstat -f $PBS_JOBID | grep resources_used.cput`
     
    # Sleep enough for more output
    sleep 100
    cpu2=`qstat -f $PBS_JOBID | grep resources_used.cput`
    if [ "$cpu1" == "$cpu2" ]
    then
             echo "Job Seems to have stalled. Killing and restarting"
             kill $PID
             qsub $0
             echo "Job stats for debugging"
             qstat -f ${PBS_JOBID}
             exit 1
    fi

    wait $PID
    RET=$?
    qstat -f ${PBS_JOBID}


    #return the output of the main program
    exit $RET

These solutions are nice work arounds because, if it works, the scripts just
restarts your job until it runs and gets the research done. However, using
this hack does not get at the root of the problem. Actually there are two
problems:

  1. Something is broken causing the job to hang. This could be a race condition in the code, a bad node, bad file I/O, bad network connections, etc. All depends on what the code is doing.
  2. Code hangs insteads of quitting and reporting an error. Well engineered code should not hang. For example, file and network access should have timeouts so that code is not running forever.

Researchers, should first notify the HPCC if they are using this hack so we
can try to track down problems with the nodes. Researchers should also work to
modify their code to report an error if something hanges. This will also help
track down the problem.

  * Dirk

  * [dirk](/label/~colbrydi@msu.edu/dirk)

[ ![User icon: colbrydi@msu.edu](/images/icons/profilepics/default.svg) ](
/display/~colbrydi@msu.edu

) [2014-12-16 HPCC workshop slides and
handouts](/display/~colbrydi@msu.edu/2014/12/15/2014-12-16+HPCC+workshop+slides+and+handouts)

Unknown User (colbrydi@msu.edu) posted on Dec 15, 2014

I will be teaching my bi-annual Introductory and Advanced HPCC workshops
tomorrow. Below are links to my updated slides and handouts. Registration
looks lite so feel free to drop in if you have the time. These workshops are
being provided as part of IT Services two day offering of no-charge seminars
to faculty and graduate students on technology topics on December 16 and 17.
More information and registration can be done at the following website:

<http://tech.msu.edu/events/tech-seminars/>

 **Introduction to the HPCC: A Hands-On Introduction to High Performance
Computing at MSU**

  * Tuesday, December 16th 8:30am-11:30am in 403 of the Computer Center
  * During this workshop, participants will learn through hands-on examples how to get started with the MSU High Performance Computing Center (HPCC). Topics include: connecting to the HPCC; copying files to your home directories; navigating the command line interface; accessing available software; testing and running programs interactively; editing files; writing job scripts; submitting jobs to the queue; and monitoring jobs running on the system.
  * This is an interactive workshop held in a computer lab, although participants are encouraged to bring their research laptops if applicable. Multiple instructors will be available to help you get started and diagnose problems.

 **12:00-1:15pm Lunch is provided in the MSU Ballroom (registered participants
only)**

 **Making Your Research Go Faster: Advance Topics in Getting the Most Out of
the MSU HPCC**

  * Tuesday, December 16th 1:30pm-4:30pm in 403 of the Computer Center
  * During this workshop, participants already familiar with using the HPCC systems will be shown advanced techniques on how to use the system more effectively. Topics include: techniques for effective scheduling of pleasantly parallel jobs; shared memory jobs and shared network jobs; running jobs longer than a week; and making jobs fault tolerant.
  * This is an interactive workshop held in a computer lab, although participants are encouraged to bring their research laptops if applicable. Multiple instructors will be available to help get you started and diagnose problems.

SLIDES:

  * [2014-12-16 Introduction to HPCC.pdf](https://wiki.hpcc.msu.edu/download/attachments/5411653/2014-12-16%20Introduction%20to%20HPCC.pdf?version=1&modificationDate=1418669575000&api=v2)

  * [2014-12-16_Advanced HPCC.pdf](https://wiki.hpcc.msu.edu/download/attachments/5411653/2014-12-16_Advanced%20HPCC.pdf?version=1&modificationDate=1418669589000&api=v2)

HANDOUTS:

  * [2014-12-16 [HPCC_WORKSHOP_COMMANDS.pdf](./images/HPCC_WORKSHOP_COMMANDS.pdf)](https://wiki.hpcc.msu.edu/download/attachments/5411653/2014-12-16%20[HPCC_WORKSHOP_COMMANDS.pdf](./images/HPCC_WORKSHOP_COMMANDS.pdf)?version=1&modificationDate=1418670002000&api=v2)
  * [HPCC_Command_Summary.pdf](https://wiki.hpcc.msu.edu/download/attachments/5411653/HPCC_Command_Summary.pdf?version=1&modificationDate=1418669599000&api=v2)
  * <http://files.fosswire.com/2007/08/fwunixref.pdf>

  * [slides](/label/~colbrydi@msu.edu/slides)
  * [dirk](/label/~colbrydi@msu.edu/dirk)

[ ![User icon: colbrydi@msu.edu](/images/icons/profilepics/default.svg) ](
/display/~colbrydi@msu.edu

) [2014-12-05 Western Michigan University, Introduction to iCER
slides](/display/~colbrydi@msu.edu/2014/12/04/2014-12-05+Western+Michigan+University%2C+Introduction+to+iCER+slides)

Unknown User (colbrydi@msu.edu) posted on Dec 04, 2014

Here are a copy of my slides and the handout for our two hour introductory
talk at Western Michigan University:

  * [2014-12-05 WMU iCER Talk.pdf](https://wiki.hpcc.msu.edu/download/attachments/5411650/2014-12-05%20WMU%20iCER%20Talk.pdf?version=1&modificationDate=1417709173000&api=v2)
  * [2014-12-05 WMU iCER Talk Commands.pdf](https://wiki.hpcc.msu.edu/download/attachments/5411650/2014-12-05%20WMU%20iCER%20Talk%20Commands.pdf?version=1&modificationDate=1417709861000&api=v2)

  * [slides](/label/~colbrydi@msu.edu/slides)
  * [dirk](/label/~colbrydi@msu.edu/dirk)

[ ![User icon: colbrydi@msu.edu](/images/icons/profilepics/default.svg) ](
/display/~colbrydi@msu.edu

) [zsh job number
autocomplete](/display/~colbrydi@msu.edu/2014/11/16/zsh+job+number+autocomplete)

Unknown User (colbrydi@msu.edu) posted on Nov 16, 2014

We do not directly support zsh users on our system. However, many of our more
advanced users enjoy some of the modern and advanced features provided by zsh.
One of these users shared a code snippet that he uses in his ~/.zshrc file to
autocomplete job id numbers. He is letting us share the code for use to our
users who prefer zsh over bash.

 **.zshrc**



    _jobs_list() {
    qstat | grep $USER | cut -d' ' -f1 | cut -d'.' -f1
    }

    _jshow_complete() {
    if (( CURRENT ==2)); then
    jjobs=( $(_jobs_list))
    _multi_parts / jjobs
    else
    _files
    fi
    }

    compdef _jshow_complete showstart jdel checkjob qstat

This code should work on any system that used PBS Torque.

I hope you find it useful,

  * Dirk

  * [example](/label/~colbrydi@msu.edu/example)
  * [dirk](/label/~colbrydi@msu.edu/dirk)

[ ![User icon: colbrydi@msu.edu](/images/icons/profilepics/default.svg) ](
/display/~colbrydi@msu.edu

) [2014-10-23 Advanced High Performance
Computing](/display/~colbrydi@msu.edu/2014/10/23/2014-10-23+Advanced+High+Performance+Computing)

Unknown User (colbrydi@msu.edu) posted on Oct 23, 2014

Here are the slides for the Advanced HPC class:

[2014-10-24_CI-Days Advanced
HPCC.pdf](https://wiki.hpcc.msu.edu/download/attachments/5411677/2014-10-24_CI-
Days%20Advanced%20HPCC.pdf?version=1&modificationDate=1414073145000&api=v2)

And here is the handout:

[2014-10-24_CI-Days Advanced HPCC
Handout.pdf](https://wiki.hpcc.msu.edu/download/attachments/5411677/2014-10-24_CI-
Days%20Advanced%20HPCC%20Handout.pdf?version=1&modificationDate=1414077081000&api=v2)

  * Dirk

  * [slides](/label/~colbrydi@msu.edu/slides)
  * [dirk](/label/~colbrydi@msu.edu/dirk)

[View Online](https://wiki.hpcc.msu.edu/display/~colbrydi@msu.edu/Blog) *
[View Changes
Online](https://wiki.hpcc.msu.edu/pages/diffpagesbyversion.action?pageId=5411630&revisedVersion=14&originalVersion=13)

Blogpost migrated from [ICER Wiki](https://wiki.hpcc.msu.edu/display/~colbrydi@msu.edu/Blog) using custom python script. Comment on errors below.