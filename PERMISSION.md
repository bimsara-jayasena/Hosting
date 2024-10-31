# Change Ownership of the Directory
This is the most straightforward approach. You can change the ownership of the project directory to your current user to allow file creation without needing elevated privileges.

```bash

sudo chown -R $USER:$USER /path/to/your/project
```
Replace /path/to/your/project with the actual path to your project directory.
$USER will automatically use your current username.
