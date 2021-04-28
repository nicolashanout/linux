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

## Questions

**Comment on the frequency of exits – does the number of exits increase at a stable rate? Or are there more exits performed during certain VM operations? Approximately how many exits does a full VM boot entail?**

There are a large number of exits, which do not increase at a stable rate. Depending on what the user is doing (specifically on varying levels of hardware usage), there will be more or less exits accordingly. More exits are performed during VM operations that need access to the hardware (e.g. web browsers need networking hardware, whose access increases the number of exits). A full VM boot typically consists of 580,000 exits, with about 13 billion clock cycles spent processing exits.
