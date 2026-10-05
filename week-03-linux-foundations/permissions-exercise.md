# Week 3 - File Permissions Exercise

## Objective

The purpose of this exercise was to practise Linux file permissions and understand how access is controlled for the file owner, group members, and other users.

The exercise was completed using the Ubuntu Linux virtual machine.

---

## Create the Security Lab Directory

The following commands were used to create a directory and two files:

```bash
cd ~
mkdir security-lab
cd security-lab
touch public.txt
touch secret.txt
```

The `security-lab` directory was created in the user's home directory.

The two files created were:

* `public.txt`
* `secret.txt`

---

## Check the Initial Permissions

The following command was used to view the initial permissions:

```bash
ls -l
```

The output was recorded before changing the permissions.

Example format:

```text
-rw-rw-r-- 1 ushani ushani 0 Oct  2 10:41 public.txt
-rw-rw-r-- 1 ushani ushani 0 Oct  2 10:41 secret.txt
```

---

## Change the File Permissions

The permissions of `secret.txt` were changed to `600`:

```bash
chmod 600 secret.txt
```

The permissions of `public.txt` were changed to `644`:

```bash
chmod 644 public.txt
```

The final permissions were then checked using:

```bash
ls -l
```

The expected permission patterns are:

```text
-rw-------  secret.txt
-rw-r--r--  public.txt
```

---

## Understanding Linux Permission Fields

Linux displays file permissions using a sequence of characters.

For example:

```text
-rw-r--r--
```

The first character indicates the file type:

```text
-    regular file
d    directory
```

The remaining nine characters are divided into three groups:

```text
rw-   r--   r--
 |     |     |
 |     |     └── Other users
 |     └──────── Group
 └────────────── Owner
```

Each permission group contains three positions:

```text
r   w   x
|   |   |
read write execute
```

Therefore:

* `r` = read permission
* `w` = write permission
* `x` = execute permission
* `-` = permission is not granted

---

## `secret.txt` - Permission 600

The command used was:

```bash
chmod 600 secret.txt
```

The permission value `600` means:

```text
Owner:  read + write
Group:  no permissions
Other:  no permissions
```

In symbolic form:

```text
rw-------
```

The numeric values are:

```text
4 = read
2 = write
1 = execute
```

Therefore:

```text
6 = 4 + 2 = read + write
0 = no permissions
0 = no permissions
```

So:

```text
600 = rw------- 
```

This means that the owner can read and modify `secret.txt`, while users in the group and other users do not have permission to access the file.

---

## `public.txt` - Permission 644

The command used was:

```bash
chmod 644 public.txt
```

The permission value `644` means:

```text
Owner:  read + write
Group:  read
Other:  read
```

In symbolic form:

```text
rw-r--r--
```

The numeric values are:

```text
6 = read + write
4 = read
4 = read
```

Therefore:

```text
644 = rw-r--r--
```

This allows the owner to read and modify the file, while group members and other users can read the file but cannot modify it.

---

## Difference Between 600 and 644

The main difference between `600` and `644` is the access given to the group and other users.

| Permission | Owner        | Group     | Other     |
| ---------- | ------------ | --------- | --------- |
| `600`      | Read + Write | No access | No access |
| `644`      | Read + Write | Read      | Read      |

Therefore:

* `600` provides more restricted access.
* `644` allows other users to read the file.
* Neither permission gives execute permission.

---

## Which Setting Is Appropriate for Owner-Only Information?

The `600` permission setting is more appropriate for information that should only be accessible by the file owner.

For example:

```bash
chmod 600 secret.txt
```

This gives the owner read and write access while preventing group members and other users from accessing the file.

This is useful for files containing private or sensitive information where access should be restricted to the owner.

---

## Security Observation

This exercise demonstrated how Linux file permissions can be used as a basic access-control mechanism.

The `secret.txt` file was assigned permission `600`, which restricts access to the owner.

The `public.txt` file was assigned permission `644`, which allows the owner to modify the file while allowing group members and other users to read it.

Using appropriate file permissions helps reduce unauthorized access to information stored on a Linux system.

---

## Commands Used

```bash
cd ~
mkdir security-lab
cd security-lab
touch public.txt
touch secret.txt
ls -l
chmod 600 secret.txt
chmod 644 public.txt
ls -l
```

---

## Conclusion

The file permissions exercise helped me understand how Linux controls access to files using owner, group, and other permissions.

I learned that `600` gives the owner read and write access while denying access to everyone else. I also learned that `644` gives the owner read and write access while allowing group members and other users to read the file.

Therefore, the `600` permission is more suitable for information that should only be accessible to its owner, while `644` can be used when a file needs to be readable by other users.
