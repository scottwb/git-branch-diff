git-branch-diff
===============

The git-branch-diff tool is a simple script for performing a directory compare of the current state of the working tree of a given branch since it was originally branched. This is largely intended to be used with a graphical directory compare tool.

Installation
------------

Download the `git-branch-diff` script, save it somewhere in your PATH, and make sure it is executable.

By default, this will use the command-line `diff -r` command as the directory compare. The whole reason this was created was to enable the use of a graphical directory compare tool. To configure the tool you would like to use, you can set a git configuration, such as:

    git config --global branchdiff.cmd /path/to/dir/cmp/tool

or you can set it in an environment variable, e.g.:

    export GIT_BRANCH_DIFF_CMP=/path/to/dir/cmp/tool

The tool you use should take two parameters: the left-side directory (i.e.: the "before" tree) and the right-side directory (i.e.: the branch's current working tree).

I highly recommend [SmartSynchronize](http://www.syntevo.com/smartsynchronize/index.html), by [syntevo GmbH](http://www.syntevo.com/). From their website: 

> SmartSynchronize is a multi-platform file and directory compare tool. It helps you to compare two files or directory trees, to merge changes between files and directories, i.e. to synchronize them, or to 3-way-merge changes of one file which has been changed independently.

To configure `git-branch-diff` to use SmartSynchronize, on Mac OS X, for example:

    git config --global branchdiff.cmd /Applications/SmartSynchronize/smartsynchronze.sh

Usage
-----

This script is intended to be run from the root directory of the working copy of the branch you wish to compare to ise "parent". For example, say you have a repository that looks like:

              u---v---w---[uncommitted]       <--- my_topic
             /
    a---b---c---d---e---f                     <--- master

You would want to run this script from the `my_topic` branch to see all the differences between the current state of the `my_topic` branch and the point at which it branched from its parent branch (e.g.: commit 'c'). In this example, that amounts to the changes from: x + y + z + uncommitted. E.g.:

    git checkout my_topic
    git branch-diff master

If the name of the parent branch is 'master', then it may be omitted, as this is assumed as the default.

Technically, this script does not necessarily diff against the point at which the branch was created. It diffs against the last point the branch has in common with the specified parent. For example, if the `my_topic` branch had the head of `master` merged into it again later, the diff would be against that point in `master`:

             u---v---w---x---y---z---[uncommmited]  <--- my_topic
            /           /
    a---b---c---d---e---f---g                       <--- master

In this case the differences shown amount to the changes from x + y + z + uncommited changes.

Disclaimer
----------

This script was quickly slapped together in Ruby, but mostly using shell commands. It is probalby not optimal or even well tested. There may even be a better more concise way to do this with some built-in git command I couldn't find. It has really only been tested on Mac OS X 10.6 (Snow Leopard) with git version 1.7.0.3 using SmartSynchronize 3.1.6. Your mileage may vary.

Licensed Under The BSD License
------------------------------

Copyright (c) 2010, Scott W. Bradley <scottwb@gmail.com>
All rights reserved.

Redistribution and use in source and binary forms, with or without
modification, are permitted provided that the following conditions are met:

* Redistributions of source code must retain the above copyright notice,
  this list of conditions and the following disclaimer.
* Redistributions in binary form must reproduce the above copyright notice,
  this list of conditions and the following disclaimer in the documentation
  and/or other materials provided with the distribution.
* Neither the names Scott W. Bradley, scottwb, Strings Inc., nor the
  names of their contributors may be used to endorse or promote products
  derived from this software without specific prior written permission.

THIS SOFTWARE IS PROVIDED BY THE COPYRIGHT HOLDERS AND CONTRIBUTORS "AS IS"
AND ANY EXPRESS OR IMPLIED WARRANTIES, INCLUDING, BUT NOT LIMITED TO, THE
IMPLIED WARRANTIES OF MERCHANTABILITY AND FITNESS FOR A PARTICULAR PURPOSE
ARE DISCLAIMED. IN NO EVENT SHALL THE COPYRIGHT HOLDER OR CONTRIBUTORS BE
LIABLE FOR ANY DIRECT, INDIRECT, INCIDENTAL, SPECIAL, EXEMPLARY, OR
CONSEQUENTIAL DAMAGES (INCLUDING, BUT NOT LIMITED TO, PROCUREMENT OF
SUBSTITUTE GOODS OR SERVICES; LOSS OF USE, DATA, OR PROFITS; OR BUSINESS
INTERRUPTION) HOWEVER CAUSED AND ON ANY THEORY OF LIABILITY, WHETHER IN
CONTRACT, STRICT LIABILITY, OR TORT (INCLUDING NEGLIGENCE OR OTHERWISE)
ARISING IN ANY WAY OUT OF THE USE OF THIS SOFTWARE, EVEN IF ADVISED OF THE
POSSIBILITY OF SUCH DAMAGE.

