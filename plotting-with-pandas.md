# Plotting With Pandas

There are multiple plotting libraries one can use as a data scientist:

The ones I know are:

- matplotlib - the OG
- plotly - for more interactive plots
- seaborn for a more pleasing aesthetic, includes additonal statistical visualisations, still static

pandas can also be configured with alternate rendering backends.


eg,

```python
pd.options.plotting.backend = "plotly"
```

This way instead of using a functions call like:

```python
plt.plot(df[col].values())
```


we can directly plot without using the library explicitly:

```python
df.plot()
```

or

```python
df.plot.box()
```

or 

```python
# equivalent to df.plot.box()
df.boxplot()
```

However, each backend does not support all plot types. For example, with plotly backend we cannot do

df.plot.violin()

but we CAN do df.plot(kind='violin')

so make sure to check whether explicit calls are suported.

In general, either:

a) .plot.<PLOT TYPE> OR 
b) .plot(kind='<PLOT TYPE>') should be supported 

If not supported, you will ge the following error:

>NotImplementedError: kind='kde' not yet supported for plotting.backend='plotly'
