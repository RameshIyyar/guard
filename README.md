# GUARD
Guarding the system against failures by permanently isolating faulty units.
guard records contains the permanently isolated components details.
GUARD repository provides the libraries and tools to create/list/delete guard records
## Building
Need `meson` and `ninja`. Alternatively, source an OpenBMC ARM/x86 SDK.
```
meson build && ninja -C build
```
To use power system device for guard FRU's.
```
meson build -Ddevtree=enabled && ninja -C build
```
To build libguard with verbose level to get required trace.\
Supported verbose level:\
`0` - Emergency, `1` - Alert, `2` - Critical, `3` - Error, `4` - Warning,
`5` - Notice, `6` - Info, `7` - Debug\
By default verbose level is Error.
```
meson build -Dverbose=7 && ninja -C build
```
## To run unit tests
Tests can be run in the CI docker container, or with an OpenBMC x86 sdk(see
below for x86 steps).
```
meson -Doe-sdk=enabled -Dtests=enabled build
ninja -C build test
```
## Usage of GUARD tool
```

guard --help
GUARD Tool
Usage: guard [OPTIONS]

Options:
  -h,--help              Use the below listed functions.
                         Warning: Don't try guard on non guardable units
                         (sys, perv)
  -c,--create TEXT       Create GUARD record, expects physical path as input.
  -d,--delete TEXT       Delete GUARD record, expects physical path as input.
  -l,--list              Listing of GUARDed resources.
  -r,--clearall          Clears GUARD states for all resources.
  -v,--version           Version of GUARD tool.
```
**Note:** Physical path can be fetched from device tree, using ATTR_PHYS_DEV_PATH
attribute of the corresponding target.
Physical path formats supported by guard tool:-

* physical:sys-0/node-0/proc-0/mc-0/mi-0/mcc-0
* /sys-0/node-0/proc-0/mc-0/mi-0/mcc-0
* sys-0/node-0/proc-0/mc-0/mi-0/mcc-0

### Examples

* To create a guard record.
```
guard -c sys-0/node-0/proc-0/mc-0/mi-0/mcc-0
Success
```
* To list the guard records present in a system.
```
guard -l
ID       | ERROR    |  Type  | Path 
00000001 | 00000000 | manual | physical:sys-0/node-0/proc-0/mc-0/mi-0/mcc-0
```
* To clear all the guard records
```
guard -r
```
* To clear a particular guard record
```
guard -d sys-0/node-0/proc-0/mc-0/mi-0/mcc-0
```
