# Anvi'o
[![https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg](https://www.singularity-hub.org/static/img/hosted-singularity--hub-%23e32929.svg)](https://singularity-hub.org/collections/1659)

Singularity container running Anvi'o.

Example job script:
```
#!/bin/bash
#PBS -N anvio
#PBS -l nodes=1:ppn=1
#PBS -l mem=5gb
#PBS -l walltime=1:00:00
#PBS -j oe

cd $PBS_O_WORKDIR

# tunnel info
PORT=$(shuf -i8000-9999 -n1)
SUBMIT_HOST=$(echo ${PBS_O_HOST%%.*}.rcc.mcw.edu)

# print tunneling instructions
echo -e "
SSH tunnel from your workstation using the following command:

ssh -L ${PORT}:${HOSTNAME}:${PORT} ${USER}@${SUBMIT_HOST}

and point your web browser to http://localhost:${PORT}.
"

# load modules
module load anvio/5.1

# run anvio interactive
anvi-interactive -P ${PORT} -c example.db -p example/PROFILE.db
```
***Please note that the ```-P``` flag is required to assure a unique port. The other file names will vary.

## Start the job
1. Copy contents of anvio.pbs (example above) to a file in your home directory.

2. Open terminal on cluster login node and submit the job script:

```
$ qsub anvio.pbs
```

## Connect to Anvi'o session
1. Check output file (*jobname*.o*JOBNUM*) for details.

Example output file: anvio.o268392
```
SSH tunnel from your workstation using the following command:

ssh -L 9183:node01:9183 tester@loginnode

and point your web browser to http://localhost:9183.
```

2. Open second terminal and run tunneling command from the output file:
```
$ ssh -L 9183:node01:9183 tester@loginnode
```
3. Open a browser and enter the URL from the output file:
```
http://localhost:9183
```

You should now be connected to your Anvi'o browser session that is running on a cluster compute node. To close the session, close the browser window. If you need to reconnect, repeat step 3. If you're done with your session, remember to stop the job with qdel.

