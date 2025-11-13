En dataframe kan anses som en tabel.

Den nemmeste måde at lave en dataframe på er bruge en dictionary:

```
import pandas as pd

data = {
  'city': ['Brooklyn', 'Seoul', 'Barcelona', 'Mexico City'],
  'country': ['US', 'South Korea', 'Spain', 'Mexico'],
  'population': [2646000, 9411000, 1636000, 9209944]
}

df = pd.DataFrame(data)
```

Man kan også gøre det fra en liste og manuelt angive kolonne navne:
```
import pandas as pd

# En samling af lister med innerlists
data = [
  ['Brooklyn', 'US', 2646000],
  ['Seoul', 'South Korea', 9411000],
  ['Barcelona', 'Spain', 1636000],
  ['Mexico City', 'Mexico', 9209944]
]

df = pd.DataFrame(data, columns=['city', 'country', 'population'])
```

Man kan også indlæse fra en CSV fil:
```
import pandas as pd

df = pd.read_csv('my_filename.csv')
```

Har man brug for at indlæse filer med en anden separator end kommaer, kan man også bruge *pd.read_csv* men med angivelse af en parameter:
```
import pandas as pd

df = pd.read_csv('my_filename.tsv', delimiter='\t')
```