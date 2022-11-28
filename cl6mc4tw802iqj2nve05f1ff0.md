# SQL DB Backup using PowerShell GUI - Tutorial

## Into
I have non-technical users in my company that must initiate a SQL backup at least once per month during a critical business process of printing millions of dollars worth of checks. We must make a backup prior to the start and then again immediately afterwards. 

My predecessor had trained and taught the staff to utilize SQL Server Management Studio (SSMS) and to execute a maintenance plan that performed the backup. This process worked, however, 🚩 it always bothered me that non-technical staff were accessing SSMS where great harm could be done to the production database. While the chances of that happening were low, the risk was still there, and only trained database administrators should be accessing SSMS. 

## Project Idea
I wanted to make a quick and simple way for the staff to create the SQL backup without having to access SSMS. There are plenty of options such as 3rd party tools, PowerShell SQL commands, command prompt scripts, etc.  The goal was for the staff to have a 1-click backup capability and to be able to retrieve the date/time of the last backup. 

I first began to write a PowerShell Script that used invoke commands to execute the predefined maintenance plan. This worked and created a backup, or at least started the job. The problem was the user experience, the user did not get helpful error messages or any indication whether the job actually finished, nor a way to monitor progress since they cannot continue their job function until the backup is complete. 

### DBATools
After doing a bit of research, I stumbled upon DBATools, (https://dbatools.io/) which offers a PowerShell module with built in commands for creating the backup, but also provides progress and feedback to the user. 

### PowerShell GUI
In my experience, end users are not comfortable running scripts within PowerShell or Comment Prompt, any time they see the window open, they get nervous. End users prefer a graphical user interface with easy to click options to simply the process. 

PowerShell GUI is quite powerful, reminiscent of creating windows forms applications in C#. You can add buttons, labels, progress bars, drop down menus, combo boxes, and so much more.  

## Solution
PowerShell GUI + DBATools = Amazing User Experience!

I was able to combine PowerShell GUI features to add labels and buttons to a form dialog which allows users to create a simple 1-click backup of the databases, and a 1-click button to view the last backup date/time 

![PowerShell graphical user interface with two buttons; view last backup, and start. Also labels indicating the last backup date](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/yx00kcrvv0o2eh95suxk.png)

## Tutorial 
This project will require administrative access on your local system and the ability to install PowerShell modules, at least in current user scope. I recommend that you type all of these commands instead of Copy and Paste since your memory retention will be much better if you get used to typing these commands. 

### Assumptions
- You have some familiarity with PowerShell
- You have access to SQL Server Management Studio 
- Your Windows Account has access to run SQL Server Management Studio commands
- You have administrative privileges on your workstation to install PowerShell Modules

### DBATools Install
We will be installing DBATools.io using PowerShell Gallery and a simple install-module command. The [DBATools.io](https://dbatools.io/download) website provides additional installation instructions should you need alternative installation or additional information. 

### One time install from Administrative PowerShell Window:
```PowerShell
Install-Module dbatools
```
- You will need to allow NuGet provider, if this is your first time installing a PowerShell module. 
- You will also need to indicate you trust the repository

![PowerShell window showing the installation steps and input from running install-module dbatools](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/8l7jslwnx1a3vlj3g11t.png)
 
### Test if the install was successful:
To test if the install was successful we are going to try to import the module. If the installation was successful the prompt should simply return to a new line, if the installation was not successful then you will get an error message and you may need to repeat the steps or try another method. 

### One time install:
```PowerShell
Import-Module dbatools
```

![PowerShell window with above command ran](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/rnzuzce4t9w3eg39ub6m.png)
 
## PowerShell ISE
For the rest of this tutorial we will be utilizing PowerShell ISE which allows you to view and test your code in a single window. 
Note: If you do not have the script pane at the top, you can press 'CTRL + R' on Windows or go to View -> Script Pane at the top. 

### Build basic form
We are going to build our basic form and ensure we can get information to display. 

Type the following information into your Script Pane which is the upper portion of your PowerShell ISE window with line numbers. This will give us a blank square window that's 300px by 300px and can re-size automatically if needed. 

The final line is the most critical, if you do not tell the form to show up `$main_form.ShowDialog()` then nothing will happen when you run the script. I kept this at the bottom since it will always be our last line of code in this whole script. 

```PowerShell
#Setup Form
Add-Type -assembly System.Windows.Forms
Add-Type -AssemblyName System.Drawing
$main_form = New-Object System.Windows.Forms.Form
$main_form.Text ='DB Backup Program'
$main_form.Width = 300
$main_form.Height = 300
$main_form.AutoSize = $true

#### Show Dialog
$main_form.ShowDialog()
```

![Basic Form](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/343algyxa9oju3wp7bfe.png)

### Adding Labels
We are going to add 4 labels
1. Get Backup History - for our View Last Backup button
2. Last Backup Title- will show next to the last backup label that will be updated by the script
3. Last Backup - will show 'unknown' or the last backup date/time
4. Run Backup - for our Start button
5. Error - to show any errors on the screen for the user 

When adding labels, you must define a variable for the label, give it some text, give it a location on the GUI, and then tell the form to add the control. 

 Add the following labels to your script pane, below `$main_form.AutoSize = $true` and above `$main_form.ShowDialog()`

```PowerShell
### ... main form content

######## Add content to form
## Last Backup Label for the View Last Backup button
$LastBackup_Label = New-Object System.Windows.Forms.Label
$LastBackup_Label.Text =  "Get Backup History"
$LastBackup_Label.Location = New-Object System.Drawing.Point(10, 20)
$main_form.Controls.Add($LastBackup_Label)

## Last Backup Title Text Label
$LastBackupTest_LabelTitle = New-Object System.Windows.Forms.Label
$LastBackupTest_LabelTitle.Text =  "Last Backup:"
$LastBackupTest_LabelTitle.Location = New-Object System.Drawing.Point(10, 50)
$main_form.Controls.Add($LastBackupTest_LabelTitle)

## Last Backup Text Label - will be updated by this program to show last date/time of backup
$LastBackupTest_Label = New-Object System.Windows.Forms.Label
$LastBackupTest_Label.Text =  "Unknown"
$LastBackupTest_Label.Location = New-Object System.Drawing.Point(120, 50)
$main_form.Controls.Add($LastBackupTest_Label)

## Error Label
$Error_Label = New-Object System.Windows.Forms.Label
$Error_Label.Text =  ""
$Error_Label.Location = New-Object System.Drawing.Point(10, 120)
$main_form.Controls.Add($Error_Label)

## DB Backup Label for Start button
$Label = New-Object System.Windows.Forms.Label
$Label.Text = "Run Backup"
$Label.Location  = New-Object System.Drawing.Point(10,80)
$Label.AutoSize = $true
$main_form.Controls.Add($Label)

###.... bottom
#### Show Dialog
$main_form.ShowDialog()
```

### Adding Buttons
We are going to add 2 buttons to the GUI. There will be a button to get the last backup and there will be a button to start a backup. I will explain what each button will be doing. Each button must be defined as a variable and given text, size, and a location. I will also be defining the background color though that's optional. 

Type the following in your script pane below the labels but above the `$main_form.ShowDialog()`

```powershell
### .... labels
# Last Backup Button
$LastBackupButton = New-Object System.Windows.Forms.Button
$LastBackupButton.Size = New-Object System.Drawing.Size(120,23)
$LastBackupButton.Location = New-Object System.Drawing.Size(200,20)
$LastBackupButton.Text = "Get Last Backup"
$LastBackupButton.BackColor = 'LIGHTBLUE'
$main_form.Controls.Add($LastBackupButton)

## Start Button
$StartButton = New-Object System.Windows.Forms.Button
$StartButton.Size = New-Object System.Drawing.Size(120,23)
$StartButton.Location = New-Object System.Drawing.Size(200,80)
$StartButton.Text = "START"
$StartButton.BackColor = 'GREEN'
$main_form.Controls.Add($StartButton)

#### Show Dialog
$main_form.ShowDialog()
```
### Test GUI
Let's run the script to make sure that our content is showing correctly. 

![PowerShell GUI with 3 labels and 2 buttons, one light blue that says Get Last Backup and the other green and says Start](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/pn8ko7x2s8a10y54jd65.png)
 
### Adding Click Events
Right now our buttons are doing nothing, because we haven't told them what to do yet. Let's add functionality to the buttons. We will do these separately since they are running different functions. 

#### adding some variables
Let's add some variables that we will use in the next steps.  These values will be different for your setup for the Server Name and the Databases being backed up. Type these below the button code but above the ShowDialog. 

```powershell
## Variables
$db = Start-Job -ScriptBlock {Import-Module dbatools}
$server = 'SERVER NAME'
$databases = @('DB_1','DB1_Audit')
```

#### Get Last Backup Button Action
This button will comprise of a button click event which will run a function and if successful it will return the last backup. We need to define the 'Add-Click()' event with a try/catch/finally block. We will also make visual changes such as button color, button text, to indicate to the end user that the program is running. 

```powershell
 ## LastBackupButton Click Action Event - VIEW LAST BACKUP
$LastBackupButton.Add_Click( { 
try {

    ## We are going to indicate to the user the script is running by changing the button
    ## background color to yellow and the text to 'Processing...'. 
    $LastBackupButton.BackColor = 'YELLOW' 
    $LastBackupButton.Text = "Processing..." 
    # to get the update to show on the form we must tell the form application to update from the events
    [System.Windows.Forms.Application]::DoEvents()
    #Let's now run our getLastBackup function 
    getLastBackup
    }
    catch
    {
        #if an error happens we are going to let the user know
    $Error_Label.Text = "There was an error"
    [System.Windows.Forms.Application]::DoEvents()

    }
    finally {
        ### if the try block was successful then let's update the button back to normal so the user knows the program is done
    $LastBackupButton.Text = "View Last Backup"
    $LastBackupButton.BackColor = 'LIGHTBLUE' 
    [System.Windows.Forms.Application]::DoEvents()  
    }
} )

function getLastBackup {
    # we are going to update the label text by running the dbatools Get-DbaDbBackupHistory command on our databases
    # we only want the end date/time so we are going to extract just that information 
    $LastBackupTest_Label.text = Get-DbaDbBackupHistory -SqlInstance $server -Database $databases[0] -Last -DeviceType Disk | foreach {$_.End}
    # to get the label to update we must tell the form application to update from the text event
    [System.Windows.Forms.Application]::DoEvents()

}

###.... bottom
#### Show Dialog
$main_form.ShowDialog()
```

#### Testing
Let's run the script and click View Last Backup button. 

_Note: This will only work if you have updated the variables for your environment and your user account has access to SQL, which is outside the scope of this tutorial._

![PowerShell GUI application showing yellow processing button](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/e68fcb7z9bsa7zt9pv5m.png)

#### Start Backup Click Action
Now let's implement the Start Backup Click Action which will use DBATools `BackupDbaDatabase` command to initiate a backup. This tutorial will save the backup to a local disk, however it's advised your SQL backups be stored somewhere other than local server. _The path must also exist._ 

The Start Backup is going to update the start button to indicate to the user that it's running. This process can take 5-10 minutes or longer so we want the user to wait until it's done. This will also call the View Last Backup button's function to update the last backup label with the most recent backup which was the one just performed. 

```powershell

## StartButton Click Action Event - START BACKUP
$StartButton.Add_Click( { 
try {
    ## Added to show end user that it's running by changing background
    ## to yellow and text to 'Running...' since this can take 5+ minutes
    $StartButton.BackColor = 'YELLOW' 
    $StartButton.Text = "Running..." 
    # tell form to update
    [System.Windows.Forms.Application]::DoEvents()
    # run startBackup function
    startBackup
}
catch {
    # If there is an error let's tell the user there was an error
    $Error_Label.Text = "error in application"
    [System.Windows.Forms.Application]::DoEvents()
}
finally {
    ## run getLastBackup function to update label with the backup that was just done
    getLastBackup

    ## set start button back to default 
    $StartButton.Text = "START"
    $StartButton.BackColor = 'GREEN'
    [System.Windows.Forms.Application]::DoEvents()
}
 } )

function startBackup {
ForEach($db in $databases){
                ## local variables so we can add date & time to backup file name
                $date = Get-Date -Format 'yyyyMMdd'
                $time = Get-Date -Format 'HHmmss'
                # set local path variable Note: this path must exist
                $path = 'E:\SQL\DBBackup\' + $server + '\' + $db + '\'
                # set file name
                $fileName = $server + '_' + $db + '_FULL_' + $date + '_' + $time + '.bak'
                try {
                    Backup-DbaDatabase -SqlInstance $server -Database $db -Path $path -FilePath $fileName -CompressBackup -IgnoreFileChecks
                    }
                catch 
                {
                    # write output to console screen which will show behind GUI when running
                     Write-Host "Error: $($_.Exception.Message)"
                }
}
}

###.... bottom
#### Show Dialog
$main_form.ShowDialog()
```
![PowerShell GUI showing backup is running, start button changes from green to yellow and from start to running](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/ercc9v1gkcssq0mkqep1.png)


## Conclusion
This was a fairly simple example of the PowerShell GUI capabilities and DBATools to perform a simple query on the last backup and a backup of the database.

The contents of this tutorial were modified from the production version of the script to show case a more simple version. The production version uses AES256 encryption via certificate and stores the backup on a different server. It also has cleanup capabilities to remove old backups after they are no longer needed. Additional error handling was also configured to show the output of the error in the bottom of the window instead of just indicating an error occurred. I encourage you to explore adding additional functionality if you use something like this in a business setup. 
 

 I’m on @buymeacoffee. If you like my work, you can buy me a taco and share your thoughts 🎉🌮
<a href="https://www.buymeacoffee.com/ChrisBenjamin" target="_blank">![Image description](https://dev-to-uploads.s3.amazonaws.com/uploads/articles/x324kww91qsmaaysxss8.png)
 </a>
