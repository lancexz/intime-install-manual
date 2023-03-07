# INtime Installation

## Config Wins 10
Enter BIOS and disable the following options:
```
Settings -> Security 	-> Secure Boot					   
                       	-> Boot -> Fast Boot
Features -> Advanced CPU Configuration 	-> Hyper-Threading
            							-> Intel C-State
            							-> Intel Turbo Boost
										-> Inter Turbo Boost Max Technology 3.0
                                        -> Intel Speed Shift Thchnology                            
Save and restart
```


Control Panel

```
Programs -> Turn Windows features on or off -> .NET Framework 4.8 Advanced Services -> Enable all -> Apply changes
System and Security -> Administrative Tools -> Services -> Windows update(double click) -> Disable
					-> Windows update(rightclick) -> Properties -> Recovery -> Reset fail count after: 9999
																			-> First/Secoun/Subsequent failure(s): Take No Action
					-> Power Options -> Change settings for the plan: Balanced -> both change to 'Never' -> Save changing
									 -> System Settings -> Disable the 'Turn on fast startup(recommanded)', 'Sleep', 'Hibernate', 'Lock'
														-> button behaviors: both change to 'Do nothing'

```

Browser search
```
Win10Rcap -> Download and install
VS 2019 Community -> Download -> Run as administrator -> Select 'Desktop development with C++' and '.NET desktop development' -> Install
```

Uninstall Windows Defender
```

```