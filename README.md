# Théâtre Classique Stage Cleanup
The XML from http://theatre-classique.fr/index.html suffer from an especially bad markup for stage directions. Many stage directions are hidden in speech prefixes or scene/act headings. I have split them through a computer-assisted but mainly manual process. I have corrected some minor OCR mistakes as well. Aside from that, the files here are textually identical to Théâtre Classique as of January 2024.

# Two Types of Mistakes
## Stage in Speaker
In many cases, stage directions are hidden within speaker names, as the following example from Théâtre Classique's markup of Gaspard Abeille’s <i>Argélie, reine de Thessalie</i> (1673) shows:
```bash
<sp stage="fight" who="ISMÈNE"><speaker>ISMÈNE, se jetant sur Dione.</speaker>[…]</sp>
```
I have corrected mistakes like that as follows:
```bash
<sp who="ISMÈNE"><speaker>ISMÈNE</speaker><stage stage="fight">, se jetant sur Dione.</stage>[…]</sp>
```
## Stage in Head
Another common mistake is the conflation of stage directions and act/scene headings. Abeille’s <i>Argélie</i> again:
```bash
<head>SCÈNE I. Argélie, Clytie. </head>
```
I have corrected this in the following manner:
```bash
<head>SCÈNE I. </head><stage>Argélie, Clytie. </stage>
```

# Results
The number of stage directions has increased from 43,318 to 117,577, i.e., by over 271%. The number of words now explicitely marked up as stage directions has increased from 333,360 to 642,176, i.e., by over 190%.



