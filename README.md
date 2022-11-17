# bash-scripting_projects

<details><summary>:blossom: Check System Health all hosts in the network  </summary>
<p>

```bash
  ip_list=("172.16.0.11" "172.16.0.12" "172.16.0.13" "172.16.0.14")

for ip_address in ${ip_list[@]}; do
	echo "$ip_address"
	status=$(bash is_active.sh "$ip_address")
	if [[ "$status" == "is active" ]]; then 

		mem=$(bash memory_freeIP.sh "$ip_address") #172.16.0.12
		
		if (( $mem < 20 )); then
			echo "send email --warning--memory is getting low, it is : $mem%"
		fi

		disk=$(bash disk_freeIp.sh "$ip_address")
		if (( $disk < 20 )); then 
			echo "send email --warning-- disk getting low, it is: $disk%"
		fi

		bad_user=$(bash check_usersHealth.sh "$ip_address")
		
		if [[ $bad_user == "yes" ]]; then
			echo "all good"
		else 
			echo "send email--warning --Bad user detected"
		fi
	else
		echo "the ip address $ip_address is not active"
	fi
	echo 
		
done
```
is_active.sh
```bash
received=$(ping -c 4 $1 | grep "received")
IFS=" "
rev=($received)
unset IFS

if (( ${rev[3]} > 2 )); then
	
	echo "is active"
else
	echo "not active"

fi
```
memory_freeIP.sh
```bash
mem_free=$(ssh "student@$1" "free | grep Mem")
IFS=" "
mem_percent=($mem_free)
unset IFS

total=${mem_percent[1]}
free=${mem_percent[3]}

mem_free=$(( ($free * 100) / $total  ))

echo "$mem_free"
```
disk_freeIP.sh
```bash
disk_free=$(ssh "student@$1" "df --total | grep total")
IFS=" "
diskF=($disk_free)
unset IFS
free=${diskF[4]}
free=${free:0:-1}

echo "$free"
```
check_usersHealth.sh
```bash
ssh "student@$1" "who" > user_login.txt
login_user=()
while read line ; do

	IFS=" "
	line=($line)
	unset IFS
	login_user+=(${line[0]})

		
done < user_login.txt


good_user=()
while read good_user ; do

	IFS=" "
	good_user=($good_user)
	unset IFS
	good_user+=(${good_user[0]})
		
done < good_users.txt

all_good="yes"

for user in "${login_user[@]}"; do
	found_match="no"
	for good in "${good_user[@]}"; do
		if [[ $user == "$good" ]]; then 
			found_match="yes"
	
		fi
	done	
	
	if [[ $found_match == "no"  ]]; then
		
		all_good="no"

	fi
done

echo "$all_good"
```
</p>
</details>

<details><summary>:blossom:Check user IP address </summary>
<p>

```bash
  echo "enter IP address: "
read ip_address

ssh student@$ip_address "who" > user_login.txt
#$1 from system health.sh $ip_address

#ssh student@$1 "who" > user_login.txt
login_user=()
while read line ; do

	IFS=" "
	line=($line)
	unset IFS
	login_user+=(${line[0]})

		
done < user_login.txt

echo
good_user=()
while read good_user ; do

	IFS=" "
	good_user=($good_user)
	unset IFS
	good_user+=(${good_user[0]})
		
done < good_users.txt

all_good="yes"

for user in "${login_user[@]}"; do
	found_match="no"
	for good in "${good_user[@]}"; do
		if [[ $user == "$good" ]]; then 
			found_match="yes"
	
		fi
	done	
	
	if [[ $found_match == "no"  ]]; then
		
		all_good="no"

	fi
done
if [[ $all_good == "yes" ]]; then
	
	echo "All good"
else 
	echo "Bad user detected"
fi

 
```
</p>
</details>

<details><summary>:blossom: subscript (call script from another script) </summary>
<p>

```bash
echo "Hello World"
echo "Something Else"
echo "$1" the 1st argument from another script 
```
call subscript from main_script
```
#bash sub_script.sh
bash sub_script.sh "Good bye"
```
</p>
</details>

<details><summary>:blossom: SSH to others linux</summary>
<p>
to automate the SSH, we will use SSH-keygen and SSH-copy-id
```bash
 ssh student@172.16.0.12
mkdir I_connected
echo "I Connected"
exit  
```
</p>
</details>


<details><summary>:blossom: check instruder to network </summary>
<p>

```bash
   who > user_login.txt
login_user=()
while read line ; do

	IFS=" "
	line=($line)
	unset IFS
	login_user+=(${line[0]})

		
done < user_login.txt

echo
good_user=()
while read good_user ; do

	IFS=" "
	good_user=($good_user)
	unset IFS
	good_user+=(${good_user[0]})
		
done < good_users.txt

all_good="yes"

for user in "${login_user[@]}"; do
	found_match="no"
	for good in "${good_user[@]}"; do
		if [[ $user == "$good" ]]; then 
			found_match="yes"
	
		fi
	done	
	
	if [[ $found_match == "no"  ]]; then
		
		all_good="no"

	fi
done
if [[ $all_good == "yes" ]]; then
	
	echo "All good"
else 
	echo "Bad user detected"
fi


```
</p>
</details>
