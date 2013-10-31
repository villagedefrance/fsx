Microsoft Flight Simulator (FSX) RESTful API and user interface to control user aircraft

Provides easy to use web technology RESTful interface to FSX via SIMCONNECT. The RESTful interface can be directly consumed by Javascript apps running within web browsers for rich user interface on any device including PC, tablets, smart phones. A touch based interface can be created to control FSX. In addition scripts and apps written in any language can also use the RESTful interface to monitor and control the user aircraft. The RESTful interface is far more convinient than writing apps directly against SIMCONNECT.

The script is two parts. The first part is an embedded c# class that is compiled to a DLL in memory everytime the script is run. This c# class is required to interface to SIMCONNECT which has types such as struct and enum that PowerShell does not have. Also the c# class uses multi-threading to poll FSX for simulation variables such as user aircraft current altitude etc. Convinient multi-threading is not yet possible with PowerShell.

Part of the c# class is dynamically generated by PowerShell based on fsx.xml file. You can put all the EVENT IDs you want to transmit to FSX to control the aircraft and all the simulation variables you want to bring back to the script in this file. Please refer to Microsoft Flight Simulator SDK,SIMCONNECT help file for a complete list and description EVENT IDs and Simulation Variables. The help file name is fsxsdk.chm; you may be able to find it online if you do not own the SDK.

The script also has a micro web server that provides HTTP RESTful interface to client programs. Client programs, including Javascript programs within web browser, can use this interface to send commands to control user aircraft and also get back Simulation Variables.

FEATURES

    * Control aircraft from tablets, smart phones, PC, Mac
    * Use multiple devices at once (no limit)
    * Very easy to develop your own control dashboards using HTML (and Javascript for advanced customization)
    * Create user interface for each aircraft, for each screen size, and then load the appropriate one
    * Load different interface on each device. For example, use tablet 1 for Auto Pilot controls, tablet 2 for Radio etc.
    * No tools required to extend and customize. All you need is Notepad

REQUIRES

    PowerShell v3 or higher, FSX SP2, .Net 4.5, fsx.xml (included with the script)

INSTALL

      1.  Copy the script fsx.ps1 and configuration file fsx.xml to any folder.
      
      2.  If you are running Windows Vista or better from an administratively privileged command prompt run the following command:
          netsh http add urlacl url=http://+:8000/ user=DOMAIN\user
          The url should exactly match the url provided to listener.prefixes.add in the script
      
          For other operating systems please refer to:
          http://msdn.microsoft.com/en-us/library/ms733768.aspx
          
      3.  Allow inbound TCP connections to port 8000 from your network in Windows firewall
      
      4.  If you are running 64 bit OS, start the 32 bit (x86) version of PowerShell with administrative privileges.     
          if you are running 32 bit OS, start PowerShell with administrative privileges
          Execute the following command on PowerShell prompt:
          
          set-executionpolicy bypass
          
          This will allow 32 bit PowerShell scripts to run on your system unrestricted. If you get access denied error
          while running this command you are not running PowerShell in administratively privileged mode

USAGE

Start FSX. Now run the script. On 64 bit OS use the 32 bit (x86) version of PowerShell. This is because SIMCONNECT library and FSX are 32 bit apps. From a command prompt change to folder where script is located and then type:
    
    powershell .\fsx.ps1
    
If all goes well you will see "Listening ..." on the console
    
To test the RESTful interface you can use the Chrome browser with the "Simple REST Client" plugin available free from Chrome market place. On the computer where the script is running start Chrome, and start the Simple REST Client:
    
    URL: http://localhost:8000/getall
    Method: GET

Press Send.

You should see all the simulation variable values such as altitude of user aircraft etc. You can add or remove simulation variables (there are hundreds) in the fsx.xml file. You will have to restart the script whenever you make changes to the fsx.xml file.

You can also send a command to FSX from Chrome Simple REST Client.

    URL: http://localhost:8000/cmd
    Method: POST
    Headers: Content-type: application/json (the space between : and application is important)
    Data: {"cmd":"AUTOPILOT_ON"}

Press Send. The Autopilot on your aircraft should be engaged. To turn it off send AUTOPILOT_OFF. Look at the fsx.xml event IDs to see what commands you can send. You can add more events to fsx.xml.

You can test connection from other computers as well. Instead of localhost use the IP address of the computer running the script. You can now write programs in any language on any computer to control FSX. You can also write Javascript programs to run within browser so that you can have rich user interface. Sample browser apps will be provided in the near future.