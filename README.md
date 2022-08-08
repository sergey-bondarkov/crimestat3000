# crimestat3000

A tool for automated parsing of Russian crime statistics reports (form 4-ЕГС) from [crimestat.ru](http://crimestat.ru). All you need to know is which section of report you need, which sheets and columns. (**Beware**: these tend to change over the years so make sure to check for that and if needed separate you parsing process into several parts with different configurations.) 

There's no need to download files manually -- `crimestat3000` will take care of that without generating temporary files. But if you happend to have the files locally pass the path to their location to `local_dir` argument to slightly increase processing speed. 

A 4-ЕГС report shows cumulative sums since the beginning of the year. By default `crimestat3000` turns them into monthly values -- one can swith it off by setting `cumsum` argument to `True`.

You can also optionally specify the level of detail you need. Some sheets contain information on a previously mentioned article's specific part or paragraph -- you can drop those or keep those or just start by parsing all the sheets there are to decide knowingly later. Finally you can set `shorten_descr` argument to `True` to turn column names like `Строка 12: умышленное причинение легкого вреда здоровью, совершенное по мотивам политической, идеологической, расовой, национальной или религиозной ненависти или вражды либо по мотивам ненависти или вражды в отношении какой-либо социальной группы п. «б» ч. 2 ст. 115 УК РФ` to `115_ч2_б`.

Here's an example call:
```
import crimestat3000 as cs

kwargs = {
    'first_month': '01-2016',
    'last_month' : '12-2016',
    'section'    : 2,

    # optional arguments                                defaults
    # ==================                                ========
    # 'sheets'       : {'all' or list of sheets}        # 'all'
    # 'keep'         : {'all', 'article', 'article+'}   # 'all'
    'columns'      : ['C', 'E'],                        # 'C', usually the sheet's total
    'shorten_descr': True                               # False
    # 'local_dir'    : {None, path to local directory}  # None
    # 'cumsum'       : {True, False}                    # False
}

table_2016 = cs.parse.period(**kwargs)
```

`OLE2 inconsistency` warnings may pop up sometimes -- don't worry about that: it happens while reading an .xls file content into pandas because some of the files at crimestat.ru are a little bit malformed -- but it doesn't affect anything.


