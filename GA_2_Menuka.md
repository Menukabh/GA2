## Part A: Starting a Git repository
mkdir GA2
cd GA2
mkdir data results
touch README.md

1. Add a header to your README file to indicate that this file (and dir) is for your assignment.<br> 
echo "# Header for README file" > README.md

2. Initialize a Git repository inside your new directory.  
    ```bash
    git init
    Initialized empty Git repository in /fs/ess/PAS2880/users/menuka/GA2/.git/
    ```

3. Add & stage and then commit the README file with an appropriate commit message.
    ```bash
    git add README.md
    echo "This class is fun" >> README.md
    git status
    git commit -m "About the class"
    ```
    git log

# Part B: Exploring the GTF file with Unix data tools

4. Copy the genome annotation GTF file GCF_016801865.2.gtf.gz that is in your copy of the Garrigós et al. (2025) data to a file data/annot.gtf.gz (i.e., into the data dir you just created).
```bash
cp ../garrigos-data/ref/GCF_016801865.2.gtf.gz data/annot.gtf.gz 
```
5.  Check the file size of annot.gtf.gz. Then, decompress the GTF file with the gunzip command below. Finally, check the size of the resulting annot.gtf file. How large is the size difference between the compressed and uncompressed files?

    ```bash
    ls -lh data/annot.gtf.gz
    -rw-rw----+ 1 menuka PAS0471 5.1M Sep 19 15:35 data/annot.gtf.gz
    gunzip data/annot.gtf.gz
    ls -lh data/annot.gtf
    -rw-rw----+ 1 menuka PAS0471 123M Sep 19 15:35 data/annot.gtf
    5.1M to 123M after decompressing
    ```

6. The total number of “genomic features” (genes, exons, etc.) listed in a GTF file is simply the number of lines in its main/table section.

- Count the total number of lines in annot.gtf.
- Take a look at the contents of the file and count the number of header lines by eye.
- Based on your knowledge of what the GTF header lines look like, combine grep -v and wc -l to count the number of genomic features in the file.
    ```bash 
    wc -l data/annot.gtf
    408402 data/annot.gtf
    cat data/annot.gtf | head -n 10
    less data/annot.gtf | head -n 10
    4
    grep -c '^#' data/annot.gtf
    grep -v '^#' data/annot.gtf | wc -l
    408397
    ```

7. Does the difference between the two line counts you just performed correspond to the number of header lines you counted visually? If not, can you figure out which other line(s) than the header lines contain the pattern you used with grep above?

    ```bash
    No, because the tail had the # sign too
    grep -n '#' data/annot.gtf
    cat data/annot.gtf | tail -n 10
    ```

8. How many distinct “sequences” (chromosome/scaffolds/contigs) are represented in the annotation file? Store a list of these distinct sequences in a file results/scaffolds.txt.
    ```bash
    grep -v "^#" data/annot.gtf | cut -f 1 | uniq | wc -l
     grep -v "^#" data/annot.gtf | cut -f 1 | uniq > results/scaffolds.txt
    ```
167

9. Print a “count table” (like we did towards the end of this section) of feature types in the annotation: this should have a count of the number of times each feature type occurs. (Such a table will for example show how many genes have been annotated in this genome – quite useful!)
    ```bash
    grep -v "^#" data/annot.gtf | cut -f 3 | sort | uniq -c
    141444 CDS
    166177 exon
    19673 gene
    25864 start_codon
    25892 stop_codon
    29347 transcript
    ```

10. Check the status of your Git repository. You should see that your README has changed but also that Git has detected the files in the data and results dirs. You don’t want to commit any files in the data and results dirs, so create a .gitignore file that makes Git ignore them. Check the status of your repository again – is your .gitignore file working as intended?
##my results directory was empty so the git statuts was not showing anything in it
```bash
echo "data/" > .gitignore
echo "results/" >> .gitignore
I need to stage it for it to work
git status
```

11. Stage and commit the .gitignore file.
    ```bash
    git add .gitignore
    git commit -m "Gitignore file added"
    ```

12. Stage and commit the README.md file again.
    ```bash
    git add README.md
    git commit -m "File updated"
    ```

Part B: Exploring the FASTQ files with Unix data tools
13. Copy the FASTQ file ERR10802863_R1.fastq.gz of the Garrigós et al. (2025) data to a file of the same name inside the same data dir you copied the GTF file into.
    ```bash
    cp ../garrigos-data/fastq/ERR10802863_R1.fastq.gz data/
    ```

14. Check the status of your Git repository. Does the FASTQ file show up? Why/why not?
git status
The fastq file does not show up because it is inside data/ which is asked to ignore in .gitignore file

15. How many reads does the FASTQ file contain?
    ```bash
    zcat data/ERR10802863_R1.fastq.gz | grep "@" | wc -l
    zgrep "@" data/ERR10802863_R1.fastq.gz | wc -l
    ```
    500000

16. How many reads in the FASTQ file contain the sequence ACGT? (You can assume this string does not occur outside of the lines with sequences.)
    ```bash
    zgrep -c "ACGT" data/ERR10802863_R1.fastq.gz
    ```
    100465

17. How many reads contain at least 10 consecutive Ns (uncalled bases)? (You can assume that stretches of N’s do not occur in the sequence quality lines.)
```bash
    zgrep -c "NNNNNNNNNN" data/ERR10802863_R1.fastq.gz
    zgrep -A 1 "@" data/ERR10802863_R1.fastq.gz | grep -c "NNNNNNNNNN"
```
32612

18. Using the read length information in the FASTQ read header lines, print a count table of read lengths.
```bash
    zgrep "@" data/ERR10802863_R1.fastq.gz | cut -d '=' -f 2 | sort | uniq -c | head -n 10
```


19. Compare the number you got in the previous question with the number of 35-bp reads from the question before that. What does this seem to tell you? To confirm, can you check what a sample of 35-bp reads look like?
```bash
    zgrep "length=35" data/ERR10802863_R1.fastq.gz | wc -l
    zgrep -c "length=35" data/ERR10802863_R1.fastq.gz 
    zgrep -A 1 "length=35" data/ERR10802863_R1.fastq.gz | head -n 10
    32678
```

20. Stage and commit the README.md file again.
```bash
    git add README.md
    git commit -m "End of recitation"
```
