
# Bash bits

[bash option discussion](https://vaneyckt.io/posts/safer_bash_scripts_with_set_euxo_pipefail/)

When using pipes the exit code of the last item is returned!

Which in many cases might not be desired!

Improved example:

```
#!/bin/bash
#
set -o pipefail 
./do.sh | bzip2 > res.txt  
echo "ret: "$?
```

And the do.sh

```
echo "Hello world!"
>&2 echo "Error!"
exit 1
```

# Bash array

to build parameters from array quoting is crucial:

```
#!/bin/bash
function param
{
  local opt
  opt=()
  echo "Conn ${rsynconn}"
  if [ ! -z "${rsynconn}" ] ; then
    opt+=(-e "${rsynconn}")
  fi
  opt+=("rpf@localhost:shell/test.sh")
  opt+=("test2/")
  for i in "${!opt[@]}" ; do
     echo "opt $i = ${opt[$i]}"
  done
  echo "Rsync ${opt[@]}"
  rsync "${opt[@]}" 
  if [ $? -ne 0 ] ; then
     echo "Error rsync!"
   else 
      echo "Rsync ok"
   fi
}
rsynconn="ssh -p 22"
param
