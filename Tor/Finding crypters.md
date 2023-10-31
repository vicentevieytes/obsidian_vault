Hi, I'm searching services or ppl who can crypt .net x86/x64 .net4.7(.exe).

Search:  
- on xss/exploit/bhf/whatever forum you like, not here.  
- for private stubs, avoid everything monthly paid / with shared stubs.  
- for devs with old accounts and great reputation.  
- for devs that allow middleman services.  
- only deal through forum's contracts.  
- (optional) native tools (hosted CLR / even droppers?)  
! A lot of crypters, without av killer, will only get you over the execution point, not evade runtime, as they unpack into memory the original bytes that already have signatures (so the AV will close the process).  
! Also, good emulation is more than necessary, a lot of skids ask you to not upload to xyz.com (ex: virustotal), which confirms the fact that they are bad at their job. In the wild your payload will get sent to virustotal 5 times a day with ease, and good emulation will make sure your stub isn't getting detected by their analytics tools in the first 3 days (eventually, your payload WILL become detectable, there is no way to avoid that - virustotal shares the samples with a lot of AV companies, and also each AV software itself sends copies of the downloaded file (your payload from the victim's computer) to their servers so your binary will someday get r*ped).