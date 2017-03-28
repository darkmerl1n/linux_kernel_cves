# linux_kernel_cves
This is a simple project to track CVEs in the upstream linux kernel. 
Currently all output for this is stored in the CVEs.txt file. The output
was generated automatically through a set of tools that has not been 
fully tested or made public yet. There are separate files for each LTS 
stream. These files list all CVEs that possibly affect that stream and 
when/if they were fixed.

### Reading stream reports

Below is a list of definitions for certain strings you might see in a 
stream report. **The only CVEs that should appear in the stream document 
are ones that potentially affect that stream.** (ie. ones that were not 
fixed prior to the first release version and were not introduced after 
the release version) If no fixing commit is known for a CVE, then by 
default it is assumed to present in all streams after it was introduced.

  - 'Fix unknown': No fixing commit in the commit maps or the commit is 
  invalid
  - 'Fixed with X': Fixing commit was seen in the stream and first 
  appears in version X
  - 'Fix not seen in stream': The fixing commit is known and valid, 
  but not seen in this stream (ie. stream is still vulnerable)
  
### Overview of Process

The process for generating these documents is focused on being as 
automated as possible. Below is the general outline of steps.

  1) Take list of all kernel CVEs (Currently limited to after 2004, see 
  [#3](../../issues/3)).
  2) If the issue is marked as Vendor specific, ignore it.
  3) Get the Breaking/Fixing Commits. This is retrieved from the 
  internal cache first, if not present it pulls from Ubuntu, Debian, 
  etc to try and fill that information in.
  4) Using those commit ids, get the first tags in the mainline that 
  they appear.
  5) Using that version timeline, for each stream that would be 
  vulnerable perform steps 6 through 8.
  6) Find the commit who has the commit message that matches the commit 
  message from the mainline. This is the fixing commit in that stream. 
  7) Record the commit id and get the earliest tag in the stream which 
  has that commit.
  8) Output information to stream document. 
  9) Update JSONs.

### Accuracy

This is currently autogenerated and will go through testing before any 
promises of accuracy are made. The eventual goal would be to have a
community curated list of CVEs along with when the code was introduced 
and when it was fixed.

### Development

Want to contribute? Great! Due to the vary early stages of this work the 
best places is through github issues to start discussions and requests. 
Updating the commit maps with the breaks/fixes information is also very 
helpful. File a merge request and it will accepted when the information 
is validated. If/When this idea matures, direct contributions will of 
course be accepted.

### Known Issues

  - Breaks/Fixes lists accuracy not 100% (This is slowly being corrected)
  - Multiple commits to fix a CVE not handled
  - If a commit fixes a previous incomplete fix the version timeline 
  may not be correct (Update: In progress)
  - Haven't verified if all CVEs are on the list yet (in the works)
