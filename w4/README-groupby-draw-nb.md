---
jupytext:
  cell_metadata_filter: all, -hidden, -heading_collapsed, -run_control, -trusted
  notebook_metadata_filter: all, -jupytext.text_representation.jupytext_version, -jupytext.text_representation.format_version,
    -language_info.version, -language_info.codemirror_mode.version, -language_info.codemirror_mode,
    -language_info.file_extension, -language_info.mimetype, -toc
  text_representation:
    extension: .md
    format_name: myst
kernelspec:
  display_name: Python 3 (ipykernel)
  language: python
  name: python3
language_info:
  name: python
  nbconvert_exporter: python
  pygments_lexer: ipython3
---

# dessins par groupby

et lire plusieurs feuillets depuis excel

+++

le fichier excel `data/groupby-draw.xlsx` contient ici plusieurs feuillets

+++

![](media/groupby-draw-excel.png)

+++

les deux feuillets contiennent 2 données différentes pour les mêmes sites / dates

+++

## ce qu'il faut faire

+++

A. visualiser les deux données (Sessions et Waiters) en fonction du temps par site

e.g.

<img src="media/groupby-draw-sessions.png" width=600px>

+++

B. mêmes chiffres mais agrégés sur les sites, les deux caratéristiques sur une seule figure (ici avec seaborn)

<img src="media/groupby-draw-both.png" width=600px>

```{code-cell} ipython3
# à vous

# imports ...
import pandas as pd
xls = pd.ExcelFile('data/groupby-draw.xlsx')
print(xls.sheet_names)
df1 = pd.read_excel(xls, 'Sessions')
df2 = pd.read_excel(xls, 'Waiters')
df2
```

```{code-cell} ipython3
df1_group =df1.groupby('Site Name')

df1_Riv=df1_group.get_group('Riviera Mall')
df1_Car=df1_group.get_group('Carlton')
df1_Rit=df1_group.get_group('Ritz')
df1_Esq=df1_group.get_group('Esquinade')

df1_Riv=df1_Riv.set_index(['Date'])
df1_Rit=df1_Rit.set_index(['Date'])
df1_Car=df1_Car.set_index(['Date'])
df1_Esq=df1_Esq.set_index(['Date'])

df1_plot=pd.DataFrame(index=df1['Date'])

df1_plot['Riviera Mall']=df1_Riv['Sessions']
df1_plot['Carlton']=df1_Car['Sessions']
df1_plot['Ritz']=df1_Rit['Sessions']
df1_plot['Esquinade']=df1_Esq['Sessions']

df1_plot.plot()
#pour le deuxiemme feuillet il suffit de remplacer df1 par df2 et Sessions par waiters...
```

```{code-cell} ipython3
df2_group=pd.pivot_table(df2,columns='Site Name',index='Date',values='Waiters')
df2_group.plot()
df1_group=pd.pivot_table(df1,columns='Site Name',index='Date',values='Sessions')
df1_group.plot()
```

```{code-cell} ipython3
df2_group =df2.groupby('Site Name')

df2_Riv=df2_group.get_group('Riviera Mall')
df2_Car=df2_group.get_group('Carlton')
df2_Rit=df2_group.get_group('Ritz')
df2_Esq=df2_group.get_group('Esquinade')

df2_Riv=df2_Riv.set_index(['Date'])
df2_Rit=df2_Rit.set_index(['Date'])
df2_Car=df2_Car.set_index(['Date'])
df2_Esq=df2_Esq.set_index(['Date'])

df2_plot=pd.DataFrame(index=df2['Date'])

df2_plot['Riviera Mall']=df2_Riv['Waiters']
df2_plot['Carlton']     =df2_Car['Waiters']
df2_plot['Ritz']        =df2_Rit['Waiters']
df2_plot['Esquinade']   =df2_Esq['Waiters']

df2_plot.plot()
```

```{code-cell} ipython3
#2e question
#sans seaborn c'est pas beau mais cohérent avec la correction
import matplotlib.pyplot as plt
df1['Waiters']=df2['Waiters']
df1_date=df1.set_index(['Date'])[['Sessions','Waiters']]
plt.style.use('seaborn')
plt.errorbar(, data, yerr=marge,fmt='.b',capsize=5);
df1_date.plot()
```

---
