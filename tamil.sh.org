#!/bin/bash


function ft()
{
local i=$1	
thismonth=$(date | awk '{ print $2}')
lastmonth=$(date -d " last month" '+%c' | awk '{print $3 }')	

/root/android_tv/adb connect $i
/root/android_tv/adb -s $i shell rm -rf /sdcard/Download/$lastmonth
/root/android_tv/adb -s $i shell rm -rf /sdcard/Download/$thismonth
/root/android_tv/adb -s $i shell mkdir  /sdcard/Download/$thismonth
/root/android_tv/adb -s $i push         /data/Tamil/*.mp4    /sdcard/Download/$thismonth 
if  [[ $? -eq 0 ]]; then
echo $i >> ./tamil-success-ip.txt  &&  echo "The video content is moved successfully to this device $i ." | mail -s "Tamil tv content movement of The $thismonth month" it@vcaregroup.in
echo "done"
fi
}

function abcd()
{

echo "hello world"

}


for j in $(cat ./tamil-ipaddr.txt | awk '{ print $1 }')
do	 
ping -4 -c4 $j >> /dev/null
if [ $? -eq 0 ]; then

aa=$( cat /root/ai/tamil-success-ip.txt | grep -w $j )

if [ "$j" != "$aa" ]; then

ft $j > /dev/null &
#abcd &

fi

fi
done





