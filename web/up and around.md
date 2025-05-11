## 🎹 Challenge Name: Up and Around
**Category**: Web  


### Investigation:
A quick look at source code of web page reveals the suspicious /img?p=xxx endpoint. We can specify our file here. Trying some random word, server responds with error trying to open some whole other filename. Trying to look at it as a cipher, i found out its ROT13.

### Directory Traversal:
Now i could read any known image file, but ofcourse, flag isn't in this directory and we had to somehow go up. Trying to use ../xxx would reflect as just xxx which meant a filter is imposed which just turns "../" to "". I thought it would check only once, and we could add it in the middle of itself, such that after filtering, side chars join to form "../". After some tries, this worked - ".....//./". It maybe longer than an efficient one, but it worked, so i would leave it at that.

### Finding flag
Now we have LFI, its just exploring files. Its an express app, so my first target was package.json as it would indicate base folder of the app. I found it at ../../package.json or `/img/?p=.....//./.....//./cnpxntr.wfba`. Now I tried looking for flag.txt in this directory, which failed, so I tried only "flag" without extension, which worked and got the flag.
