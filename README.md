# Linux-Lab-04 Multi-User Shared Directory (Group-Based Access Control)
# This lab demonstrates how to create a shared directory with controlled cross‑team access using Linux groups, permissions, and the setgid bit.

# Step 1 — Create a Shared Group
# A shared group was created and one team lead from each previously created team (person3, person6, person9) was added to it.
```bash
sudo addgroup sharedgroup
for user in person3 person6 person9; do sudo usermod -aG sharedgroup $user; done
```
<img width="1589" height="672" alt="sharedgroup plus teamleads" src="https://github.com/user-attachments/assets/594735f0-694d-49cc-8b88-0bc8f178517c" />


# Step 2 — Create Shared Directory
# A shared directory and test file were created for selected team leads only, and permissions were configured to enforce group-based access with inheritance.
```bash
sudo mkdir /data/shared
sudo touch /data/shared/sharedtestfile.txt
sudo chown root:sharedgroup /data/shared
sudo chmod 770 /data/shared
sudo chmod g+s /data/shared
```
<img width="1583" height="801" alt="creating multi test file" src="https://github.com/user-attachments/assets/9a3d07c0-34eb-49a6-b797-39c00c06e098" />

<img width="1595" height="678" alt="chown and chmod for shareddir" src="https://github.com/user-attachments/assets/cab067ba-5721-4691-ac39-34d76781addb" />


# Step 3 — Verify Access
# Verified that team members can access only their own directories and that sharedgroup members can enter /data/shared.
# Issue discovered: users could read and execute but not write to the shared file.
```bash
su person6
nano /data/shared/sharedtestfile.txt   # access denied
```
<img width="1591" height="755" alt="verifynf access of team leads (2)" src="https://github.com/user-attachments/assets/0def7c92-c493-4336-8418-28b55a5dde42" />


# Step 4 — Troubleshoot
# The shared file was created before permissions, ownership, and inheritance were applied, so it did not inherit the correct group or write permissions.
```bash
rm /data/shared/sharedtestfile.txt
touch /data/shared/sharedtestfile.txt
```
Removed the existing file and re-created it


# Step 5 — Re‑Verify
# Tested again using su with person3, person6, and person9. All sharedgroup members can now edit the shared file as intended.
```bash
su person3
nano /data/shared/sharedtestfile.txt
su person6
nano /data/shared/sharedtestfile.txt
su person9
nano /data/shared/sharedtestfile.txt
```
<img width="1575" height="804" alt="verifynf access of team leads (1)" src="https://github.com/user-attachments/assets/0e783689-a4a2-44a5-b9f1-9a555446fb03" />



# Step 6 — Verify Group Membership
# Confirmed that the correct users are inside the sharedgroup.

<img width="1590" height="898" alt="groups" src="https://github.com/user-attachments/assets/decb7c2d-6d5e-4434-bb35-6a19a97d57f7" />


