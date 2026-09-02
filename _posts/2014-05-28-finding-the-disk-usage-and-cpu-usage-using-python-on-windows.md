---
layout: post
title: "Finding the Disk Usage and CPU Usage Using Python on Windows"
date: 2014-05-28 14:36:00
tags: ["Windows", "_winreg", "Windows Registry", "Python"]
---

_Originally posted on my old blog on 2014-05-28._

while fixing a bug on mozilla regression project i found a bug which wanted to determine wether my system is win32 or win64 and by default make download 32 bit version 

i havnt fixed the bug yet 

but i think i have found a way to determine whether the system is using windows 32 bit or 64 bit using the following code 

which is a very short code actually 
    
    
    import _winreg
    windowsbit = get_registry_value(
    "HKEY_LOCAL_MACHINE",
    "SYSTEM\\CurrentControlSet\Control\\Session Manager\\Environment",
    "PROCESSOR_ARCHITECTURE")
    print windowsbit
    

  


  


  


  


i had a itch with my windows system i wanted to explore into the windows registry and get way to find the ram and cpu usage of the system . so i wrote a code to get them 
    
    
            
    import ctypes
    
    def ram():
        kernel32 = ctypes.windll.kernel32
        c_ulong = ctypes.c_ulong
        class MEMORYSTATUS(ctypes.Structure):
            _fields_ = [
                ('dwLength', c_ulong),
                ('dwMemoryLoad', c_ulong),
                ('dwTotalPhys', c_ulong),
                ('dwAvailPhys', c_ulong),
                ('dwTotalPageFile', c_ulong),
                ('dwAvailPageFile', c_ulong),
                ('dwTotalVirtual', c_ulong),
                ('dwAvailVirtual', c_ulong)
            ]
    
        memoryStatus = MEMORYSTATUS()
        memoryStatus.dwLength = ctypes.sizeof(MEMORYSTATUS)
        kernel32.GlobalMemoryStatus(ctypes.byref(memoryStatus))
        mem = memoryStatus.dwTotalPhys / (1024*1024)
        availRam = memoryStatus.dwAvailPhys / (1024*1024)
        if mem >= 1000:
            mem = mem/1000
            totalRam = str(mem) + ' GB'
        else:
    #        mem = mem/1000000
            totalRam = str(mem) + ' MB'
        return (totalRam, availRam)
    print"ram is :"
    print ram()

  


This next code snippet is used to find out what processor is in the client PC:

i am using the wmi and pythoncom module if i fail to get the registry values using _winreg (windows registry errors like windows registry path doesnot exist ) 
    
    
    def cpu():
        try:
            cputype = get_registry_value(
                "HKEY_LOCAL_MACHINE", 
                "HARDWARE\\DESCRIPTION\\System\\CentralProcessor\\0",
                "ProcessorNameString")
        except:
            import wmi, pythoncom
            pythoncom.CoInitialize() 
            c = wmi.WMI()
            for i in c.Win32_Processor ():
                cputype = i.Name
            pythoncom.CoUninitialize()
     
        if cputype == 'AMD Athlon(tm)':
            c = wmi.WMI()
            for i in c.Win32_Processor ():
                cpuspeed = i.MaxClockSpeed
            cputype = 'AMD Athlon(tm) %.2f Ghz' % (cpuspeed / 1000.0)
        elif cputype == 'AMD Athlon(tm) Processor':
            import wmi
            c = wmi.WMI()
            for i in c.Win32_Processor ():
                cpuspeed = i.MaxClockSpeed
            cputype = 'AMD Athlon(tm) %s' % cpuspeed
        else:
            pass
        return cputype
