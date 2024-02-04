# Théâtre Classique Stage Cleanup
The XML from http://theatre-classique.fr/index.html suffer from an especially bad markup for stage directions. Many stage directions are hidden in speech prefixes or scene/act headings. I have split them through a computer-assisted but mainly manual process. I have corrected some minor OCR mistakes as well. Aside from that, the files here are textually identical to Théâtre Classique as of January 2024.

# Two typical of mistakes
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

# The rare and more interesting cases
Some of the markup decisions of TC go beyond the previously discussed predictable cases and introduce changes that reveal an underlying problem with our understanding of dramatic structure (what we might call the typographic dispositive [^1] or the typographic condition [^2] of drama). We generally expect the textual layers of dramatic literature to be divisible into dialog (the text spoken), speech prefixes (the name of the speaker), act/scene-headings (the macro-divisions of the text), and stage directions (the layer within the macro-structure that is distinct from dialog and speech prefixes). This dispositive seems so firmly anchored that we try to apply it even in highly divergent cases. 

## The anxiety about prefix-less speeches
We generally assume that dialogical passages in dramatic texts are introduced by a speech prefix. However, plentiful examples prove that a stage direction can do the trick as well. As an example, look at the following passage from Louis-Sébastien Mercier’s <i>La Destruction de la Ligue Ou La Réduction de Paris</i> (1782):

![Mercier_La Destruction de la Ligue ou La réduction de Paris (5)](https://github.com/jjokisch/theatre-classique-clean/assets/112176243/c88ab565-1f39-4df3-8945-9feecfc1e4ce)


Speech is here introduced by stage direction. This might look odd to us as we might expect to find:
> UNE VOIX <i>s'écriant au-dehors de la porte</i> <br/> Ouvrez, par miséricorde ; ouvrez, au nom de dieu : ouvrez, je vous en conjure

The original TC markup does not care much about that:
```bash
<sp who='UNE-VOIX'><speaker>Au-dehors de la porte une voix s'écrie : </speaker>
<p id='11'>
<s id='1'>Ouvrez, par miséricorde ; ouvrez, au nom de dieu : ouvrez, je vous en conjure !</s>
</p>
```
My correction changed the speaker node into a stage node, skipping the speaker node altogether:
```bash
<sp who='UNE-VOIX'><stage>Au-dehors de la porte une voix s'écrie : </stage>
<p id='11'>
<s id='1'>Ouvrez, par miséricorde ; ouvrez, au nom de dieu : ouvrez, je vous en conjure !</s>
</p>
```
We can find an even more egregious example in the TC markup of Jean de La Fontaine’s <i>Daphné</i> (1682). First, the beautiful the original passage:

![Poëmes_du_quinquinia_et_autres_ La_Fontaine_bpt6k576021](https://github.com/jjokisch/theatre-classique-clean/assets/112176243/8fd8421b-b4ac-4b5b-8ba5-e01b4585ec33)

TC, in its need for stage directions renders it as follows:
```bash
<stage type="action/danse">Mercure revole au ciel, ayant laissé Pégase sur le double mont. Quatre auteurs lyriques et
autant de Muses du même genre viennent danser en témoignage de joie ; puis les ridicules se mêlent avec eux, formant de
différentes figures avec des branches de laurier qu'ils portent tous, et dont ils se font des espèces de berceaux. C'est
le grand ballet. </stage>
</sp>
<sp stage="sing" who='UNE MUSE LYRIQUE'>
<speaker>Après qu'ils ont dansé une fois, UNE MUSE DU GENRE LYRIQUE chante ceci.</speaker>
<l id='896'>Il n'est que de s'enflammer ;</l>
```
The stage direction is primarily assigned to the previous speech, its last sentence plucked from the continuous paragraph to be assigned as a speaker to the following speech. Also note how <i>une Muse du genre lyrique</i> suddenly finds herself entirely in capital letters to emulate a dispositive that is absent from the original.

## The unspoken rule about unspoken stage directions
We generally expect stage directions to be devoid of any spoken words. After all, that is what the dialogic layer of the dramatic text is for. However, stage directions can and at times do contain spoken text. Denying this leads to some questionable markup choices. One of the many examples of stage directions with dialogic content in Stéphanie Félicité Genlis’ <i>L'île heureuse</i> (1781) reads:

![Genlis_-_Theatre_a_l_usage_des_jeunes_personnes_1_(1781) djvu](https://github.com/jjokisch/theatre-classique-clean/assets/112176243/5880c0a6-767e-4e91-a6ac-5cff5b3a6152)

Contrary to the clear typographic distinction of the text, TC encodes it as follows:
```bash
<sp who="ZULMÉE"><speaker>ZULMÉE.</speaker>
<p id="7"> <s id="1">C'est une multitude de pauvres gens qui l'attendaient à son passage.</s></p>
</sp>
<sp who="FOULE"><speaker>On entend crier distinctement.</speaker>
<p id="8"><s id="1">Vive la princesse Clarinde, vive notre généreuse bienfaitrice.</s></p>
</sp>
```
The stage direction is ripped apart and turned into a speech prefix and dialog. Additionally, the assigning of the stage direction to the Zulmée speech is ignored. There is even a textual change to force the text to conform to the unfitting interpretation inherent to the markup choice. My markup corrects these problems:
```bash
<sp who="ZULMÉE">
<speaker>ZULMÉE.</speaker>
<p id="7"><s id="1">C'est une multitude de pauvres gens qui l'attendaient à son passage.</s></p>
<stage>On entend crier distinctement : Vive la princesse Clarinde, vive notre généreuse bienfaitrice.</stage>
</sp>
```

## The impossible to encode stage direction
The TEI, ultimately just modelling the typographic dispositive, can encounter impossibility in fairly simple yet uncommon expressions. Since a combination of speech prefix and stage direction generally follows the syntax: PREFIX <i>direction</i> (as we have seen earlier), the TEI only allows a speaker node as the first child of a sp node. This makes the markup of structures like <i>direction</i> PREFIX (<i>direction</i>) impossible or at least needlessly complicated. A good example exists in Jean-Baptiste Legouvé’s <i>Attilie</i> (1776):

![Legouvé_Attilie tragédie 72](https://github.com/jjokisch/theatre-classique-clean/assets/112176243/ed7054ee-c596-4be4-821e-2b205c8769ab)

TC, unsurprisingly, treats it as:
```bash
<sp who='ADRIEN'><speaker>Pendant ce temps, ADRIEN.</speaker>
```
The TEI does not offer any better options. We might want to encode it as:
```bash
<sp who='ADRIEN'><stage>Pendant ce temps, </stage><speaker>ADRIEN.</speaker>
```
or
```bash
<sp who='ADRIEN'><stage>Pendant ce temps, <speaker>ADRIEN.</speaker></stage>
```
but the markup restrictions of TEI prohibit this. The best option the TEI might offer us is:
```bash
<stage>Pendant ce temps, </stage>
<sp who='ADRIEN'><speaker>ADRIEN.</speaker>
```
It should be obvious, however, that this is not a desirable outcome. The problem does not result from a general problem of overlapping hierarchies in the text, but simply from the arbitrary condition that a speaker node can only appear as the first child of a sp node.
A similar problem arises from the attribution of the same dialog to two speakers in Marc-René Montalembert <i>La statue</i> (1784):

![Montalembert_La statue-55](https://github.com/jjokisch/theatre-classique-clean/assets/112176243/03786f4e-8a77-4625-94b1-94d2aeb2afcf)

Ignoring the parallelism in speech expressed typographically through the two columns, the text assigns each column to two speakers, with each receiving their own direction. While it is common to have one dialogic passage delivered by two speaker, it becomes almost impossible once they receive their own stage directions. Thus, while the following markup might appear sensible, it is invalid within the TEI:
```bash
<sp who='LA-COMTESS:JULIE'>
<speaker>LA COMTESS</speaker>
<stage>au Marquis</stage>
<speaker>JULIE</speaker>
<stage>au Chevalier</stage>
```
Due to a lack of proper markup option, I opted to keep the handful of "impossible" stage directions in their original form as encoded by TC.

# Results
Employing my changes to the best of my knowledged, the number of stage directions has increased from 43,386 to 120,321, i.e., by over 277%. The number of words now explicitely marked up as stage directions has increased from roughly 340,000 to roughly 655,000, i.e., by over 190%.


[^1]: Wehde, Susanne. Typographische Kultur: eine zeichentheoretische und kulturgeschichtliche Studie zur Typographie und ihrer Entwicklung. M. Niemeyer, 2000, pp. 14, 125.
[^2]: Weber, Alexander. Episierung im Drama: ein Beitrag zur transgenerischen Narratologie. de Gruyter, 2017, p. 30.
