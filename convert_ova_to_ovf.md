macos

```
cd /Applications/VMware Fusion.app/Contents/Library/VMware OVF Tool
```

linux
```
search binary
```

tips
```
dir=`find /*  -name "*ovftool" 2>&1 | head -n 1|sed 's/ovftool//g'`
cd $dir
```

```
./ovftool --lax /Users/operator/Downloads/WIN7/Win7Box.ova /Users/operator/Downloads/WIN7/Win7Box.ovf
```
