# adversarial-postspec
The implementation of a GAN with an auxiliary distance loss to post-specialise word embeddings. Code for the paper:

Edoardo Maria Ponti, Ivan Vulić, Goran Glavaš, Nikola Mrkšić, and Anna Korhonen. 2018. **Adversarial Propagation and Zero-Shot Cross-Lingual Transfer of Word Vector Specialization**. In Proceedings of EMNLP 2018.

If you use this software for academic research, please cite the paper in question:
```
@inproceedings{ponti2018adversarial,
  title={Adversarial Propagation and Zero-Shot Cross-Lingual Transfer of Word Vector Specialization},
  author={Ponti, Edoardo Maria and Vulić, Ivan and Glavaš, Goran and Mrkšić, Nikola and Korhonen, Anna},
  booktitle={Proceedings of EMNLP 2018},
  year={2018}
}
```

## Directory structure

* code: contains main scripts to train and test AuxGAN for post-specialization
* evaluation: script and databases for intrinsic evaluation
* results: where the output of the post-specialization procedure is saved
* vectors: contains the training data (i.e. original vectors and vectors specialized by Attract-Repel)
* vocab: list of words to be excluded from the training data because they are present in the evaluation databases.

## Data

All SGNS-BOW2 and Glove300 vectors (and necessary subsets of vectors) are available from Google Drive at the following [link](https://drive.google.com/open?id=1K5VJTHFPql5WvtYB-GiZrgSgayh54KNB).

## Train

```
 cd code
 python adversarial.py --seen_file ../vectors/SEEN_VECTORS --adjusted_file ../vectors/AR_SPECIALIZED_VECTORS \\
     --unseen_file ../vectors/ALL_ORIGINAL_VECTORS --out_dir ../results/EXPERIMENT_NAME 
```

After completing the epochs, the script saves two files in the folder specified as ```--out_dir```: ```gold_embs.txt``` and ```silver_embs.txt```. They are based on two different settings where AR specialized vectors are saved when available (gold) and only post-specialized vectors are saved (silver). The paper reports the gold setting.

## Evaluate

To evaluate with SimLex-999 (or SimVerb-3500), you have to call the evaluation script in the ```evaluation/``` directory:

```
python simlex_evaluator.py simlexorig999.txt ../out_dir/<output_file>
```

Our papers reports state-of-art scores for both SimLex and SimVerb: in the disjoint setting, the words appearing in such datasets were discarded from the Attract-Repel constraints.

<table>
  <tr>
    <td> </td>
    <td colspan="6">Disjoint</td>
    <td colspan="6">Full</td>
  </tr>
  <tr>
    <td> </td>
    <td colspan="2">glove-cc</td> <td colspan="2">fasttext</td> <td colspan="2">sgns-w2</td>
    <td colspan="2">glove-cc</td> <td colspan="2">fasttext</td> <td colspan="2">sgns-w2</td>
  </tr>
  <tr>
    <td> </td>
    <td> SL </td> <td> SV </td> <td> SL </td> <td> SV </td> <td> SL </td> <td> SV </td>
    <td> SL </td> <td> SV </td> <td> SL </td> <td> SV </td> <td> SL </td> <td> SV </td>
  </tr>
  <tr>
<td>Distributional</td> <td>.407</td> <td>.280</td> <td>.383</td> <td>.247</td> <td>.414</td> <td>.272</td> <td>.407</td> <td>.280</td> <td>.383</td> <td>.247</td> <td>.414</td> <td>.272</td> </tr>
  <tr>
<td>Specialized: Attract-Repel</td> <td>.407</td> <td>.280</td> <td>.383</td> <td>.247</td> <td>.414</td> <td>.272</td> <td>.781</td> <td>.761</td> <td>.764</td> <td>.744</td> <td>.778</td> <td>.761</td> </tr>
  <tr>
<td>Post-Specialized: MLP</td> <td>.645</td> <td>.531</td> <td>.503</td> <td>.340</td> <td>.553</td> <td>.430</td> <td>.785</td> <td>.764</td> <td>.768</td> <td>.745</td> <td>.781</td> <td>.763</td> </tr>
  <tr>
<td>Post-Specialized: AuxGAN</td> <td><b>.652</b></td> <td><b>.552</b></td> <td><b>.513</b></td> <td><b>.394</b></td> <td><b>.581</b></td> <td><b>.434</b></td> <td>.789</td> <td>.764</td> <td>.766</td> <td>.741</td> <td>.782</td> <td>.762</td> </tr>
</table>

## Acknowledgements

Part of the code has been borrowed from the GAN implementation in [MUSE](https://github.com/facebookresearch/MUSE), with some changes. The link contains a copy of the original license.
