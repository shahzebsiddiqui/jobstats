JobStats
========


``jobstats`` is a command line utility that reports slurm job statistics that can be useful
for getting a summary of jobs, report of historical jobs, account information, fairshare, etc...
``jobstats`` is a wrapper to few slurm commands like ``sacct``, ``squeue``, ``sreport`` to retrieve slurm
accounting data.

Setup
-------

To get started, please clone this repo::

  git clone https://github.com/shahzebsiddiqui/jobstats.git
  
You will need a python 3.x or higher to run jobstats.

You may want to add jobstats to your $PATH::

  cd jobstats
  export PATH=$PWD:$PATH

Documentation
---------------

You can run ``--help`` option to see available options for jobstats program::

  $ jobstats --help
  usage: jobstats [-h] [-u USER] [-S START] [-E END] [-j] [--state {COMPLETED,FAILED,TIMEOUT,CANCELLED}] [-a] [-v]

  slurm utility for display user job statistics, reporting, and account detail.

  optional arguments:
    -h, --help            show this help message and exit
    -u USER, --user USER  Select a user
    -S START, --start START
                          Start Date Format: YYYY-MM-DD
    -E END, --end END     End Date Format: YYYY-MM-DD
    -j, --jobsummary      Display job summary for user
    --state {COMPLETED,FAILED,TIMEOUT,CANCELLED}
                          Filter by Job State
    -a, --account         Display information on account shares that user belongs to
    -v, --version         Print version

  Developed by Shahzeb Siddiqui <shahzebmsiddiqui@gmail.com>
  
  
jobstats will report jobs completed, failed, cancelled and timeout including the default slurm account and
list of slurm accounts a user belongs to

::

  $ jobstats
  User: shahzeb
  Default Account: admin
  User is part of the following slurm accounts ['admin']
  User Raw Share: 100
  User Raw Usage: 38966329
  Number of Pending Jobs: 0
  Number of Running Jobs: 0
  Total Jobs Completed: 1
  Total Jobs Completed Successfully: 0
  Total Jobs Failed: 0
  Total Jobs Cancelled: 0
  Total Jobs Timeout: 0

  Today: 27/03/2019 14:39:27 sreport
  --------------------------------------------------------------------------------
  Top 10 Users 2019-03-26T00:00:00 - 2019-03-26T23:59:59 (86400 secs)
  Usage reported in CPU Hours
  --------------------------------------------------------------------------------
    Cluster     Login     Proper Name         Account     Used   Energy
    --------- --------- --------------- --------------- -------- --------
    slurm_cl+  shahzeb        Siddiqui          admin       24        0

``jobstats`` will display running and pending jobs if you have any active
jobs while running the command.::

  $ jobstats
  User: shahzeb
  Default Account: admin
  User is part of the following slurm accounts ['admin']
  User Raw Share: 100
  User Raw Usage: 38960679
  Number of Pending Jobs: 0
  Number of Running Jobs: 2
  Total Jobs Completed: 3
  Total Jobs Completed Successfully: 0
  Total Jobs Failed: 0
  Total Jobs Cancelled: 0
  Total Jobs Timeout: 0

  Today: 27/03/2019 14:48:28 sreport
  --------------------------------------------------------------------------------
  Top 10 Users 2019-03-26T00:00:00 - 2019-03-26T23:59:59 (86400 secs)
  Usage reported in CPU Hours
  --------------------------------------------------------------------------------
    Cluster     Login     Proper Name         Account     Used   Energy
  --------- --------- --------------- --------------- -------- --------
  slurm_cl+  shahzeb        Siddiqui          hpceng       24        0

                              Running Jobs
  ________________________________________________________________________________
               JOBID PARTITION     NAME     USER ST       TIME  NODES NODELIST(REASON)
               19705    medium     wrap shahzeb  R       0:04     13 c[001-013]
               19704      long     wrap shahzeb  R       0:11     13 c[001-013]

                    Running + Pending Jobs
  ________________________________________________________________________________
               JOBID PARTITION PRIOR     NAME     USER    STATE       TIME  TIME_LIMIT  NODES CPUS   GRES           START_TIME     NODELIST(REASON)      QOS
               19705    medium 10002     wrap shahzeb  RUNNING       0:04     8:20:00     13  500 (null)  2019-03-27T14:48:24     c[001-013]          normal
               19704      long 10002     wrap shahzeb  RUNNING       0:11     8:20:00     13  500 (null)  2019-03-27T14:48:17     c[001-013]          normal



``jobstats`` can give you a summary of jobs completed, the default  time window
is current day but this can be tweaked. To see a job summary use option ``-j`` or
``--jobsummary``::

  $ jobstats -j
  User: shahzeb
  Default Account: admin
  User is part of the following slurm accounts ['admin']
  User Raw Share: 100
  User Raw Usage: 39028847
  Number of Pending Jobs: 0
  Number of Running Jobs: 0
  Total Jobs Completed: 3
  Total Jobs Completed Successfully: 2
  Total Jobs Failed: 0
  Total Jobs Cancelled: 0
  Total Jobs Timeout: 0

  Today: 27/03/2019 14:51:52 sreport
  --------------------------------------------------------------------------------
  Top 10 Users 2019-03-26T00:00:00 - 2019-03-26T23:59:59 (86400 secs)
  Usage reported in CPU Hours
  --------------------------------------------------------------------------------
    Cluster     Login     Proper Name         Account     Used   Energy
  --------- --------- --------------- --------------- -------- --------
  slurm_cl+  shahzeb        Siddiqui          admin       24        0


                         Today Job Summary
  ________________________________________________________________________________
         JobID  Partition      NCPUS   NNodes              Submit    Elapsed CPUTimeRAW               Start                 End      State
  ------------ ---------- ---------- -------- ------------------- ---------- ---------- ------------------- ------------------- ----------
  19196               viz          1        1 2019-03-25T14:30:51 2-00:21:00     174060 2019-03-25T14:30:52             Unknown    RUNNING
  19704              long        500       13 2019-03-27T14:48:17   00:01:11      35500 2019-03-27T14:48:17 2019-03-27T14:49:28  COMPLETED
  19705            medium        500       13 2019-03-27T14:48:24   00:01:11      35500 2019-03-27T14:48:24 2019-03-27T14:49:35  COMPLETED


By default the time window is current day but this can be controlled with
``--start`` and ``--end`` option that are date fields.

If ``--start`` is specified without ``--end`` option then end time window will
be current time.

::

  $ jobstats -j -S 2019-03-10
  User: shahzeb
  Default Account: admin
  User is part of the following slurm accounts ['admin']
  User Raw Share: 100
  User Raw Usage: 39028847
  Number of Pending Jobs: 0
  Number of Running Jobs: 0
  Total Jobs Completed: 3
  Total Jobs Completed Successfully: 2
  Total Jobs Failed: 0
  Total Jobs Cancelled: 0
  Total Jobs Timeout: 0

  Today: 27/03/2019 14:55:17 sreport
  --------------------------------------------------------------------------------
  Top 10 Users 2019-03-26T00:00:00 - 2019-03-26T23:59:59 (86400 secs)
  Usage reported in CPU Hours
  --------------------------------------------------------------------------------
    Cluster     Login     Proper Name         Account     Used   Energy
  --------- --------- --------------- --------------- -------- --------
  slurm_cl+  shahzeb        Siddiqui          admin       24        0


                         Today Job Summary
  ________________________________________________________________________________
         JobID  Partition      NCPUS   NNodes              Submit    Elapsed CPUTimeRAW               Start                 End      State
  ------------ ---------- ---------- -------- ------------------- ---------- ---------- ------------------- ------------------- ----------
  18554               viz          1        1 2019-03-12T00:37:42   01:27:11       5231 2019-03-12T00:37:42 2019-03-12T02:04:53 CANCELLED+
  18555              long         50        2 2019-03-12T01:02:55   00:00:11        550 2019-03-12T01:02:55 2019-03-12T01:03:06  COMPLETED
  18556              long         50        2 2019-03-12T01:09:10   00:01:11       3550 2019-03-12T01:09:11 2019-03-12T01:10:22  COMPLETED
  18557              long         50        2 2019-03-12T01:09:11   00:01:10       3500 2019-03-12T01:09:14 2019-03-12T01:10:24  COMPLETED
  18558              long         50        2 2019-03-12T01:09:12   00:01:10       3500 2019-03-12T01:09:14 2019-03-12T01:10:24  COMPLETED
  18559              long         50        2 2019-03-12T01:09:12   00:01:10       3500 2019-03-12T01:09:14 2019-03-12T01:10:24  COMPLETED
  18560              long         50        2 2019-03-12T01:09:13   00:01:11       3550 2019-03-12T01:09:14 2019-03-12T01:10:25  COMPLETED
  18561              long         50        2 2019-03-12T01:09:13   00:01:10       3500 2019-03-12T01:09:14 2019-03-12T01:10:24  COMPLETED
  18562              long         50        2 2019-03-12T01:09:13   00:01:10       3500 2019-03-12T01:09:14 2019-03-12T01:10:24  COMPLETED
  18563            medium        500       13 2019-03-12T01:09:17   00:01:10      35000 2019-03-12T01:09:18 2019-03-12T01:10:28  COMPLETED
  18564            medium        500       15 2019-03-12T01:09:18   00:01:10      35000 2019-03-12T01:09:18 2019-03-12T01:10:28  COMPLETED
  18565              long        500       13 2019-03-12T01:09:18   00:01:10      35000 2019-03-12T01:10:29 2019-03-12T01:11:39  COMPLETED
  18566            medium        500       13 2019-03-12T01:09:18   00:01:10      35000 2019-03-12T01:10:29 2019-03-12T01:11:39  COMPLETED
  18567              long        500       13 2019-03-12T01:09:19   00:01:10      35000 2019-03-12T01:11:40 2019-03-12T01:12:50  COMPLETED
  18568            medium        500       13 2019-03-12T01:09:19   00:01:13      36500 2019-03-12T01:11:40 2019-03-12T01:12:53  COMPLETED
  18569              long        500       13 2019-03-12T01:09:20   00:01:11      35500 2019-03-12T01:12:50 2019-03-12T01:14:01  COMPLETED
  18570            medium        500       13 2019-03-12T01:09:20   00:01:11      35500 2019-03-12T01:12:53 2019-03-12T01:14:04  COMPLETED
  18571              long        500       13 2019-03-12T01:09:21   00:01:10      35000 2019-03-12T01:14:01 2019-03-12T01:15:11  COMPLETED
  18572              long        500       13 2019-03-12T02:03:48   00:01:04      32000 2019-03-12T02:03:49 2019-03-12T02:04:53 CANCELLED+
  18573            medium        500       13 2019-03-12T02:03:49   00:01:01      30500 2019-03-12T02:03:52 2019-03-12T02:04:53 CANCELLED+
  18574        express,s+        500        1 2019-03-12T02:03:50   00:00:00          0 2019-03-12T02:04:53 2019-03-12T02:04:53 CANCELLED+
  18575        express,s+        500        1 2019-03-12T02:03:51   00:00:00          0 2019-03-12T02:04:53 2019-03-12T02:04:53 CANCELLED+
  18576        express,s+        500        1 2019-03-12T02:03:51   00:00:00          0 2019-03-12T02:04:53 2019-03-12T02:04:53 CANCELLED+
  18577        express,s+        500        1 2019-03-12T02:03:52   00:00:00          0 2019-03-12T02:04:53 2019-03-12T02:04:53 CANCELLED+
  19196               viz          1        1 2019-03-25T14:30:51 2-00:24:25     174265 2019-03-25T14:30:52             Unknown    RUNNING
  19704              long        500       13 2019-03-27T14:48:17   00:01:11      35500 2019-03-27T14:48:17 2019-03-27T14:49:28  COMPLETED
  19705            medium        500       13 2019-03-27T14:48:24   00:01:11      35500 2019-03-27T14:48:24 2019-03-27T14:49:35  COMPLETED

Shown below is a job summary for time window **2019-01-01** - **2019-01-10**.

::

  $ jobstats -j -S 2019-01-01 -E 2019-01-10
  User: shahzeb
  Default Account: admin
  User is part of the following slurm accounts ['admin']
  User Raw Share: 100
  User Raw Usage: 39023187
  Number of Pending Jobs: 0
  Number of Running Jobs: 0
  Total Jobs Completed: 3
  Total Jobs Completed Successfully: 2
  Total Jobs Failed: 0
  Total Jobs Cancelled: 0
  Total Jobs Timeout: 0

  Today: 27/03/2019 15:01:25 sreport
  --------------------------------------------------------------------------------
  Top 10 Users 2019-03-26T00:00:00 - 2019-03-26T23:59:59 (86400 secs)
  Usage reported in CPU Hours
  --------------------------------------------------------------------------------
    Cluster     Login     Proper Name         Account     Used   Energy
  --------- --------- --------------- --------------- -------- --------
  slurm_cl+  shahzeb        Siddiqui          admin       24        0


                         Today Job Summary
  ________________________________________________________________________________
         JobID  Partition      NCPUS   NNodes              Submit    Elapsed CPUTimeRAW               Start                 End      State
  ------------ ---------- ---------- -------- ------------------- ---------- ---------- ------------------- ------------------- ----------
  3558              short          8        8 2019-01-04T16:30:15   00:00:01          8 2019-01-04T16:30:16 2019-01-04T16:30:17     FAILED
  3560            express         20        1 2019-01-05T17:58:40   00:03:21       4020 2019-01-05T17:58:41 2019-01-05T18:02:02  COMPLETED
  3561             medium          3        3 2019-01-05T18:00:28   00:16:41       3003 2019-01-05T18:00:28 2019-01-05T18:17:09  COMPLETED


``jobstats`` can query historical jobs by the following job state

- **FAILED**
- **COMPLETED**
- **TIMEOUT**
- **CANCELLED**

This would be effective when used by start/end option as shown in query below

::

  $ jobstats --state FAILED -S 2019-01-01 -E 2019-02-01
  User: shahzeb
  Default Account: admin
  User is part of the following slurm accounts ['admin']
  User Raw Share: 100
  User Raw Usage: 39017527
  Number of Pending Jobs: 0
  Number of Running Jobs: 0
  Total Jobs Completed: 3
  Total Jobs Completed Successfully: 2
  Total Jobs Failed: 0
  Total Jobs Cancelled: 0
  Total Jobs Timeout: 0

  Today: 27/03/2019 15:11:39 sreport
  --------------------------------------------------------------------------------
  Top 10 Users 2019-03-26T00:00:00 - 2019-03-26T23:59:59 (86400 secs)
  Usage reported in CPU Hours
  --------------------------------------------------------------------------------
    Cluster     Login     Proper Name         Account     Used   Energy
  --------- --------- --------------- --------------- -------- --------
  slurm_cl+  shahzeb        Siddiqui          admin       24        0

                              Start Date:  2019-01-01
                                End Date:  2019-02-01
                     Job Summary by State: FAILED
  ________________________________________________________________________________
         JobID      User    JobName  Partition    Account  AllocCPUS ExitCode              Submit    Elapsed               Start                 End      State
  ------------ --------- ---------- ---------- ---------- ---------- -------- ------------------- ---------- ------------------- ------------------- ----------
  3558          shahzeb   io500.sh      short     admin          8      1:0 2019-01-04T16:30:15   00:00:01 2019-01-04T16:30:16 2019-01-04T16:30:17     FAILED
  4777          shahzeb helloWorl+    express     admin         16    127:0 2019-01-14T14:38:36   00:00:07 2019-01-14T14:38:37 2019-01-14T14:38:44     FAILED
  4778          shahzeb helloWorl+    express     admin         16    127:0 2019-01-14T14:40:05   00:00:01 2019-01-14T14:40:06 2019-01-14T14:40:07     FAILED
  6487          shahzeb interacti+    express     admin          1    127:0 2019-01-22T19:12:44   00:00:10 2019-01-22T19:12:44 2019-01-22T19:12:54     FAILED
  6490          shahzeb interacti+    express     admin          1      2:0 2019-01-22T19:27:55   00:00:09 2019-01-22T19:27:55 2019-01-22T19:28:04     FAILED
  6518          shahzeb   hostname        viz     admin          0      1:0 2019-01-23T14:06:01   00:00:00 2019-01-23T14:06:01 2019-01-23T14:06:01     FAILED
  6519          shahzeb   hostname        viz     admin          0      1:0 2019-01-23T14:06:09   00:00:00 2019-01-23T14:06:09 2019-01-23T14:06:09     FAILED
  6520          shahzeb   hostname        viz     admin          0      1:0 2019-01-23T14:06:25   00:00:00 2019-01-23T14:06:25 2019-01-23T14:06:25     FAILED
  6521          shahzeb   hostname        viz     admin          0      1:0 2019-01-23T14:06:38   00:00:00 2019-01-23T14:06:38 2019-01-23T14:06:38     FAILED
  6527          shahzeb   hostname        viz     admin         30      1:0 2019-01-23T14:10:25   00:00:00 2019-01-23T14:10:25 2019-01-23T14:10:25     FAILED


jobstats defaults to current user but you can select a different user by using ``-u`` or ``--user`` option
and use all the above commands mentioned above.

If you want to find user association to slurm account and fairshare usage you can use the ``-a`` option.

::

  $ jobstats -a -u user1
  User: admin
  Default Account: hpceng
  User is part of the following slurm accounts ['admin']
  User Raw Share: 100
  User Raw Usage: 38228406
  Number of Pending Jobs: 0
  Number of Running Jobs: 0
  Total Jobs Completed: 1
  Total Jobs Completed Successfully: 0
  Total Jobs Failed: 0
  Total Jobs Cancelled: 0
  Total Jobs Timeout: 0

  Today: 28/03/2019 14:45:52 sreport
  --------------------------------------------------------------------------------
  Top 10 Users 2019-03-27T00:00:00 - 2019-03-27T23:59:59 (86400 secs)
  Usage reported in CPU Hours
  --------------------------------------------------------------------------------
    Cluster     Login     Proper Name         Account      Used   Energy
  --------- --------- --------------- --------------- --------- --------




                        Shares for Account admin
               Account       User  RawShares  NormShares    RawUsage  EffectvUsage  FairShare
  -------------------- ---------- ---------- ----------- ----------- ------------- ----------
  admin                             200    0.076894   102749687      0.041010   0.690955
  admin                user1        100    0.010985           0      0.000000   1.000000
  admin                shahzeb      100    0.010985    38228406      0.006392   0.668093
   
