## Performance Analysis & Scalability:

Built for reSolVer model, this microhomology detection tool efficiently identifies overrepresented k-mers in genomic regions using a sliding-window approach. 
For small datasets (e.g., 1 kbp regions), it processes sequences in milliseconds, making it ideal for targeted analyses. 
However, runtime scales linearly with sequence length and exponentially with k-mer size (O(4ᵏ)), so performance varies significantly for larger inputs.

Processing a full human chromosome (e.g., chr20 at 64 Mbp) takes approximately 10 minutes for k=5, dominated by background frequency estimation (build_kmer_background). Whole-genome analysis (3 Gbp) is not recommended without optimization, as runtime extends to hours. Key bottlenecks include repeated FASTA parsing and the combinatorial explosion of possible k-mers (e.g., 16,384 combinations for k=7).

## Optimization Recommendations for reSolVer implementation:  

### Parallelize Background Sampling: 

Using multiprocessing (4 cores) reduces runtime by ~4× for large chromosomes.
Cache Background Frequencies: Store precomputed k-mer frequencies to avoid reprocessing (10× speedup for repeated runs).
Pre-index FASTA Files: Tools like samtools faidx enable rapid random access to genomic regions.
Adjust Parameters: For chromosomes >100 Mbp, reduce num_samples or increase sample_size in build_kmer_background to balance accuracy and speed.
Use Case Guidance

## Recommended: 
Focused studies of microhomology in specific loci (e.g., disease-associated breakpoints) with k≤7.
Not Recommended: Whole-genome analysis or k>7 due to computational constraints. For these, consider specialized SV callers like Manta or Delly.
For questions or collaboration opportunities, feel free to reach out!


