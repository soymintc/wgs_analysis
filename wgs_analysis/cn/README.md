# Copy number analysis
```python
from wgs_analysis.cn import cn_utils
```

## Initiate CN data for selected cohorts
```python
gene_list_path = '/juno/work/shah/users/chois7/tickets/cohort-cn-qc/resources/gene_list.txt'
cohorts = ['SPECTRUM', 'Metacohort'] # or ['SPECTRUM'], ['Metacohort'], ['SPECTRUM-DLP']
cn = cn_utils.CopyNumberChangeData(gene_list=gene_list_path, cohorts=cohorts)
cohort_symbol = '_'.join(cn.cohorts) # for filename
```

## Plot cohort-level aggregated CN 
```python
for signature in cn.signature_counts:
    if cn.signature_counts[signature] > 5:
        cn.plot_pan_chrom_cn(group=signature, out_path=f'{cohort_symbol}.{signature}.pdf')
        cn.plot_per_chrom_cn(group=signature, out_path=f'{cohort_symbol}.{signature}.per-chrom.pdf')
```

## Fisher's exact test for CN enrichment in signatures
```python
gene_cn = cn.get_gene_cn_counts()
results = cn_utils.evaluate_enrichment(cn.signatures, cn.signature_counts, 
    cn.gene_list, cn.sample_counts, padj_cutoff=0.05)
results.to_csv(f'enrichment.{cohort_symbol}.tsv', sep='\t', index=False)
```
