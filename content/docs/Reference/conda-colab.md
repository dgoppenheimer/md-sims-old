---
categories: ["Reference"]
tags: ["conda"]
title: "Conda Install"
linkTitle: "Conda Install"
date: 2022-02-05
description: >
  A quick guide to installing `conda` on Google Colab
resources:
- src: ""
  params:
    byline: ""
---

Some packages recommend using `conda` for installation on Google Colab. Here is a quick guide to doing so.

See [How to install / use Conda on Google Colab](https://inside-machinelearning.com/en/how-to-install-use-conda-on-google-colab/), a guide to installing Conda when using Google Colab.

1. Check to see if conda is already installed:

```py
!conda --version
```

You should get the error, `/bin/bash: conda: command not found`.

2. Install conda:

```py
!pip install -q condacolab
import condacolab
condacolab.install()
```

Note that the kernel will reboot and you will see an error stating that *Your session crashed for an unknown reason.* It is okay to dismiss this notice.

3. Confirm that installation was successful:

```py
!conda --version
```

You should get `conda 4.9.2`. Success!

Now you can use conda for installation of other packages that are not available by `pip` or that run better after installation with `conda`.
