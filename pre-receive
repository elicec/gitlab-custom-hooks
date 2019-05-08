#!/usr/bin/env python
import re
import sys
import subprocess

CREDBG = '\33[41m'
CEND = '\33[0m'
CRED = '\33[91m'
CGREEN = '\33[92m'

def isPushTag(msg):
    return 'refs/tags' in msg

def git(args):
	args = ['git'] + args
	git = subprocess.Popen(args, stdout = subprocess.PIPE)
	details = git.stdout.read()
	details = details.decode('utf-8','replace').strip()
	return details

def getCommitMsg(old, new):
    out = git(['log', old+'..'+new, '--pretty=format:%s'])
    return out

def isAutoMsg(msg):
    pat = re.compile(r'^(?:Merge|Rvert)')
    return pat.match(msg)

#TODO check content  in []
def verifyCommitMsgs(msgs):
    match = None
    pat = re.compile(r'^\[.{0,16}\]\[.*\]\[.*\]')#[test][test][test]
    for msg in msgs:
        if isAutoMsg(msg):# Merge or Revert msg
            continue
        match = pat.match(msg)
        if not match:
            print(CREDBG + msg + ' [ERROR format!!]' + CEND)
            return False
        else:
            continue

    return True

def main():
    print(CGREEN + 'Start Verify Commit Message' + CEND)
    (old, new, branch) = sys.stdin.read().split()
    print("old:"+old)
    print("new:"+new)
    print("branch:"+branch)
    if isPushTag(branch):
        print("is a tag")
        sys.exit(0)#success exit
    com_msg = getCommitMsg(old, new).split()
    result = verifyCommitMsgs(com_msg)
    if result:
        print(CGREEN + 'Verify Commit Message success' + CEND)
        sys.exit(0)
    else:
        sys.exit(1)

if __name__ == '__main__':
    main()
