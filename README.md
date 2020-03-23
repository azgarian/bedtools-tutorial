#### 1. Check the bedtools version

`bedtools --version`

#### 2. Which exon and intron regions contains a damage site?

`bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_damage.bed | less`

#### 3. Which exon and intron regions contains a damage site? 

Give the original info of damage sites as well.

`bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_damage.bed -wa -wb | less`

#### 4. Which exon and intron regions contains a damage or repair site? 

Give the original info of damage or repair sites as well.

`bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_damage.bed UV_repair.bed -wa -wb | less`

#### 5. Which exon and intron regions do NOT contain a damage or repair site? 

`bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_damage.bed UV_repair.bed -v | less`

#### 6. Which exon and intron regions contains a damage or repair site at the same strand? 

Give the original info of damage or repair sites as well.

`bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_damage.bed UV_repair.bed -wa -wb -s | less`

#### 7. Which exon and intron regions contains a damage site? How many base pairs overlapping?

`bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_damage.bed -wo | less`

#### 8. Which exon and intron regions contains a repair site? How many base pairs overlapping?

Get the region only if at least half of the repair site is overlapping.

`bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_repair.bed -wo -F 0.5 | less`

#### 9. How many damage site overlapped to each region?

`bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_damage.bed -c | less`

#### 10. How long did the last operation take? 

`time bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_damage.bed -c > /dev/null`

Sort the bed files. How long did it take after sorting?

`sort -k1,1 -k2,2n UV_damage.bed > UV_damage_sorted.bed`

`time bedtools intersect -a hg38_gencode_v32_exons_introns.bed -b UV_damage_sorted.bed -c -sorted > /dev/null`

#### 11. Combine all the regions at exon and intron bed file that are closer each other than 100 base pairs.

`bedtools merge -i hg38_gencode_v32_exons_introns.bed -d 100 | less`

#### 12. Combine all the regions at exon and intron bed file that are closer each other than 100 base pairs. 

For each line, show how many regions combined.

`bedtools merge -i hg38_gencode_v32_exons_introns.bed -d 100 -c 1 -o count | less`

#### 13. Combine all the regions at exon and intron bed file that are closer each other than 100 base pairs. 

For each line, show how many regions combined. What are the names of the regions that are combined?

`bedtools merge -i hg38_gencode_v32_exons_introns.bed -d 100 -c 1,4 -o count,collapse | less`

#### 14. What are the regions that are NOT covered at exon and intron bed file?

`bedtools complement -i hg38_gencode_v32_exons_introns.bed -g genome.txt | less`

#### 15. What is the genome-wide coverage of the repair bed file?

`bedtools genomecov -i UV_damage_sorted.bed -g genome.txt`

#### 16. Get the regions 10 base pair up and downstream of the damage sites.

`bedtools flank -i UV_damage_sorted.bed -g /cta/groups/adebali/data/reference_genomes/human/gencode/38/genome.fa.fai -b 10 | less`

#### 17. Split the genome into 5 kb bins and get how many damage site overlaps those bins.

`bedtools makewindows -b genome.bed -w 5000 > genome_5kb.bed`

`bedtools intersect -a genome_5kb.bed -b UV_damage.bed | less`

#### 18. Convert repair bed file to fasta.

`bedtools getfasta -fi /cta/groups/adebali/data/reference_genomes/human/gencode/38/genome.fa -bed UV_repair.bed -fo UV_repair.fa`

