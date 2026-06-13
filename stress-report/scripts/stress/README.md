# Stress Script Execution Notes

Run remote stress stages as background jobs to avoid SSH/client timeouts.

Default remote layout:

```text
~/stress/cpu/
~/stress/gpu/
~/stress/disk/
```

Start examples:

```bash
cd ~/stress/cpu
nohup ./cpu_mem_stress_report.sh 3600 2 > run_cpu_mem_$(date +%Y%m%d_%H%M%S).log 2>&1 & echo $!

cd ~/stress/gpu
nohup ./gpu_stress_report.sh 3600 2 > run_gpu_$(date +%Y%m%d_%H%M%S).log 2>&1 & echo $!

cd ~/stress/disk
nohup ./disk_stress_report.sh 3600 2 ~/stress/disk > run_disk_$(date +%Y%m%d_%H%M%S).log 2>&1 & echo $!
```

Poll:

```bash
pgrep -af 'cpu_mem_stress_report|gpu_stress_report|disk_stress_report|stress-ng|gpu_burn' || true
find ~/stress -type f \( -name '*_report_*.txt' -o -name 'run_*.log' \) -printf '%p\t%s bytes\t%TY-%Tm-%Td %TH:%TM\n' | sort
```
