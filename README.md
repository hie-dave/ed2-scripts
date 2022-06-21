# ed-scripts

Scripts for running ED2 on gadi.

## Usage

```bash
./submit -c <config-file>
```

`submit` is the main job submission script, and will submit an ED2 job to PBS. It does not need to be run with the `qsub` command; instead, it will invoke `qsub` internally when it runs ED2 via OpenMPI. The script requires 1 argument - the path to a config file. See the [configuration](#configuration) section for more details.

## Configuration

The script requires a config file which contains the run configuration such as the namelist file, ED2 binary, number of CPUs to be used, etc. An example configuration file is provided in this repository ([`example.conf`](https://github.com/hie-dave/ed2-scripts/blob/master/example.conf)).

The following configuration options are required, and the script will fail gracefully if they are not set, or have invalid values:

- `BINARY`: Path to ED2 binary (relative or absolute)
- `NAMELIST`: Path to namelist (ED2IN) file (relative or absolute)
- `NPROCESS`: Number of CPUs allocated by PBS
- `WALLTIME`: Maximum wall time for job (hh:mm:ss)
- `MEMORY`: Memory allocated to the job by PBS, with units (e.g. 64GB)
- `QUEUE`: PBS queue to which the job will be submitted (run `qstat -q` for a list of available queues)
- `PROJECT`: Project name
- `EMAIL`: Email address of job owner, used for job status updates
- `EMAIL_NOTIFICATIONS`: 1 to receive email notifications when job status changes, 0 otherwise.
- `JOB_NAME`: Job name

The configuration file will be sourced by the submit script, so the syntax should be bash variable syntax - ie `name=value` on each line, and lines starting with `#` are comments. Note that, because the file will be sourced, tilde (~) expansion for home directory will not work. Use `${HOME}` instead.

## Output

Standard output and error streams from the job will be written to a single file called `<JOB_NAME>.log`, inside the same directory as the namelist (ED2IN) file.
