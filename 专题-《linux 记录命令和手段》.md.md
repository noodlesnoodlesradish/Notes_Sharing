## 一、使用 shell 历史记录

1. 默认情况下，大多数 Linux shell（如 Bash）会自动记录用户输入的命令。可以通过查看`~/.bash_history`文件来查看历史命令记录。例如，使用`cat ~/.bash_history`命令可以显示历史命令。
2. 设置历史记录大小和保存选项。可以通过修改`~/.bashrc`文件中的`HISTSIZE`和`HISTFILESIZE`变量来调整历史记录的大小。例如，可以将`HISTSIZE`设置为较大的值以保存更多的历史命令。
3. 使用历史命令扩展。可以使用`!!`来重复上一个命令，使用`!n`来重复历史记录中的第 n 个命令，使用`!keyword`来重复最近一个以 “keyword” 开头的命令等。
4. 持久化的命令历史记录。

> 方法1：在`~/.bashrc`文件中添加以下函数
>
> ```bash
> function record_permanent_command {
>  echo "$(date) - $(whoami) - $1" >> ~/permanent_command_history.log
> }
> PROMPT_COMMAND='record_permanent_command "$(history 1)"'
> ```
>
> ```bash
> source ~/.bashrc
> ```
>
> 方法2：在`~/.bashrc`文件中添加以下内容
>
> ```bash
> HISTSIZE=10000
> HISTFILESIZE=10000
> 
> shopt -s histappend
> PROMPT_COMMAND="history -a; history -c; history -r; $PROMPT_COMMAND"
> ```
>
> ```bash
> source ~/.bashrc
> ```

## 二、使用脚本记录

1. 创建一个脚本文件，例如`record.sh`，并在其中添加以下内容：

```bash
#!/bin/bash
while true; do
  date >> log.txt
  whoami >> log.txt
  pwd >> log.txt
  echo "Command: "
  read command
  eval "$command" >> log.txt 2>&1
done
```

这个脚本会不断循环，记录当前日期、用户、当前工作目录和用户输入的命令及其输出到一个名为`log.txt`的文件中。

1. 赋予脚本执行权限：`chmod +x record.sh`。
2. 运行脚本：`./record.sh`。

## 三、使用系统日志

1. 系统日志文件通常会记录一些系统活动和用户操作。例如，`/var/log/auth.log`文件可能会记录用户登录和认证相关的操作。
2. 可以使用工具如`journalctl`来查看系统日志。例如，`journalctl -u service_name`可以查看特定服务的日志，`journalctl -f`可以实时查看日志更新。

## 四、使用第三方工具

1. 有一些第三方工具可以用来记录用户操作，例如`script`命令。使用`script`命令可以将终端会话记录到一个文件中。例如，`script logfile.txt`会将后续的终端会话记录到`logfile.txt`文件中。
2. 安装和使用审计工具，如`auditd`。`auditd`可以记录系统中的各种事件，包括文件访问、进程启动、系统调用等。可以通过配置`auditd`规则来指定要记录的事件类型。