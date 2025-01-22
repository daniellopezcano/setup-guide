shows the status of your job and a forecast of when it will start
```
squeue -u dlopez --start
```

shows stats for running or completed jobs (mem usage, consumed cpu hours...)
```
seff jobid
```

slurm score for priority (1 = maximum priority, 0 = minimum priority)
```
sshare -u dlopez
```

 jobs run by user
```
sacct -S 2022-08-15 -u dlopez
```