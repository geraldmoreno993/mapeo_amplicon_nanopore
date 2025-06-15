# mapeo_amplicon_nanopore
Pipeline con BWA para mapeo de reads de Nanopore

#bwa index reference.fasta

bwa index reference.fasta
bwa mem -t 4 reference.fasta reads.fastq > mapeado.sam
samtools view -bS mapeado.sam > mapeado.bam
samtools sort mapeado.bam -o mapeado.sorted.bam
samtools index mapeado.sorted.bam
samtools faidx reference.fasta

for r1 in *bam
do
prefix=$(basename $r1 .bam)
#2#estimate Ns#
samtools mpileup -aa -A -d 0 -Q 0 $r1 | ivar consensus -p ${prefix}.fasta -q 25 -t 0.6 -m 10 ;
done ; 
ls ;
