status: in progress

# Campfire-1

![intro](./img/intro.png)

This very easy machine is a good training in digital forensics and incident response.

I see some files once that I decompress the file with 7-zip
![files](./img/files.png)
and also some prefetch logs? 
![pref](./img/pref.png)

>prefetch logs

with PECmd.exe i converted the prefetch files in one file so i can analyze it

```sh
PECmd.exe -d "./././prefetch" --csv . --csvf outputfilename.csv`
![PECmd](./img/pecmd.png)
```


## Task 1

> Analyzing Domain Controller Security Logs, can you confirm the date & time when the kerberoasting activity occurred?
![time](./img/time%20hour.png)


## Task 2
> What is the Service Name that was targeted?

![mssql](./img/mssql.png)


## Task 3

> It is really important to identify the Workstation from which this activity occurred. What is the IP Address of the workstation?
![ip](./img/ip.png)

## Task 4

> Now that we have identified the workstation, a triage including PowerShell logs and Prefetch files are provided to you for some deeper insights so we can understand how this activity occurred on the endpoint. What is the name of the file used to Enumerate Active directory objects and possibly find Kerberoastable accounts in the network?

![enum](./img/enumfile.png)

## Task 5

> When was this script executed?
![creatime](./img/creatime.png)



## Task 6

> What is the full path of the tool used to perform the actual kerberoasting attack?
![path](./img/path.png)

## Task 7

> When was the tool executed to dump credentials?
![dump](./img/dump.png)