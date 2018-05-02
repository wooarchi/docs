# Galera cluster bootstraping

## 진단
- VM 상태 확인
```
$ bosh -e paas-bosh -d cf-mysql vms
Using environment '10.10.10.10' as client 'admin'

Task 2875. Done

Deployment 'cf-mysql'

Instance                                         Process State  AZ  IPs           VM CID                                VM Type  
arbitrator/34ba14cb-6594-4867-8f70-582ec0680fe8  running        z3  10.10.10.203  e62ffc45-0975-4225-b29a-7145fd8460e2  medium   
broker/8c49b584-7e5f-43c8-93e5-5daa662cd7a3      failing        z2  10.10.10.207  59de90d4-3063-4f36-b3ce-d23519c3abcc  small    
broker/b17d1a1a-c766-41ba-8a46-958a046f9367      failing        z1  10.10.10.206  dcdf8085-d39c-432c-812a-f546dae010e2  small    
mysql/2170898b-a654-4a07-8d38-d2587e43643c       failing        z1  10.10.10.201  f1e09da2-545b-4ee2-beb5-9d1230b61da2  large    
mysql/9e46191c-cf12-4ca0-8d3d-f540a51e3a87       failing        z2  10.10.10.202  dc9a06a9-bbb6-495b-b229-83257a38e119  large    
proxy/a30ed930-d447-4514-8c20-f24180b8aac4       running        z2  10.10.10.205  51f758bc-6f95-4bb6-9b05-a753a39c6e50  small    
proxy/f093b999-4ecd-46ab-b120-f1a07bafcc30       running        z1  10.10.10.204  ca638818-359c-4d44-b733-22dea0d77b4f  small    

7 vms

Succeeded
```
- bosh errand로 복구
```
$ bosh -e paas-bosh -d cf-mysql run-errand bootstrap

Using environment '10.10.10.10' as client 'admin'

Using deployment 'cf-mysql'

Task 2879

01:52:18 | Preparing deployment: Preparing deployment
01:52:19 | Preparing package compilation: Finding packages to compile (00:00:00)
01:52:19 | Preparing deployment: Preparing deployment (00:00:01)
01:52:19 | Creating missing vms: bootstrap-vm/f344f5db-877d-4b4a-ad8d-e21b7c4e8336 (0) (00:00:55)
01:53:14 | Updating instance bootstrap-vm: bootstrap-vm/f344f5db-877d-4b4a-ad8d-e21b7c4e8336 (0) (canary) (00:00:30)
01:53:44 | Running errand: bootstrap-vm/f344f5db-877d-4b4a-ad8d-e21b7c4e8336 (0)(00:03:18)
01:57:02 | Fetching logs for bootstrap-vm/f344f5db-877d-4b4a-ad8d-e21b7c4e8336 (0): Finding and packing log files (00:00:01)

Started  Wed May  2 01:52:18 UTC 2018
Finished Wed May  2 01:57:03 UTC 2018
Duration 00:04:45

Task 2879 done

Exit Code  0                              
Stdout     Started bootstrap errand ...   
           Successfully repaired cluster  
           Bootstrap errand completed     
                                          
Stderr     -                              

1 errand(s)

Succeeded
```


- menual 복구
