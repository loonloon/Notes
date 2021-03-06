#### Windows Kernel Debugging Fundamentals by Bruce Mackenzie-Low ####

#### Debugging Crashes vs. Hangs  ####

#### System Crashes ####
* Crashes are unexpected, unhandled exceptions that occur in kernel mode
* The state of the operating system becomes questionable
* The operating system stops to avoid corrupting data
* Also known as the Blue Screen of Death (BSOD), Stops and Bugchecks
* The contents of RAM is saved in a file called Memory.dmp

#### System Hangs ####
* Hangs occur when processors or peripheral devices become unresponsive
* Software induced hangs include:
  * Depleted (耗尽) system resources (memory pool)
  * Runaway high priority compute-bound threads
  * Synchronization mechanisms (deadlocks & spinlocks)
  
* Hardware originated hangs include:
  * Faulty devices causing false interrupts
  * Failling processors
  * Corrupted RAM
  
* Memory dump must be manually forced on a hung system

#### Common Culprits (罪魁祸首) for Crashes ####
* Old antivirus software
* New drivers
  * Storport (over 50 hotfixes in Winodes 2003)
  * MPIO (over dozen hotfixes in Winodes 2008)
 * Incompatible drivers
   * Old Storport driver with a new miniport driver
 * Too many filter drivers
   * Antivirus, Deduplication, Disk quoto, Mirroring
 * Memory corruption
 * Hardware failures
 * Operating system bugs
 * Depleted system resources (memory pool)
 * Broken applications with deadlocks or spinlock hangs
 * High priority compute-bound (runaway) applications
 * Old antivirus software
 * New drivers
 * Incompatible driver
 * Broken hardware
 
 #### Determining Driver Dependencies ####
 * Dependency Walker (Depends.exe)
 
 #### How Memory Dumps are Created ####
 
![dump](https://user-images.githubusercontent.com/5309726/49303326-66e8b080-f504-11e8-8ec1-3e08617750f8.png)

 #### Types of Memory Dumps ####
 
 Dump      | Description
 --------- | --------------------------------------------------------------------------------------------------------------
 Small     | 64K size, containing minimal debugging information (stop code, parameters, stack, drivers).
 Kernel    | Medium size, containing kernel data structures, drivers and current process & thread information.
 Complete  | Large size, containing complete contents of memory. It's takes time.
 Automatic | New to Windows Server 2012 and 8, same as kernel memory dump but uses a smaller page file when system managed page files are used. Useful for PCs with SSD and lots of RAM.
 
 #### Swap / Page File ####
* Swap file (or swap space or, in Windows NT, a pagefile) is a space on a hard disk used as the virtual memory extension of a computer's real memory (RAM). 
* Having a swap file allows your computer's operating system to pretend that you have more RAM than you actually do. 
* The least recently used files in RAM can be "swapped out" to your hard disk until they are needed later so that new files can be "swapped in" to RAM. In larger operating systems (such as IBM's OS/390), the units that are moved are called pages and the swapping is called paging.
* One advantage of a swap file is that it can be organized as a single contiguous space so that fewer I/O operations are required to read or write a complete file.
 
 #### Dedicated Dump File ####
* Basically is a page file that is reserved for use only by the system crash dump routines.  It is not used for paging virtual memory. 
* Like a page file, the system process keeps an open handle to the dedicated dump file, which prevents it from being deleted. 
* When you manually initiate a memory dump, or the system crashes on its own, the data is written into the dedicated dump file instead of the page file on the system drive.
* The dedicated dump file can be stored on any local disk.
* This feature is enabled by setting the following registry value:
  * Location: HKEY_LOCAL_MACHINE\System\CurrentControlSet\Control\CrashControl
  * Name: DedicatedDumpFile
  * Type: REG_SZ
  * Value: A dedicated dump file together with a full path, such as D:\dedicateddumpfile.sys

 #### What needs to be configured in the debugger? ####
 * Symbol File Path
   * Provide meaningful names (function and variables) instead of hexadecimal numbers
   * Types of Symbols
     * Public
     * Private
 * Source File Path
 * Image File Path
 
 #### How do I get Symbols? ####
 * Symbols must match the OS version being debugged

<table>
 <tr>
  <td>From the WinDbg GUI</td>
  <td>srv*c:\symbols*http://msdl.microsoft.com/download/symbols</td>
 </tr>
 <tr>
  <td>From the CLI</td>
  <td>Windbg -y srv*c:\symbols*http://msdl.microsoft.com/download/symbols</td>
 </tr>
 <tr>
  <td>from the WinDbg command prompt</td>
  <td>.sympath srv*c:\symbols*http://msdl.microsoft.com/download/symbols</td>
 </tr>
 <tr>
  <td>SET the _NT_SYMBOL_PATH environment variable</td>
  <td>SET_NT_SYMBOL_PATH=srv*c:\symbols*http://msdl.microsoft.com/download/symbols</td>
 </tr>
</table>

#### Additional Symbol Commands to Know ####
<table>
 <tr>
  <td>.symfix</td>
  <td>sets the symbol path to use the Microsoft Symbol Server</td>
 </tr>
 <tr>
  <td>!sympath</td>
  <td>displays the current symbol file path</td>
 </tr>
 <tr>
  <td>!sym noisy</td>
  <td>turns on verbose mode</td>
 </tr>
 <tr>
  <td>!sym quiet</td>
  <td>turns off verbose mode</td>
 </tr>
 <tr>
  <td>.reload</td>
  <td>reloads symbols after changing the symbol path</td>
 </tr>
</table>

#### Source File Path ####
* Display corresponding C/C++ source code in the Debugger

<table>
 <tr>
  <td>From the WinDbg GUI</td>
  <td>c:\mySource</td>
 </tr>
 <tr>
  <td>From the CLI</td>
  <td>windbg -srcpath c:\mySource</td>
 </tr>
 <tr>
  <td>from the Debugger command prompt</td>
  <td>.srcpath c:\mySource</td>
 </tr>
 <tr>
  <td>SET the _NT_SYMBOL_PATH environment variable</td>
  <td></td>
 </tr>
</table>

#### Strategies for Analyzing Crashes ####
* Start with !analyze -v
* Look up the Stop code with the Bug Check Code Reference section in Help
* Examine the stack trace to determine what drives were executing (kv)
* Determine what process and thread was running (!process, !thread)
* Review loaded driver dates for old or brand new drives (lm t n, !lmi)
* Track down any outstanding I/O request packets (!irp, !object, !devstack)

#### Strategies for Analyzing Hangs ####
* Start with !analyze -v -hang
* Check for deadlocks or spinlocks (!locks, !qlocks)
* Check for compute-bound runaway threads (!thread, !running -t, !stacks)
* Check for depleted system memory resources (!vm, !memusage)
* Determine memory pool consumers (!poolused)
* Search http://support.microsoft.com (KBs), Bing and Google

#### Displaying the Stack Trace ####
* k Command

<table>
 <tr>
  <td>k</td>
  <td>Display child stack pointer, return address, function name</td>
 </tr>
 <tr>
  <td>kb</td>
  <td>Displays child SP, ret.Addr, function name, first 3 parameters</td>
 </tr>
 <tr>
  <td>kv</td>
  <td>Displays child SP, ret.Addr, function name, first 3 parameters, FPO info</td>
 </tr>
 <tr>
  <td>kc</td>
  <td>display function name</td>
 </tr>
 <tr>
  <td>kn</td>
  <td>display frame numbers</td>
 </tr>
 <tr>
  <td>kd</td>
  <td>display raw stack data</td>
 </tr>
</table>

#### Alternative way to view the same stack data ####
* dds esp (for x86 memory dump)
* dqs rsp (for x64 memory dump)
* !stacks to display a summary of kernel stack
* !stacks2 to display a full stack trace for all threads
