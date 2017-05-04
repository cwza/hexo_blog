title: Supervisord run command before stop
categories: linux
tags: [linux, shell, supervisord]
date: 2017-05-04 14:46:57
---

Supervisord requires that the programs it is configured to run don’t daemonize themselves. Instead, they should run in the foreground and respond to the stop signal (TERM by default) by properly shutting down.


Use Following shell script to wrapper your background program.
``` sh
#!/bin/bash

# run something background
./run &
# get nearest running background pid
$PID=$!

# Perform program exit housekeeping
function clean_up {
  # dosomething for cleanup
  rm -rf dump_file
  # remove trap
  trap - SIGHUP SIGINT SIGTERM SIGQUIT SIGKILL
  exit $?
}

# Add trap for catch sistem signal
trap clean_up SIGHUP SIGINT SIGTERM SIGQUIT SIGKILL
wait $PID
```