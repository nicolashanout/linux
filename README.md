# CMPE283 : Virtualization
# Assignment 3: Instrumentation via hypercall
**Heng Jerry Quan** <br>
- Compiled list of enabled system exits


**Nicolas Hanout** <br>
- Compiled list of valid system exits

**Both** <br>
Implemented functionality and code, including
- Create data structure to hold counts
- Export data structure
- Flow control to check valid and enabled exits
- Increment appropriate counter on exit

## Steps

## Questions
3. The rate of exits of different types is not the same between types. Some, like exit code 0 (General Protection Exception), occur very frequently, while some others, like exit code X, barely occur at all. We also don't expect the rate of exits to be uniform, as some exits are bound to happen more than others.
Exits after Boot:
```
0- 8407, ffff97e0, 0, ffffffff

1- 25481, ffff97e0, 1, ffffffff

2- 0, ffff97e0, 2, ffffffff

3- 0, 0, 0, 0

4- 0, 0, 0, 0

5- 0, 0, 0, 0

6- 0, 0, 0, 0

7- 6352, ffff97e0, 7, ffffffff

8- 0, ffff97e0, 8, ffffffff

9- 0, ffff97e0, 9, ffffffff

10- 132076, ffff97e0, a, ffffffff

11- 0, 0, 0, 0

12- 27229, ffff97e0, c, ffffffff

13- 0, ffff97e0, d, ffffffff

14- 0, ffff97e0, e, ffffffff

15- 0, ffff97e0, f, ffffffff

16- 0, 0, 0, 0

17- 0, 0, 0, 0

18- 0, ffff97e0, 12, ffffffff

19- 0, ffff97e0, 13, ffffffff

20- 0, ffff97e0, 14, ffffffff

21- 0, ffff97e0, 15, ffffffff

22- 0, ffff97e0, 16, ffffffff

23- 0, ffff97e0, 17, ffffffff

24- 0, ffff97e0, 18, ffffffff

25- 0, ffff97e0, 19, ffffffff

26- 0, ffff97e0, 1a, ffffffff

27- 0, ffff97e0, 1b, ffffffff

28- 18175, ffff97e0, 1c, ffffffff

29- 2, ffff97e0, 1d, ffffffff

30- 132365, ffff97e0, 1e, ffffffff

31- 720, ffff97e0, 1f, ffffffff

32- 21545, ffff97e0, 20, ffffffff

33- 0, 0, 0, 0

34- 0, 0, 0, 0

35- 0, 0, 0, ffffffff

36- 0, ffff97e0, 24, ffffffff

37- 0, ffff97e0, 25, ffffffff

38- 0, 0, 0, ffffffff

39- 0, ffff97e0, 27, ffffffff

40- 0, ffff97e0, 28, ffffffff

41- 0, ffff97e0, 29, ffffffff

42- 0, 0, 0, ffffffff

43- 0, ffff97e0, 2b, ffffffff

44- 11, ffff97e0, 2c, ffffffff

45- 0, ffff97e0, 2d, ffffffff

46- 0, ffff97e0, 2e, ffffffff

47- 0, ffff97e0, 2f, ffffffff

48- 35708, ffff97e0, 30, ffffffff

49- 30422, ffff97e0, 31, ffffffff

50- 0, ffff97e0, 32, ffffffff

51- 0, 0, 0, 0

52- 11029, ffff97e0, 34, ffffffff

53- 0, ffff97e0, 35, ffffffff

54- 9, ffff97e0, 36, ffffffff

55- 0, ffff97e0, 37, ffffffff

56- 0, ffff97e0, 38, ffffffff

57- 0, ffff97e0, 39, ffffffff

58- 0, ffff97e0, 3a, ffffffff

59- 0, ffff97e0, 3b, ffffffff

60- 0, ffff97e0, 3c, ffffffff

61- 0, ffff97e0, 3d, ffffffff

62- 0, ffff97e0, 3e, ffffffff

63- 0, 0, 0, 0

64- 0, 0, 0, 0

65- 0, 0, 0, ffffffff

66- 0, 0, 0, 0

67- 0, 0, 0, 0

68- 0, 0, 0, 0

69- 0, 0, 0, ffffffff

```

After using the vm:
```
0- 8407, 0xffff97e0, 0x0, 0xffffffff

1- 240800, 0xffff97e0, 0x1, 0xffffffff

2- 0, 0xffff97e0, 0x2, 0xffffffff

3- 0, 0x0, 0x0, 0x0

4- 0, 0x0, 0x0, 0x0

5- 0, 0x0, 0x0, 0x0

6- 0, 0x0, 0x0, 0x0

7- 59433, 0xffff97e0, 0x7, 0xffffffff

8- 0, 0xffff97e0, 0x8, 0xffffffff

9- 0, 0xffff97e0, 0x9, 0xffffffff

10- 134982, 0xffff97e0, 0xa, 0xffffffff

11- 0, 0x0, 0x0, 0x0

12- 141722, 0xffff97e0, 0xc, 0xffffffff

13- 0, 0xffff97e0, 0xd, 0xffffffff

14- 0, 0xffff97e0, 0xe, 0xffffffff

15- 0, 0xffff97e0, 0xf, 0xffffffff

16- 0, 0x0, 0x0, 0x0

17- 0, 0x0, 0x0, 0x0

18- 0, 0xffff97e0, 0x12, 0xffffffff

19- 0, 0xffff97e0, 0x13, 0xffffffff

20- 0, 0xffff97e0, 0x14, 0xffffffff

21- 0, 0xffff97e0, 0x15, 0xffffffff

22- 0, 0xffff97e0, 0x16, 0xffffffff

23- 0, 0xffff97e0, 0x17, 0xffffffff

24- 0, 0xffff97e0, 0x18, 0xffffffff

25- 0, 0xffff97e0, 0x19, 0xffffffff

26- 0, 0xffff97e0, 0x1a, 0xffffffff

27- 0, 0xffff97e0, 0x1b, 0xffffffff

28- 18175, 0xffff97e0, 0x1c, 0xffffffff

29- 2, 0xffff97e0, 0x1d, 0xffffffff

30- 140960, 0xffff97e0, 0x1e, 0xffffffff

31- 1630, 0xffff97e0, 0x1f, 0xffffffff

32- 202901, 0xffff97e0, 0x20, 0xffffffff

33- 0, 0x0, 0x0, 0x0

34- 0, 0x0, 0x0, 0x0

35- 0, 0x0, 0x0, 0xffffffff

36- 0, 0xffff97e0, 0x24, 0xffffffff

37- 0, 0xffff97e0, 0x25, 0xffffffff

38- 0, 0x0, 0x0, 0xffffffff

39- 0, 0xffff97e0, 0x27, 0xffffffff

40- 0, 0xffff97e0, 0x28, 0xffffffff

41- 0, 0xffff97e0, 0x29, 0xffffffff

42- 0, 0x0, 0x0, 0xffffffff

43- 0, 0xffff97e0, 0x2b, 0xffffffff

44- 11, 0xffff97e0, 0x2c, 0xffffffff

45- 0, 0xffff97e0, 0x2d, 0xffffffff

46- 0, 0xffff97e0, 0x2e, 0xffffffff

47- 0, 0xffff97e0, 0x2f, 0xffffffff

48- 36283, 0xffff97e0, 0x30, 0xffffffff

49- 72421, 0xffff97e0, 0x31, 0xffffffff

50- 0, 0xffff97e0, 0x32, 0xffffffff

51- 0, 0x0, 0x0, 0x0

52- 61146, 0xffff97e0, 0x34, 0xffffffff

53- 0, 0xffff97e0, 0x35, 0xffffffff

54- 9, 0xffff97e0, 0x36, 0xffffffff

55- 0, 0xffff97e0, 0x37, 0xffffffff

56- 0, 0xffff97e0, 0x38, 0xffffffff

57- 0, 0xffff97e0, 0x39, 0xffffffff

58- 0, 0xffff97e0, 0x3a, 0xffffffff

59- 0, 0xffff97e0, 0x3b, 0xffffffff

60- 0, 0xffff97e0, 0x3c, 0xffffffff

61- 0, 0xffff97e0, 0x3d, 0xffffffff

62- 0, 0xffff97e0, 0x3e, 0xffffffff

63- 0, 0x0, 0x0, 0x0

64- 0, 0x0, 0x0, 0x0

65- 0, 0x0, 0x0, 0xffffffff

66- 0, 0x0, 0x0, 0x0

67- 0, 0x0, 0x0, 0x0

68- 0, 0x0, 0x0, 0x0

69- 0, 0x0, 0x0, 0xffffffff
```
4. In testing our new leaf function, we found that HLT had the most exits with 0x5D38A = 381834 exits, while the having the least number of exits is shared by many exit types all having 0 exits.
____________________________________________________________________________________________

# Assignment 2: Modifying instruction behavior in KVM
## Team members
**Heng Jerry Quan** <br>
- Researched and implemented exporting and loading global variables
- Researched and implemented methods to load eax in C, ended up using \_\_asm\_\_()


**Nicolas Hanout** <br>
- Researched and implemented atomic variables and associated functions
- Researched, implemented, and verified u64 to int32 conversion


## Steps

1. On OpenSUSE tumbleweed kernel version: 5.11.15
2. on github fork repo: `https://github.com/torvalds/linux`
3. clone to vm.
4. in local repo copy `cp /boot/config-5.11.15-1-default .config`
5. `sudo zypper in gcc flex bison`
6. `make oldconfig` and accept all defaults
7. using the yast software managment tool, open the patterns tab and install the kernel development software
8. `sudo zypper in dwarves`
9. `make -j $(nproc) modules && make -j $(nproc) && sudo make -j $(nproc) modules_install && sudo make -j $(nproc) install`
10. `sudo reboot` then using `uname -a` should show you the new version of the kernel
11. in the repo, modify code:
  - in `arch/x86/kvm/cpuid.c` modify the `kvm_emulate_cpuid` function by creating a new leaf function for eax value `0x4FFFFFFF`,
     ``` c 
     if(eax == 0x4FFFFFFF)
        {
           // set eax, ebx and ecx to the
        } else
        {
          // original behaviour
        }
      ```
    and declare atomic 32-bit and 64-bit integers for the number of exits and cycles and export them.
      ``` c
      atomic_t cmpe283_exit_counter = ATOMIC_INIT(0);
      atomic64_t cmpe283_total_cycles = ATOMIC64_INIT(0);
      EXPORT_SYMBOL(cmpe283_exit_counter);
      EXPORT_SYMBOL(cmpe283_total_cycles);
      ```
  - in `arch/x86/kvm/vmx/vmx.c` declare the previous external variable
    ``` c
    extern atomic_t cmpe283_exit_counter;
    extern atomic64_t cmpe283_total_cycles;
    ```
    and increment them in `vmx_handle_exit` function.
    ``` c
      u64 start, end;
      start = rdtsc();
      atomic_inc(&cmpe283_exit_counter);
      //
      // Handle exit
      //
      end = rdtsc();
      atomic64_fetch_add(end-start, &cmpe283_total_cycles);
    ```
  - in `arch/x86/kvm/cpuid.c` when the leaf function `0x4FFFFFFF` is called place the appropriate values into `eax`, `ebx`, and `ecx`.
    ``` c
          eax = (u32)atomic_read(&cmpe283_exit_counter);
          total_cycles = (u64)atomic64_read(&cmpe283_total_cycles);
          ebx = (u32)((total_cycles & 0xFFFFFFFF00000000LL) >> 32);
          ecx = (u32)(total_cycles & 0xFFFFFFFFLL);
    ```
12. repeat step 9 and reboot again
13. using `YaST`>`Virtualization`>`Install Hypervisor and Tools`,  instal kvm server and manager.
14. create a vm and install a guest OS (we used Ubuntu 20.04.02 LTS)
15. from inside the vm call execute a program that calls cpuid and the number of total exits will be stored in `eax` while the high-32 and low-32 bits of the total number of clock cycles spent handleing exits will be stored in `ebx` and `ecx` respectivly  
sample output: `CPUID(0x4FFFFFFF), exits=583999, cycles spent in exit=13436425563`

## Questions

**Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?**

There are a large number of exits, which do not increase at a stable rate. Depending on what the user is doing (specifically during bootup and depending on varying levels of hardware usage like network access, disk usage, etc...), there will be more or less exits accordingly. More exits are performed during VM operations that need access to the hardware (e.g. web browsers need networking hardware, whose access increases the number of exits). for our test VM, a full VM boot typically consists of around 580,000 exits, with about 13 billion clock cycles spent processing exits.
