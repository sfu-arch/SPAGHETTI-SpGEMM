# SPAGHETTI

Sparse--Sparse matrix multiplications are widely used across multiple domains, but the regularity of the computation is dependent on the input sparsity pattern. The majority of recent hardware accelerators are based on the inner-product method and propose new storage formats to regularize the computation. We find that these storage formats are more suited for denser matrices and inherently suffer from high DRAM accesses for sparse matrices (<1% density). Some works have adopted the outer-product algorithm which targets matrices stored in CSR/CSC format and thus are better suited for highly sparse matrices. State-of-the-art outer-product accelerators condense inputs to improve output reuse, but then spoil input reuse, requiring a complex memory hierarchy (e.g., prefetch caches). The condensing effectiveness also varies across inputs (and even row-to-row within an input) which leads to high variance in DRAM utilization and speedup across inputs.

We propose SPAGHETTI, an open-source Chisel generator for creating FPGA-optimized sparse GEMM accelerators. The key novelty in Spaghetti is a new pattern-aware software scheduler that analyzes the sparsity pattern and schedules row-col pairs of the inputs onto the fixed microarchitecture. Spaghetti takes advantage of our observation that in outer-product the rows in the input matrix lead to mutually independent rows in the final output. Thus the scheduler can partition the input into tiles that maximize reuse and eliminate re-fetching of the partial matrices from the DRAM. The microarchitecture template we create has the following key benefits i) we can statically schedule the inputs in a streaming fashion and maximize DRAM utilization, ii) we can parallelize the merge phase and generate multiple rows of the output in parallel maximally using the output DRAM bandwidth, iii) we can adapt to the varying logic resources and bandwidth across various FPGA boards and attain maximal roofline performance (only limited by memory-bandwidth). We auto-generate GEMM accelerators on Amazon AWS FPGAs and demonstrate that we can achieve performance improvement over CPUs and GPUs between 1.1--34.5x. For highly sparse inputs compared to state-of-the-art outer-product accelerators, we improved performance on average 2.6x, and reduce DRAM accesses on average 4x compared to the state-of-the-art outer product accelerator.  

## Experimental Results

![Accelerator](https://www.dropbox.com/s/vg22bob4h3w3gbd/SpaghettiToSpArch_DRAM.png?raw=1)




