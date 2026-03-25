Most people never think about packet size.

Default ethernet packets max out at 1500 bytes. Jumbo frames push that to 9000.

Same data, fewer packets, less overhead.

The catch? Every single device in the path needs to support it. Miss one and packets get dropped in ways that are really hard to debug.

Great inside a data centre. Risky everywhere else.

#Networking #DevOps #LearningInPublic
