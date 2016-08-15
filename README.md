# easySFS
Convert VCF to dadi/fastsimcoal style SFS for demographic analysis

## Install & Run
The script assumes you have matplotlib and dadi installed.
* Clone this repo
** `git clone https://github.com/isaacovercast/easySFS.git`
* `cd easySFS`
* `chmod 777 easySFS`
* `./easySFS`

## General workflow
Converting VCF to SFS is a 2 step process. The first step is to run a preview to identify the values for projecting down each population. The second step is to actually do the conversion specifying the projection values. It looks like this:

`./easySFS -i input.vcf -p pops_file.txt --preview`

Which will output something like this:
```
Pop1
(2, 45.0)   (3, 59.0)   (4, 58.0)   (5, 49.0)   (6, 41.0)   (7, 35.0)   (8, 27.0)   (9, 20.0)   (10, 13.0)  (11, 8.0)   (12, 8.0)   (13, 5.0)   (14, 2.0)   (15, 2.0)   (16, 1.0)   

pop2
(2, 68.0)   (3, 96.0)   (4, 106.0)  (5, 110.0)  (6, 108.0)  (7, 89.0)   (8, 76.0)   (9, 66.0)   (10, 56.0)  (11, 49.0)  (12, 42.0)  (13, 39.0)  (14, 34.0)  (15, 29.0)  (16, 27.0)  (17, 26.0)  (18, 24.0)  (19, 23.0)  (20, 21.0)  (21, 22.0)  (22, 20.0)  (23, 19.0)  (24, 16.0)  (25, 16.0)  (26, 15.0)  (27, 15.0)  (28, 13.0)  (29, 13.0)  (30, 14.0)  (31, 14.0)  (32, 14.0)  (33, 13.0)  (34, 12.0)  (35, 9.0)   (36, 9.0)   (37, 8.0)   (38, 8.0)   (39, 8.0)   (40, 6.0)   (41, 6.0)   (42, 6.0)   (43, 5.0)   (44, 5.0)   (45, 5.0)   (46, 4.0)   (47, 4.0)   (48, 4.0)   (49, 3.0)   (50, 3.0)   (51, 3.0)   (52, 3.0)   (53, 3.0)   (54, 3.0)   (55, 2.0)   (56, 2.0)   (57, 2.0)   (58, 2.0)   (59, 2.0)   (60, 2.0)   (61, 2.0)   (62, 0.0)   (63, 0.0)   (64, 0.0)   (65, 0.0)   (66, 0.0)   (67, 0.0)   (68, 0.0)   (69, 0.0)   (70, 0.0)   (71, 0.0)   (72, 0.0)   (73, 0.0)   (74, 0.0)   (75, 0.0)   (76, 0.0)
```
Each column is the number of samples in the projection and the number of segregating sites at that projection value.
The dadi manual recommends maximinzing the number of segregating sites, but at the same time if you have lots of missing data then you might have to balance # of segregating sites against # of samples to avoid downsampling too far.

Next run the script with the values for projecting for each population, like this:

`./easySFS -i input.vcf -p pops_file.txt --proj 12,20`

## Outputs
If you specify the `-o` flag you can pass in an output directory which will be created, otherwise output files are written to the default directory `output`. There will be two directories created here `dadi` and `fastsimcoal2`

## Running example files




## Usage
You can get usage info any time by: `./easySFS.py`
```
usage: easySFS.py [-h] [-a] -i VCF_NAME -p POPULATIONS [--proj PROJECTIONS]
                  [--preview] [-o OUTDIR] [--ploidy PLOIDY] [--prefix PREFIX]
                  [--GQ GQUAL] [-f] [-v]

optional arguments:
  -h, --help          show this help message and exit
  -a                  Keep all snps (default == False)
  -i VCF_NAME         name of the VCF input file being converted
  -p POPULATIONS      Input file containing population assignments per
                      individual
  --proj PROJECTIONS  List of values for projecting populations down to
                      different sample sizes
  --preview           Preview the number of segragating sites per population
                      for different projection values.
  -o OUTDIR           Directory to write output SFS to
  --ploidy PLOIDY     Specify ploidy. Default is 2. Only other option is 1 for
                      haploid.
  --prefix PREFIX     Prefix for all output SFS files names.
  --GQ GQUAL          minimum genotype quality tolerated
  -f                  Force overwriting directories and existing files.
  -v                  Set verbosity. Dump tons of info to the screen
```
