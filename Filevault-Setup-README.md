I created this to be utilized by our deployment team when providing an and user a new computer. Can be heavily modified.


# Filevault-Setup-Solution-Automated


########## Filevault-Setup-Step-1 

Edit Line #5 (Message to display)

- End Filevault-Setup-Step-1 modifications - 




########## Filevault-Setup-Step-2

The script in Filevault-Setup-Step-2 was extracted from 
https://github.com/jamf/FileVault2_Scripts/blob/master/reissueKey.sh
& Slightly modified.

Edit Lines #4 & #5 with the local admin account on the machine / authorized filevault user on the account. 
(Should Be Encrypted)

- End Filevault-Setup-Step-2 modifications - 




########## Filevault-Setup-Step-3 

You don't need to do anything here, you can add a message if you'd like. This could really be added to 
Filevault-Setup-Step-2 but I split them up. Up to you.

- End Filevault-Setup-Step-3 modifications - 



###########################################
########### How It Works ##########
###########################################

The way this works is simple. 

Step 1. You must have a managed / local admin account already Filevault Enabled. 
During my provisioning process, I enable our Local Admin Account for FV2 Encryption.

Step 2. For End Users, In Jamf, Create a policy that runs once per User, Per Computer. With a trigger of your choice.
My Triggers are Log In, Network State Change, Recurring Check-In, & a Custom Event Trigger in the event we need to start it.

Step 3. Include the 3 Scripts & Run In Sequential Order.

1st. Filevault-Setup-Step-1

2nd. Filevault-Setup-Step-2

3rd. Filevault-Setup-Step-3

& Thats it.

When the user logs in, the applescript (Filevault-Setup-Step-1) will prompt them for there password, they will enter it twice, if it fails to match,
it will prompt them twice again. It will then write out to the temporary text file -> /Library/Scripts/TempFVPassword.txt.

Then the following script, Filevault-Setup-Step-2 will run, read the text file, pass the password where it needs to be in the
fdesetup command & utilize the local fv admin to allow the logged in user to become filevault encrypted.

Finally, Filevault-Setup-Step-3 runs & updates the preboot volume & restart the computer back to the login window. 
Updating preboot will guarantee the user will appear at reboot to login with FV & the restart guarantees you did it right.
