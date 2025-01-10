# 在 AWS Ubuntu 实例中禁用 "Last login" 消息

在 AWS 的 Ubuntu 实例中，默认设置是在每次 SSH 登录时都会显示 "Last login" 消息。然而，有时候，用户可能希望禁用这个消息。本文将介绍如何通过修改 SSH Daemon 的配置来实现这个目标。

**背景**

"Last login" 消息是由 PAM（Pluggable Authentication Modules）和 SSH Daemon 生成的，这个消息的生成并不是在 `.bashrc` 文件中控制的。虽然 `.bashrc` 文件可以控制很多登录时的行为，但是 "Last login" 消息并不在其中。

**解决方法**

要禁用 "Last login" 消息，你需要编辑 `/etc/ssh/sshd_config` 文件。以下是具体的步骤：

1. 使用 root 用户（或具有 sudo 权限的用户）登录到你的 AWS Ubuntu 实例。

2. 打开 `/etc/ssh/sshd_config` 文件进行编辑。例如，你可以输入以下命令使用 Vim 来编辑文件：
```bash
sudo vim /etc/ssh/sshd_config
```

3. 在这个文件中找到 `PrintLastLog` 这一行。如果找不到，你可以在文件的底部添加新的一行。

4. 将 `PrintLastLog` 的值设置为 `no`。所以这一行应该看起来像这样：
```bash
PrintLastLog no
```

5. 在 Vim 中，按 `Esc` 键，然后输入 `:wq` 并按 `Enter` 键来保存并关闭文件。

6. 为了使这个更改生效，你需要重新启动 SSH 服务。你可以使用以下命令来做这件事：
```bash
sudo service ssh restart
```

完成以上步骤后，当你再次登录到你的 Ubuntu 实例时，就不会再看到 "Last login" 消息了。





scp ubuntu@13.239.53.17:~/.ssh/id_rsa E:\desktop\Downloads
