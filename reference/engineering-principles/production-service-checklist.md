| Engineering Question          | systemd Directive   | Why it Matters                                               |
| ----------------------------- | ------------------- | ------------------------------------------------------------ |
| What is this service?         | `Description=`      | Helps operators immediately identify the service.            |
| What starts the application?  | `ExecStart=`        | Defines the executable systemd will supervise.               |
| Who should run it?            | `User=`             | Applies least privilege instead of running as root.          |
| Where should it execute?      | `WorkingDirectory=` | Prevents failures caused by incorrect relative paths.        |
| What happens if it crashes?   | `Restart=`          | Improves operational resilience without manual intervention. |
| Should it start after reboot? | `WantedBy=`         | Makes the service part of the normal boot sequence.          |


**Engineers do not memorize directives. They ask operational questions. The directives are simply the answers.**
