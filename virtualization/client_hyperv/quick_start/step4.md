ms.ContentId: C5DAC361-D339-4C3B-90E4-2C133BE6812E
title: Step 4: Create a Windows virtual machine from an .iso file

# Create a Windows virtual machine from an .iso file #

For this step, if you already have a .iso file for a supported operating system, you can use that. If not, you can download the .iso for [Windows 8.1 Enterprise](http://www.microsoft.com/en-us/evalcenter/evaluate-windows-8-1-enterprise). These instruction assume that your .iso file is in the Downloads directory, replace the path in the instructions with the path to your file if you have it stored somewhere else.

1. In Hyper-V Manager, click on the **Action** menu > **New** > **Virtual machine**. 
2. In the virtual machine wizard, make the following choices:

    <table>
    <tr><td>Page</td><td>Entry</td></tr>
    <tr><td>Name:</td><td>Type in <b>Windows Walkthrough VM</b></td></tr>
    <tr><td>Generation:</td><td><b>Generation 2</b></td></tr>
    <tr><td>Startup Memory:</td><td><b>1024</b> and don't select dynamic memory</td></tr>
    <tr><td>Connect virtual hard disk:</td><td><b>Create a virtual hard disk</b> (keep the other default values) </td></tr>
    <tr><td>Installation Options:</td><td><b>Install an operating system from a bootable CD/DVD-ROM</b>. Under <b>Media</b>, select <b>Image file (iso)</b> and then click <b>Browse</b> to point to the .iso file.</td></tr>
    </table>
  
3. The Summary page of the wizard should look like this:
	
	<!-- need screenshot -->
4. When everything looks right, click **Finish**. 

# Next Step: #
[Step 5: Connect to the virtual machine and finish the installation](step5.md)