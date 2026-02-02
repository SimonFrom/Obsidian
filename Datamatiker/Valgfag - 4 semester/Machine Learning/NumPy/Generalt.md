[NumPy quick tutorial](https://colab.research.google.com/github/google/eng-edu/blob/main/ml/cc/exercises/numpy_ultraquick_tutorial.ipynb?hl=da#scrollTo=7cT9fXS_JUpa "NumPy quick tutorial")


NumPy er et bibliotek til at skabe og manipulere matrixer.

*En matrix er et matematisk objekt til at opbevare tal i rækker og kolonner*

I Python vil en matrix være en liste data type. 
NumPy kalder dem for arrays, TensorFlow vil tensors. Kært barn mange navne.

```
- Med .array() kan man lave et en dimensionelt array:

one_dimensional_array = np.array([1.2, 2.4, 3.5, 4.7, 6.1, 7.2, 8.3, 9.5])

- Curly brackets fungerer som escape, i den forstand at sætter man flere kan man lave fler dimensionerede arrays:

multi_dimensional_array = np.array([1.2, 2.4], [3.5, 4.7], [6.1, 7.2], [8.3, 9.5])

Man kan også kalde .zeros() eller .ones() for at fylde et array med 0 eller 1.

- NumPy tillader også at man bearbejder hele arrayet med matematiske operationer, plus og minus f.eks.

base_array = np.array([1, 2, 3])
add_one_to_each_cell + 23




```

