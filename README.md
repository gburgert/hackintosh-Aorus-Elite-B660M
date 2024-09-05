# Hackintosh 12100F - 21/8/24
  
Gigabyte Aorus Elite B660M DDR4  
Seguindo as instruções de https://dortania.github.io/OpenCore-Install-Guide/macos-limits.html#cpu-support  

  
## Levantar a conf do sistema  
https://github.com/KernelWanderers/OCSysInfo/releases  
```
 ─ CPU
  └── 12th Gen Intel(R) Core(TM) i3-12100F Alder Lake
      ├── Cores: 4
      ├── Threads: 8
      ├── SSE: SSE4.2
      └── SSSE3: Supported
  
─ Motherboard
  ├── Model: B660M AORUS ELITE DDR4
  └── Manufacturer: Gigabyte Technology Co., Ltd.
  
─ GPU
  └── Radeon RX 570 Series
      ├── Device ID: 0x67DF
      ├── Vendor: 0x1002
      ├── PCI Path: PciRoot(0x0)/Pci(0x1,0x0)/Pci(0x0,0x0)
      ├── ACPI Path: \_SB.PC00.PEG1.PEGP
      └── Codename: Polaris 10
  
─ Memory  
  ├── KF3600C17D4/8GX (Part-Number)
  │   ├── Type: DDR4
  │   ├── Slot
  │   │   ├── Bank: BANK 0
  │   │   └── Channel: DDR4-A2
  │   ├── Frequency (MHz): 2400 MHz
  │   ├── Manufacturer: Kingston
  │   └── Capacity: 8192MB
  └── KF3600C17D4/8GX (Part-Number)
      ├── Type: DDR4
      ├── Slot
      │   ├── Bank: BANK 0
      │   └── Channel: DDR4-B2
      ├── Frequency (MHz): 2400 MHz
      ├── Manufacturer: Kingston
      └── Capacity: 8192MB
  
─ Network
  └── RTL8125 2.5GbE Controller
      ├── Device ID: 0x8125
      ├── Vendor: 0x10EC
      ├── PCI Path: PciRoot(0x0)/Pci(0x1c,0x2)/Pci(0x0,0x0)
      └── ACPI Path: \_SB.PC00.RP03.PXSX
  
─ Audio
  ├── RV635 HDMI Audio [Radeon HD 3650/3730/3750]
  │   ├── Device ID: 0xAA01
  │   └── Vendor: 0x1002
  └── Realtek ALC897
      ├── Device ID: 0x0897
      └── Vendor: 0x10EC
  
─ Input
  └── Logitech USB Input Device (USB)
      ├── Product ID: 0xC52B
      └── Vendor ID: 0x046D
  
─ Storage
  ├── VendorCo ProductCode
  │   ├── Type: Unspecified
  │   ├── Connector: USB
  │   └── Location: External
  ├── CT240BX500SSD1
  │   ├── Type: Solid State Drive (SSD)
  │   ├── Connector: Serial ATA (SATA)
  │   └── Location: Internal
  ├── CT240BX500SSD1
  │   ├── Type: Solid State Drive (SSD)
  │   ├── Connector: Serial ATA (SATA)
  │   └── Location: Internal
  └── CT1000MX500SSD1
      ├── Type: Solid State Drive (SSD)
      ├── Connector: Serial ATA (SATA)
      └── Location: Internal
```

## Ferramentas Windows  
Ferramentas que precisa pra fazer um boot funcional no windows.  
\
**Instalar choco**  
Abrir powershell admin  
```
Get-ExecutionPolicy  
--> Restricted  
Set-ExecutionPolicy AllSigned  
Get-ExecutionPolicy  
--> ByPass  
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://community.chocolatey.org/install.ps1'))  
choco  
-->v2.3.0  
```
\
**Instalar python3**  
```
choco install python  
```
\
**Instalar git**  
Abrir powershell com permissão de admin  
```
choco install git  
```
Precisa fechar e abrir de novo o powershell pro git entrar no path  
\
**Baixar propertree**  
https://github.com/corpnewt/ProperTree  
```
git clone https://github.com/corpnewt/ProperTree  
ProperTree\Scripts\AssociatePlistFiles.bat  
```
\
**Gerador de serial. Tentei seguir sem gerar o serial, nem boota**  
```
git clone https://github.com/corpnewt/GenSMBIOS  
```
\
**Vai precisar do Explorer++ no Windows**  
```
choco install explorerplusplus  
```
\
**SDDTTime - vários SSDTs**  
https://github.com/corpnewt/SSDTTime  
```  
git clone https://github.com/corpnewt/SSDTTime.git  
```
\
**Instala o usb tool do Windows**  
https://github.com/USBToolBox/tool  
  
## Ferramentas MacOS  
Ferramentas para o refinamento do sistema.  
\
**Mountefi**  
https://github.com/corpnewt/MountEFI  
Para atualizar o config.plist pelo mac.  
```
git clone https://github.com/corpnewt/MountEFI  
cd MountEFI  
chmod +x MountEFI.command  
```
\
**IORegistry**  
https://github.com/khronokernel/IORegistryClone/blob/master/ioreg-302.zip  
Ajuda num monte de coisa. Usei pra verificar se precisa ou não do SSDT-PLUG.  
\
**Hakintool**  
https://github.com/benbaker76/Hackintool/  
Como ele mesmo diz, canivete suíço do hackintosh. Se não usar agora, vai usar em algum momento.  
  
**MaciASL**  
https://github.com/acidanthera/MaciASL
Esse cara vai abrir os aml (tabela ACPI) que o SSDTTime vai fazer o dump. 
  
## Preparando o EFI  
https://dortania.github.io/OpenCore-Install-Guide/installer-guide/opencore-efi.html  
Copiar OpenCore-1.0.1-DEBUG/X64/EFI. Daqui pra frente usa a cópia dessa pasta.  
### Drivers  
**Drivers Opencore**  
--> em EFI\OC\Drivers deixa ResetNvramEntry.efi, OpenRuntime.efi  
  
**Driver Hfsplus**  
https://github.com/acidanthera/OcBinaryData/blob/master/Drivers/HfsPlus.efi  
--> pega HfsPlus.efi e coloca no EFI\OC\Drivers  
  
### Tools  
**Tools opencore**  
--> em EFI\OC\Tools fica com OpenShell.efi, MmapDump.efi, ResetSystem.efi  
  
### Kexts  
**kexts lilo**  
https://github.com/acidanthera/Lilu/releases  
--> fica com lilo.kext  
  
**kexts virtualsmc**  
https://github.com/acidanthera/VirtualSMC/releases  
--> fica com SMCProcessor.kext, SMCSuperIO.kext, VirtualSMC.kext  
  
**Sensores radeon**  
https://github.com/ChefKissInc/SMCRadeonSensors  
--> fica com SMCRadeonSensors.kext  
  
**kexts WhateverGreen**  
https://github.com/acidanthera/WhateverGreen/releases  
--> fica com WhateverGreen.kext  
  
**kexts Audio**  
https://github.com/acidanthera/AppleALC/releases  
--> fica com AppleALC.kext  
  
**Rede RTL8125**   
https://www.insanelymac.com/forum/files/file/1004-lucyrtl8125ethernet/  
--> fica com LucyRTL8125Ethernet.kext  
  
**kexts usb**   
https://github.com/USBToolBox/kext  
--> fica com USBToolBox.kext. Vou fazer o kext no windows  
  
**kext tscsync - macos slow**  
https://github.com/acidanthera/CpuTscSync/releases  
--> fica com CpuTscSync.kext  

**kext nvme**  
https://github.com/acidanthera/NVMeFix/releases  
--> fica com NVMeFix.kext  
  
**kext RestrictEvents - deixa o sistema mais estável**  
https://github.com/acidanthera/RestrictEvents  
--> fica com RestrictEvents.kext  
  
**Mapeamento USB**  
No windows, roda o usbtool  
```
C - change settings
C - bind companions disable
B - back
D - discover port
```
Plugar um pendrive usb2 em cada porta, depois um pendrive usb3 em cada porta. Anota quem é USB2 e USB3 porque o guess não é bom.  
```
S - select ports and buils kext
T:17,19:9 (USBC com switch)
T:2,4,6,8,9,10,11:0 (USB2 A)
T:18,20,25,26:3 (USB3 A)
T:3:8 (USBC - usb2). Deixei esse caso tenha algum maluco usb2 que seja tipo C. É a porta da frente do gabinete.
```
Total 15 portas. Desliguei o companion porque tem mais de 15 portas reconhecidas e queria desativar o USB2 em algumas, mas ele desliga o USB3 junto se tiver o companion ligado e também porque tinha portas fantasmas ligando junto com as USB2.  
```
2,3,4,6,7,8,9,10,11,17,18,19,20,25,26
K - build kext
```
-->vai usar o UTBMap.kext  
  
Colocar tudo em EFI\OC\Kexts  
  
*FIM DOS KEXTS*  
  
### ACPI/SSDT  
Dicas aqui  
https://chriswayg.gitbook.io/opencore-visual-beginners-guide/advanced-topics/using-alder-lake#ssdts  
  
**SSDTTime**  
https://dortania.github.io/Getting-Started-With-ACPI/ssdt-methods/ssdt-easy.html#running-ssdttime  
```
SSDTTIME\SSDTTime.bat
p. dump acpi tables
1 - fix IRQ, C (default), fez SSDT-HPET.aml
2 - fake EC for Catalina and newer users, fez SSDT-EC.aml
4 - usbx, B (build), fez SSDT-USBX.aml
5 - plugintype, fez SSDT-PLUG-ALT.aml
7 - rtcawac, fez SSDT-RTCAWAC.aml
```
Em SSDTTIME\results vai ter os ssdts e os patches para acrescentar no config.plist  
  
**SSDT-PLUG is not required on macOS 12.3 and up.**  
https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#enabling-x86platformplugin  
Precisa. Usar o SSDT-PLUG-ALT.aml gerado pelo SSDTTime.  
  
**SSDT-RHUB Gigabyte and AsRock motherboards do not need this SSDT**  
https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#acpi  
De fato não precisa. Removi.  
  
**XCPM power management pro Alder Lake**  
https://github.com/acidanthera/OpenCorePkg/blob/master/Docs/AcpiSamples/Source/SSDT-PLUG-ALT.dsl  
Usei o gerado pelo SSDTTime. Deixo aqui vai que precisa.  
  
**SSDT Comet Lake**  
https://dortania.github.io/Getting-Started-With-ACPI/ssdt-platform.html#desktop  
Sobrou EC-USBX e AWAC. Vou pegar os gerados pelo SSDTTime.  
SSDT-EC.aml  
SSDT-USBX.aml  
SSDT-RTCAWAC.aml  
  
**IRQ fix**  
https://dortania.github.io/Getting-Started-With-ACPI/Universal/irq.html  
Entra com o SSDT-HPET.aml e os patches *HPET _STA to XSTA Rename*, *HPET _CRS to XCRS Rename* e *IRQ Patches* gerados pelo SSDTTime.  
  
**SSDTs a verificar o que são**  
SSDT-SBUS.aml - tá no [Cuticuti](##Cuticuti)  
SSDT-PCID.aml - dmac. Não vi necessidade   

## Montar o config.plist  
https://dortania.github.io/OpenCore-Install-Guide/config.plist/  
```
cd hackintosh\12100f\kext
copy OpenCore-1.0.1-DEBUG\Docs\Sample.plist ..\EFI\OC\config.plist
```
  
**Modelo do 13thdemarch**  
https://github.com/13thdemarch/b660m-aorus-pro-hackintosh  
Config.plist dele foi a base que deu o 1o boot bem sucedido.  
  
**Modelo do Luchina**  
https://github.com/luchina-gabriel/EFI-GIGABYTE-Z690-AORUS-ELITE-AX-12900K-RX6900XT  
Baixei o config.plist dele pra comparar. Usei o dele pra conferir com o de cima. 
  
**Abrir propertree, abrir config.plist**  
File -> OC Clean Snapshot  
apontar para EFI/OC  
  
Depois de analisar o do 13thdemarch e o do Luchina  
**Root->Booter->Quirks**  
```
DevirtualiseMmio:       True
EnableWriteUnprotector: False
ProtectUefiServices:    True
RebuildAppleMemoryMap:  True
ResizeAppleGpuBars:     -1 (vou deixar o resizeblebar desligado por enquanto)
SetupVirtualMap:        false
SyncRuntimePermissions: True
```
  
**Root->DeviceProperties->add**  
https://dortania.github.io/OpenCore-Install-Guide/config.plist/comet-lake.html#add-2
Removi PciRoot(0x0)/Pci(0x1b,0x0) - You can delete this property outright as it's unused for us at this time. We'll be using the boot-arg alcid=xxx instead to accomplish this.  
    
**Root->Kernel->Emulate***  
https://chriswayg.gitbook.io/opencore-visual-beginners-guide/advanced-topics/using-alder-lake#kernel-greater-than-emulate  
```
Cpuid1Data    55060A00000000000000000000000000
Cpuid1Mask    FFFFFFFF000000000000000000000000
MinKernel     19.0.0
```
  
**Root->Kernel->Quirks**  
```
AppleXcpmCfgLock:        False  (Not needed if CFG-Lock is disabled in the BIOS)
DisableIoMapper:         True 	 (Para não ter de desligar o VT-D no BIOS)
DisableLinkeditJettison  True
PanicNoKextDump:         True
PowerTimeoutKernelPanic: True
ProvideCurrentCpuInfo:   True (precisa se bem me lembro por causa do fake cpu)
```
  
**Root->Kernel-Patch**   
Relevante quando tem E-cores. Não fez diferença no número de threads, mas deixo ele aqui pra não perder.  
```
sysctl -n hw.ncpu (mostra # threads)
1
Arch: String any
Base: string _cpu_thread_alloc
Comment: string force HT enabled for Mojave or later
Count: number 1
Find: data 8B889401 0000
Identifier: string kernel
MinKernel: string 18.0.0
Replace: data B9FF0000 0090
```
 
**Root->Kernel->Scheme->KernelArch**: (string) x86_64  
  
**Root->Misc->Debug**  
```
AppleDebug:      True
ApplePanic:      True
DisableWatchDog: True
Target:          67
```
  
**Root->Misc->Security**  
```
AllowSetDefault:      True (seta default boot com Ctrl+Enter no menu)
ExposeSensitiveData:  8 (0x08 máximo documentado)
ScanPolicy:           0
SecureBootModel:      Default
Vault:                Optional
```  
**Root->NVRam->add**  
Teclado https://github.com/acidanthera/OpenCorePkg/blob/master/Utilities/AppleKeyboardLayouts/AppleKeyboardLayouts.txt  
[128] pt_BR - Brazilian-ABNT2 (lingua:teclado)  
```
->C436110-AB2A-4BBB-A880-FE41995C9F82  
boot-args: debug=0x100 alcid=3  
prev-lang:kbd String en-US:128
```
```
->4D1FDA02-38C7-4A6A-9CC6-4BCCA8B30102
revcpuname:   String    Intel i3-12100F
revcpu:       Number    1
```  
```
->7C436110-AB2A-4BBB-A880-FE41995C9F82
boot-args:         -v  keepsyms=1 alcid=12
csr-active-config: 03080000
prev-lang:kbd:     en-US:128
```
  
**Root->NVRam->delete**  
Tive problema pra atualizar o valor do csr e esse delete ajudou.  
```
->7C436110-AB2A-4BBB-A880-FE41995C9F82
2: (String) csr-active-config
```
  
**Root->PlatformInfo->Generic**  
Roda o gensmbios. Até inserir esses dados no config.plist a máquina não completava o boot. Suspeito que seja pela falta da ROM.  
```
cd \hackintosh\GenSMBIOS  
GenSMBIOS.bat  
-> opção x, MacPro7,1 (2019)  
============  
Type:         MacPro7,1
Serial:       SSSSERIAL
Board Serial: SSSSBOARD
SmUUID:       SSSSUUID
Apple ROM:    A46706B84CA2
============  
```  
Testa aqui https://checkcoverage.apple.com/ . Precisa dar inválido. Se retornar produto gera outro.  
```
MLB:                SSSSBOARD
ROM:                A46706B84CA2
SystemProductName:  MacPro7,1
SystemSerialNumber: SSSSERIAL
SystemUUID:         SSSSUUID
```
  
### Processor type para restricted events  
https://github.com/acidanthera/RestrictEvents  
ProcessorType=0x601  (menos de 8 cores)  
  
**Root->UEFI->Quirks->ReleaseUsbOwnership**: False (system freeze on boot. Not recommended unless specifically required)  
  
## Intel BIOS settings  
### Disable  
    Fast Boot  
    Secure Boot  
    Serial/COM Port  
    Parallel Port  
    Compatibility Support Module (CSM) (Must be off in most cases, GPU errors/stalls like gIO are common when this option is enabled)  
    Thunderbolt  
    Intel SGX  
    Intel Platform Trust  
    CFG Lock (MSR 0xE2 write protection)(This must be off, if you can't find the option then enable AppleXcpmCfgLock under Kernel -> Quirks. Your hack will not boot with CFG-Lock enabled)  
  
### Enable  
    VT-x  
    Above 4G Decoding  
        2020+ BIOS Notes: When enabling Above4G, Resizable BAR Support may become an available on some Z490 and newer motherboards. Please ensure that Booter -> Quirks -> ResizeAppleGpuBars is set to 0 if this is enabled.  
    Hyper-Threading  
    Execute Disable Bit  
    EHCI/XHCI Hand-off  
    OS type: Windows 10 UEFI Mode  
    DVMT Pre-Allocated(iGPU Memory): 64MB or higher  
    SATA Mode: AHCI  
  
### Pra gravar o efi no windows  
https://superuser.com/questions/662823/how-do-i-mount-the-efi-partition-on-windows-8-1-so-that-it-is-readable-and-write  
Abrir powershell como admin  
```diskpart  
list vol  
sel vol 4  
assign  
```  
Para abrir o drive EFI precisa rodar o explorer++ como admin e copiar os arquivos.  
Colocar o EFI montado na partição EFI do pendrive de instalação do MacOS.  
  
## Cuticuti  
  
Refinamentos e coisas que só dá pra rodar depois do MacOS instalado.  

**SMBus**  
https://dortania.github.io/Getting-Started-With-ACPI/Universal/smbus-methods/manual.html#verify-it-s-working  
Tem um receitão falando como faz o SMBus. Fiz nada disso. No final executa um comando pra ver que deu certo  
```
kextstat | grep -E "AppleSMBus"
Executing: /usr/bin/kmutil showloaded
No variant specified, falling back to release
  114    0 0xffffff7f98f8e000 0x1000     0x1000     com.apple.driver.AppleSMBusPCI (1.0.14d1) BEA35DB8-3718-30CA-AB29-8FBF5143D4B6 <16 7 6 3>
  163    1 0xffffff7f98f82000 0x7000     0x7000     com.apple.driver.AppleSMBusController (1.0.18d1) FE4A1901-4945-38E8-9F09-BAE75DBAB72C <162 16 15 7 6 3>
```  
O SMBus tá funcionando sem precisar fazer nada. 
  
**CPUFriend**  
https://dortania.github.io/OpenCore-Post-Install/universal/pm.html#using-cpufriend  
https://chriswayg.gitbook.io/opencore-visual-beginners-guide/advanced-topics/using-alder-lake#ssdts  

```
git clone https://github.com/corpnewt/CPUFriendFriend.git
cd CPUFriendFriend
./CPUFriendFriend.command
```
* LFM:        0x08 (800MHz)  
* EPP value:  0x00 (Modern iMac)  
* Perf Bias:  0x01 (Modern iMac)  
* MacBook Air SMBIOS: n  

Usei 0x0A (1000MHz) no LFM. O Intel Power Gadget apresenta o core min em 0,8. Não achei essa informação na internet.  
```
  #######################################################
 #                  CPUFriendFriend                    #
#######################################################

Building CPUFriendDataProvider.
Saving to Mac-27AD2F918AE68F61.plist...
Running ResourceConverter.sh...
Compiling SSDTs...
 - ssdt_data.dsl

Done.
```
Gravou CPUFriendDataProvider.kext no Results.    
https://github.com/acidanthera/CPUFriend/releases  
Colocar o CPUFriend.kext e o CPUFriendDataProvider.kext em EFI/OC/Kexts. Insere no config.plist e reboot.  
  
Não funcionou. Peguei o CPUFriendDataProvider do 13thdemarch  
https://github.com/13thdemarch/b660m-aorus-pro-hackintosh/tree/master/EFI/OC/Kexts/CPUFriendDataProvider.kext/Contents  

Esse foi. Clock máximo foi de 3.3GHz pra 4.3GHz.  
|Geekbench 6.3.0|sem CPUFriend|com CPUFriend|
|----------:|------------:|------------:|
|Single-Core|         1643|         1866|
| Multi-Core|         6128|         7626|
  
https://github.com/stevezhengshiqi/one-key-cpufriend?tab=readme-ov-file#before-install  
  NOTE: It is recommended to disable CPUFriend.kext and CPUFriendDataProvider.kext before a macOS upgrade. You need to re-generate CPUFriendDataProvider.kext whenever you update to a new macOS version; otherwise, you may suffer from bad PM or even kernel panic.  
  Uma idéia seria tentar gerar o DataProvider sem o ID de CPU fake (Kernel->Emulate)  
  
**DMAR**  
https://dortania.github.io/Getting-Started-With-ACPI/Universal/dmar-methods/manual.html#creating-our-customized-dmar-table  
Clicar em SDDTTime/Results/ACPI/DMAR.aml vai abrir o MaciASL. Procurar por *Reserved Memory*.
```
[000h 0000   4]                    Signature : "DMAR"    [DMA Remapping table]
[004h 0004   4]                 Table Length : 00000050
[008h 0008   1]                     Revision : 01
[009h 0009   1]                     Checksum : 42
[00Ah 0010   6]                       Oem ID : "INTEL "
[010h 0016   8]                 Oem Table ID : "EDK2    "
[018h 0024   4]                 Oem Revision : 00000002
[01Ch 0028   4]              Asl Compiler ID : "    "
[020h 0032   4]        Asl Compiler Revision : 01000013

[024h 0036   1]           Host Address Width : 26
[025h 0037   1]                        Flags : 01
[026h 0038  10]                     Reserved : 00 00 00 00 00 00 00 00 00 00

[030h 0048   2]                Subtable Type : 0000 [Hardware Unit Definition]
[032h 0050   2]                       Length : 0020

[034h 0052   1]                        Flags : 01
[035h 0053   1]                     Reserved : 00
[036h 0054   2]           PCI Segment Number : 0000
[038h 0056   8]        Register Base Address : 00000000FED91000

[040h 0064   1]            Device Scope Type : 03 [IOAPIC Device]
[041h 0065   1]                 Entry Length : 08
[042h 0066   2]                     Reserved : 0000
[044h 0068   1]               Enumeration ID : 02
[045h 0069   1]               PCI Bus Number : 00

[046h 0070   2]                     PCI Path : 1E,07


[048h 0072   1]            Device Scope Type : 04 [Message-capable HPET Device]
[049h 0073   1]                 Entry Length : 08
[04Ah 0074   2]                     Reserved : 0000
[04Ch 0076   1]               Enumeration ID : 00
[04Dh 0077   1]               PCI Bus Number : 00

[04Eh 0078   2]                     PCI Path : 1E,06
```
Não tem, então não preciso fazer esse.  
  
**Overclock Radeon**  
https://github.com/5T33Z0/OC-Little-Translated/blob/main/11_Graphics/GPU/AMD_Radeon_Tweaks/Polaris_PowerPlay_Tables.md  

**Logs de boot**  
```
sudo log show --predicate "processID == 0" --start $(date "+%Y-%m-%d") --debug
```
Procura por  **=== system boot:** pra seguir a partir do boot. O log tá limpo, exceto por um erro 
>(AppleACPIPlatform) Could not install PciConfig handler for Root Bridge PC00  

Depois eu vejo o que é isso.  

Metade do log vem com \<private>. Tem uma receita aqui deixo indicado mas não precisei dos privates por enquanto https://forums.developer.apple.com/forums/thread/676706
  
**MacOS autologin**
- Apple menu  > System Preferences.
- Click on  Security & Privacy.
- Click on lock
- Untick Disable automatic login
  
**MMio**  
```
09:895 00:065 OCABC: MMIO devirt start
09:963 00:067 OCABC: MMIO devirt 0xC0000000 (0x10000 pages, 0x8000000000000001) skip 0
10:031 00:068 OCABC: MMIO devirt 0xFC000000 (0x10 pages, 0x800000000000100D) skip 0
10:101 00:069 OCABC: MMIO devirt 0xFE000000 (0x11 pages, 0x8000000000000001) skip 0
10:170 00:069 OCABC: MMIO devirt 0xFEC00000 (0x1 pages, 0x8000000000000001) skip 0
10:239 00:069 OCABC: MMIO devirt 0xFED00000 (0x1 pages, 0x8000000000000001) skip 0
10:308 00:068 OCABC: MMIO devirt 0xFEE00000 (0x1 pages, 0x8000000000000001) skip 0
10:375 00:067 OCABC: MMIO devirt 0xFF000000 (0x1000 pages, 0x800000000000100D) skip 0
10:445 00:070 OCABC: MMIO devirt end, saved 278672 KB
10:516 00:070 OCABC: Only 146/256 slide values are usable!
10:585 00:069 OCABC: Valid slides - 0-94, 205-255
```
Essa mensagem de 146/256 tem ação a tomar.  
  
**Mais links**
https://github.com/tarbaII/OpenCore-Install-Guide/blob/alderlake/config.plist/alder-lake.md
