## Task 10: File Permission Correction
After conducting a security audit within the Stratos DC, the Nautilus security team discovered misconfigured permissions on critical files. To address this, corrective actions are being taken by the production support team. Specifically, the file named /etc/hosts on Nautilus App 2 server requires adjustments to its Access Control Lists (ACLs) as follows:

1. The file's user owner and group owner should be set to root.
2. Others should possess read only permissions on the file.
3. User mark must not have any permissions on the file.
4. User eric should be granted read only permission on the file.

### Solution:
#### 1️⃣ Owner এবং Group root করুন
```
sudo chown root:root /etc/hosts
```
#### 2️⃣ Others কে শুধু read permission দিন

Permission হবে:
`
rw-r--r--
`
কমান্ড:
```
sudo chmod 644 /etc/hosts
```
Explanation:   
```
Owner  = rw-   
Group  = r--   
Others = r--   
```   
#### 3️⃣ User mark এর সব permission remove করুন
```
sudo setfacl -x u:mark /etc/hosts
```
#### 4️⃣ User eric কে read permission দিন
```
sudo setfacl -m u:eric:r /etc/hosts
```
#### 5️⃣ Verify করুন
```
getfacl /etc/hosts
```
Expected output এর মতো হবে:
```
file: etc/hosts
owner: root
group: root

user::rw-
user:eric:r--
group::r--
mask::r--
other::r--
````
