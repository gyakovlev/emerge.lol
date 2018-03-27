

## Format description:

* timestamp:pid something like `0123456789:1234`
* event: `started` `finished` `failed`
* action: `emerge`, `sync`, `test fetch unpack compile install merge unmerge`
* item_name: `cmdline` `repository` `package`
* atom:

	`action_emerge? (cmdline)` 

	`item_name_repository? (name[gitcommit || rsync_timestamp || websync_snapshotname])`

	`iten_name_package? ${CATEGORY}/${PF}:${SLOT}:${REPO}[-flag(-) flag(+) abi_x86_64(+) abi_x86_32(-)]`

	same goes for python/ruby targets, anything significant, but we don't want to implement single-line VDB
	ideally atom should match the info given my emerge --info <pkg> in a more compact and parseable format
* extra:
	`event_failed? (short reason)`

### atom examples:

```Hack
sys-apps/portage-mgorny-2.3.24.4::gentoo[ipc(+) native-extensions(+) xattr(+) -build(-) -selinux(m) abi_x86_64 python_targets_python2_7 python_targets_python3_5 -python_targets_pypy -python_targets_python3_4 -python_targets_python3_6]

sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="(ipc) native-extensions xattr -build (-selinux)" ABI_X86="(64)" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]

sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
```


### sync log format 

```
0123456789:1234 started sync of repository gentoo [timestamp of the repo or commit hash or something else to identify current state]
0123456799:1234 finished sync of repository gentoo [87c68809cee10bff305f03654f345fa9f31e4a40 or timestamp or something to identify reached state]
0123456799:1234 failed sync of repository gentoo [short reason]
```

### example of emerge --sync (don't forget of emaint sync -a):
```
0123456779:1233 started emerge "--sync"
0123456780:1233 started sync of repository gentoo[20180315]
0123456782:1233 finished sync of repository gentoo[20180317]
0123456783:1233 started sync of repository rust[11c68809cff10bff348f03654f345fa9f31e4a90]
0123456784:1233 finished sync of repository rust[87c66709cee10bff305f03654f345fa8f21e6a40]
0123456785:1233 finished emerge "--sync"
```

### example of updating a package with FEATURES=test-fail-continue:
```
0123456787:1234 started emerge "-uDN world --with-bdeps=y"
0123456787:1234 started fetch of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456788:1234 finished fetch of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456789:1234 started unpack of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456790:1234 finished unpack of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456791:1234 started compile of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456792:1234 finished compile of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456793:1234 started test of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456794:1234 failed test of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456795:1234 started install of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456796:1234 finished install of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456797:1234 started merge of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456798:1234 finished merge of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456799:1234 started unmerge of package sys-apps/portage-mgorny-2.3.20.1::gentoo[USE="ipc -native-extensions -xattr build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_4 -pypy -python3_5 -python3_6"]
0123456800:1234 finished unmerge of package sys-apps/portage-mgorny-2.3.20.1::gentoo[USE="ipc -native-extensions -xattr build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_4 -pypy -python3_5 -python3_6"]
0123456800:1234 finished emerge "-uDN world --with-bdeps=y"
```

### in case test falures are fatal:
```
...
0123456793:1234 started test of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456794:1234 failed test of package sys-apps/portage-mgorny-2.3.24.4::gentoo[USE="ipc native-extensions xattr -build -selinux" ABI_X86="64" PYTHON_TARGETS="python2_7 python3_5 -pypy -python3_4 -python3_6"]
0123456800:1234 failed emerge "-uDN world --with-bdeps=y"
```
