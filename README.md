## This is a Header

git init
git add README.md 
git commit -m "This class is fun"

mkdir data results

cp ../garrigos-data/meta

ls -lh data/annot.gtf.gz
gunzip data/annot.gtf.gz
ls -lh data/

wc -l data/annot.gtf
cat data/annot.gtf | head -n 10

grep -v "#"  data/annot.gtf | wc -l
408402 data/annot.gtf
408397

grep -n "#"  data/annot.gtf

grep -v "#" data/annot.gtf | cut -f 1 | sort | uniq

grep -v "#" data/annot.gtf | cut -f 1 | sort | uniq > results/scaffolds.txt

grep -v "#" data/annot.gtf | cut -f 3 | sort | uniq -c

echo "data/" >> .gitignore
echo "results/" > .gitignore

zcat data/ERR10802863_R1.fastq.gz | wc -l
zgrep "@" data/ERR10802863_R1.fastq.gz | wc -l

zgrep -c "ACGT" data/ERR10802863_R1.fastq.gz
zgrep -c "NNNNNNNNNN" data/ERR10802863_R1.fastq.gz


zgrep "@" data/ERR10802863_R1.fastq.gz | cut -d '='  -f 2 | sort | uniq -c

zgrep "length=35" data/ERR10802863_R1.fastq.gz | wc -l

zgrep "length=35" data/ERR10802863_R1.fastq.gz | head -n 10
zgrep -A 1 "length=35" data/ERR10802863_R1.fastq.gz | head -n 10




