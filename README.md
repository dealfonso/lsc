# License Shell Code (LSC)
LSC is an application to generate shell applications that need a license to be run.

## Why LSC?

LSC is a proof of concept that enables to generate a signed script that needs a license (or password) to be run. It is just a generalization of the compression of _bash scripts_.

Having a bash script in file `example`, compressing it is very simple. You can just see the next commands that create the `example` script and generates a gzipped version:

```console
$ cat > example <<EOT
#!/bin/bash

echo "hello world"
EOT
$ cat > compressedscript <<EOT
#!/bin/bash
eval "\$(echo "$(cat example | gzip | base64)" | base64 -d | gunzip)"
EOT
$ chmod +x compressedscript
$ ./compressedscript
hello world
```

LSC is just a generalization of that mechanism, that enables to encode the script using a password.

**NOTES:** Providing your users with a LICENSE code give them a _more professional_ distribution, and a _more tailored_ solution. Getting the code also implies a knowledge of what they are doing and so it also includes a first barrier (both from the point of the knowledge and from the ethics).

## Installation

Get the proper package for your distribution in [this page]() and follow the instructions:

**Ubuntu**

```console
$ apt update
$ apt install ./lsc_0.1-0.deb
```

**Centos**

```console
$ yum update
$ yum install ./lsc-0.1-0.noarch.rpm
```

**From source**

You can also get the source code and use it:

```console
$ git clone https://github.com/dealfonso/lsc
$ cd lsc
$ ./lsc -N mynewapp /path/to/my/original/script
```

## Configuration
You can adjust the template to use to generate the executables. At this moment, there are two templates included: `lsce.template` and `lscec.template` which stand for _LSC executable_ and _LSC executable compressed_.

They are the same code, but the `lscec` version is the compressed version of the `lsce` one. That compressed version gives the end-user a _magic like_ feeling because of the compression.

Feel free to create your own templates, if you want. They just need to include the tags `<<APPNAME>>` and `<<CODE>>` that will be substituted for the new application's ones.

In order to adjust the template to use, you just need to modify the file `/etc/lsc/lsc.conf` and set the path to the template to use:

```bash
# The template file (it has to include the <<APPNAME>> and the <<CODE>> tags that will be replaced by lsc)
TEMPLATE=/etc/lsc/lscec.template
```

## Example

Imagine that you have the next code in a file named **example**:

```bash
#!/bin/bash

echo "hello world"
```

Then you can apply **lsc** to generate the licensed code by issuing a command like the next one:

```console
$ lsc -N licensedexample example 
using template 'lscec.template' found in file etc/lsc.conf
Overwrite existing file licensedexample? (y/N) y
b97c06f3-7aca-4ce4-9901-3d1a8b55095e

Your new licensed application has been generated in file 'licensedexample'.
Please take note of your license number, because you need it to run
your application. The license number is unique for that application.
Have in mind that if you lose that license, your application will be 
unusable.

In order to run your application, you can use the next command:

LICENSE=b97c06f3-7aca-4ce4-9901-3d1a8b55095e ./licensedexample

Otherwise you can create a config file in a common configuration place
and put the next content:

LICENSE=b97c06f3-7aca-4ce4-9901-3d1a8b55095e

e.g. 
  /etc/licensedexample.conf
  ~/.licensedexample.conf
```

The result is a new file named **licensedexample** whose content is the next one:

```bash
#!/bin/bash

# The encoded code
LICENSEDCODE="U2FsdGVkX1/RCCmnB38BhWzuBym7nBqEEaMIuKe7ioRKG68jbywCI4zF/CGnktdsrmkrPtyttUxzxQ2zBMTtqYawGcmXGasniMOXlHIodRQ="
# Config files where to find the LICENSE number
CONFIGFILES="etc/licensedexample.conf etc/licensedexample/licensedexample.conf $HOME/.licensedexample.conf /etc/licensedexample.conf /etc/licensedexample/licensedexample.conf /etc/default/licensedexample.conf"

RUNTIME="H4sIANj4mFsAA2VUa0/bMBT9nl9xMBFtYSGANqSpDaPqY6royrRKfFgXIE2c1iK1OycBBmO/fXbi
pg/6pfZ9nHvPudeJcx5mTHAs76iUQtYbeLUAGs4FiH1JcHFwZr1Z8SpM0iAKBY/ZzEQmIgwS9AfD
3qj9recR+5Qo60Qlr2wEngdC4AjswaFbHh8HBwozyyXHqcpL5yzOKtSb9o+xV4d9iUZlGxVG+3Vf
/08u/bfK07ke9QdfPXIfBtlWkb9IaYRa6u4fH7rurLY23E7wK/N3jKXN3jS6t7Yb1e5JVWsIjivc
qPvTnCW0kAXDJiKhTACLMdESDBX3f7idBM5L2/l555vDifP5zj/yjg9t+H4T2ZzyIg0Y9MdezauV
eKoAWq1WgWP8sZCo17l30uStQokmPzpqNKrC+qcLvxbq2Nx/K9W3r4zWU4X8YEIjwak56oZhczWe
37ALYGw3htWCgLAZF5LxGR6DJKdFSwrexNEkpVVKwcK+MiRuSOWgz0shM+Va0WLWTvL7aisRiljd
uoEtx07UkhYklFiDTm80rtZuzUN32umDcZgkvSLjSrv1amvUPoEBqia6ib23g73Vc8JCylMtTc4j
XS/WO6IxTeR6ChUby5B/DyLFApQ/Min4gvKMWCrH2keXhiKiuj70wfo+bA9GneuufoN1835Nu11t
1u9gGqT0/COcCGcXcCP66PI8SZRDLFWtNFF1Qu11Apo6Z5/OnXCq7opAEZtmkTo6Is+quz47D5vC
7ADPcv7CllvWBojufxAj4H9QshVhmEtJow94ojWVGKRpvtDk1FNWDCUFSxFgKcU0oQs8sWxeMDci
laO3v8BRa3GyMZZ3avJ8MaVSo3GR6Q1mkR4KfWaZ+gCVyvaeaZhnG9JSFac4VgqT/zPG+Ck2BQAA"

eval "$(echo "$RUNTIME" | base64 -d | gunzip)"
```

And now you can run the encoded application, provided that you provide the license number to it (whether in the environment LICENSE variable or in the config file):

```console
$ ./licensedexample 
license number is not valid
$ LICENSE=b97c06f3-7aca-4ce4-9901-3d1a8b55095e ./licensedexample
license from environment
hello world
```

## FAQ

**Q:** Is it safe for redistributing code?

**A:** No. Once you have the license number, the code can be obtained from the file. In case that you want a binary solution in which it is harder to get the code, you should use solutions like [shc](https://github.com/neurobin/shc).

**Q:** In which scenario would it be interesting?

**A:** If you want to put your code in a public repository, but do not want to let others to use it. For those who want to obfuscate (or make harder to get) your code. Maybe others.
