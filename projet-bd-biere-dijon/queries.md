Prix moyen de la bière par quartier:

```sql
SELECT q.nom, ROUND(AVG(p.valeur), 2)
FROM bars.quartier q
INNER JOIN bars.bar b ON b.id_quartier = q.id_quartier
INNER JOIN bars.reference_de_biere r ON r.id_bar = b.id_bar
INNER JOIN bars.prix p ON p.id_prix = r.id_prix
GROUP BY q.nom;
```

Bars vendant l'IPA la moins cher

```sql
SELECT bi.nom AS biere, b.nom AS bar, p.valeur AS prix
FROM bars.biere bi
INNER JOIN bars.reference_de_biere r on r.id_biere = bi.id_biere
INNER JOIN bars.prix p ON r.id_prix = p.id_prix
INNER JOIN bars.bar b ON r.id_bar = b.id_bar
WHERE bi.nom LIKE '%IPA%'
ORDER BY p.valeur ASC
LIMIT 5;
```
J'ai personnellement choisi d'afficher grâce à cette requête les 5 bars vendant l'IPA la moins chère.

Bières vendues dans >= 5 bars

```sql
SELECT bi.nom, COUNT(r.id_bar) AS nombre_de_bars
FROM bars.biere bi
INNER JOIN bars.reference_de_biere r ON r.id_biere = bi.id_biere
GROUP BY bi.nom
HAVING COUNT(r.id_bar) >= 5;
```

Bars sans bière à moins de 6€

```sql
SELECT b.nom, MIN(p.valeur) AS prix_minimal_biere
FROM bars.bar b
INNER JOIN bars.reference_de_biere r ON b.id_bar = r.id_bar
INNER JOIN bars.prix p ON r.id_prix = p.id_prix
GROUP BY b.nom
HAVING MIN(p.valeur) >= 6;
```
Dans le cas présent aucun bar n'est sans bière à moins de 6€