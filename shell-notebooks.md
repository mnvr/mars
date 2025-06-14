### Jupyter notebooks with a bash kernel

[Jupyter](https://jupyter.org/) is many things, for historical reasons, but for our current purpose, we need only one of them - the Jupyter notebook.

```sh
apk search jupyter
apk add jupyter-notebook
```

We want to run shell commands, and for this we need to install the [bash_kernel](https://github.com/takluyver/bash_kernel) (and bash).

```sh
apk add bash
apk add py3-pip
pip install --break-system-packages bash_kernel
python -m bash_kernel.install
```

Now we can run

```sh
jupyter notebook
```

The next cell in this notebook runs a bash command interactively and stores its output within the notebook.


```bash
ls
```

    [0;0mLICENSE[m                [0;0mgit.ipynb[m
    [0;0mREADME.md[m              [0;0mshell-notebooks.ipynb[m


### Conversion to HTML

```sh
jupyter nbconvert --to html --template classic *.ipynb
```
