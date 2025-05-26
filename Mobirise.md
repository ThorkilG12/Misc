The windows user "Bruger" got the install file from Mobirise (Ver 6.0.5) and it was installed with defaults.
Language choosen was English and "don't ask again"
Installed in "C:\Program Files (x86)\Mobirise"
Appdata has this: "C:\Users\bruger\AppData\Roaming\Mobirise"

By default a site called "My Site" exists after installation.
I change a bit in the Header for the Site, and du "Publish"
I choose "Local Drive Folder" and the name is "MyMobiFolder". AFter that, this folder exist "C:\Users\bruger\Documents\MyMobiFolder" and index.html and other files and fiolders was created inside.

When doing a "Preview in Browser" this was created with the whole project: "C:\Users\bruger\AppData\Local\Temp\Mobirise\20250526_163301\"

Now I try to publish to "C:\Users\bruger\Documents\Mobirise\www_ghkurser_dk" even though the folder \Mobirise\ does not exist
Mobirise creates both folders: \Mobirise\www_ghkurser_dk

New sites that you create within mobirise by just giving the site a name, goes into here:
"C:\Users\bruger\AppData\Local\Mobirise.com\Mobirise\projects" with a foldername like:
"\project-2025-05-26_202311"

If you have published to local folder on another pc, then copy the folder with the site. It HAS TO contain the project.mobirise file. If not, you have a problem. The .mobirise is essential for the import to work.
The do a "Create new site or import" and point to the folder where you have the project.mobirise file.

That should work
