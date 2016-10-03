# The CORSIKA wrapper [![Build Status](https://travis-ci.org/fact-project/corsika_wrapper.svg?branch=master)](https://travis-ci.org/fact-project/corsika_wrapper)

A wrapper for [CORSIKA](https://www.ikp.kit.edu/corsika/)
- Run multiple CORSIKA instances in parallel (thread safe)
- Call CORSIKA from anywhere, systemwide on your machine
- Specify the output path in the CORSIKA call on the command line [optional `-o`]
- Have write access to your CORSIKA output files
- Store the CORSKA stdout and stderror next to your output [optional `-s`]

## Usage
### 1st) (only needed once)
```bash
user@machine:~$ corsika -c /home/user/corsika/corsika-74005/run/corsika74005Linux_QGSII_urqmd
```
### 2nd) 
```bash
user@machine:~$ corsika -i /home/user/reaserch/my_steering_card.txt -o /home/user/results/my_run.evtio
```
## Why
Calling CORSIKA is special. CORSIKA demands stdin, it can only be called in a certain 'run' directory environment, and it must not be called in parallel within the same working directory. (Told so by Konrad Bernloeh and Dieter Heck).
Further, the output path can not be specified on the command line and the output files are write protected.

## How
### Once
Tell the corsika wrapper which CORISKA executable it has to use using `-c`
```bash
user@machine:~$ corsika -c /home/user/corsika/corsika-74005/run/corsika74005Linux_QGSII_urqmd
```

### For every CORSIKA call
The CORSIKA wrapper first creates a temporary directory. Second, all CORSIKA dependencies in CORSIKA's 'run' directory are copied into the temporary working directory. Third, CORSIKA is called in the temporary working directory and gets the steering card piped in via stdin. If an output path is specified on the command line [-o], the output path in the steering card piped into CORSIKA is overwritten. Fourth, when CORSIKA is done, the temporary working directory is removed. Finally the output file's write protection is removed. The CORSIKA wrapper returns the CORSIKA return value. Optionally [-s], CORSIKA's stdout and stderr can be dumped into textfiles in the output path which shadow the name of the output.
