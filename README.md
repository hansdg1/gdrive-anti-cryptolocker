# Google Drive CryptoLocker restore script

## Introduction

One of our colleges got hit today by the CryptoLocker virus, some of the affected files were synced to Google Drive. Thankfully Google Drive stores all versions of modified files, so an older, unencrypted version can be restored. The virus encrypted about ~1000 files on his Google Drive, so we've written a script to programmatically restore them all.

One of my friends got hit today by a CryptoLocker variant dubbed Osiris, and some of the affected files were synced to Google Drive. Thankfully Google Drive stores all versions of modified files, so an older, unencrypted version can be restored. The ransomware encrypted hundreds files on his Google Drive, so we've written a script to programmatically restore them all.

I found this script and forked it to work.

## Requirements / Usage
This script can be executed via [Google Apps Scripts](https://www.google.com/script/start/). You have to enable those, as well as [Advanced Drive Services](https://developers.google.com/apps-script/advanced/drive) for accessing file revisions. After that you can save the decryptr.gs into your Google Drive using the Webinterface and just run the `decryptr` function. You can now take a look at your log file, if those look good enough you can switch the `dry` switch to `false` to really recover those files.

# How it works
This script was designed to crawl through your whole Google drive, removing all revisions of files having ~~`.enc`~~ `.osiris` as file ending. Afterwards it gets the file's original filename from the revision history postfix from them (the filename is not getting restored when removing the revisions).

By default, the script crawls through all folders in your Google Drive and looks for files ending with `.osiris`. When it finds one, it removed the encrypted versions of that fileId. Originally this script renamed the file by just stripping the file extention from the filename. Unfortunately, that did not work for me because the malware actually renamed each file to a random alphanumeric string. Ex: 78E360E8--0DB1--28FD--F7525F85--D7DE3870A52F.osiris

Fortunately the file's Revision History keeps filename changes, and so I had to find a way to use that.

I know there are likely a lot of inefficiencies in this script, and if you have a lot of files in your Drive, you might have to restrict the search to a particular folderId. (See near the top of the `decryptr` function.)
