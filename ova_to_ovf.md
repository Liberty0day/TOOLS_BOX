# Virtual box OVA to VMware OVF

- [ ] You need VMware workstation or VMwareFusion or Player


- [ ] You need found ovftool folder

```
dir=`find /*  -name "*ovftool" 2>&1 | head -n 1|sed 's/ovftool//g'`
```
```
cd $dir
```
- [ ] You need make folder 
```
mkdir WIN7
```
- [ ] You need convert now

MACOS

```
./ovftool --lax /Users/operator/Downloads/WIN7/Win7Box.ova /Users/operator/Downloads/WIN7/Win7Box.ovf

```

LINUX

```
./ovftool --lax /home/operator/Downloads/WIN7/Win7Box.ova /home/operator/Downloads/WIN7/Win7Box.ovf

```

- [ ] You need patch ovf and update mf, you can if you want use the scriupt patch-ovf.sh

[patch-ovf.sh](https://github.com/Liberty0day/TOOLS_BOX/blob/main/patch-ovf.sh)


```
#!/bin/bash

function nameVM(){
echo "[!] Give me the name of your vm example Win7Box"
read yourVM
file=$yourVM;
}

function patchMaOS(){
sed  -i '' 's|<vssd:VirtualSystemType>virtualbox-2.2</vssd:VirtualSystemType>|<vssd:VirtualSystemType>vmx-07</vssd:VirtualSystemType>|g' $file.ovf
sed  -i '' 's|<rasd:Caption>sataController0</rasd:Caption>|<rasd:Caption>SCSIController</rasd:Caption>|g' $file.ovf
sed  -i '' 's|<rasd:Description>SATA Controller</rasd:Description>|<rasd:Description>SCSI Controller</rasd:Description>|g' $file.ovf
sed  -i '' 's|<rasd:ElementName>sataController0</rasd:ElementName>|<rasd:ElementName>SCSIController</rasd:ElementName>|g' $file.ovf
sed  -i '' 's|<rasd:ResourceSubType>AHCI</rasd:ResourceSubType>|<rasd:ResourceSubType>lsilogic</rasd:ResourceSubType>|g' $file.ovf
sed  -i '' 's|<rasd:ResourceType>20</rasd:ResourceType>|<rasd:ResourceType>6</rasd:ResourceType>|g' $file.ovf
}

function patchLinux(){
sed  -i 's|<vssd:VirtualSystemType>virtualbox-2.2</vssd:VirtualSystemType>|<vssd:VirtualSystemType>vmx-07</vssd:VirtualSystemType>|g' $file.ovf
sed  -i 's|<rasd:Caption>sataController0</rasd:Caption>|<rasd:Caption>SCSIController</rasd:Caption>|g' $file.ovf
sed  -i 's|<rasd:Description>SATA Controller</rasd:Description>|<rasd:Description>SCSI Controller</rasd:Description>|g' $file.ovf
sed  -i 's|<rasd:ElementName>sataController0</rasd:ElementName>|<rasd:ElementName>SCSIController</rasd:ElementName>|g' $file.ovf
sed  -i 's|<rasd:ResourceSubType>AHCI</rasd:ResourceSubType>|<rasd:ResourceSubType>lsilogic</rasd:ResourceSubType>|g' $file.ovf
sed  -i 's|<rasd:ResourceType>20</rasd:ResourceType>|<rasd:ResourceType>6</rasd:ResourceType>|g' $file.ovf
}

function sha256(){
SHA256OVF=$(sha256sum $file.ovf|sed 's/  ['$file'.ovf]*//g'| awk '{print tolower($0)}')
SHA256VMDK=$(sha256sum $file-disk1.vmdk|sed 's/  ['$file']*[-]*[disk1]*[.vmdk]*//g'| awk '{print tolower($0)}')
rm $file.mf
cat <<EOF >> $file.mf
SHA256($file.ovf)= $SHA256OVF
SHA256($file-disk1.vmdk)= $SHA256VMDK
EOF
}

function backup(){
mkdir backup
cp $file.ovf backup/$file.ovf 
cp $file.mf backup/$file.mf
}

while :;
do

clear
echo "-------------------------------------------------------------------------------"
echo "convert $file.ova ---> $file.ovf ---> patch $file.mf -----X lunch vmware"
echo "-------------------------------------------------------------------------------"
yourVM                      
echo "[!] Give me your operating system name to make your ova file vmware compatible"
echo "choice 0) exit 1) backup 2) Linux 3) MacOS"
read os


case "$os" in
   "0") 
            echo "[+] EXIT :: bye" 
            exit 1
   ;;   
   "1") 
            echo "[+] BACKUP :: save $file.ovf and $file.mf" 
            backup
   ;;
   "2") 
            echo "[+] LINUX :: Patch $file.ovf file" 
            patchLinux
            echo "[+] Update $file.mf"
            sha256
   ;;
   "3") 
            echo "[+] MACOS :: Patch .ovf file"
            patchMaOS
            echo "[+] Update $file.mf"
            sha256
   ;;
esac
done

```

- [ ] If you have error message 
 -  Cannot connect the virtual device sata0:1 because no corresponding
 -  CLICK OK 
 -  IF YOU HAVE ERROR MESSAGE CLICK OK OR NEXT

- [ ] Enjoy with your system bye

