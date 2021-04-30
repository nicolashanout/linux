# CMPE283 : Virtualization
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
