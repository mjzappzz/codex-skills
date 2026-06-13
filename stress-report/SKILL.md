---
name: stress-report
description: Generate evidence-based server stress test reports from remote Linux server stress data, especially ~/stress CPU, memory, GPU, and disk test outputs. Use when Codex is asked to run, summarize, validate, archive, sync, or regenerate server pressure/stability reports from stress-ng, gpu-burn, nvidia-smi, fio/iostat, CSV, TXT, LOG, or XLSX result files.
---

# Stress Report

## Scope

Use this skill for server stress test reports, including:

- CPU and memory stress data.
- GPU stress data.
- Disk stress data.
- Combined server stress reports.
- Report archive naming and organization.

Do not use this skill for SSH onboarding. Server connection/key workflow stays in `AGENTS.md`.

## Core Rules

- Do not invent data.
- Only report hardware, software, logs, metrics, and conclusions actually detected or provided.
- If a hardware category is absent, write direct status in that section, such as `未检出 GPU`.
- Do not add a final missing-items section for customer-facing reports.
- Prefer command output, original TXT reports, CSV summaries, and logs over assumptions.
- Keep the report engineering-focused. Avoid background explanation.
- If original report says PASS and no contradictory evidence is found, preserve PASS.
- If evidence is insufficient for a test category, do not mark PASS; write the actual detected state and stop there.
- Keep original units: `%`, `MB/s`, `IOPS`, `ms`, `MiB`, `GiB`, `W`, `Gflop/s`, `°C`.
- Before running any stress test, output a plan and require user confirmation for all variable parameters.
- User-controlled variables include at least: CPU+memory duration, GPU duration, disk duration, sample interval, optional disk test target mountpoint/device/directory, and whether to skip any test category.
- Treat the confirmed remote stress directory as the common working directory for CPU, GPU, disk scripts, raw data, logs, and final reports.
- Disk stress target is separate from the common working directory. By default, disk stress targets the confirmed remote stress directory itself, which means it tests the filesystem/disk where that directory resides.
- If the user explicitly specifies another disk target such as `/data`, `data`, a mountpoint, or a directory, resolve it to a mounted filesystem path and run disk stress there, so the requested mounted disk is tested.
- When using a user-specified mounted path for disk stress, prefer a test subdirectory under that path unless the user explicitly asks to write directly in the mount root.
- Before running disk stress, verify `df`/`lsblk` for the target path and report the resolved mountpoint/device in the plan.
- Do not run disk stress directly against a raw block device unless the user explicitly requests raw-device testing and acknowledges data risk.
- If required variables are missing, stop at the plan/confirmation stage. Do not start background jobs.

## Chat Output Rules

When running remote stress tests, keep user-facing updates short and operational.

Use an execution-ticket style instead of verbose field blocks. Prefer one headline line plus one optional explanation line:

```text
[CPU+内存压测] 运行中 0%｜预计 2 分钟
说明：已启动远端后台任务。
```

Completed stage:

```text
[CPU+内存压测] PASS
说明：stress-ng 退出码 0，未发现重大异常。
```

Skipped stage:

```text
[GPU预检] 跳过
原因：nvidia-smi / nvcc 不可用。
```

Failure or blocker:

```text
[磁盘压测] FAIL
关键错误：...
处理方案：...
是否执行：...
```

Use stable stage names. Prefer these names unless the task requires fewer stages: `计划确认`, `目录准备`, `脚本上传`, `CPU+内存压测`, `GPU预检`, `磁盘压测`, `报告生成`, `报告同步`, `结果验证`.

Use `PASS` / `FAIL` only for stages that have a real validation result, such as CPU+memory stress, GPU stress/preflight, disk stress, report sync verification, and residual process verification. For operational stages without a test verdict, use `待确认`, `完成`, `运行中`, `跳过`, or `阻塞`.

Do not include raw commands, long logs, polling details, CSV row counts, process lists, or progress bars in the explanation line.

For long-running stress tests, default to low-noise timer progress plus quiet remote verification. Do not report normal polling details such as process counts, CSV row counts, sample counts, repeated "running" status, or unchanged log tails. User-facing updates should only be emitted for:

- plan / confirmation.
- stage start.
- compact 0% / 25% / 50% / 75% / 100% timer progress for fixed-duration CPU, GPU, and disk stress stages.
- stage PASS / FAIL / skipped.
- actionable blocker.
- final report paths and verification status.

Do not emit repeated `运行中` updates during normal polling. Exception: for a confirmed fixed-duration stress stage, show low-noise local timer progress at 0%, 25%, 50%, 75%, and 100%. Progress is based on the planned duration, not remote polling output. At 100%, run one remote completion check and report PASS / FAIL / skipped.

Progress rules:

- Use a single compact line per progress update, such as `[CPU+内存压测] 运行中 50%｜已运行 1 分钟`.
- For each fixed-duration stage, emit only 0%, 25%, 50%, 75%, and 100% progress updates unless a blocker occurs.
- Emit progress points sequentially during the stage. Do not print all progress points at once before time has elapsed.
- Do not attempt real-time or same-line progress bars; chat UIs render each update as a new message.
- Do not include SSH commands, process lists, log tails, CSV counts, or raw polling output in progress updates.
- If the planned duration has elapsed but the remote job is still finishing report generation, output one concise `[阶段] 收尾中` update, then check again after a short delay.

Avoid long-lived foreground remote polling commands when the UI would surface every wait event to the user. Start the remote stress job in the background, run the local 0% / 25% / 50% / 75% / 100% timer progress, then perform discrete quiet remote status checks only at the planned completion time or on failure suspicion. Suppress normal polling command text and do not expose repeated `Waited for background terminal` style updates as progress.

Do not use the old verbose field format (`阶段` / `状态` / `摘要` / `下一阶段`) unless the user explicitly requests it.

Do not print noisy intermediate command output unless the user explicitly asks. Suppress or summarize:

- full `ssh` / `scp` command text.
- `pgrep` output.
- `tail` output.
- `find` file lists.
- normal successful upload/download output.
- long script logs, CSV, XLSX, or TXT report contents.
- generated final report body/content.

When GPU preflight fails, do not ask the user to choose. Report a compact status and continue:

```text
[GPU预检] 跳过
原因：nvidia-smi / nvcc 不可用。
```

Final response should include only:

- CPU + memory result.
- GPU result or skip reason.
- disk result.
- report path.
- raw data path.
- remote sync path and verification status.
- residual stress process status.

Do not paste or display the generated final report content in chat unless the user explicitly asks to view it. After generating a report, state that it was generated and provide local and remote paths plus verification status.

## Default Remote Data Layout

When connected to a Linux server, inspect the confirmed remote stress directory first.

Default remote stress directory:

```bash
~/stress
~/stress/cpu
~/stress/gpu
~/stress/disk
```

Before running a new stress test, tell the user the resolved directory, such as `/root/stress` for root or `/home/USER/stress` for a normal user, and ask for confirmation if the user has not already approved it.

If the confirmed stress directory already exists and contains files or subdirectories, stop before changing it and report:

```text
远端压测目录已存在且非空:
PATH

请选择:
1. 清空该目录后重新压测
2. 创建新的目录压测
```

For option 1, clear only the confirmed stress directory. For option 2, create a timestamped sibling or child directory such as `~/stress_YYYYMMDD_HHMMSS` or `~/stress/run_YYYYMMDD_HHMMSS`, then use that directory consistently for scripts, raw data, logs, report sync, and final paths. Do not silently merge new stress output into a non-empty existing directory.

Common files:

```bash
cpu_mem_report_*.txt
cpu_mem_monitor_*.csv
stress_cpu_mem_*.log
cpu_mem_error_*.log

gpu_stress_report_*.txt
gpu_monitor_*.csv
stress_gpu_*.log

disk_stress_report_*.txt
disk_monitor_*.csv
stress_disk_*.log
disk_error_*.log
```

## Bundled Stress Scripts

Reusable scripts are stored in this repo:

```text
scripts/stress/cpu_mem_stress_report.sh
scripts/stress/gpu_stress_report.sh
scripts/stress/disk_stress_report.sh
```

When the user asks Codex to run stress tests on a server, copy only the needed scripts to the remote user's stress directory:

```bash
~/stress/cpu/cpu_mem_stress_report.sh
~/stress/gpu/gpu_stress_report.sh
~/stress/disk/disk_stress_report.sh
```

Default output directories:

```text
~/stress/cpu/
~/stress/gpu/
~/stress/disk/
```

For disk stress tests, write scripts, logs, CSV, TXT, and XLSX directly under `~/stress/disk/`.

Skip hard disk speed-only scripts. Use only the disk random-write stability script for disk pressure testing.

If an orchestration script is needed to chain stages, place it in `/tmp` or delete it after completion. Do not leave orchestration `.sh` files in `~/stress` unless the user explicitly asks to keep them.

## Remote Execution Mode

Run stress tests as background jobs, not foreground SSH commands, to avoid client/tool timeouts.

For each stage:

1. Copy the script into its target directory.
2. Do not convert line endings by default. The bundled scripts are stored as Linux LF.

Only run line-ending cleanup if execution reports CRLF/BOM symptoms such as `/bin/bash^M`, `$'\r': command not found`, or `bad interpreter`:

```bash
python3 - <<'PY'
from pathlib import Path
for p in Path("~/stress").expanduser().rglob("*.sh"):
    data = p.read_bytes().replace(b"\xef\xbb\xbf", b"").replace(b"\r\n", b"\n").replace(b"\r", b"\n")
    p.write_bytes(data)
    p.chmod(0o755)
PY
```

3. Start with `nohup` and write a runner log:

```bash
cd ~/stress/cpu && nohup ./cpu_mem_stress_report.sh DURATION INTERVAL > run_cpu_mem_YYYYMMDD_HHMMSS.log 2>&1 & echo $!
cd ~/stress/gpu && nohup ./gpu_stress_report.sh DURATION INTERVAL > run_gpu_YYYYMMDD_HHMMSS.log 2>&1 & echo $!
cd ~/stress/disk && nohup ./disk_stress_report.sh DURATION INTERVAL TEST_DIR > run_disk_YYYYMMDD_HHMMSS.log 2>&1 & echo $!
```

4. Poll every 30-60 seconds with `pgrep` and latest report/log checks.
5. Start the next stage only after the previous stage has completed and produced its report.
6. If the runner exits without a report, inspect the latest `run_*.log` and report the exact error.

For multi-stage orchestration, keep one runner log only:

```text
~/stress/run_all_YYYYMMDD_HHMMSS.log
```

Do not create a separate `orchestrator_*.log` when stdout/stderr is already redirected to a `run_*.log`; it duplicates the runner log and should not be retained.

Recommended polling commands:

```bash
pgrep -af 'cpu_mem_stress_report|gpu_stress_report|disk_stress_report|stress-ng|gpu_burn' || true
find ~/stress -type f \( -name '*_report_*.txt' -o -name 'run_*.log' \) -printf '%p\t%s bytes\t%TY-%Tm-%Td %TH:%TM\n' | sort
```

GPU stress preflight is mandatory. Before running GPU stress, check:

```bash
nvidia-smi
nvcc -V
```

If `nvidia-smi` or `nvcc -V` fails, skip GPU stress automatically, record the exact missing command or error in the runner log and final report, and continue with the remaining non-GPU stages. Do not ask the user to choose unless the whole task depends on GPU validation. Do not attempt to install GPU drivers or CUDA automatically. Do not precheck `/opt/software/gpu-burn/gpu_burn`; the GPU stress script handles gpu-burn build/use itself after driver and CUDA are ready.

## Required Checks

Collect only what is needed:

```bash
STRESS_DIR="$HOME/stress"
hostname
date '+%F %T %Z'
uptime
cat /etc/os-release
uname -r
lscpu
free -h
lsblk -d -o NAME,MODEL,SIZE,TYPE
df -hT "$HOME" "$STRESS_DIR" 2>/dev/null
nvidia-smi --query-gpu=index,name,driver_version,memory.total,power.limit --format=csv,noheader,nounits 2>/dev/null
find "$STRESS_DIR" -maxdepth 3 -type f -printf '%p\t%s bytes\t%TY-%Tm-%Td %TH:%TM\n' | sort
```

Set `STRESS_DIR` to the confirmed stress directory before running checks.

## Report Content

Use Markdown by default.

Recommended sections:

1. 服务器硬件配置总览
2. 报告摘要
3. 测试对象与环境
4. 测试范围与方法
5. 结果总览
6. CPU + 内存压测
7. GPU 压测
8. 磁盘压测
9. 异常、风险与处理建议
10. 综合结论
11. 原始数据与附件索引

Include a section only when evidence exists.

## Recommended Template Details

### 服务器硬件配置总览

Place this first so customers immediately see the full machine configuration.

Include detected items in compact tables:

- 主机名、服务器 IP、厂商、型号、主板型号、BIOS 版本/日期.
- CPU model, sockets, cores, threads, logical CPUs, NUMA.
- Memory total, memory type/speed/vendor/part number if detected.
- GPU model/count/driver/CUDA/memory/power limit if detected; if no GPU, write `未检出 GPU`.
- Disk name/model/size/type, mountpoint, filesystem.
- Network adapters only if detected and relevant.

Do not include serial numbers in customer reports unless the user explicitly asks.

### 报告摘要

Use a compact summary table:

| 项目 | 结果 |
|---|---|
| 服务器 IP | detected IP |
| 主机名 | hostname |
| 测试结论 | PASS / FAIL |
| 覆盖项目 | CPU / 内存 / GPU / 磁盘 |
| 主要风险 | 无 / list actual risks |
| 报告时间 | YYYY-MM-DD HH:MM:SS |

### 测试对象与环境

Include:

- 操作系统、内核、运行时间、当前负载。
- 测试数据路径。
- 测试开始/结束时间、计划时长、实际采样数、采样间隔。
- 测试工具和版本 when detected.

### 硬件与软件配置

If a separate hardware overview already exists, use this section only for software/runtime versions:

- Performance-relevant software: stress-ng, gpu-burn, fio, nvidia-smi, CUDA Toolkit if detected.

### 测试范围与方法

Use a matrix:

| 类型 | 工具 | 参数/模式 | 线程/并发 | 时长 | 数据校验 | 原始日志 |
|---|---|---|---:|---:|---|---|

Do not invent parameters. If the original script/report provides parameters, preserve them.

### 结果总览

Use a PASS matrix:

| 项目 | 结论 | 关键指标 | 异常 |
|---|---|---|---|
| CPU + 内存 | PASS | load / memory / swap | none |
| GPU | PASS | util / temp / power / errors | none |
| 磁盘 | PASS | MB/s / IOPS / await / util | none |

### Per-Test Metrics

For each test, include available statistics:

- Start time, end time, duration, sample count.
- Average, maximum, minimum for core metrics.
- p95/p99 only if available or safely computed from CSV.
- Exit code.
- Error count or serious log findings.

CPU / memory metrics:

- load1/load5/load15 average and maximum.
- memory used average/maximum.
- memory available minimum.
- swap used maximum.
- stress-ng exit code.

GPU metrics:

- Gflop/s average/min/max if gpu-burn output exists.
- utilization.gpu average/min/max.
- temperature average/min/max.
- power.draw average/min/max.
- memory.used average/min/max.
- gpu-burn errors, exit code.
- Xid/CUDA/device lost findings if logs expose them.

Disk metrics:

- test target, mountpoint, source device, filesystem.
- rw mode, block size, iodepth, numjobs when fio data exists.
- bandwidth average/max.
- IOPS average/max.
- await/latency average/max/p95/p99 when available.
- util maximum.
- filesystem/device/controller errors.

### 异常、风险与处理建议

Only include actual findings. Suggested categories:

| 级别 | 项目 | 证据 | 建议 |
|---|---|---|---|

If no findings:

```text
未发现影响本次压测结论的重大异常。
```

### 原始数据与附件索引

Always list source files used:

| 类型 | 路径 | 用途 |
|---|---|---|

Include TXT, CSV, LOG, XLSX paths when present.

## Pass Criteria

CPU / memory PASS requires evidence such as:

- stress-ng exit code 0.
- No OOM, MCE, ECC uncorrected error, thermal throttling, verification failed, segmentation fault, or killed process.
- Swap usage within the test script threshold.

GPU PASS requires evidence such as:

- gpu-burn exit code 0.
- gpu-burn error count 0.
- No CUDA error, Xid error, GPU lost, or device reset.
- nvidia-smi detects the GPU.

Disk PASS requires evidence such as:

- stress-ng/fio exit code 0.
- No serious I/O error, filesystem error, controller error, or data verification error.
- High await or util alone is not failure; report it as performance observation unless original script marks failure.

## Data Extraction Rules

- Prefer existing `*_report_*.txt` summary values when they are internally consistent.
- Use CSV to verify start/end time, sample count, and obvious min/avg/max values.
- If TXT and CSV disagree, report the discrepancy in 异常、风险与处理建议 instead of silently choosing one.
- Treat zero-utilization startup samples as valid unless the original report excludes warm-up samples.
- Do not calculate business-level capacity conclusions from synthetic stress data.
- Do not compare against internet benchmark rankings unless the user explicitly asks.
- If charts are requested, generate simple line charts for utilization/temperature/power/load/bandwidth and save them beside the report.

## Quality Gate Before Finalizing

Before delivering a report, verify:

- The report filename follows the naming rule.
- The archive directory exists.
- Every PASS/FAIL has evidence.
- Every hardware/software item appears in command output or source files.
- Every source path listed actually exists on the server or was provided by the user.
- No temporary tool logs or chat text are included.
- Customer-facing final output does not include a generic `未检测 / 未验证项` section.

## Output Location And Naming

Local archive directory:

```text
服务器压测报告存档/
```

Filename:

```text
IP_服务器压测报告_YYYY-MM-DD_HHMMSS.md
```

If the user provides a customer name:

```text
客户名_IP_服务器压测报告_YYYY-MM-DD_HHMMSS.md
```

Preferred customer-facing filename when the user provides a customer name:

```text
客户名_服务器压测报告_YYYY-MM-DD_HHMMSS.md
```

Use the customer name instead of the IP at the front of the filename and report title. Example: `阿里云_服务器压测报告_2026-06-03_143012.md`.
The Markdown H1 title must also use the customer name: `# 客户名 服务器压测报告`. Do not use `# IP 服务器压测报告` when a customer name is provided.

If syncing back to the server, use:

```text
CONFIRMED_STRESS_DIR/LOCAL_REPORT_FILENAME
```

Default behavior: keep reports locally and sync one copy back to the confirmed remote stress directory unless the user explicitly says not to sync. The remote synced report filename must match the local report filename exactly, including customer name and timestamp, unless the user explicitly requests another filename. After syncing, verify the remote file with `ls -lh CONFIRMED_STRESS_DIR/LOCAL_REPORT_FILENAME`. If the sync or verification fails, report `FAIL` and the key error line.

## Report Style

- Use concise tables.
- In customer-facing reports, bold important values in hardware overview and summary tables, especially IP, hostname, server model, CPU, memory, GPU, disks, test conclusion, and main risk.
- Put PASS/FAIL/未验证 clearly near each test section.
- Preserve raw file paths in the 原始数据 section.
- Do not include tool work logs or conversational history.
- Do not add generic procurement, acceptance, or unrelated software sections.
