# AutoHotkey V2 Wrapper for Monitor Configuration WinAPI Functions

V1 GitHub Repo: https://github.com/jNizM/Class_Monitor

V1 AHK Forum Thread: https://www.autohotkey.com/boards/viewtopic.php?f=6&t=7854



```autohotkey
; =================================================================================================== ;
;
; [EXAMPLES AND DOCUMENTATION]
;
;
; NOTE:
; All examples assume you have:
; 
;	(1) Included the Monitor Class within your script with or without 
;		using the #Include directive:
;
	#Include "Monitor Class.ahk"
; 
;	(2) created a new class instance already in your script by using:
; 
	mon := Monitor.New() ; Create new class instance
;
;
; =================================================================================================== ;	
	
; =================================================================================================== ;	
;
; ================= 
; == GET METHODS == 
; ================= 
;
;
; NOTE:
; All Get___() Methods in this class will function identically with the following 
; examples and documentation shown for the GetBrightness() Method (affecting their 
; respective setting; e.g. "Contrast", "Red Gain", etc.), with the exception of the 
; following Methods:
; 

	GetInfo()
	GetGammaRamp()
	GetVCPFeatureAndReply()	
	CapabilitiesRequestAndCapabilitiesReply()	
	GetCapabilitiesStringLength()	
	GetCapabilities()
	GetSupportedColorTemperatures()
	GetColorTemperature()
	GetTechnologyType()


; GetBrightness() Method:
;
	GetBrightness([Display])
;
; Returns a Map object with details about a monitor's Brightness Level:
; the Current Value, as well as the Minumum Value and Maximum Value supported 
; by the monitor. Typically, this is a value range of [0 to 100], with [0]  
; being the lowest value supported and [100] being the highest value supported.	
; 
; [PARAMETERS]
;
;	DISPLAY (Optional, defaults to primary monitor)
;		Type:		 Integer or "String"
;		Description: The number or name of the monitor to query. If omitted,
;						defaults to primary monitor.
;       Example:     Integer: 2 
;                    String : "\\.\DISPLAY2"
;
;
; A visualized structure of the Map object with key-value names:
;
;    -MAP{ key	           -value
;		  -["Minimum"]-MinimumValue   
;		  -["Current"]-CurrentValue  
;		  -["Maximum"]-MaximumValue  
;		  }
;
;	
; Example 1 - Return the Current Value, Minimum Value, and Maximum Value of
;			Brightness supported by the primary monitor:
	
	InfoList := ""
	Brightness := mon.GetBrightness()
	for k, v in Brightness
		InfoList .= k ":`n" v "`n`n"
	
	MsgBox InfoList
;	
; Example 2 - Return and display the Minimum Value of Brightness supported  
;			by monitor #1:

	MsgBox MinBrightnessLevel := mon.GetBrightness(1)["MinimumValue"]
	
; Example 3 - Return and display the Current Value of Brightness of
;			a specific monitor ("\\.\DISPLAY1"):
	
	MsgBox CurrentBrightnessLevel := mon.GetBrightness("\\.\DISPLAY1")["CurrentValue"]
;	
;
;   NOTE:
;   The following Methods will function identically with the above
;   documentation and examples shown for GetBrightness(), with the  
;   exception of instead affecting their respective setting  
;	(e.g. "Contrast", "Red Gain", "DisplayAreaWidth", etc.):
;
	GetContrast()
	GetRedDrive()
	GetGreenDrive()
	GetBlueDrive()
	GetRedGain()
	GetGreenGain()
	GetBlueGain()
	GetDisplayAreaWidth()
	GetDisplayAreaHeight()
	GetDisplayAreaPositionHorizontal()
	GetDisplayAreaPositionVertical()
;
; =================================================================================================== ;
	
	
; =================================================================================================== ;	
;
; GetInfo() Method:
;
	GetInfo()
;
; Returns an array with nested Map object containing info for each monitor found: 
; Handle, Name, Number, X & Y coordinates for total display area and work area. 
; The value of key["Primary"] is true (i.e "1") if it is the Primary monitor
; and is false (i.e. "0") if it is not the primary monitor.
;
; A visualized structure of the Array objeict with key-value names:
;
; -ARRAY[key-value
;		-[1]-MAP{ key		 -value
;				-["Handle"]  -Handle  
;				-["Name"]    -Name  
;				-["Number"]  -Number  
;				-["Left"]    -Left  
;				-["Top"]     -Top  
;				-["Right"]   -Right 
;				-["Bottom"]  -Bottom  
;				-["WALeft"]  -WALeft  
;				-["WATop"]   -WATop  
;				-["WARight"] -WARight 
;				-["WABottom"]-WABottom
;				-["Primary"] -Primary
;				}
;	    -[n]-MAP{ key		 -value
;				-["Handle"]  -Handle  
;				-["Name"]    -Name  
;				-["Number"]  -Number  
;				-["Left"]    -Left  
;				-["Top"]     -Top  
;				-["Right"]   -Right 
;				-["Bottom"]  -Bottom  
;				-["WALeft"]  -WALeft  
;				-["WATop"]   -WATop  
;				-["WARight"] -WARight 
;				-["WABottom"]-WABottom
;				-["Primary"] -Primary
;				}
;		]		
;
;	
; Example 1 - Return all info found for all monitors found:	
		
	AllInfo := ""
	MonitorInfo := mon.GetInfo()
	for MonitorNumber, InfoMap in MonitorInfo
		for InfoName, InfoValue in InfoMap
			AllInfo .= InfoName ":`t`t" InfoValue "`n"
	
	MsgBox AllInfo
	
; Example 2 - Identify which monitor # is the Primary monitor
	
	for Info in mon.GetInfo()
		if Info["Primary"]
			MsgBox "Your system's Primary Monitor is Monitor #" A_Index "."
	
; Example 3 - Retreive handle to specified monitor # (Monitor #1 in this example)
	
	MsgBox mon.GetInfo()[1]["Handle"]
;
; =================================================================================================== ;


; =================================================================================================== ;
; GetGammaRamp() Method:
;
	GetGammaRamp([Display])
;
; Returns a Map object with details about a monitor's current Gamma Color   
; Output Levels:
; Red gamma, Green gamma, and Blue Gamma. These are within a value range 
; of [0 to 100], with [0] being the lowest value supported and [100] 
; being the highest value supported.	
; 
; [PARAMETERS]
;
;	DISPLAY (Optional, defaults to primary monitor)
;		Type:		 Integer or "String"
;		Description: The number or name of the monitor to query. If omitted,
;						defaults to primary monitor.
;       Example:     Integer: 2 
;                    String : "\\.\DISPLAY2"
;
;
; A visualized structure of the Map object with key-value names:
;
;     -MAP{ key	    -value
;		  -["Red"]  -RedGamma   
;		  -["Green"]-GreenGamma 
;		  -["Blue"] -BlueGamma
;		  }
;
;	
; Example 1 - Return the Current Red, Green, and Blue gamma color output 
;			levels of the primary monitor:
	
	InfoList := ""
	Gamma := mon.GetGammaRamp()
	for k, v in Gamma
		InfoList .= k ":`n" v "`n`n"
	
	MsgBox InfoList
;	
; Example 2 - Return and display the Minimum Value of Brightness supported  
;			by monitor #1:

	MsgBox RedGammaLevel := mon.GetGammaRamp(1)["Red"]
	
; Example 3 - Return and display the Current Value of Green gamma color 
;			output for a specific monitor ("\\.\DISPLAY1"):
	
	MsgBox GreenGammaLevel := mon.GetGammaRamp("\\.\DISPLAY1")["Green"]
;	
; =================================================================================================== ;


; =================================================================================================== ;	
;
; GetVCPFeatureAndReply() Method:
;
	GetVCPFeatureAndReply(VCPCode[, Display])
;
; Returns a Map object with details about a monitor's VCP Code (if supported):
; the VCP Code queried, VCP Code Type, Current Value and Maximum Value.	
; 
; [PARAMETERS]
;
;	VCPCODE
;		Type:		 "String"
;		Description: A specific VCP code to query, which can be found at 
;						https://milek7.pl/ddcbacklight/mccs.pdf.
;       Example:    Integer (decimal)    : 2
;                   Integer (hexidecimal): 0xD6
;
;	DISPLAY (Optional, defaults to primary monitor)
;		Type:		 Integer or "String"
;		Description: The number or name of the monitor to query. If omitted,
;						defaults to primary monitor.
;       Example:     Integer: 2 
;                    String : "\\.\DISPLAY2"
;
;
; A visualized structure of the Map object with key-value names:
;
;     -MAP{ key	           -value
;		  -["VCPCode"]	   -VCPCode  
;		  -["VCPCodeType"] -VCPCodeType  
;		  -["CurrentValue"]-CurrentValue  
;		  -["MaximumValue"]-MaximumValue  
;		  }
;
;	
; Example 1 - Return and display the details about the Power Mode of a specific
;			monitor ("\\.\DISPLAY1") and it's associated VCP Code:
;	
	InfoList := ""
	VCPInfo := mon.GetVCPFeatureAndReply(0xD6, "\\.\DISPLAY1")
	for k, v in VCPInfo
		InfoList .= k ":`n" v "`n`n"
	
	MsgBox InfoList

;	A Current Value of 1   = ON
;	A Current Value of 2-4 = STANDBY / SUSPENDED / OFF
;	A Current Value of 5   = POWER OFF
;	
; Example 2 - Return and display the Minimum Value of Luminance (Brightness) 
;			supported by the primary monitor:

	MsgBox MaxBrightnessLevel := mon.GetVCPFeatureAndReply(0x10)["MaximumValue"]
	
; Example 3 - Return and display the Current Value of the Contrast of
;			monitor #2:
	
	MsgBox CurrentContrastLevel := mon.GetVCPFeatureAndReply(0x12, 2)["CurrentValue"]
;	
; =================================================================================================== ;


	
; =================================================================================================== ;	
;
; ================= 
; == SET METHODS == 
; ================= 
;
;
; All Set___() Methods in this class will function identically with the following 
; examples and documentation shown for the SetBrightness() Method (affecting their 
; respective setting; e.g. "Contrast", "Red Gain", etc.), with the exception of the 
; following Methods:

	SetVCPFeature()	
	SetColorTemperature()


; SetBrightness() Method:
;
	SetBrightness(Brightness[, Display])
;
; Sets a monitor's Brightness level, then returns the monitor's updated
; Brightness Level.
;
; [PARAMETERS]
;
;	BRIGHTNESS
;		Type:		 Integer
;		Description: The new Brightness Level to set the monitor to.
;       Example:     Integer: 50 
;
;	DISPLAY (Optional, defaults to primary monitor)
;		Type:		 Integer or "String"
;		Description: The number or name of the monitor to query. If omitted,
;						defaults to primary monitor.
;       Example:     Integer: 2 
;                    String : "\\.\DISPLAY2"
;
;	
; Example 1 - Set, return, and display new brightness level (50) of primary 
;           monitor:
;				
	MsgBox mon.SetBrightness(50)
;	
; Example 2 - Set, return, and display new brightness level (100) of  
;           monitor #1:

	MsgBox mon.SetBrightness(100, 1)
	
; Example 3 - Set, return, and display new brightness level (0) of  
;			a specific monitor ("\\.\DISPLAY1"):
	
	MsgBox mon.SetBrightness(0, "\\.\DISPLAY1")
;	
;
;   NOTE:
;   The following Methods will function identically with the above
;   documentation and examples shown for GetBrightness(), with the  
;   exception of instead affecting their respective setting  
;	(e.g. "Contrast", "Red Gain", "DisplayAreaWidth", etc.):
;
	SetContrast()
	SetRedDrive()
	SetGreenDrive()
	SetBlueDrive()
	SetRedGain()
	SetGreenGain()
	SetBlueGain()
	SetDisplayAreaWidth()
	SetDisplayAreaHeight()
	SetDisplayAreaPositionHorizontal()
	SetDisplayAreaPositionVertical()
;
; =================================================================================================== ;
	
	
; =================================================================================================== ;
; SetGammaRamp() Method:
;
	SetGammaRamp([Red, Green, Blue, Display])
; 
; Sets the Red, Blue, and Green Gamma Color Output Levels for a monitor,
; within a range of [0 to 100], then returns a Map object with the 
; updated Gamma Color Output Levels.
;
;	PRO TIP: Increasing / decreasing all Gamma Color Output Levels by the  
;	same amount will mimic the native backlight brightness found on laptops
;	by reducing / increasing screen brightness via RGB light output. 
;	NOTE: this is DIFFERENT than the brightness level adjusted by the 
;	SetBrightness() Method, which may not be supported on laptops.
;
;	Example: 
    SetGammaRamp() ; sets primary monitor to full gamma color output (100%) for all colors
    SetGammaRamp(70, 70, 70) ; minus 30 for all colors mimics native backlight brightness reduction
    SetGammaRamp(50, 50, 50) ; minus 20 for all colors mimics native backlight brightness reduction
    SetGammaRamp(90, 90, 90) ; plus  40 for all colors mimics native backlight brightness increase
;
;   ***WARNING*** USE WITH CAUTION
;   Exercise extreme caution with this Method when setting ALL GammaRamp 
;   values to 0, as this will cause complete screen blackout. Without 
;   measures in place to increase the GammaRamp values after setting them 
;   to 0, your monitor will be stuck blacked out.. worst case scenario 
;   would be doing this to all monitors on your computer and even if you 
;   restart ;your computer, it may boot with 0% color output AKA complete 
;   blackout. SEE EXAMPLE 4 BELOW AND SERIOUSLY CONSIDER PUTTING A CUSTOM 
;   SAFETY MEASURE IN PLACE!!
;
;   Additionally, setting GammaRamp values outside of the [0 to 100] range 
;   (not recommended) may cause some extreme colors on your monitor where 
;   you are unable to see anything well enough to get your monitor back to 
;   normal.
;
;   For added safety, there is a safety check which doesn't allow the user
;   to set Gamma color output values outside of the [0 to 100] range.
;   This can only be altered by removing / altering the ternary operator 
;   check found in the GammaSetting() Method.
;
; [PARAMETERS]
;
;	RED (Optional, defaults to 100)
;		Type:		 Integer
;		Description: The new value of Red Gamma Color Output Level to set 
;						a monitor to. If omitted, defaults to a value of   
;                       100 (typically the default value for monitors).
;       Example:     Integer: 90 
;
;	GREEN (Optional, defaults to 100)
;		Type:		 Integer
;		Description: The new value of Green Gamma Color Output Level to set 
;						a monitor to. If omitted, defaults to a value of   
;                       100 (typically the default value for monitors).
;       Example:     Integer: 0 
;
;	GREEN (Optional, defaults to 100)
;		Type:		 Integer
;		Description: The new value of Blue Gamma Color Output Level to set 
;						a monitor to. If omitted, defaults to a value of   
;                       100 (typically the default value for monitors).
;       Example:     Integer: 50 
;
;	DISPLAY (Optional, defaults to primary monitor
;		Type:		 Integer or "String"
;		Description: The number or name of the monitor to query. If omitted,
;						defaults to primary monitor.
;       Example:     Integer: 2 
;                    String : "\\.\DISPLAY2"
;
;
; A visualized structure of the Map object with key-value names:
;
;     -MAP{ key	    -value
;		  -["Red"]  -RedGamma   
;		  -["Green"]-GreenGamma 
;		  -["Blue"] -BlueGamma
;		  }
;
;	
; Example 1 - Set, return, and display the new Red, Green, and Blue gamma
;           color output levels of the primary monitor all to 100%:
	
	InfoList := ""
	Gamma := mon.SetGammaRamp()
	for k, v in Gamma
		InfoList .= k ":`n" v "`n`n"
	
	MsgBox InfoList
;	
; Example 2 - Set and return the new Red, Green, and Blue gamma color
;           output levels for monitor #1 to 50%, 20%, 0%, respectively.
;           Then, display the updated Blue gamma color output level:

	MsgBox BlueGammaLevel := mon.SetGammaRamp(50, 20, 0, 1)["Blue"]
	
;   NOTE:
;   Setting a Gamma color output value to 0% effectively stops your 
;   monitor from emitting that color. In this example, all Blue light
;   is "blocked", creating a Blue Light Blocker for your computer,
;   which has been known to help developers like us with eye strain.
;
;
; Example 3 - Set and return the new Red, Green, and Blue gamma color
;           output levels for a specific monitor ("\\.\DISPLAY1") to 
;           50%, 0%, 0%, respectively. Then, display the updated 
;           Green gamma color output level:
	
	MsgBox GreenGammaLevel := mon.SetGammaRamp(50, 0, 0, "\\.\DISPLAY1")["Green"]
;	
; Example 4 - In case you set your monitor to a point where it is unusable,
;             it is wise to have a couple backup measures in place:
;
;   Consider having an emergency hotkey
    !x::
    {        
        monitorCount := mon.GetInfo().Length ; Retreive total monitor count
        Loop( monitorCount )
            mon.SetGammaRamp( , , , A_Index)
    }

;   ...or an OnExit() function
    OnExit("ResetGammaOutput")
    ResetGammaOutput(*){

        monitorCount := mon.GetInfo().Length
        Loop( monitorCount ) 
            mon.SetGammaRamp( , , , A_Index)
            
    }
;
; =================================================================================================== ;
```
